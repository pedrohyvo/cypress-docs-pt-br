# Novas tentativas de teste


>**O que você vai aprender:**
>
>* O que são novas tentativas de teste?
>* Por que as novas tentativas de teste são importantes?
>* Como configurar novas tentativas de teste

## Introdução

Os testes ponta a ponta (E2E) são excelentes para testar sistemas complexos. No entanto, ainda existem comportamentos que
são difíceis de verificar e tornam os testes instáveis ​​(ou seja, não confiáveis) e às vezes falham devido a condições 
imprevisíveis (por exemplo, interrupções temporárias em dependências externas, erros de rede aleatórios, etc.). Algumas 
outras condições de corrida comuns que podem resultar em testes não confiáveis ​​incluem:

* Animações
* Chamadas de API
* Teste de disponibilidade do servidor / banco de dados
* Disponibilidade de dependências de recursos
* Problemas de rede

Com novas tentativas de teste, o Cypress é capaz de repetir os testes que falharam para ajudar a reduzir a fragilidade do
teste e as falhas de construção de integração contínua (CI). Ao fazer isso, você economizará tempo e recursos valiosos 
para que você possa se concentrar no que é mais importante para você.

## Como funciona

Por padrão, os testes não serão repetidos quando falharem. Você precisará [habilitar novas tentativas de teste em sua
 configuração](test-retries.md#Introdução) para usar este recurso.

Depois que as novas tentativas de teste são habilitadas, os testes podem ser configurados para ter um número X de 
tentativas de repetição. Por exemplo, se as novas tentativas de teste foram configuradas com 2 novas tentativas,
 o Cypress tentará novamente os testes até 2 vezes adicionais (para um total de 3 tentativas) antes de ser 
 potencialmente marcado como um teste com falha.

Quando cada teste é executado novamente, os seguintes ganchos também serão executados novamente:

* `beforeEach`
* `afterEach`

> No entanto, as falhas em `after` e `before` hooks não dispararão uma nova tentativa.

## A seguir está um exemplo detalhado passo a passo de como funcionam as novas tentativas de teste


Supondo que configuramos novas tentativas de teste com `2` tentativas (para um total de 3 tentativas), aqui está como os
 testes podem ser executados:

1. Um teste é executado pela primeira vez. Se o teste passar, o Cypress seguirá em frente com todos os testes restantes,
 como de costume.

2. Se o teste falhar, o Cypress informará que a primeira tentativa falhou e tentará executar o teste uma segunda vez. 

    ![Exemplo de nova tentativa após falha](https://docs.cypress.io/_nuxt/img/attempt-2-start.c3c9971.png)

3. Se o teste for aprovado após a segunda tentativa, o Cypress continuará com os testes restantes.

4. Se o teste falhar uma segunda vez, o Cypress fará a terceira tentativa final para executar o teste novamente.

    ![Exemplo de nova tentativa após segunda falha](https://docs.cypress.io/_nuxt/img/attempt-3-start.0ac793f.png)

5. Se o teste falhar pela terceira vez, o Cypress marcará o teste como reprovado e, em seguida, executará os testes restantes.

[//]: <> (TODO - Adicionar cypress run quando traduzido)

A seguir está uma captura de tela de como as novas tentativas de teste se parecem no mesmo teste com falha quando 
executado através do [cypress run](https://docs.cypress.io/guides/guides/command-line#cypress-run).

![Exemplo de nova tentativa rodando via cypress run](https://docs.cypress.io/_nuxt/img/cli-error-message.88a73e6.png)

[//]: <> (TODO - Adicionar cypress open e Log de comandos quando traduzido)

Durante o [cypress open](https://docs.cypress.io/guides/guides/command-line#cypress-open), você poderá ver o número de 
tentativas feitas no [Log de comandos](https://docs.cypress.io/guides/core-concepts/test-runner#Command-Log) e expandir
 cada tentativa para revisão e depuração, se desejar.

## Configurar novas tentativas de teste

### Configuração Global

[//]: <> (TODO - Adicionar arquivo de configuração quando traduzido)

Normalmente, você desejará definir diferentes tentativas de repetição para a execução do Cypress e a abertura do 
Cypress. Você pode configurar isso em seu 
[arquivo de configuração](https://docs.cypress.io/guides/guides/command-line#cypress-open-config-file-lt-config-file-gt)
 (`cypress.json` por padrão), passando a opção de `retries` em um objeto com as seguintes opções:

* `runMode` permite-lhe definir o número de tentativas de teste ao executar o `cypress run`

* `openMode ` permite-lhe definir o número de tentativas de teste ao executar o `cypress open`


````javascript
{
  "retries": {
    // Configurar novas tentativas para `cypress run`
    // Padrão é 0
    "runMode": 2,
    // Configurar novas tentativas para `cypress open`
    // Padrão é 0
    "openMode": 0
  }
}
````

#### Configurar novas tentativas para todos os modos

[//]: <> (TODO - Adicionar arquivo de configuração quando traduzido)

Se você deseja configurar as tentativas de repetição para todos os testes executados no `cypress run` e no 
`cypress open`, você pode configurar isso em seu 
[arquivo de configuração](https://docs.cypress.io/guides/guides/command-line#cypress-open-config-file-lt-config-file-gt)
(`cypress.json` por padrão) definindo a 
propriedade `retries` e configurando o número de tentativas desejado.

````javascript
{
  "retries": 1
}
````

### Configurações personalizadas

#### Teste(s) Individual(is) 

Se quiser configurar novas tentativas em um teste específico, você pode definir isso usando a 
[configuração de teste](../core-concepts/writing-and-organizing-tests.md#Configuração-de-Teste)


````javascript
// Personalize novas tentativas para um teste individual
describe('User sign-up and login', () => {
  // bloco de teste `it` sem configuração personalizada
  it('should redirect unauthenticated user to sign-in page', () => {
    // ...
  })

  // bloco de teste `it` com configuração personalizada
  it(
    'allows user to login',
    {
      retries: {
        runMode: 2,
        openMode: 1,
      },
    },
    () => {
      // ...
    }
  )
})
````

#### Conjunto(s) de teste(s)

Se quiser configurar tentativas de tentativa para um conjunto de testes, você pode fazer isso definindo a configuração 
do conjunto.

````javascript
// Personalização de novas tentativas para um conjunto de testes
describe('User bank accounts', {
  retries: {
    runMode: 2,
    openMode: 1,
  }
}, () => {
  // A configuração por suíte é aplicada a cada teste
  // Se um teste falhar, ele será repetido
  it('allows a user to view their transactions', () => {
    // ...
  })

  it('allows a user to edit their transactions', () => {
    // ...
  })
})
````

[//]: <> (TODO - Adicionar Configuração de teste quando traduzido)
Você pode encontrar mais informações sobre configurações personalizadas aqui: [Configuração de teste](https://docs.cypress.io/guides/references/configuration#Test-Configuration)

## Capturas de tela

[//]: <> (TODO - adicionar cy.screenshot quando traduzido)

Quando um teste é repetido, o Cypress continuará a fazer capturas de tela para cada tentativa falhada ou 
[cy.screenshot()](https://docs.cypress.io/api/commands/screenshot) com sufixo a nova captura de tela com `(attempt n)`, 
correspondendo ao número da tentativa de repetição atual.

Com o seguinte código de teste, você veria os nomes de arquivo de captura de tela abaixo quando todas as 3 tentativas falhassem:

````javascript
describe('User Login', () => {
  it('displays login errors', () => {
    cy.visit('/')
    cy.screenshot('user-login-errors')
    // ...
  })
})
````

````javascript
// nome do arquivo de captura de tela de cy.screenshot() na primeira tentativa
'user-login-errors.png'
// nome do arquivo de captura de tela na primeira tentativa falhada
'user-login-errors (failed).png'
// nome do arquivo de captura de tela de cy.screenshot() na 2ª tentativa
'user-login-errors (attempt 2).png'
// nome do arquivo de captura de tela na segunda tentativa falhada
'user-login-errors (failed) (attempt 2).png'
// nome do arquivo de captura de tela de cy.screenshot() na terceira tentativa
'user-login-errors (attempt 3).png'
// nome do arquivo de captura de tela na terceira tentativa falhada
'user-login-errors (failed) (attempt 3).png'
````

## Videos

Você pode usar o listener de evento `after:spec` do Cypress que dispara após cada arquivo de especificação ser 
executado para excluir o vídeo gravado para especificações que não tiveram novas tentativas ou falhas. A exclusão de 
vídeos aprovados e não repetidos após a execução pode economizar espaço de recursos na máquina e também ignorar o tempo 
usado para processar, compactar e enviar o vídeo para o [Serviço de Dashboard](../dashboard/introduction.md).

### Envie apenas vídeos para especificações com testes reprovados ou repetidos

O exemplo abaixo mostra como excluir o vídeo gravado para especificações que não tiveram novas tentativas ou falhas ao 
usar novas tentativas de teste Cypress.

````javascript
// plugins/index.js

// precisa instalar essas dependências
// npm i lodash del --save-dev
const _ = require('lodash')
const del = require('del')

module.exports = (on, config) => {
  on('after:spec', (spec, results) => {
    if (results && results.video) {
      // Temos falhas para qualquer tentativa de repetição?
      const failures = _.some(results.tests, (test) => {
        return _.some(test.attempts, { state: 'failed' })
      })
      if (!failures) {
        // exclua o vídeo se a especificação for aprovada e nenhum teste for repetido
        return del(results.video)
      }
    }
  })
}
````

## Dashboard

Se você estiver usando o [Cypress Dashboard](../dashboard/introduction.md), as informações 
relacionadas às novas tentativas de teste serão exibidas na guia 'Test Results' para uma execução. Selecionar o filtro 
'Flaky' mostrará os testes que foram repetidos e foram aprovados durante a execução.

Esses testes também são indicados com um emblema "Flaky" na página 'Latest Runs' e na guia 'Test Results  na página 
'Run Details'.

[//]: <> (TODO - adicionar Flaky Test Management quando traduzido)

[Video com exemplo de um teste com o filtro 'flaky'](https://docs.cypress.io/_nuxt/videos/flaky-test-filter.e60c680.mp4)

Clicar em um 'Test Result' abrirá a tela 'Test Case History'. Isso demonstra o número de tentativas 
malsucedidas, as capturas de tela e/ou vídeos das tentativas malsucedidas e o erro das tentativas malsucedidas.

![Flake artefatos e erros](https://docs.cypress.io/_nuxt/img/flake-artifacts-and-errors.7b8ab6f.png)

Você também pode ver a 'Flaky Rate' (taxa de instabilidade) para um determinado teste.

![Flaky rate](https://docs.cypress.io/_nuxt/img/flaky-rate.cf30c01.png)

[//]: <> (TODO - adicionar Flaky Test Management quando traduzido)

Para uma visão abrangente de como o floco está afetando seu conjunto de testes geral, você pode revisar os recursos de 
[Flake Detection](https://docs.cypress.io/guides/dashboard/flaky-test-management#Flake-Detection) e 
[Flake Alerting](https://docs.cypress.io/guides/dashboard/flaky-test-management#Flake-Alerting) 
destacados em 'Test Flake Management Guide'.

## Perguntas frequentes (FAQs)

### Os testes repetidos serão contados como mais de um resultado de teste em meu faturamento?

Não. Os testes gravados durante o `cypress run` com o sinalizador `--record` serão contados da mesma forma com ou sem 
novas tentativas de teste.

Consideramos cada vez que a função `it()` é chamada um único teste para fins de faturamento. A repetição do teste não 
contará como resultados de teste extras em seu faturamento.

Você sempre pode ver quantos testes você registrou na página 'Billing & Usage ' da sua organização no [Dashboard](https://on.cypress.io/dashboard).

### Posso acessar o contador de tentativas atual do teste?

Sim, embora normalmente você não precise, pois este é um detalhe de baixo nível. Mas se quiser usar o número da tentativa
 atual e o total de tentativas permitidas, você pode fazer o seguinte:

````javascript

it('does something differently on retry', { retries: 3 }, () => {
  // cy.state('runnable') retorna o objeto de teste atual
  // podemos pegar a tentativa atual e 
  // o total de tentativas permitidas de suas propriedades
  const attempt = cy.state('runnable')._currentRetry
  const retries = cy.state('runnable')._retries
  // use os valores  "attempt" e "retries" de alguma forma
})

````

A variável de `attempt` acima terá valores de 0 a 3 (a primeira execução de teste padrão mais três tentativas permitidas).
 A constante de novas tentativas neste caso é sempre 3.

[//]: <> (TODO - adicionar api/utilities/ quando traduzido)

Dica: Cypress agrupa a [biblioteca Lodash](https://docs.cypress.io/api/utilities/_). Use seus métodos auxiliares para 
acessar com segurança uma propriedade de um objeto. Vamos nos certificar de que a função oferece suporte a diferentes 
versões do Cypress voltando aos valores padrão.

````javascript
it('does something differently on retry', { retries: 3 }, () => {
  // _.get: se o objeto ou propriedade estiver faltando use o valor padrão fornecido
  const attempt = Cypress._.get(cy.state('runnable'), '_currentRetry', 0)
  const retries = Cypress._.get(cy.state('runnable'), '_retries', 0)
  // use os valores  "attempt" e "retries" de alguma forma
})
````

[Voltar para o topo](#Novas-tentativas-de-teste)
