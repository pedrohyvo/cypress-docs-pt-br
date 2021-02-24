## Cadeias de Comandos

É muito importante entender o mecanismo que o Cypress usa para encadear comandos. Ele gerencia uma cadeia de Promises para você, e cada comando gera um "sujeito" para o próximo comando, até que a cadeia termine ou seja encontrado um erro. O desenvolvedor geralmente não precisa usar Promises diretamente, mas é útil entender como elas funcionam!

### Interagindo com Elementos

Como vimos no exemplo inicial, o Cypress permite clicar e digitar em elementos da página usando os comandos [`.click()`](https://docs.cypress.io/api/commands/click.html) e [`.type()`](https://docs.cypress.io/api/commands/type.html) com um comando [`cy.get()`](https://docs.cypress.io/api/commands/get.html) ou [`cy.contains()`](https://docs.cypress.io/api/commands/contains.html). Este é um ótimo exemplo de encadeamento em ação. Vamos vê-lo novamente:

```JS
cy.get("textarea.post-body")
  .type("Este é um ótimo artigo");
```

Estamos encadeando [`.type()`](https://docs.cypress.io/api/commands/type.html) no comando [`cy.get()`](https://docs.cypress.io/api/commands/get.html), solicitando que ele digite no sujeito gerado pelo comando [`cy.get()`](https://docs.cypress.io/api/commands/get.html), que será um elemento do DOM.

Veja a seguir outros comandos de ação que o Cypress oferece para interagir com seu aplicativo:

- [`.blur()`](https://docs.cypress.io/api/commands/blur.html) - Tira o foco de um elemento do DOM focalizado.
- [`.focus()`](https://docs.cypress.io/api/commands/focus.html) - Focaliza um elemento do DOM.
- [`.clear()`](https://docs.cypress.io/api/commands/clear.html) - Limpa o valor de um input ou textarea.
- [`.check()`](https://docs.cypress.io/api/commands/check.html) - Marca caixas de seleção ou botões de opção.
- [`.uncheck()`](https://docs.cypress.io/api/commands/uncheck.html) - Desmarcas caixas de seleção.
- [`.select()`](https://docs.cypress.io/api/commands/select.html) - Seleciona um `<option>` dentro de um `<selection>`.
- [`.dblclick()`](https://docs.cypress.io/api/commands/dblclick.html) - Clica duas vezes em um elemento do DOM.
- [`.rightclick()`](https://docs.cypress.io/api/commands/rightclick.html) - Clica com o botão direito do mouse em um elemento do DOM.

Esses comandos oferecem [algumas garantias](https://docs.cypress.io/guides/core-concepts/interacting-with-elements.html) em relação a qual deve ser o estado dos elementos antes de eles executarem suas ações.

Por exemplo, quando você escreve um comando [`.click()`](https://docs.cypress.io/api/commands/click.html), o Cypress garante que é possível interagir com o elemento (da mesma forma que um usuário real faria). Ele irá esperar automaticamente até que o elemento tenha um estado "acionável" confirmando que o elemento:

- Não está oculto
- Não está coberto
- Não está desativado
- Não está sendo animado

Isso também ajuda a evitar erros ao interagir com sua aplicação nos testes. Normalmente você pode substituir esse comportamento com a opção `force`.

> **Conceito importante**
>
> O Cypress oferece um algoritmo simples mas poderoso para [interagir com elementos](https://docs.cypress.io/guides/core-concepts/interacting-with-elements.html).

### Asserções sobre elementos

As asserções permitem fazer coisas como confirmar que um elemento está visível ou tem determinado atributo, classe CSS ou estado. As asserções são comandos que permitem descrever o estado _desejado_ da sua aplicação. O Cypress irá esperar automaticamente até que seus elementos tenham esse estado ou reprovará o teste se as asserções não forem atendidas. Veja a seguir uma breve demonstração das asserções em ação:

```JS
cy.get(':checkbox').should('be.disabled')

cy.get('form').should('have.class', 'form-horizontal')

cy.get('input').should('not.have.value', 'US')
```

Em cada um dos exemplos, é importante observar que o Cypress _espera_ automaticamente até que essas asserções sejam atendidas. Isso evita a necessidade de saber ou definir o momento exato em que seus elementos terão esse estado.

Aprenderemos mais sobre as [asserções](https://docs.cypress.io/guides/core-concepts/introduction-to-cypress.html#Assertions) posteriormente neste guia.

### Gerenciamento de sujeitos

Uma nova cadeia do Cypress sempre começa com `cy.[comando]`, e o resultado gerado pelo `comando` determina quais outros comandos podem ser chamados em seguida (encadeados).

Alguns métodos geram `null` e, portanto, não podem ser encadeados, tais como [`cy.clearCookies()`](https://docs.cypress.io/api/commands/clearcookies.html).

Alguns métodos, tais como [`cy.get()`](https://docs.cypress.io/api/commands/get.html) ou [`cy.contains()`](https://docs.cypress.io/api/commands/contains.html), geram um elemento do DOM, permitindo que outros comandos sejam encadeados neles (pressupondo que eles esperam um sujeito do DOM), tais como [`.click()`](https://docs.cypress.io/api/commands/click.html) ou até mesmo [`cy.contains()`](https://docs.cypress.io/api/commands/contains.html) novamente.

<!-- markdownlint-disable MD026 -->

#### Alguns comandos podem ser encadeados...

- apenas em `cy`, ou seja, eles não operam sobre um sujeito: [`cy.clearCookies()`](https://docs.cypress.io/api/commands/clearcookies.html).

- em comandos que geram determinados tipos de sujeitos (como elementos do DOM): [`.type()`](https://docs.cypress.io/api/commands/type.html).

- em `cy` _e também_ em um comando que gera um sujeito: [`cy.contains()`](https://docs.cypress.io/api/commands/contains.html).

<!-- markdownlint-disable MD026 -->

#### Alguns comandos geram...

- `null`, ou seja, nenhum comando pode ser encadeado em seguida: [`cy.clearCookies()`](https://docs.cypress.io/api/commands/clearcookies.html).

- o mesmo sujeito que foi gerado originalmente: [`.click()`](https://docs.cypress.io/api/commands/click.html).

- um novo sujeito, conforme apropriado para o comando [`.wait()`](https://docs.cypress.io/api/commands/wait.html).

Esse processo na verdade é muito mais intuitivo do que parece.

<!-- markdownlint-disable MD026 -->

#### Exemplos:

```JS
cy.clearCookies() // Terminou: 'null' foi gerado, não há possibilidade de encadeamento

cy.get('.container-principal') // Gera um array de elementos do DOM correspondentes
  .contains('Títulos')         // Gera o primeiro elemento do DOM com o conteúdo especificado
  .click()                     // Gera o mesmo elemento do DOM do comando anterior
```

> **Conceito importante**
>
> Os comandos do Cypress não retornam sujeitos, apenas os geram. Lembre-se: os comandos do Cypress são assíncronos e são enfileirados para execução posterior. Durante a execução, os sujeitos são gerados de um comando para o seguinte, e muito código auxiliar do Cypress é executado entre cada comando para garantir que tudo funcione corretamente.

> Para contornar a necessidade de referenciar elementos, o Cypress tem um recurso [conhecido como aliasing](https://docs.cypress.io/guides/core-concepts/variables-and-aliases.html). O aliasing ajuda a armazenar e salvar referências de elementos para uso futuro.

#### Usando .then() para manipular um sujeito

Quer assumir o controle do fluxo de comandos e manipular diretamente o sujeito? Sem problema, basta adicionar `.then()` à cadeia de comandos. Quando o comando anterior for resolvido, ele chamará sua função callback, em que o sujeito gerado será o primeiro argumento.

Se você quiser continuar encadeando comandos após seu `.then()`, precisará especificar o sujeito que deseja gerar para esses comandos, o que pode ser feito com um valor de retorno diferente de `null` ou `undefined`. O Cypress irá gerá-lo para o próximo comando para você.

#### Vejamos um exemplo:

```JS
cy
  // Encontre o elemento com o id 'some-link'
  .get('#some-link')

  .then(($myElement) => {
    // ...manipule o elemento com um código qualquer

    // pegue a propriedade href do elemento
    const href = $myElement.prop('href')

    // remova o caractere "hashtag" e tudo que vem depois dele
    return href.replace(/(#.*)/, '')
  })
  .then((href) => {
    // agora o href é o sujeito
    // com o qual podemos trabalhar
  })
```

> **Conceito importante**
>
> Temos muito mais exemplos e casos de uso de `cy.then()` em nosso [Guia de Conceitos Básicos](https://docs.cypress.io/guides/core-concepts/variables-and-aliases.html), que ensina como lidar corretamente com código assíncrono, quando usar variáveis e o que é aliasing.

#### Usando aliases para fazer referências a sujeitos anteriores

O Cypress tem recursos adicionais para fazer referência a sujeitos anteriores chamados [aliases](https://docs.cypress.io/guides/core-concepts/variables-and-aliases.html). É mais ou menos assim:

```JS
cy
  .get('.my-selector')
  .as('myElement') // define o alias
  .click()

/* muitas outras ações */

cy
  .get('@myElement') // consulta novamente o DOM como antes (apenas se necessário)
  .click()
```

Isso nos permite reutilizar nossas consultas ao DOM para tornar os testes mais rápidos quando o elemento ainda está no DOM, processando automaticamente novas consultas quando o elemento não é imediatamente encontrado no DOM. Isso é particularmente útil quando lidamos com frameworks de front-end que fazem muitas renderizações consecutivas!

### Os comandos são assíncronos

É muito importante entender que os comandos do Cypress não fazem nada no momento em que são chamados, mas são enfileirados para serem executados mais tarde. Isso é o que queremos dizer quando dizemos que os comandos do Cypress são assíncronos.

#### Veja este pequeno teste, por exemplo:

```JS
it('muda a URL ao clicar em "awesome"', () => {
  cy.visit('/my/resource/path') // Nada acontece ainda

  cy.get('.awesome-selector')   // Nada acontecendo ainda
    .click()                    // Não, nada

  cy.url()                      // Nada novo ainda
    .should('include', '/my/resource/path#awesomeness') // Nada.
})

// OK, a função de teste terminou de ser executada...
// Enfileiramos todos esses comandos e agora o
// Cypress começará a executá-los em sequência!
```

O Cypress só faz a mágica da automação do navegador depois que a função de teste é encerrada.

#### Mesclando código assíncrono e síncrono

É importante lembrar que os comandos do Cypress são executados de forma assíncrona se você for tentar mesclar os comandos do Cypress com o código síncrono. O código síncrono é executado imediatamente e não espera que os comandos do Cypress anteriores sejam executados.

⚠️ Uso incorreto

No exemplo abaixo, `el` é avaliado imediatamente, antes que `cy.visit()` tenha sido executado, portanto, sempre será avaliado como um array vazio.

```JS
it('não funciona como esperamos', () => {
  cy.visit('/my/resource/path') // Nada acontece ainda

  cy.get('.awesome-selector') // Ainda nada acontece
    .click()                  // Não, nada

  // Cypress.$ é síncrono, portanto, é avaliado imediatamente
  // ainda não há nenhum elemento a ser encontrado porque
  // cy.visit() foi apenas enfileirado com visit
  // e não visitou realmente a aplicação
  let el = Cypress.$('.new-el') // é avaliado imediatamente como []

  if (el.length) {              // é avaliado imediatamente como 0
    cy.get('.another-selector')
  } else {
    // esta partee sempre será executada
    // porque 'el.length' é 0
    // quando o código é executado
    cy.get('.optional-selector')
  }
})

// Ok, a função de teste terminou de ser executada...
// Enfileiramos todos esses comandos e agora
// o Cypress começará a executá-los em sequência!
```

✅ Uso correto

Veja a seguir uma forma como o código acima poderia ser reescrito para garantir que os comandos sejam executados como esperado.

```JS
it('não funciona como esperamos', () => {
  cy.visit('/my/resource/path')  // Nada acontece ainda

  cy.get('.awesome-selector')       // Ainda nada
    .click()                        // Não, nada
    .then(() => {
      // colocar este código dentro do .then() garante que ele será
      // executado depois que os comandos do Cypress forem executados

      let el = Cypress.$('.new-el') // é avaliado depois de .then()

      if (el.length) {
        cy.get('.another-selector')
      } else {
        cy.get('.optional-selector')
      }
    })
})

// Ok, a função de teste terminou de ser executada...
// Enfileiramos todos esses comandos e agora
// o Cypress começará a executá-los em sequência!
```

⚠️ Uso incorreto

No exemplo abaixo, a verificação do valor de `username` é avaliada imediatamente, antes que `cy.visit()` seja executado, portanto sempre será avaliada como `undefined`.

```JS
it('test', () => {

  let username = undefined        // é avaliado imediatamente como undefined

  cy.visit('https://app.com')     // Nada acontece ainda
  cy.get('.user-name')            // Nada ainda
    .then(($el) => {              // Nada acontece ainda
      // esta linha é avaliada depois que .then é executado
      username = $el.text()
  })

  // o seguinte é avaliado antes do .then() acima
  // então username ainda é undefined
  if (username) {                 // é avaliado imediatamente como undefined
    cy.contains(username).click()
  } else {
    // esta parte sempre será executada
    // porque username sempre será
    // avaliado como undefined
    cy.contains('My Profile').click()
  }
})

// Ok, a função de teste terminou de ser executada...
// Enfileiramos todos esses comandos e agora
// o Cypress começará a executá-los em sequência!
```

✅ Uso correto

Veja a seguir uma forma como o código acima poderia ser reescrito para garantir que os comandos sejam executados como esperado.

```JS
it('test', () => {
  let username = undefined     // é avaliado imediatamente como undefined

  cy.visit('https://app.com') // Nada acontece ainda
  cy.get('.user-name')        // Nada ainda
    .then(($el) => {          // Nada acontece ainda
      // esta linha é avaliada depois que .then é executado
      username = $el.text()

      // o seguinte é avaliado depois que .then é executado
      // é o valor correto obtido de $el.text()
      if (username) {
        cy.contains(username).click()
      } else {
        cy.get('My Profile').click()
      }
    })
})

// Ok, a função de teste terminou de ser executada...
// Enfileiramos todos esses comandos e agora
// o Cypress começará a executá-los em sequência!
```

> **Conceito importante**
>
> Cada comando (e cadeia de comandos) do do Cypress retorna imediatamente, e foi simplesmente adicionado a uma fila de comandos a serem executados posteriormente.
>
> Não é possível fazer nada de útil diretamente com o valor de retorno de um comando. Os comandos são enfileirados e gerenciados totalmente por baixo dos panos.
>
> Projetamos nossa API assim porque o DOM é um objeto altamente mutável que fica desatualizado constantemente. Para que o Cypress evite falhas e saiba quando prosseguir, gerenciamos os comandos de uma forma altamente controlada e determinista.

> **Por que não posso usar async/await?**
>
> Se você é um programador JS moderno, talvez ouça a palavra "assíncrono" e pense: por que não posso simplesmente usar `async/wait` em vez de aprender uma API especializada?
>
> As APIs do Cypress são desenvolvidas de forma muito diferente das práticas com as quais você provavelmente está acostumado: porém, esses padrões de projeto são totalmente intencionais. Entraremos em mais detalhes mais adiante neste guia.

### Os comandos são executados em série

Depois que uma função de teste termina de ser executada, o Cypress executa os comandos que foram enfileirados usando as cadeias de comandos `cy.*`.

#### Vamos analisar novamente um exemplo

```JS
it('altera a URL ao clicar em "awesome"', () => {
  cy.visit('/my/resource/path')                          // 1.

  cy.get('.awesome-selector')                            // 2.
    .click()                                             // 3.

  cy.url()                                               // 4.
    .should('include', '/my/resource/path#awesomeness')  // 5.
})
```

O teste acima causaria uma execução na seguinte ordem:

1. Visitar uma URL.
2. Encontrar um elemento pelo seletor.
3. Executar uma ação de clique nesse elemento.
4. Pegar a URL.
5. Afirmar que a URL deve incluir uma string específica.

Essas ações sempre acontecerão em série (uma após a outra), nunca em paralelo (ao mesmo tempo). Por quê?

Para ilustrar isso, vamos revisitar essa lista de ações e expor alguns dos ✨ truques ✨ ocultos que o Cypress faz a cada passo:
