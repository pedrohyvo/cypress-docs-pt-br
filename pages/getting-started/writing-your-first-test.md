# Escrevendo o Primeiro Teste

```markdown
O que você vai aprender

- Como começar a testar um novo projeto com Cypress
- Como são reportados testes com sucesso e falhas
- Testando a navegação web, buscando por elementos no DOM e escrevendo asserções
```

[Vídeo de exemplo.](https://vimeo.com/237115455)

## Adicione um arquivo de teste

Assumindo que você tenha [instalado](installing-cypress.md#instalando) e 
[aberto](installing-cypress.md#abrindo-o-cypress) o Cypress Test Runner, 
chegou a hora de escrever o primeiro teste. Vamos então:

1. Criar um arquivo `sample_spec.js`.
2. Checar que o Cypress atualizou a lista de arquivos de teste.
3. Executar o Cypress Test Runner.

Vamos criar um novo arquivo na pasta `cypress/integration`, a qual já foi criada:

```touch {your_project}/cypress/integration/sample_spec.js```

Uma vez que o arquivo tenha sido criado, o Cypress Test Runner deve exibi-lo na lista de testes 
de integração. O Cypress monitora mudanças em seus arquivos de teste e os exibe imediatamente.

Mesmo que ainda não tenha escrito nenhum teste - tudo bem - clique em `sample_spec.js` e veja o 
Cypress iniciar o navegador.

> O Cypress abre o teste em um navegador previamente instalado em seu sistema operacional. Você 
pode ler mais sobre como isso é feito em Iniciando Navegadores.

[Vídeo de exemplo](https://docs.cypress.io/img/snippets/empty-file-30fps.mp4)

[//]: <> (TODO - Adicionar link Test Runner quando for traduzido)
Agora estamos oficialmente no 
[Test Runner](https://docs.cypress.io/guides/core-concepts/test-runner.html). 
É nele onde vamos gastar a maioria do tempo testando.

> Note que o Cypress exibe a mensagem que não pode encontrar nenhum teste. 
Isso é normal - testes ainda não foram escritos! Você verá essa mensagem também caso exista algum
 erro ao analisar os arquivos de teste. Você pode abrir o Dev Tools para inspecionar o console e 
 buscar por erros de sintaxe que impediu o Cypress de ler seus arquivos de testes.

## Escreva seu primeiro teste

Chegou a hora de escrever o nosso primeiro teste. Vamos então:

1. Escrever nosso primeiro teste com sucesso.
2. Escrever nosso primeiro teste com falha.
3. Ver o Cypress recarregar o teste em tempo real.

A medida que salvamos nosso novo teste, veremos que o navegador recarrega automaticamente em tempo 
real.

Abra sua IDE favorita e adicione o código abaixo no arquivo de teste `sample_spec.js`.

```javascript
describe('My First Test', () => {
  it('Does not do much!', () => {
    expect(true).to.equal(true)
  })
})
```

Quando salvar esse arquivo você deve ver o navegador recarregar.

Embora não faça nada, esse é nosso primeiro teste com sucesso!

[//]: <> (TODO - Adicionar link Test Runner quando for traduzido)
No [log de comandos](https://docs.cypress.io/guides/core-concepts/test-runner.html#Command-Log) 
você vai ver que o Cypress exibe a suíte de teste, o teste e a primeira asserção (que deve estar 
em verde - sucesso).

![Primeiro teste com sucesso](https://docs.cypress.io/img/guides/first-test.88031830.png)

[//]: <> (TODO - Adicionar links Test Runner e Visit Command quando forem traduzidos)
> Observe que o Cypress exibe uma mensagem sobre essa ser a página padrão 
[à direita](https://docs.cypress.io/guides/core-concepts/test-runner.html#Application-Under-Test).
Cypress assume que você irá sair e [visitar](https://docs.cypress.io/api/commands/visit.html) outra 
página na internet - mas também pode funcionar bem sem isso.

Agora vamos escrever nosso primeiro teste com falha.

```javascript
describe('My First Test', () => {
  it('Does not do much!', () => {
    expect(true).to.equal(false)
  })
})
```

Quando você salvar, poderá observar que o Cypress exibe o teste com falha em vermelho, uma vez 
que `true` não é igual a `false`.

[//]: <> (TODO - Adicionar links File Opener Preference e Debugging Errors quando forem traduzidos)
Cypress também exibe o log do erro e o pedaço do código onde a asserção falhou 
(quando disponível). Você pode clicar no link do arquivo em azul para abrir o arquivo em que o 
erro ocorreu 
[em sua IDE](https://docs.cypress.io/guides/tooling/IDE-integration.html#File-Opener-Preference). 
Para ler mais sobre exibição de erros, leia sobre 
[Debugando Erros](https://docs.cypress.io/guides/guides/debugging.html#Errors).

![Teste com falha](https://docs.cypress.io/img/guides/failing-test.971461e3.png)

[//]: <> (TODO - Adicionar link Test Runner quando for traduzido)
Cypress fornece um ótimo 
[Test Runner](https://docs.cypress.io/guides/core-concepts/test-runner.html) 
que exibe uma estrutura visual das suítes, testes e asserções. Em breve você verá 
também comandos, eventos, requisições de rede e mais.

[Vídeo do primeiro teste com falha](https://docs.cypress.io/img/snippets/first-test-30fps.mp4)

[//]: <> (TODO - Adicionar link Bundled Tools quando for traduzido)
> O que é describe, it e expect?
Todas essas funções vem das 
[Bundled Tools](https://docs.cypress.io/guides/references/bundled-tools.html) que fazem parte do 
Cypress. Describe e it vem do [Mocha](https://mochajs.org/). 
Expect vem do [Chai](http://www.chaijs.com/).

> Cypress utiliza dessas ferramentas e frameworks populares, os quais é esperado que você já tenha 
alguma familiaridade e conhecimento prévio. Mas se não tiver, tudo bem.

## Escreva um teste real

Um teste sólido geralmente cobre 3 fases:

1. Configurar o estado da aplicação
2. Realizar uma ação
3. Fazer uma asserção sobre o resultado da ação

Também conhecido pela tríade "Given, When, Then", ou "Arrange, Act, Assert". Mas a ideia é: 
primeiro a aplicação é colocada em um estado específico, então uma ação é realizada na 
aplicação, o que causa uma mudança, e finalmente o resultado da apicação é checado.

Hoje, vamos ver esses passos de perto e mapeá-los claramente em comandos do Cypress:

1. Visitar uma página web.
2. Consultar um elemento.
3. Interagir com o elemento.
4. Verificar o conteúdo da página.

### Passo 1: Visitar uma página web

Primeiro, vamos visitar uma página web. Neste exemplo vamos visitar a aplicação 
[Kitchen Sink](https://docs.cypress.io/examples/examples/applications.html#Kitchen-Sink)
para que você possa experimentar o Cypress sem se preocupar em procurar uma página para 
testar.

Podemos passar a URL que queremos visitar para o `cy.visit()`. Vamos substituir nosso teste 
anterior pelo teste abaixo que de fato visita uma página:

```javascript
describe('My First Test', () => {
  it('Visits the Kitchen Sink', () => {
    cy.visit('https://example.cypress.io')
  })
})
```

Salve o arquivo e retorne ao Cypress Test Runner. Você vai perceber algumas coisas:

1. O [Log de Comandos](https://docs.cypress.io/guides/core-concepts/test-runner.html#Command-Log)
agora exibe a nova ação `visit`.
2. A aplicação Kitchen Sink foi carregada no painel de 
[pré visualização da aplicação](https://docs.cypress.io/guides/core-concepts/test-runner.html#Overview).
3. O teste está verde, mesmo não tendo nenhuma asserção.
4. O `visit` exibe um estado pendente em azul até que a página termine de carregar.

Se essa solicitação tivesse retornado com um código de status diferente de 2xx, como 404 ou 500, 
ou se houvesse um erro de JavaScript no código do aplicativo, o teste teria falhado.

[Veja o exemplo acima neste vídeo](https://docs.cypress.io/img/snippets/first-test-visit-30fps.mp4).

```markdown
Teste apenas aplicações que você controla

Embora neste guia estamos testando nossa aplicação exemplo, você não deve testar aplicações que você 
não controla. Por quê?

1. São passíveis de mudanças a qualquer momento, o que pode quebrar os testes.
2. Podem fazer testes A/B o que torna impossível ter resultados consistentes.
3. Podem detectar que o usuário é um script e bloquear o acesso (como o Google faz).
4. Podem ter funcionalidades de segurança habilitadas que previne o Cypress de funcionar corretamente.

O ponto do Cypress é ser uma ferramente de uso diário para construir e testar suas próprias aplicações.

Cypress não é uma ferramenta de automação de propósito geral. É pouco adequado para scripts em sites que 
estão produção, e não estão sob seu controle.
```

### Passo 2: Consultar um elemento

Agora que temos a página web carregada, precisamos fazer algo nela. Porque não clicamos em um link?
Parece ser fácil, vamos ver qual deles gostamos... que tal `type`?

Para encontrar esse elemento pelo seu conteúdo, usamos `cy.contains()`.

Vamos adicionar isso em nosso teste e ver o que acontece:

```javascript
describe('My First Test', () => {
  it('finds the content "type"', () => {
    cy.visit('https://example.cypress.io')

    cy.contains('type')
  })
})
```

Agora o teste deve exibir o comando `CONTAINS` e ainda estar verde.

Mesmo sem adicionar uma asserção, sabemos que está tudo certo! Isso acontece pois muitos dos comandos
do Cypress são construídos para falhar caso não encontram o que é esperado. Isso é conhecido como uma
[Asserção Padrão](https://docs.cypress.io/guides/core-concepts/introduction-to-cypress.html#Default-Assertions).

Para confirmar, substitua `type` por qualquer palavra que não esteja na página, como `hype`.
Perceba que o teste falha, mas só depois de 4 segundos!

Entende o que o Cypress está fazendo? Automaticamente espera e tenta novamente pois é esperado que
eventualmente o elemento será encontrado no DOM. O teste não falha imediatamente.

![Teste com falha](https://docs.cypress.io/img/guides/first-test-failing-contains.529e740a.png)

```markdown
Mensagens de Erro

Tomamos o cuidado de escrever centenas de mensagens de erro personalizadas que tentam explicar claramente
o que deu errado no Cypress. Nesse caso, o Cypress atingiu o tempo limite tentando encontrar o texto hype
em toda a página. Para ler mais sobre a exibição do erro, leia sobre
[Erros de depuração](https://docs.cypress.io/guides/guides/debugging.html#Errors).
```

Antes de adicionar outro comando - vamos consertar esse teste. Substitua `hype` por `type`.

[Vídeo de exemplo do teste acima](https://docs.cypress.io/img/snippets/first-test-contains-30fps.mp4)

### Passo 3: Clicar em um elemento

Ok, agora queremos clicar no link que encontramos. Como fazer isso? Adicione o comando `.click()`
no final do comando anterior, como abaixo:

```javascript
describe('My First Test', () => {
  it('clicks the link "type"', () => {
    cy.visit('https://example.cypress.io')

    cy.contains('type').click()
  })
})
```

É quase possível ler o teste como uma história! Cypress chama isso de encadeamento e comandos são
encadeados para contruir testes que realmente expressam o que a aplicação faz de forma declarativa.

Note também que o painel da aplicação foi atualizado depois do clique, abrindo o link e mostrando a
página de destino.

Agora podemos fazer uma asserção nessa nova página!

[Vídeo de exemplo do teste acima](https://docs.cypress.io/img/snippets/first-test-click-30fps.mp4)

```markdown
Você pode ver o IntelliSense em seus arquivos de teste ao adicionar uma única linha de comentário especial.
Leia sobre isso [aqui](https://docs.cypress.io/guides/tooling/IDE-integration.html#Triple-slash-directives).
```

### Passo 4: Fazer uma asserção

WIP
