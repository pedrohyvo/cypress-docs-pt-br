# Migrating from Protractor to Cypress

```markdown
O que você aprenderá
- Benefícios de usar Cypress em aplicativos Angular
- Como o Cypress pode criar testes e2e confiáveis para aplicativos Angular
- Como migrar testes de transferidor para Cypress
```

## Introdução

O Protractor tem sido uma ferramenta de teste ponta a ponta popular para aplicativos Angular e AngularJS. 
No entanto, o Protractor não está mais incluído em novos projetos Angular a partir do Angular 12.
 Nós abordamos você aqui com este guia de migração para ajudar você e sua equipe na transição do Protractor para o Cypress.

 Se você notar alguma imprecisão neste guia ou sentir que algo foi deturpado, inicie uma discussão aqui[https://github.com/cypress-io/cypress/discussions/new]

Para começar, vamos dar uma olhada em um exemplo de código rápido para ver como o Cypress acessível está vindo do Protractor.
 Neste cenário, temos um teste para validar se um usuário pode se inscrever para uma nova conta.

Antes: **Protractor**

 describe('Testes de autorização', () => {
  it('Permite que o usuário se inscreva para uma nova conta', () => {
    browser.get('/signup')
    element(by.css('#email-field')).sendKeys('user@email.com')
    element(by.css('#confirm-email-field')).sendKeys('user@email.com')
    element(by.css('#password-field')).sendKeys('testPassword1234')
    element(by.cssContainingText('button', 'Create new account')).click()
    expect(browser.getCurrentUrl()).toEqual('/signup/success')
  })
})

Depois: **Cypress**

describe('Testes de autorização', () => {
  it('Permite que o usuário se inscreva para uma nova conta', () => {
    cy.visit('/signup')
    cy.get('#email-field').type('user@email.com')
    cy.get('#confirm-email-field').type('user@email.com')
    cy.get('#password-field').type('testPassword1234')
    cy.get('button').contains('Create new account').click()
    cy.url().should('include', '/signup/success')
  })
})

## Benefícios do uso de Cypress


Como muitos desenvolvedores podem atestar, o teste de ponta a ponta é uma daquelas coisas que eles sabem que devem fazer,
mas geralmente não fazem. Ou, se eles executam testes, eles costumam ser instáveis e muito caros, devido ao tempo que  podem
levar para serem executados. E embora muitas vezes existam ideais de cobertura de código completa, as realidades dos negócios
e prazos geralmente têm precedência e os testes não são escritos, ou pior, são ignorados quando os erros estão sendo relatados
porque não são confiáveis.
 O Cypress não apenas garante que seus testes serão confiáveis, mas também fornece aos desenvolvedores ferramentas que tornam o 
 teste do e2e um recurso para o desenvolvimento, em vez de um obstáculo.


## Interaja com seus testes em um navegador


Quando o Protractor executa testes, a automação do navegador inicia uma instância do navegador e geralmente executa os testes
muito rápido para o olho humano. Sem configuração adicional, isso geralmente leva a uma dependência de mensagens de terminal
longas que podem ser caras do ponto de vista da troca de contexto.

Com o Cypress Test Runner [https://docs.cypress.io/guides/core-concepts/test-runner], seus testes são executados em um 
ambiente de navegador interativo em tempo real. O log [https://docs.cypress.io/guides/core-concepts/test-runner#Command-Log] 
de comando do Cypress Test Runner exibe os testes de seu conjunto de testes e suas afirmações. Quando você clica em um
comando ou asserção no log de comando [https://docs.cypress.io/guides/core-concepts/test-runner#Clicking-on-Commands],
o Cypress Test Runner exibe um instantâneo DOM daquele ponto no tempo para que você possa ver a aparência do aplicativo em
teste no momento da execução do teste. Isso permite que você veja a IU renderizada real e o comportamento do aplicativo nas
interações reais do usuário.
Como o aplicativo é carregado em um navegador real, você também pode explorar manualmente seu comportamento enquanto está
no estado de um cenário de teste desejado.
O Test Runner também ajuda você a escrever seus testes, tornando tão fácil quanto possível encontrar os seletores CSS corretos
para os elementos DOM em seu aplicativo com seu Selector Playground  [https://docs.cypress.io/guides/core-concepts/test-runner#Selector-Playground].
O Selector Playground ajuda a reduzir o tempo gasto para encontrar os seletores certos para que você possa se concentrar no que é importante:
escrever testes que verificam a lógica do seu aplicativo.

## Loops de feedback mais rápidos


Quando se trata de seus testes de ponta a ponta, ser capaz de ver seus testes enquanto eles são executados é fundamental
para permitir que você itere com segurança mais rápido. Com o Cypress, seus testes são executados automaticamente após
o salvamento do arquivo à medida que você faz a iteração neles.
Ter seu editor de código e aplicativo sob teste em um navegador lado a lado (mostrado abaixo) enquanto a reexecução de
testes ao salvar é um fluxo de trabalho altamente produtivo. Ele fornece um loop de feedback instantâneo que permite iterar
mais rápido com confiança.

## Viagem no tempo por meio de testes


O Cypress Test Runner oferece recursos de viagem no tempo para ver exatamente como seu aplicativo estava se comportando
em qualquer ponto durante a execução do teste. O Cypress tira instantâneos DOM de seu aplicativo em teste enquanto o Test
Runner executa os comandos e asserções em seus testes. Isso permite que você visualize a IU real de seu aplicativo a qualquer
momento durante a execução de seus testes. Ao clicar de um comando para outro no log de comandos, você pode ver quais elementos
o Cypress agiu e como seu aplicativo respondeu ao comportamento do usuário real simulado.

## Ganhe visibilidade no modo sem cabeça com capturas de tela e vídeos

Running browser tests in headless mode (locally or in continuous integration pipeline) can be a bit of a black-box without
much visibility. When tests fail, error messages by themselves can often fall short in painting the picture of why something
failed, especially if assertions were not explicit enough or too indirect. To understand the reason behind test failures
it also helps to see the state of the app UI at the point of failure or see the events that led up to the failure.
Cypress assists with debugging in headless mode, by automatically taking a screenshot of the app UI and command log at
the exact point of test failure. To help see everything that happened prior to test failure, Cypress provides a video
recording (as an MP4 file) of a full test spec run by default.

## Novas tentativas de teste


Os testes de ponta a ponta podem ser complicados porque os aplicativos da web modernos também são complexos. Você pode
descobrir que alguns recursos de seu aplicativo da web são difíceis de testar ou os testes falham esporadicamente.
Chamamos esses testes de "escamosos". O Cypress permite que você repita os testes que falharam
[https://docs.cypress.io/guides/guides/test-retries]. Às vezes, os testes falharão em um ambiente de CI quando, de outra
forma, passariam na máquina de um desenvolvedor. Habilitar novas tentativas de teste na configuração do Cypress pode
ajudá-lo a desbloquear quando testes imprevisíveis e instáveis falharem ocasionalmente.
O Cypress Dashboard vai além e ajuda você e sua equipe a detectar testes fragmentados [https://docs.cypress.io/guides/dashboard/flaky-test-management]
que são executados em seu pipeline de CI / CD.

# Começando

## Instalação Recomendada


Recomendamos o uso de nosso esquema oficial do Cypress Angular para adicionar o Cypress ao seu projeto Angular

ng add @cypress/schematic

Isso instalará o Cypress, adicionará scripts para executar o Cypress no modo de execução e aberto, arquivos e diretórios
do Cypress com base no scaffold e (opcional) solicitará que você remova o Protractor e reconfigure o comando ng e2e padrão
para usar o Cypress.
Com nosso esquema instalado e o transferidor removido, você pode executar o Cypress no modo aberto com o seguinte comando:

ng e2e

Você também pode usar o seguinte comando para iniciar o Cypress no modo aberto:

ng run {your-project-name}:cypress-open

Ambos os comandos iniciarão o Cypress Test Runner em um navegador Electron.
Você também pode iniciar o Cypress por meio do modo de execução, que é executado sem controle:

ng run {your-project-name}:cypress-run

```markdown
Verifique a documentação da seção Cypress Angular Schematic Configuration para obter mais detalhes, como configurar seus testes para serem
executados em um navegador específico ou registrar os resultados do teste no Cypress Dashboard. [https://docs.cypress.io/guides/migrating-to-cypress/protractor#Angular-Schematic-Configuration]

O pacote Cypress Angular Schematic foi possível graças ao trabalho original da equipe Briebug e os contribuintes de @ briebug / cypress-schematic.
@briebug / cypress-schematic serviu como ponto de partida para melhorias e novas funcionalidades que a equipe Cypress continuará a desenvolver junto com a comunidade.
```

## Manual Installation


Embora recomendemos o uso de nosso esquema Angular oficial, você ainda pode instalar o Cypress manualmente.

npm install --save-dev cypress

Então, como o Cypress pode ser executado em paralelo com seu aplicativo, vamos instalar simultaneamente para simplificar
nosso script npm. Isso é opcional; no entanto, você precisará de outra maneira de servir seu aplicativo Angular para que
o Cypress execute testes em seu aplicativo.

npm install --save-dev concurrently

Em seguida, atualizaremos nosso package.json com os seguintes scripts:

// Exemplo package.json
{
  "scripts": {
    "cy:open": "concurrently \"ng serve\" \"cypress open\"",
    "cy:run": "concurrently \"ng serve\" \"cypress run\""
  },
  "dependencies": { ... },
  "devDependencies": { ... }
}

Agora, quando executamos:

npm run cy:open

Ele iniciará o Cypress e nosso aplicativo Angular ao mesmo tempo.
Mais uma vez, é altamente recomendável usar nosso esquema angular para instalar o Cypress e planejamos adicionar novos
recursos a ele com o tempo.

# Trabalhando com o DOM

## Obter um único elemento na página

Quando se trata de testes e2e, uma das coisas mais comuns que você precisa fazer é colocar um ou mais elementos HTML em
uma página. Em vez de dividir a busca de elemento em vários métodos que você precisa memorizar, tudo pode ser realizado
com cy.get ao usar seletores CSS ou o atributo de dados preferido

Antes: **Protractor**
// Get an element
element(by.tagName('h1'))

/// Get an element using a CSS selector.
element(by.css('.my-class'))

// Get an element with the given id.
element(by.id('my-id'))

// Get an element using an input name selector.
element(by.name('field-name'))

//Get an element by the text it contains within a certain CSS selector
element(by.cssContainingText('.my-class', 'text'))

//Get the first element containing a specific text (only for link elements)
element(by.linkText('text')

Depois: **Cypress**


