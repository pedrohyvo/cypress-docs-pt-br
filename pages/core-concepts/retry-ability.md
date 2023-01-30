# Capacidade de repetição

```markdown
## O que você aprenderá

- Como o Cypress tenta novamente comandos e afirmações
- Quando comandos são repetidos e quando não são
- Como lidar com algumas situações de testes esquisitos
```

Um recurso central do Cypress que auxilia no teste de aplicativos da Web dinâmicos é a capacidade de repetição.
Assim como uma boa transmissão em um carro, geralmente funciona sem que você perceba. Mas entender como isso 
funciona ajudará você a escrever testes mais rápidos com menos surpresas em tempo de execução.

```markdown
**Novas tentativas de teste**
Se você deseja repetir os testes um número configurado de vezes quando o teste falhar, consulte nosso guia sobre novas tentativas de teste.
```

## Comandos, Consultas e Asserções​

Embora todos os métodos que você encadeia de cy em seus testes do Cypress sejam comandos, existem alguns tipos diferentes
de comandos que é importante entender: consultas, asserções e ações têm regras especiais sobre a capacidade de repetição.
Por exemplo, existem 4 consultas, uma ação e 2 asserções no teste abaixo.

```JS
it('creates an item', () => {
  cy.visit('/')

  cy.focused() // consulta
    .should('have.class', 'new-todo') // asserção

  cy.get('.new-todo') // consulta
    .type('todo A{enter}') // ação

  cy.get('.todoapp') // consulta
    .find('.todo-list li') // consulta
    .should('have.length', 1) // asserção
})
```

