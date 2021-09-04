# Módulo API

Você pode requerer o Cypress como um módulo do seu aplicativo em teste e executar o Cypress via Node.js.
Isso pode ser útil quando você deseja acessar os resultados do teste diretamente após a execução. 
Com este fluxo de trabalho, por exemplo, você pode:

- Envie uma notificação sobre testes com falha com imagens de captura de tela incluídas
- Execute novamente um único arquivo de especificação com falha
- Dê início a outras compilações ou scripts

## cypress.run()

Executa testes do Cypress via Node.js e resolve com todos os resultados de teste. Veja o [Cypress Módulo API recipe](https://github.com/cypress-io/cypress-example-recipes#fundamentals).

```javascript
// e2e-run-tests.js
const cypress = require("cypress")

cypress.run({
  reporter: "junit",
  browser: "chrome",
  config: {
    baseUrl: "http://localhost:8080",
    video: true,
  },
  env: {
    login_url: "/login",
    products_url: "/products",
  },
})
```

Você pode então executar o Cypress executando o seguinte em seu terminal ou um script npm:

```bash
node e2e-run-tests.js
```

## Opções

Assim como as [opções de linha de comando](https://docs.cypress.io/guides/guides/command-line) para o cypress run, 
você pode passar opções que modificam como o Cypress é executado.

|      Opção      |       Tipo       | Descrição                                                                                                                                                                                                                               |
| :-------------: | :--------------: | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
|     browser     |      string      | Especifica um navegador diferente para executar os testes, seja por nome ou por caminho do sistema de arquivos                                                                                                                          |
|    ciBuildId    |      string      | Especifica um identificador exclusivo para uma execução para permitir [agrupamento](https://docs.cypress.io/guides/guides/parallelization#Grouping-test-runs) ou [paralelização](https://docs.cypress.io/guides/guides/parallelization) |
|     config      |      object      | Especifica a [configuração](https://docs.cypress.io/guides/references/configuration)                                                                                                                                                    |
|   configFile    | string / boolean | Caminho para o arquivo de configuração a ser usado. Se false for passado, nenhum arquivo de configuração será usado.                                                                                                                    |
|       env       |      object      | Especifica as [variáveis ​​de ambiente](https://docs.cypress.io/guides/guides/environment-variables)                                                                                                                                    |
|      group      |      string      | Testes registrados do [grupo](https://docs.cypress.io/guides/guides/parallelization#Grouping-test-runs) juntos em uma única execução                                                                                                    |
|     headed      |     boolean      | Exibe o navegador em vez de funcionar sem controle                                                                                                                                                                                      |
|    headless     |     boolean      | Oculte o navegador em vez de executar o cabeçalho (padrão durante a execução do cipreste)                                                                                                                                               |
|       key       |      string      | Especifica sua chave de registro secreto                                                                                                                                                                                                |
|      exit       |     boolean      | Se deve fechar o Cypress após a execução de todos os testes                                                                                                                                                                             |
|    parallel     |     boolean      | Execute especificações gravadas em [paralelo](https://docs.cypress.io/guides/guides/parallelization) em várias máquinas                                                                                                                 |
|      port       |      number      | Substituir porta padrão                                                                                                                                                                                                                 |
|     project     |      string      | Caminho para um projeto específico                                                                                                                                                                                                      |
|      quiet      |     boolean      | Se aprovado, a saída do Cypress não será impressa no stdout. Apenas a saída do [repórter Mocha](https://docs.cypress.io/guides/tooling/reporters) configurado será impressa.                                                            |
|     record      |     boolean      | Se deve registrar a execução do teste                                                                                                                                                                                                   |
|    reporter     |      string      | Especifica um repórter Mocha                                                                                                                                                                                                            |
| reporterOptions |      object      | Especifica as opções do repórter Mocha                                                                                                                                                                                                  |
|      spec       |      string      | Especifica as especificações a serem executadas, consulte os exemplos abaixo                                                                                                                                                            |
|       tag       |      string      | Identifique uma execução com uma etiqueta ou etiquetas                                                                                                                                                                                  |
|   testingType   |      string      | Especifica o tipo de testes a serem executados ou e2e ou componente. Padrões para e2e                                                                                                                                                  |

## Exemplos

### Execute um único arquivo de especificação

Aqui está um exemplo de execução programada de um arquivo de especificação. Observe que o caminho do arquivo é relativo 
ao diretório de trabalho atual.

```javascript
// e2e-run-tests.js
const cypress = require("cypress")

cypress
  .run({
    // o caminho é relativo ao diretório de trabalho atual
    spec: "./cypress/integration/examples/actions.spec.js",
  })
  .then((results) => {
    console.log(results)
  })
  .catch((err) => {
    console.error(err)
  })
```

Você pode então executar o Cypress executando o seguinte em seu terminal ou um script npm:

```bash
node e2e-run-tests.js
```

### Executar especificações usando curinga

Você pode passar um padrão curinga para executar todos os arquivos de especificações correspondentes

```javascript
const cypress = require("cypress")

cypress.run({
  // o caminho curinga é relativo ao diretório de trabalho atual
  spec: "./cypress/integration/**/api*.js",
})
```

### Uso da sintaxe moderna

Se a sua versão do Node permitir, você pode usar a sintaxe moderna async / await para aguardar a promessa retornada 
pelo método cypress.run.

```javascript
const cypress = require("cypress")

(async () => {
  const results = await cypress.run()
  // use o objeto results
})()
```

## Resultados

cypress.run() retorna uma promessa que resolve com um objeto que contém os resultados dos testes. 
Uma execução típica pode retornar algo assim:

```json
{
  "cypressVersion": "3.0.2",
  "endedTestsAt": "2018-07-11T17:53:35.675Z",
  "browserName": "electron",
  "browserPath": "path/to/browser",
  "browserVersion": "59.0.3071.115",
  "config": {...},
  "osName": "darwin",
  "osVersion": "14.5.0",
  "runs": [{
    "error": null,
    "hooks": [{
      "hookName": "before each",
      "title": [ "before each hook" ],
      "body": "function () {\n  expect(true).to.be["true"]\n}"
    }],
    "reporter": "spec",
    "reporterStats": {...},
    "shouldUploadVideo": true,
    "spec": {...},
    "stats": {
      "suites": 1,
      "tests": 1,
      "passes": 0,
      "pending": 0,
      "skipped": 0,
      "failures": 1,
      "startedAt": "2020-08-05T08:38:37.589Z",
      "endedAt": "2018-07-11T17:53:35.675Z",
      "duration": 1171
    },
    "tests": [{
      "title": [ "test" ],
      "state": "failed",
      "body": "function () {\n  expect(true).to.be["false"]\n}",
      "displayError": "AssertionError: expected true to be false\n' +
      '    at Context.eval (...cypress/integration/spec.js:5:21",
      "attempts": [{
        "state": "failed",
        "error": {
          "message": "expected true to be false",
          "name": "AssertionError",
          "stack": "AssertionError: expected true to be false\n' +
      '    at Context.eval (...cypress/integration/spec.js:5:21"
        },
        "screenshots": [{
          "name": null,
          "takenAt": "2020-08-05T08:52:20.432Z",
          "path": "User/janelane/my-app/cypress/screenshots/spec.js/test (failed).png",
          "height": 720,
          "width": 1280
        }],
        "startedAt": "2020-08-05T08:38:37.589Z",
        "duration": 1171,
        "videoTimestamp": 4486
      }]
    }],
    "video": "User/janelane/my-app/cypress/videos/abc123.mp4"
  }],
  "runUrl": "https://dashboard.cypress.io/projects/def456/runs/12",
  "startedTestsAt": "2018-07-11T17:53:35.463Z",
  "totalDuration": 212,
  "totalFailed": 1,
  "totalPassed": 0,
  "totalPending": 0,
  "totalSkipped": 0,
  "totalSuites": 1,
  "totalTests": 1,
}
```

Você pode encontrar a definição do TypeScript para o objeto de resultados no [cypress/cli/pastas de tipos](https://github.com/cypress-io/cypress/tree/develop/cli/types).

## Manipulação de erros

Mesmo quando os testes falham, a promessa resolve com os resultados do teste. 
A promessa só é rejeitada se o Cypress não puder ser executado por algum motivo (por exemplo, 
se um binário não foi instalado ou ele não consegue encontrar uma dependência de módulo). 
Nesse caso, a promessa será rejeitada com um erro detalhado.

Há uma terceira opção - o Cypress pode ser executado, mas os testes não podem ser iniciados por algum motivo. 
Nesse caso, o valor resolvido é um objeto com dois campos

```json
{
  "failures": 1, // número diferente de zero
  "message": "..." // mensagem de erro
}
```

Para lidar com esses possíveis erros, você pode adicionar uma captura para cypress.run():

```javascript
// e2e-run-tests.js
const cypress = require('cypress')

cypress.run({...})
.then(result => {
  if (result.failures) {
    console.error('Could not execute tests')
    console.error(result.message)
    process.exit(result.failures)
  }

  // imprimir resultados de teste e sair
  // com o número de testes com falha como código de saída
  process.exit(result.totalFailed)
})
.catch(err => {
  console.error(err.message)
  process.exit(1)
})

```

## cypress.open()

Abra os testes do Cypress via Node.js.

```javascript
// e2e-open-tests.js
const cypress = require("cypress")

cypress.open({
  config: {
    baseUrl: "http://localhost:8080",
  },
  env: {
    login_url: "/login",
    products_url: "/products",
  },
})
```

Em seguida, você pode abrir o Cypress executando o seguinte em seu terminal ou um script npm:

```bash
node e2e-open-tests.js
```

### Opções

Assim como o [Opções de linha de comando](https://docs.cypress.io/guides/guides/command-line), você pode passar opções 
que modificam como o Cypress é executado.

|    Opção    |       Tipo       | Descrição                                                                                                            |
| :---------: | :--------------: | -------------------------------------------------------------------------------------------------------------------- |
|   browser   |      string      | Especifique um caminho do sistema de arquivos para um navegador personalizado                                        |
|   config    |      object      | Especifique a [configuração](https://docs.cypress.io/guides/references/configuration)                                |
| configFile  | string / boolean | Caminho para o arquivo de configuração a ser usado. Se false for passado, nenhum arquivo de configuração será usado. |
|  detached   |     boolean      | Abra o Cypress em modo separado                                                                                      |
|     env     |      object      | Especifique as [variáveis ​​de ambiente](https://docs.cypress.io/guides/guides/environment-variables)                |
|   global    |     boolean      | Executar em modo global mode                                                                                         |
|    port     |      number      | Substituir porta padrão                                                                                              |
|   project   |      string      | Caminho para um projeto específico                                                                                   |
| testingType |      string      | Especifique o tipo de testes a serem executados ou e2e ou componente. Padrões para e2e                              |

## Exemplo

```javascript
// e2e-open-tests.js
const cypress = require("cypress")

cypress.open({})
```

Em seguida, você pode abrir o Cypress executando o seguinte em seu terminal ou um script npm:

```bash
node e2e-open-tests.js
```

## cypress.cli

## parseRunArguments()

Se você estiver escrevendo uma ferramenta que envolve o comando cypress.run(), você pode querer analisar 
argumentos de linha de comando fornecidos pelo usuário usando a mesma lógica que o cypress run usa.
Nesse caso, você pode usar a função parseRunArguments incluída.

```javascript
// wrapper.js
const cypress = require("cypress")

const runOptions = await cypress.cli.parseRunArguments(process.argv.slice(2))
const results = await cypress.run(runOptions)
// processar os resultados  "cypress.run()"
```

Um exemplo de uso executado em seu terminal poderia ser:

``` bash
node ./wrapper cypress run --browser chrome --config ...
```

Observação: os argumentos passados ​​para parseRunArguments devem começar com cypress run.

Usamos a análise CLI e a chamada de cypress.run para repetir testes para encontrar testes instáveis ​​e para 
[repita os testes para encontrar testes instáveis](https://github.com/bahmutov/cypress-repeat) e para 
[validar os números de teste após uma execução de teste](https://github.com/bahmutov/cypress-expect). 
leia [Wrap Cypress usando npm Module API](https://glebbahmutov.com/blog/wrap-cypress-using-npm/) para mais exemplos.

## História

| Versão |                               Mudanças                               |
| :----: | :------------------------------------------------------------------: |
| 7.3.0  |            Adicionada opção de configuração testingType.             |
| 5.0.0  | Os resultados do teste retornados de cypress.run () foram alterados. |
| 4.11.0 |        Adicionado cypress.cli com a função parseRunArguments.        |
| 4.9.0  |             Adicionada opção silenciosa a cypress.run ()             |
