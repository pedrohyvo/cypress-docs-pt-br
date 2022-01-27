# Linha de comando

```markdown
O que você vai aprender

- Como executar o Cypress na linha de comando
- Como especificar quais arquivos de especificação executar
- Como iniciar em outros navegadores
- Como registrar seus testes no Dashboard
```

## Instalação

Este guia assume que você já leu nosso guia 
[Instalando o Cypress](https://github.com/pedrohyvo/cypress-docs-pt-br/blob/master/pages/getting-started/installing-cypress.md)
e instalou o Cypress como um módulo `npm`. Após a instalação, você poderá executar todos
os comandos neste documento a partir da **raiz** do seu **projeto**.

## Como executar comandos

> Como alternativa, você pode executar o Cypress como um modulo usando nossa [módulo API](https://github.com/pedrohyvo/cypress-docs-pt-br/blob/master/pages/guides/module-api.md).

Para resumir, omitimos o caminho completo para o executável cypress na documentação de cada comando.

Para executar um comando, você precisará prefixa-los afim de localizar corretamente o executável do cypress.

```bash
$(npm bin)/cypress run
```

...ou...

```bash
./node_modules/.bin/cypress run
```

...ou... (requer `npm@5.2.0` ou superior)

```bash
npx cypress run
```

...ou usando Yarn...

```bash
yarn open
```

Você pode achar mais fácil adicionar o comando cypress ao objeto  `scripts` em seu arquivo `package.json`
e chamá-lo a partir de um [script](https://docs.npmjs.com/cli/v7/commands/npm-run-script) `npm run`.

Ao chamar um comando usando `npm run`, você precisa passar os argumentos do comando usando 
a string `--`. Por exemplo, se você tiver o seguinte comando definido em seu `package.json`.

```json
{
  "scripts": {
    "cy:run": "cypress run"
  }
}
```

...e desejar executar testes a partir de um único arquivo de especificações e 
registrar os resultados no painel, o comando deve ser:

```bash
npm run cy:run -- --record --spec "cypress/integration/my-spec.js"
```

Se você estiver usando a ferramenta [npx](https://github.com/zkat/npx),
poderá invocar o Cypress instalado localmente:

```bash
npx cypress run --record --spec "cypress/integration/my-spec.js"
```

>Leia como normalmente organizamos e executamos scripts npm na postagem do blog [Como organizo meus scripts npm](https://glebbahmutov.com/blog/organize-npm-scripts/).

## Comandos

### `cypress run`

Executa os testes Cypress até a conclusão. Por padrão, `cypress run` executará todos os testes sem controle.

```bash
cypress run [options]
```

#### Opções

|          Opção         | Descrição                                                                                                                                                                                                       |
|------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| --browser, -b          | [Executa o Cypress no navegador com o nome fornecido. Se um caminho do sistema de arquivos for fornecido, o Cypress tentará usar o navegador nesse caminho](#cypress-run---browser-browser-name-or-path).       |
| --ci-build-id          | [Especifique um identificador exclusivo para uma execução para permitir agrupamento ou paralelização](#cypress-run---ci-build-id-id).                                                                           |
| --config, -c           | [Especifique a configuração](#cypress-run---config-config)                                                                                                                                                      |
| --config-file, -C      | [Especifique o arquivo de configuração](#cypress-run---config-file-config-file)                                                                                                                                 |
| --env, -e              | [Especifique as variáveis de ambiente](#cypress-run---env-env)                                                                                                                                                  |
| --group                | [Testes gravados em grupo em uma única execução](#cypress-run---group-name)                                                                                                                                     |
| --headed               | [Exibe o navegador em vez de funcionar sem controle](#cypress-run---headed)                                                                                                                                     | 
| --headless             | Oculta o navegador em vez de executar o cabeçalho (padrão durante `cypress run`)                                                                                                                                |
| --help, -h             | Informações de uso de saída                                                                                                                                                                                     |
| --key, -k              | [Especifique sua chave de registro secreto](#cypress-run---record---key-record-key)                                                                                                                             |
| --no-exit              | [Mantenha o Cypress Test Runner aberto após os testes em um arquivo de especificação executado](#cypress-run---no-exit)                                                                                         |
| --parallel             | [Execute especificações gravadas em paralelo em várias máquinas](#cypress-run---parallel)                                                                                                                       |
| --port,-p              | [Substituir porta padrão](#cypress-run---port-port)                                                                                                                                                             |
| --project, -P          | [Caminho para um projeto específico](#cypress-run---project-project-path)                                                                                                                                       |
| --quiet, -q            | Se aprovado, a saída do Cypress não será impressa para o `stdout`. Apenas a saída do [repórter Mocha](https://docs.cypress.io/guides/tooling/reporters) configurado será impressa.                              |
| --record               | [Caso queira registrar a execução do teste](#cypress-run---record---key-record-key)                                                                                                                             |
| --reporter, -r         | [Especifique um repórter Mocha](#cypress-run---reporter-reporter)                                                                                                                                               |
| --reporter-options, -o | [Especifique as opções do repórter Mocha](#cypress-run---reporter-reporter)                                                                                                                                     |
| --spec, -s             | [Especifique os arquivos de especificação a serem executados](#cypress-run---spec-spec)                                                                                                                         |
| --tag, -t              | [Identifique uma execução com uma etiqueta ou etiquetas](#cypress-run---tag-tag)                                                                                                                                |

### cypress run --browser \<browser-name-or-path>

```bash
cypress run --browser chrome
```

O argumento "browser" pode ser definido como `chrome`, `chromium`, `edge`, `electron`, `firefox` para iniciar um 
navegador instalado em seu sistema. O Cypress tentará encontrar automaticamente o navegador instalado para você.

Para iniciar navegadores não estáveis, adicione dois pontos e o canal de lançamento desejado. Por exemplo, 
para iniciar o Chrome Canary, use `chrome:canary`.

Você também pode escolher um navegador, fornecendo um caminho:

```bash
cypress run --browser /usr/bin/chromium
```

[//]: <> (TODO - Adicionar link - solução de problemas inicinado navegadores)

[Problemas com a detecção do navegador? Confira nosso guia de solução de problemas](https://docs.cypress.io/guides/references/troubleshooting#Launching-browsers)

### cypress run --ci-build-id \<id>

Este valor deve ser detectado automaticamente pela maioria dos provedores de CI e não é necessário defini-lo, 
a menos que o Cypress não seja capaz de determiná-lo.

Normalmente, isso é definido como uma variável de ambiente dentro de seu provedor de CI, definindo
um único "build" ou "run".

```bash
cypress run --ci-build-id BUILD_NUMBER
```

[//]: <> (TODO - Adicionar link - paralelização)

Válido apenas ao fornecer um sinalizador `--group` ou `--parallel`. Leia nossa documentação de 
[paralelização](https://docs.cypress.io/guides/guides/parallelization) para saber mais.

### cypress run --config \<config>

Defina os valores de configuração. Separe vários valores com uma vírgula. Os valores definidos aqui substituem 
quaisquer valores definidos em seu arquivo de configuração.

```bash
cypress run --config pageLoadTimeout=100000,watchForFileChanges=false
```

<br />

> **Exemplo do mundo real**

[//]: <> (TODO - Adicionar link - configuração de tamanho das janelas)

> O Cypress [Real World App (RWA)](https://github.com/cypress-io/cypress-realworld-app) usa o sinalizador `--config` 
para especificar facilmente o [tamanho das janelas](https://docs.cypress.io/guides/references/configuration#Viewport) 
de visualização para testes responsivos localmente 
e em trabalhos de CI dedicados. Exemplos:

> - [Scripts npm](https://github.com/cypress-io/cypress-realworld-app/blob/07a6483dfe7ee44823380832b0b23a4dacd72504/package.json#L120)
para executar o Cypress na janela de visualização móvel.

> - [Configuração do trabalho do ciclo de CI](https://github.com/cypress-io/cypress-realworld-app/blob/07a6483dfe7ee44823380832b0b23a4dacd72504/.circleci/config.yml#L82-L100)
para executar suítes de teste na janela de visualização móvel.

### cypress run --config-file \<config-file>`

[//]: <> (TODO - Adicionar link - configuração)

Você pode especificar um caminho para um arquivo JSON onde os valores de 
[configuração](https://docs.cypress.io/guides/references/configuration) são definidos. O padrão é `cypress.json`.

```bash
cypress run --config-file tests/cypress-config.json
```

Você pode passar `false` para desabilitar totalmente o uso de um arquivo de configuração.

```bash
cypress run --config-file false
```

### cypress run --env \<env>

[//]: <> (TODO - Adicionar link - variáveis de ambiente)

Defina as [variáveis de ambiente](https://docs.cypress.io/guides/guides/environment-variables) do Cypress.

```bash
cypress run --env host=api.dev.local
```

Passe diversas variáveis usando vírgulas e sem espaços. Os números são convertidos automaticamente para strings.

```bash
cypress open --env host=api.dev.local,port=4222
```

Passe um objeto como JSON em uma string.

```bash
cypress open --env flags='{"feature-a":true,"feature-b":false}'
```

### cypress run --group \<name>

Testes gravados em grupo em uma única execução.

```bash
cypress run --group develop-env
```

Você pode adicionar vários grupos à mesma execução, passando um nome diferente. Isso pode ajudar a distinguir grupos 
de especificações uns dos outros.

```bash
cypress run --group admin-tests --spec 'cypress/integration/admin/**/*'
```

```bash
cypress run --group user-tests --spec 'cypress/integration/user/**/*'
```

Especificar `--ci-build-id` também pode ser necessário.

[//]: <> (TODO - Adicionar link - paralelização de agrupar execuções de teste)

[Leia mais sobre agrupamento.](https://docs.cypress.io/guides/guides/parallelization#Grouping-test-runs)

### cypress run --headed

Por padrão, o Cypress irá executar testes sem controle durante o `cypress run`.

Passar `--headed` forçará o navegador a ser mostrado. Corresponde à forma como você executa qualquer navegador por 
meio do `cypress open`.

```bash
cypress run --headed
```

### cypress run --no-exit

Para evitar que o Cypress Test Runner saia após a execução de testes em um arquivo de especificação, use `--no-exit`.

Você pode passar `--headed --no-exit` para ver o **log de comando** ou ter acesso às **ferramentas do desenvolvedor** 
após a execução de uma `spec`.

```bash
cypress run --headed --no-exit
```

### cypress run --parallel

[//]: <> (TODO - Adicionar link - paralelização)

Execute especificações gravadas em [paralelo](https://docs.cypress.io/guides/guides/parallelization) em várias máquinas.

```bash
cypress run --record --parallel
```

[//]: <> (TODO - Adicionar link - paralelização de agrupar execuções de teste)

Além disso, você pode passar um sinalizador `--group` para que ele apareça como um 
[grupo](https://docs.cypress.io/guides/guides/parallelization#Grouping-test-runs) nomeado.

```bash
cypress run --record --parallel --group e2e-staging-specs
```

[//]: <> (TODO - Adicionar link - paralelização)

Leia nossa documentação de [paralelização](https://docs.cypress.io/guides/guides/parallelization) para saber mais.

### cypress run --port \<port>

```bash
cypress run --port 8080
```

### cypress run --project \<project-path>

Para ver isso em ação, configuramos um [repositório de exemplo para demonstração](https://github.com/cypress-io/cypress-test-nested-projects).

```bash
cypress run --project ./some/nested/folder
```

### cypress run --record --key \<record-key>

Grave o vídeo dos testes em execução após [configurar seu projeto para gravação](https://github.com/pedrohyvo/cypress-docs-pt-br/blob/master/pages/dashboard/projects.md#configura%C3%A7%C3%A3o).
Depois de configurar seu projeto, você receberá uma **chave de registro**.

```bash
cypress run --record --key <record_key>
```

Se você definir a **chave de registro** como a variável de ambiente `CYPRESS_RECORD_KEY`, poderá omitir o sinalizador `--key`.

[//]: <> (TODO - Adicionar link - integração contínua)

Você normalmente definiria essa variável de ambiente ao executar em [integração contínua](https://docs.cypress.io/guides/continuous-integration/introduction).

```bash
export CYPRESS_RECORD_KEY=abc-key-123
```

Agora você pode omitir o sinalizador `--key`.

```bash
cypress run --record
```

Você pode ler mais sobre a [gravação de execuções aqui](https://github.com/pedrohyvo/cypress-docs-pt-br/blob/master/pages/dashboard/projects.md#configura%C3%A7%C3%A3o).

### cypress run --reporter \<reporter>

Você pode fazer testes especificando um repórter Mocha.

```bash
cypress run --reporter json
```

Você pode especificar opções de repórter usando o sinalizador `--reporter-options <reporter-options>`.

```bash
cypress run --reporter junit --reporter-options mochaFile=result.xml,toConsole=true
```

### cypress run --spec \<spec>

Execute testes especificando um único arquivo de teste a ser executado em vez de todos os testes. O caminho de 
especificação deve ser um caminho absoluto ou relativo ao diretório de trabalho atual.

```bash
cypress run --spec "cypress/integration/examples/actions.spec.js"
```

Execute testes dentro da pasta que corresponde ao glob (Observação: o uso de aspas duplas é altamente recomendado).

```bash
cypress run --spec "cypress/integration/login/**/*"
```

Execute testes especificando vários arquivos de teste a serem executados.

```bash
cypress run --spec "cypress/integration/examples/actions.spec.js,cypress/integration/examples/files.spec.js"
```

Use em combinação com o parâmetro `--project`. Imagine que os testes Cypress estão em uma subpasta `tests/e2e` 
do projeto atual:

```bash
app/
  node_modules/
  package.json
  tests/
    unit/
    e2e/
      cypress/
        integration/
          spec.js
      cypress.json
```

Se estivermos na pasta `app`, podemos executar as especificações usando o seguinte comando

```bash
cypress run --project tests/e2e --spec ./tests/e2e/cypress/integration/spec.js
```

### cypress run --tag \<tag>

Adicione uma ou mais tags à execução registrada. Isso pode ser usado para ajudar a identificar uma execução 
separada quando exibida no painel.

```bash
cypress run  --record --tag "staging"
```

Faça uma execução com várias tags.

```bash
cypress run --record --tag "production,nightly"
```

O painel exibirá todas as tags enviadas com a execução apropriada.

![Cypress executado no painel exibindo sinalizadores](https://user-images.githubusercontent.com/37879075/142554208-542b5afd-eb56-4722-a07e-283f987f57b7.png)

#### Código de saída

Quando o Cypress termina a execução dos testes, ele sai. Se não houver testes com falha, o código de saída será 0.

```bash
## Todos os testes passam
$ cypress run
...
                                        Tests  Passing  Failing
    ✔  All specs passed!      00:16       17       17        0

## imprimir código de saída no Mac ou Linux
$ echo $?
0
```

Se houver alguma falha de teste, o código de saída corresponderá ao número de testes que falharam.

```bash
## Especificação com dois testes de falha
$ cypress run
...
                                        Tests  Passing  Failing
    ✖  1 of 1 failed (100%)   00:22       17       14        2

## imprimir código de saída no Mac ou Linux
$ echo $?
2
```

Se o Cypress não puder ser executado por algum motivo (por exemplo, se nenhum arquivo de especificação for encontrado), 
o código de saída será 1.

```bash
## Nenhum arquivo de especificação encontrado
$ cypress run --spec not-found.js
...
Can t run because no spec files were found.

We searched for any files matching this glob pattern:

not-found.js

## imprimir código de saída no Mac ou Linux
$ echo $?
1
```

### `cypress open`

Abre o Cypress Test Runner.

```bash
cypress open [options]
```

#### Opções

As opções passadas para `cypress open` serão aplicadas automaticamente ao projeto que você abrir. Eles persistem em 
todos os projetos até que você saia do Cypress Test Runner. Essas opções também substituirão os valores em seu 
arquivo de configuração (`cypress.json` por padrão).

|       Opção       | Descrição                                                                                                                                      |
|-------------------|------------------------------------------------------------------------------------------------------------------------------------------------|
| --browser, -b     | [Caminho para um navegador personalizado a ser adicionado à lista de navegadores disponíveis no Cypress](#cypress-open---browser-browser-path) |
| --config, -c      | [Especifique a configuração](#cypress-open---config-config)                                                                                    |
| --config-file, -C | [Especifique o arquivo de configuração](#cypress-open---config-file-config-file)                                                               |
| --detached, -d    | Abra o Cypress em modo independente                                                                                                            |
| --env, -e         | [Especifique as variáveis de ambiente](#cypress-open---env-env)                                                                                |
| --global          | [Executar em modo global](#cypress-open---global)                                                                                              |
| --help, -h        | Informações do uso de saída                                                                                                                    |
| --port, -p        | [Substituir porta padrão](#cypress-open---port-port)                                                                                           |
| --project, -P     | [Caminho para um projeto específico](#cypress-open---project-,project-path)                                                                    |

### cypress open --browser \<browser-path>

Por padrão, o Cypress encontrará automaticamente e permitirá que você use os navegadores instalados em seu sistema.

A opção "browser" permite que você especifique o caminho para um navegador personalizado para usar com o Cypress:

```bash
cypress open --browser /usr/bin/chromium
```

Se encontrado, o navegador especificado será adicionado à lista de navegadores disponíveis no Cypress Test Runner.

Atualmente, apenas navegadores da família Chrome (incluindo o novo Microsoft Edge e Brave baseado em Chromium) 
e Firefox são suportados.

[//]: <> (TODO - Adicionar link - solução de problemas inicinado navegadores)

[Está tendo problemas para iniciar um navegador? Confira nosso guia de solução de problemas](https://docs.cypress.io/guides/references/troubleshooting#Launching-browsers)

### cypress open --config \<config>

[//]: <> (TODO - Adicionar link - configuração)

Defina os valores de [configuração](https://docs.cypress.io/guides/references/configuration). 
Separe vários valores com uma vírgula. Os valores definidos aqui 
substituem quaisquer valores definidos em seu arquivo de configuração.

```bash
cypress open --config pageLoadTimeout=100000,watchForFileChanges=false
```

### cypress open --config-file \<config-file>

[//]: <> (TODO - Adicionar link - configuração)

Você pode especificar um caminho para um arquivo JSON onde os valores de 
[configuração](https://docs.cypress.io/guides/references/configuration) são definidos. O padrão é `cypress.json`.

```bash
cypress open --config-file tests/cypress-config.json
```

Você pode passar `false` para desabilitar totalmente o uso de um arquivo de configuração.

```bash
cypress open --config-file false
```

### cypress open --env \<env>

[//]: <> (TODO - Adicionar link - variáveis de ambiente)

Defina as [variáveis de ambiente](https://docs.cypress.io/guides/guides/environment-variables) do Cypress.

```bash
cypress open --env host=api.dev.local
```

Passe várias variáveis usando vírgulas e sem espaços. Os números são convertidos automaticamente para strings.

```bash
cypress open --env host=api.dev.local,port=4222
```

Passe um objeto como JSON em uma string.

```bash
cypress open --env flags='{"feature-a":true,"feature-b":false}'
```

### cypress open --global

Abrir o Cypress no modo global é útil se você tiver vários projetos aninhados, mas quiser compartilhar uma única 
instalação global do Cypress. Neste caso, você pode adicionar cada projeto aninhado ao Cypress no modo global, dando 
a você uma bela UI para alternar entre eles.

```bash
cypress open --global
```

## cypress open --port \<port>

```bash
cypress open --port 8080
```

## cypress open --project \ <project-path>

Para ver isso em ação, configuramos um [repositório de exemplo para demonstração](https://github.com/cypress-io/cypress-test-nested-projects).

```bash
cypress open --project ./some/nested/folder
```

### `cypress info`

Imprime informações sobre o Cypress e o ambiente atual, como:
  
  [//]: <> (TODO - Adicionar link - configuração do proxy)

- Uma lista de navegadores Cypress detectados na máquina.
- Quaisquer variáveis de ambiente que controlam a [configuração do proxy](https://docs.cypress.io/guides/references/proxy-configuration).
- Quaisquer variáveis de ambiente que começam com o prefixo `CYPRESS` (com variáveis confidenciais, como [chave de 
registro](https://docs.cypress.io/guides/dashboard/projects#Record-keys) mascarada para segurança).
- O local onde os dados de tempo de execução são armazenados.
- O local onde o binário do Cypress está armazenado em cache.
- Informações do sistema operacional.
Memória do sistema incluindo espaço livre.

```shell
cypress info
Displaying Cypress info...

Detected 2 browsers installed:

1. Chrome
  - Name: chrome
  - Channel: stable
  - Version: 79.0.3945.130
  - Executable: /path/to/google-chrome
  - Profile: /user/profile/folder/for/google-chrome

2. Firefox Nightly
  - Name: firefox
  - Channel: nightly
  - Version: 74.0a1
  - Executable: /path/to/firefox

Note: to run these browsers, pass <name>:<channel> to the '--browser' field

Examples:
- cypress run --browser firefox:nightly
- cypress run --browser chrome

Learn More: https://on.cypress.io/launching-browsers

Proxy Settings: none detected
Environment Variables: none detected

Application Data: /path/to/app/data/cypress/cy/development
Browser Profiles: /path/to/app/data/cypress/cy/development/browsers
Binary Caches: /user/profile/path/.cache/Cypress

Cypress Version: 4.1.0
System Platform: darwin (19.2.0)
System Memory: 17.2 GB free 670 MB
```
  
[//]: <> (TODO - Adicionar link - variável de ambiente DEBUG)

**Dica:** defina [a variável de ambiente DEBUG](https://docs.cypress.io/guides/references/troubleshooting#Print-DEBUG-logs)
como `cypress:launcher`ao executar `cypress info` para solucionar problemas de detecção do navegador.

### `cypress verify`

Verifica se o Cypress está instalado corretamente e é executável.

```bash
cypress verify
✔  Verified Cypress! /Users/jane/Library/Caches/Cypress/3.0.0/Cypress.app
```

### `cypress version`

Imprime a versão binária do Cypress instalada, a versão do pacote do Cypress, a versão do Electron usada para 
construir o Cypress e a versão do Node agrupada.

Na maioria dos casos, as versões do binário e do pacote serão iguais, mas podem ser diferentes se você tiver instalado 
uma versão diferente do pacote e por algum motivo não tiver instalado a versão binária correspondente.

```bash
cypress version
Cypress package version: 6.0.0
Cypress binary version: 6.0.0
Electron version: 10.1.5
Bundled Node version: 12.14.1
```

Você também pode imprimir o número da versão de cada componente individualmente.

```bash
cypress version --component package
6.0.0
cypress version --component binary
6.0.0
cypress version --component electron
10.1.5
cypress version --component node
12.14.1
```

### `cypress cache [command]`

Comandos para gerenciar o cache global do Cypress. O cache do Cypress se aplica a todas as instalações do Cypress 
em sua máquina, globais ou não.

### `cypress cache path`

Imprime o `path` para a pasta de cache do Cypress. Você pode alterar o caminho onde o cache do Cypress está localizado 
seguindo [estas instruções](https://docs.cypress.io/guides/getting-started/installing-cypress#Binary-cache).

```bash
cypress cache path
/Users/jane/Library/Caches/Cypress
```

### `cypress cache list`

Imprima todas as versões existentes do Cypress instaladas. A saída será uma tabela com versões em cache e a última vez 
que o binário foi usado pelo usuário, determinada a partir do momento de acesso ao arquivo.


```bash
cypress cache list
┌─────────┬──────────────┐
│ version │ last used    │
├─────────┼──────────────┤
│ 3.0.0   │ 3 months ago │
├─────────┼──────────────┤
│ 3.0.1   │ 5 days ago   │
└─────────┴──────────────┘
```

Você pode calcular o tamanho de cada pasta de versão do Cypress adicionando o argumento `--size` ao comando. Observe 
que o cálculo do tamanho do disco pode ser lento.

```bash
cypress cache list --size
┌─────────┬──────────────┬─────────┐
│ version │ last used    │ size    │
├─────────┼──────────────┼─────────┤
│ 5.0.0   │ 3 months ago │ 425.3MB │
├─────────┼──────────────┼─────────┤
│ 5.3.0   │ 5 days ago   │ 436.3MB │
└─────────┴──────────────┴─────────┘
```

### `cypress cache clear`

Limpa o conteúdo do cache do Cypress. Isso é útil quando você deseja que o Cypress limpe todas as versões instaladas do 
Cypress que podem estar armazenadas em cache em sua máquina. Depois de executar este comando, você precisará executar 
`cypress install` antes de executar o Cypress novamente.

```bash
cypress cache clear
```

### `cypress cache prune`

Exclui todas as versões Cypress instaladas do cache, exceto a versão atualmente instalada.

```bash
cypress cache prune
```

## Comandos de depuração

### Habilitar registros de depuração

O Cypress é construído usando o módulo de [depuração](https://github.com/visionmedia/debug). 
Isso significa que você pode receber resultados de depuração 
úteis executando Cypress com ele ativado antes de executar `cypress open` ou `cypress run`.

#### No Mac ou Linux

```bash 
DEBUG=cypress:* cypress open
```

```bash 
DEBUG=cypress:* cypress run
```

#### No Windows


```bash 
set DEBUG=cypress:*
```

```bash 
cypress run
```

Cypress é um projeto bastante grande e complexo envolvendo uma dúzia ou mais de submódulos, e a saída padrão 
pode ser muito pesada.

#### Para filtrar a saída de depuração para um módulo específico

```bash 
DEBUG=cypress:cli cypress run
```

```bash 
DEBUG=cypress:launcher cypress run
```

...ou mesmo um submódulo profundo de 3º nível

```bash
DEBUG=cypress:server:project cypress run
```

## História

| Versão | Alterar                                                              |
|:------:|----------------------------------------------------------------------|
| 5.4.0  | Adicionado o subcomando `prune`  ao `cypress cache`                  |
| 5.4.0  | Adicionada a sinalização `--size` ao subcomando `cypress cache list` |
| 4.9.0  | Adicionada o sinalizador `--quiet` ao `cypress run`                  |

[Voltar para o topo](#linha-de-comando)