O [log de comandos](https://docs.cypress.io/guides/core-concepts/cypress-app#Command-Log) mostra todos os comandos,
independentemente dos tipos, com asserções de passagem exibidas em verde.

Vejamos a última cadeia de comandos:

```JS
cy.get('.todoapp') // consulta
  .find('.todo-list li') // consulta
  .should('have.length', 1) // asserção
```

Como nada é síncrono em aplicativos da Web modernos, o Cypress não pode consultar todos os elementos DOM correspondentes
a `.todo-list li` e verificar se há exatamente um deles. Há muitos exemplos de por que isso não funcionaria bem.

- E se o aplicativo não tiver atualizado o DOM no momento em que esses comandos forem executados?
- E se o aplicativo estiver esperando que seu back-end responda antes de preencher o elemento DOM?
- E se o aplicativo fizer algum cálculo intensivo antes de mostrar os resultados no DOM?

Portanto, `cy.get` e `cy.find()` precisam ser mais inteligentes e esperar que o aplicativo seja potencialmente atualizado.
`cy.get()` consulta o DOM do aplicativo, localiza os elementos que correspondem ao seletor e os passa para
`.find('.todo-list li')`. `.find()` localiza um novo conjunto de elementos e tenta a asserção a seguir 
(no nosso caso `should('have.length', 1)`) na lista de elementos encontrados.

- ✅ Se a asserção que segue `cy.find()` for aprovada, a consulta será concluída com sucesso.
- 🚨 Se a asserção que segue `cy.find()` falhar, o Cypress repetirá a consulta ao DOM do aplicativo novamente - começando
do topo da lista de cadeia. Em seguida, o Cypress tentará a afirmação contra os elementos gerados por `cy.get().find()`.
Se a asserção ainda falhar, o Cypress continua tentando até que o tempo limite de `cy.find()` seja atingido.

A capacidade de repetição permite que o teste conclua cada consulta assim que a asserção for aprovada, sem esperas embutidas
no código. Se seu aplicativo leva alguns milissegundos ou mesmo segundos para renderizar cada elemento DOM - o que não é
grande coisa, o teste não precisa ser mudado. Por exemplo, vamos introduzir um atraso artificial de 3 segundos ao atualizar
a interface do usuário do aplicativo abaixo em um exemplo de código de modelo TodoMVC:

```JS
app.TodoModel.prototype.addTodo = function (title) {
  this.todos = this.todos.concat({
    id: Utils.uuid(),
    title: title,
    completed: false,
  })

  // let's trigger the UI to render after 3 seconds
  setTimeout(() => {
    this.inform()
  }, 3000)
}
```

O teste ainda passa! `cy.get('.todo-list')` passa imediatamente - a lista de tarefas existe - mas
`.find('.todo-list li').should('have.length', 1)` mostra os indicadores giratórios, o que significa que o Cypress está
buscando novamente por eles.

Dentro de alguns milissegundos após as atualizações do DOM, `cy.find()` encontra um elemento e a asserção
`should('have.length', 1)` passa.

## Múltiplas afirmações

Consultas e asserções são sempre executadas em ordem e sempre são repetidas 'do início'. Se você tiver várias asserções,
o Cypress tentará novamente até que cada uma passe antes de passar para a próxima.

Por exemplo, o teste a seguir tem asserções `.should()` e `.and().`. `.and()` é um alias do comando `.should()`, então a
segunda asserção é realmente uma asserção de retorno de chamada personalizada na forma da função `.should(cb)` 
com 2 asserções `expect` dentro dela.

```JS
it('cria dois itens', () => {
   cy.visit('/')

   cy.get('.new-todo').type('todo A{enter}')
   cy.get('.new-todo').type('todo B{enter}')

   cy.get('.todo-list li') // consulta
     .should('have.length', 2) // asserção
     .e(($li) => {
       // mais 2 asserções
       expect($li.get(0).textContent, 'primeiro item').to.equal('todo a')
       expect($li.get(1).textContent, 'segundo item').to.equal('todo B')
     })
})
```

Como a segunda asserção `expect($li.get(0).textContent, 'primeiro item').to.equal('todo a')` falha, a terceira asserção
nunca é alcançada. O comando falha após atingir o tempo limite e o Log de Comando mostra corretamente que a primeira 
asserção encontrada `should('have.length', 2)` passar, mas a segunda asserção e o próprio comando falham.

## Asserções integradas

Frequentemente, um comando Cypress possui asserções integradas que farão com que as consultas anteriores sejam repetidas.
Por exemplo, a consulta `.eq()` será repetida mesmo sem nenhuma asserção anexada até encontrar um elemento com o índice fornecido.

```JS
cy.get('.todo-list li') // consulta
   .should('have.length', 2) // asserção
   .eq(3) // consulta
```

Somente as consultas podem ser repetidas, mas a maioria dos outros comandos ainda possui espera integrada. Por exemplo,
conforme descrito na seção "Asserções" de [.click()](https://docs.cypress.io/api/commands/click), o comando de ação `click()`
espera para clicar até que o elemento se torne [acionável](https://docs.cypress.io/guides/core-concepts/interacting-with-elements#Actionability),
incluindo a reexecução da cadeia de consulta que leva a ele caso a página seja atualizada enquanto estou esperando.

Cypress tenta agir como um usuário humano usando o navegador.

- Um usuário pode clicar no elemento?
- O elemento é invisível?
- O elemento está atrás de outro elemento?
- O elemento tem o atributo `desativado`?
- Ações - como `.click()` - esperam automaticamente até que várias asserções incorporadas como essas sejam aprovadas e,
 então, ele tentará clicar apenas uma vez.

## Tempo limite

Por padrão, cada comando que tenta rodar novamente o faz por até 4 segundos - de acordo com a configuração `defaultCommandTimeout`.

### Aumentar o tempo para tentar novamente

Você pode alterar o tempo limite padrão para todos os comandos. Consulte [Configuração: opções de substituição para 
obter exemplos de substituição dessa opção](https://docs.cypress.io/guides/references/configuration#Overriding-Options).

Por exemplo, para definir o tempo limite de comando padrão para 10 segundos por meio da linha de comando:

```JS
cypress run --config defaultCommandTimeout=10000
```

Não recomendamos alterar o tempo limite do comando globalmente. Em vez disso, passe a opção `{ timeout: ms }` do 
comando individual para tentar novamente por um período de tempo diferente. Por exemplo:

```JS
// modificamos o tempo limite que afeta o padrão + asserções adicionadas
cy.get('[data-testid="mobile-nav"]', { timeout: 10000 })
   .should('ser.visível')
   .and('contém', 'Casa')
```

O Cypress tentará novamente por até 10 segundos para encontrar um elemento visível com `data-testid` atributo 
`mobile-nav` com texto contendo "Home". Para obter mais exemplos, leia a seção
 [Timeouts](https://docs.cypress.io/guides/core-concepts/introduction-to-cypress#Timeouts) no guia "Introdução ao Cypress".

### Desativar nova tentativa

Substituir o tempo limite para 0 basicamente desabilitará a repetição da consulta ou a espera por outra, pois gastará
 0 milissegundos tentando novamente.

```JS
// verifica sincronizadamente se o elemento não existe (sem tentar novamente)
// por exemplo, logo após uma renderização do lado do servidor
cy.get('[data-testid="ssr-error"]', { timeout: 0 }).should('not.exist')
```

## Apenas as consultas são tentadas novamente

Qualquer comando que não seja uma consulta, como `.click()`, segue regras diferentes das consultas. O Cypress tentará 
novamente todas as consultas que levam a um comando, e tentará novamente todas as asserções após um comando, mas os 
próprios comandos nunca serão tentados novamente - nem qualquer outra até que sejam resolvidos.

A maioria dos comandos não é tentado novamente porque pode alterar o estado do aplicativo em teste. Por exemplo, o Cypress
não tentará novamente o comando de ação [.click()](https://docs.cypress.io/api/commands/click), porque isso pode alterar
algo no aplicativo. Depois que o clique ocorrer, o Cypress também não executará novamente nenhuma consulta antes de `.click()`.

### As ações devem estar no final das cadeias, não no meio

O teste a seguir pode ter problemas se:

- Sua estrutura JS renderizada novamente de forma assíncrona
- O código do seu aplicativo reagiu ao acionamento de um evento e removeu o elemento

### ⚠️ Comandos encadeados incorretamente

```JS
cy.get('.new-todo')
   .type('todo A{enter}') // ação
   .type('todo B{enter}') // ação após outra ação - ruim
   .should('have.class', 'active') // asserção após uma ação - ruim
```

### ✅ Terminar cadeias corretamente após uma ação

Para evitar totalmente esses problemas, é melhor dividir a cadeia de comandos acima.

```JS
cy.get('.new-todo').type('todo A{enter}')
cy.get('.new-todo').type('todo B{enter}')
cy.get('.new-todo').should('have.class', 'active')
```

Escrever seus testes dessa maneira ajudará a evitar problemas em que a página é renderizada novamente no meio do teste 
e o Cypress perde o controle de quais elementos deveria estar operando ou afirmando. Aliases - `cy.as()` - podem ajudar
a tornar esse padrão menos intrusivo.

```JS
cy.get('.new-todo').as('new')

cy.get('@new').type('todo A{enter}')
cy.get('@new').type('todo B{enter}')
cy.get('@new').should('have.class', 'active')
```

> Muito raramente você pode querer repetir um comando como `.click().` > Descrevemos um caso como esse em que os 
> ouvintes de evento são anexados a um pop-up modal somente após um atraso, fazendo com que os eventos padrão disparados
> durante `.click()` não sejam registrados. Neste caso especial, você pode querer "continuar clicando" até que o evento 
> seja registrado e a caixa de diálogo desapareça.
> Leia sobre isso em Quando o teste pode clicar? postagem no blog.
>
> Por causa das asserções incorporadas a cada comando e comandos de ação em particular, você raramente precisará desse padrão.

Como outro exemplo, ao confirmar que o componente de botão invoca o teste `click` prop com a biblioteca de montagem 
[cypress/react](https://github.com/cypress-io/cypress/tree/master/npm/react), o seguinte teste pode ou não funcionar:

### ⚠️ Verificando incorretamente se o stub foi chamado

```JS
const Clicker = ({ click }) => (
   <div>
     <button onClick={click}>Clique em mim</button>
   </div>
)

it('chama a prop click duas vezes', () => {
   const onClick = cy.stub()
   cy.mount(<Clicker click={onClick} />)
   cy.get('botão')
     .click()
     .click()
     .then(() => {
       // funciona neste caso, mas não recomendado
       // porque .click() e .then() não tentam novamente
       esperar(onClick).to.be.calledTwice
     })
})
```

O exemplo acima falhará se o componente chamar o `click` prop após um atraso.

```JS
const Clicker = ({ click }) => (
   <div>
     <button onClick={() => setTimeout(click, 500)}>Clique em mim</button>
   </div>
)
```

O teste termina antes que o componente chame a propriedade `click` duas vezes e sem repetir a asserção
 `expect(onClick).to.be.calledTwice`.

Também pode falhar se o React decidir renderizar novamente o DOM entre os cliques.

### ✅ Esperando corretamente que o stub seja chamado

Recomendamos criar um alias para o stub usando o comando `.as` e usando asserções `cy.get('@alias').should(...)`.

```JS
it('chama o click prop', () => {
   const onClick = cy.stub().as('clicker')

   cy.mount(<Clicker click={onClick} />)
   // Boa prática 💡: Não encadeie nada fora dos comandos
   cy.get('botão').click()
   cy.get('botão').click()

   // Boa prática 💡: Referencie o stub com um alias
   cy.get('@clicker').should('have.been.calledTwice')
})
```

### Use `.should()` com um retorno de chamada

Se você estiver usando comandos, mas precisar repetir toda a cadeia, considere reescrever os comandos em um 
[.should(callbackFn)](https://docs.cypress.io/api/commands/should#Function).

Abaixo está um exemplo onde o valor do número é definido após um atraso:

```JS
<div class="número-aleatório-exemplo">
   Número aleatório: <span id="número-aleatório">🎁</span>
</div>
<script>
   const el = document.getElementById('número aleatório')
   setTimeout(() => {
     el.innerText = Math.floor(Math.random() * 10 + 1)
   }, 1500)
</script>
```

### ⚠️ Aguardando valores incorretamente

Você pode querer escrever um teste como abaixo, para testar se o número está entre 1 e 10, embora
**isso não funcione conforme o esperado**. O teste produz os seguintes valores, anotados nos comentários, antes de falhar.

```JS
// ERRADO: este teste não funcionará como pretendido
cy.get('[data-testid="número-aleatório"]') // <div>🎁</div>
   .invoke('texto') // "🎁"
   .then(parseFloat) // NaN
   .should('be.gte', 1) // falha
   .and('be.lte', 10) // nunca avalia
```

Infelizmente, o comando **.then()** não é repetido. Assim, o teste executa toda a cadeia apenas uma vez antes de falhar.

### ✅ Aguardando corretamente os valores

Precisamos tentar novamente obter o elemento, invocar o método `text()`, chamar a função `parseFloat` e executar as
asserções `gte` e `lte`. Podemos conseguir isso usando o `.should(callbackFn)`.

```JS
cy.get('[data-testid="número-aleatório"]').should(($div) => {
   // todo o código aqui dentro irá tentar novamente
   // até passar ou expirar
   const n = parseFloat($div.text())

   expect(n).to.be.gte(1).and.be.lte(10)
})
```

O teste acima tenta novamente obter o elemento e invocar o texto do elemento para obter o número. 
Quando o número é finalmente definido no aplicativo, as asserções `gte` e `lte` são aprovadas e o teste é aprovado.

## Veja também

- Leia nossas postagens de blog sobre como combater [testes falhos](https://cypress.io/blog/tag/flake/).
- Você pode adicionar capacidade de repetição a seus próprios
 [comandos personalizados](https://docs.cypress.io/api/cypress-api/custom-commands) e consultas.
- Você pode repetir qualquer função com asserções anexadas usando os plug-ins de terceiros 
[cypress-pipe](https://github.com/NicholasBoll/cypress-pipe) e [cypress-wait-until](https://github.com/NoriSte/cypress-wait-until).
- O plug-in de terceiros [cypress-recurse](https://github.com/bahmutov/cypress-recurse) pode ser usado para implementar
[o teste visual com capacidade de repetição para elementos da tela](https://glebbahmutov.com/blog/canvas-testing/).
- Para saber como habilitar a funcionalidade de novas tentativas de teste do Cypress, que tenta novamente os testes que 
falham, confira nosso guia oficial sobre Novas [tentativas de teste](https://docs.cypress.io/guides/guides/test-retries).
