# Escrevendo e Organizando Testes

>O que você aprenderá
>
>-Como organizar seus arquivos de teste e suporte.
>
>-Quais idiomas são suportados em seus arquivos de teste.
>
>-Como o Cypress lida com testes de unidade versus testes de integração.
>
>-Como agrupar seus testes.

>Melhores Práticas
>
>Recentemente, demos uma palestra na conferência “Melhores práticas” na AssertJS
>(fevereiro de 2018). Este vídeo demonstra
>como dividir seu aplicativo e organizar seus testes.
>
><https://www.youtube.com/watch?v=5XQOK0v_YRE>

## Estrutura da Pasta

Depois de adicionar um novo projeto, o Cypress criará automaticamente uma estrutura de pastas sugerida. Por padrão, ele criará:

```markdown
/cypress
  /fixtures
    - example.json

  /integration
    /examples
      - actions.spec.js
      - aliasing.spec.js
      - assertions.spec.js
      - connectors.spec.js
      - cookies.spec.js
      - cypress_api.spec.js
      - files.spec.js
      - local_storage.spec.js
      - location.spec.js
      - misc.spec.js
      - navigation.spec.js
      - network_requests.spec.js
      - querying.spec.js
      - spies_stubs_clocks.spec.js
      - traversal.spec.js
      - utilities.spec.js
      - viewport.spec.js
      - waiting.spec.js
      - window.spec.js

  /plugins
    - index.js

  /support
    - commands.js
    - index.js
  ```

## Configurando a Estrutura da Pasta

Enquanto o Cypress permite que você configure onde seus testes, fixtures
e arquivos de suporte estão localizados, se você estiver iniciando
seu primeiro projeto, recomendamos que você use a estrutura acima.

Você pode modificar a configuração da pasta em seu arquivo de configuração.
Veja a [configuração](https://docs.cypress.io/guides/references/configuration.html#Folders-Files) para mais detalhes.

>__Quais arquivos devo adicionar ao meu 'arquivo .gitignore'?__
>
>O Cypress criará um [`screenshotsFolder`](https://docs.cypress.io/guides/references/configuration.html#Screenshots) e um
>[`videosFolder`](https://docs.cypress.io/guides/references/configuration.html#Videos) para armazenar as capturas de tela
>e vídeos feitos durante o teste de seu aplicativo. Muitos usuários optarão por adicionar essas pastas a seus `.gitignore`
>arquivos. Além disso, se você estiver armazenando variáveis ​​de ambiente confidenciais em seu arquivo de configuração
>(`cypress.json` por padrão) ou [`cypress.env.json`](https://docs.cypress.io/guides/guides/environment-variables.html#Option-2-cypress-env-json),
>elas também devem ser ignoradas ao fazer check-in no controle de origem.


## Arquivos de teste

Os arquivos de teste estão localizados `cypress/integration` por padrão, mas podem ser [configurados](https://docs.cypress.io/guides/references/configuration.html#Folders-Files)
em outro diretório. Os arquivos de teste podem ser escritos como:

- `.js`
- `.jsx`
- `.coffee`
- `.cjsx`

Cypress também oferece suporte pronto `ES2015` para uso. Você pode usar `ES2015 modules` ou `CommonJS modules`. Isto significa
 que pode `import` ou `require` ambos NPM pacotes e módulos relativos locais .

>__Receita de exemplo__
>
>Confira nossa receita usando os [módulos ES2015 e CommonJS](https://docs.cypress.io/examples/examples/recipes.html#Fundamentals).

Para ver um exemplo de cada comando usado no Cypress, abra a `example` [pasta](https://github.com/cypress-io/cypress-example-kitchensink/tree/master/cypress/integration/examples)
dentro da sua `cypress/integration` pasta.

Para começar a escrever testes para seu aplicativo, crie um novo arquivo como `app_spec.js` em sua `cypress/integration`
pasta. Atualize sua lista de testes no Cypress Test Runner e seu novo arquivo deve ter aparecido na lista.

## Arquivos de fixação

As luminárias são usadas como partes externas de dados estáticos que podem ser usadas por seus testes. Os arquivos de
luminárias estão localizados `cypress/fixtures` por padrão, mas podem ser [configurados](https://docs.cypress.io/guides/references/configuration.html#Folders-Files)
em outro diretório.

Você normalmente os usaria com o [`cy.fixture()`](https://docs.cypress.io/api/commands/fixture.html) comando e com mais
frequência quando estiver fazendo o stub de [Solicitações de Rede](https://docs.cypress.io/guides/guides/network-requests.html).

## Arquivos de ativos

Existem algumas pastas que podem ser geradas após a execução de um teste, contendo ativos que foram gerados durante a execução
do teste.

Você pode considerar adicionar essas pastas ao seu `.gitignore` arquivo para ignorar a verificação desses arquivos no controle
de origem.

## Download de arquivos

Todos os arquivos baixados durante o teste do recurso de download de arquivo de um aplicativo serão armazenados no, [`downloadsFolder`](https://docs.cypress.io/guides/references/configuration.html#Downloads)
que é definido como `cypress/downloads` padrão.

```markdown
/cypress
  /downloads
    - records.csv
```

## Arquivos de captura de tela

Se as capturas de tela foram tiradas por meio do [`cy.screenshot()`](https://docs.cypress.io/api/commands/screenshot.html)
comando ou automaticamente quando um teste falha, as capturas de tela são armazenadas no [`screenshotsFolder`](https://docs.cypress.io/guides/references/configuration.html#Screenshots)
qual é definido como cypress/screenshotspor padrão.

```markdown
/cypress
  /screenshots
    /app_spec.js
      - Navigates to main menu (failures).png
```

Para saber mais sobre as capturas de tela e as configurações disponíveis, consulte [Capturas de tela e vídeos](https://docs.cypress.io/guides/guides/screenshots-and-videos.html#Screenshots)

## Arquivos de vídeo

Todos os vídeos gravados da corrida são armazenados no, [`videosFolder`](https://docs.cypress.io/guides/references/configuration.html#Videos)
que é definido como `cypress/videos` padrão.

```markdown
/cypress
  /videos
    - app_spec.js.mp4
```

Para saber mais sobre os vídeos e as configurações disponíveis, consulte [Capturas de tela e vídeos](https://docs.cypress.io/guides/guides/screenshots-and-videos.html#Screenshots)

## Arquivos de plug-in

Por padrão, o Cypress incluirá automaticamente o arquivo de plug-ins `cypress/plugins/index.js` antes de cada arquivo de
especificação executado. Fazemos isso puramente como um mecanismo de conveniência para que você não precise importar esse
arquivo em cada um dos seus arquivos de especificação.

O arquivo de plug-ins importado inicial pode ser [configurado para outro arquivo](https://docs.cypress.io/guides/references/configuration.html#Folders-Files).

[Leia mais sobre como usar plug-ins para estender o comportamento do Cypress](https://docs.cypress.io/guides/tooling/plugins-guide.html).

## Arquivo de suporte

Por padrão, o Cypress incluirá automaticamente o arquivo de suporte `cypress/support/index.js`. Este arquivo é executado
antes de cada arquivo de especificação. Fazemos isso puramente como um mecanismo de conveniência para que você não precise
importar esse arquivo em cada um dos seus arquivos de especificação.

>Lembre-se de que, ao clicar em “Executar todas as especificações” depois [`cypress open`](https://docs.cypress.io/guides/guides/command-line.html#cypress-open),
>o código no arquivo de suporte é executado uma vez antes de todos os arquivos de especificação, em vez de uma vez antes
>de cada arquivo de especificação. Consulte [Execução](https://docs.cypress.io/guides/core-concepts/writing-and-organizing-tests.html#Support-file)
>para obter mais detalhes.

O arquivo de suporte importado inicial pode ser configurado para outro arquivo ou desligado completamente usando a [`supportFile`](https://docs.cypress.io/guides/references/configuration.html#Folders-Files)
configuração.

O arquivo de suporte é um ótimo lugar para colocar comportamento reutilizável, como [comandos personalizados](https://docs.cypress.io/api/cypress-api/custom-commands.html#Syntax)
ou substituições globais que você deseja aplicar e disponibilizar para todos os seus arquivos de especificação.

Do seu arquivo de suporte, você pode `import` ou `require` outros arquivos para manter as coisas organizadas.

Nós semeamos automaticamente um arquivo de suporte de exemplo, que tem vários exemplos comentados.

Você pode definir comportamentos em um `before` ou `beforeEach` em qualquer um dos `cypress/support` arquivos:

```js
beforeEach(() => {
  cy.log('I run before every test in every spec file!!!!!!')
})
```

![Ganchos globais para testes](https://docs.cypress.io/img/guides/global-hooks.993be24d.png)

>Nota: Este exemplo pressupõe que você já esteja familiarizado com os [ganchos](https://docs.cypress.io/guides/core-concepts/writing-and-organizing-tests.html#Hooks)
>Mocha.

## Execução

Cypress executa o arquivo de suporte antes do arquivo de especificação. Por exemplo, quando você clica em um arquivo de
teste denominado `spec-a.js` via [`cypress open`](https://docs.cypress.io/guides/guides/command-line.html#cypress-open),
o Test Runner executa os arquivos na seguinte ordem:

```js
<!-- bundled support file -->
<script src="support/index.js"></script>
<!-- bundled spec file -->
<script src="integration/spec-a.js"></script>
```

O mesmo acontece ao usar o [`cypress run`](https://docs.cypress.io/guides/guides/command-line.html#cypress-run) comando:
uma nova janela do navegador é aberta para cada par de arquivos de suporte e especificações.

Mas quando você clica no botão “Executar todas as especificações” depois [`cypress open`](https://docs.cypress.io/guides/guides/command-line.html#cypress-open),
o Test Runner empacota e concatena todas as especificações juntas, basicamente executando scripts como mostrado abaixo.
Isso significa que o código no arquivo de suporte é executado uma vez antes de todos os arquivos de especificação, em vez
de uma vez antes de cada arquivo de especificação.

```js
<!-- bundled support file -->
<script src="support/index.js"></script>
<!-- bundled first spec file, second spec file, etc -->
<script src="integration/spec-a.js"></script>
<script src="integration/spec-b.js"></script>
...
<script src="integration/spec-n.js"></script>
```

>Ter um único arquivo de suporte ao executar todas as especificações juntas pode executar `before` e `beforeEach` travar
>de maneiras que você pode não antecipar. Leia '[Cuidado ao executar todas as especificações juntas](https://glebbahmutov.com/blog/run-all-specs/)'
para exemplos.

## Solução de problemas

Se o Cypress não encontrar os arquivos de especificação por algum motivo, você pode solucionar o problema de sua lógica abrindo
 ou executando o Cypress com os [logs de depuração](https://docs.cypress.io/guides/references/troubleshooting.html#Print-DEBUG-logs)
habilitados:

```js
$ DEBUG=cypress:server:specs npx cypress open
# or
DEBUG=cypress:server:specs npx cypress run
```

## Escrevendo testes

Cypress é construído em cima de [Mocha](https://docs.cypress.io/guides/references/bundled-tools.html#Mocha) e [Chai](https://docs.cypress.io/guides/references/bundled-tools.html#Chai).
Apoiamos os estilos de Chai `BDD` e de `TDD` asserção. Os testes que você escreve no Cypress irão, em sua maioria, seguir
esse estilo.

Se você está familiarizado com a escrita de testes em JavaScript, escrever testes em Cypress será muito fácil.

## Estrutura de Teste

A interface de teste, emprestado do [mocha](https://docs.cypress.io/guides/references/bundled-tools.html#Mocha), fornece
`describe()`, `context()`, `it()` e `specify()`.

`context()` é idêntico `describe()` e `specify()` é idêntico a `it()`, portanto, escolha a terminologia que funciona
melhor para você.

```js
// -- Start: Our Application Code --
function add (a, b) {
  return a + b
}

function subtract (a, b) {
  return a - b
}

function divide (a, b) {
  return a / b
}

function multiply (a, b) {
  return a * b
}
// -- End: Our Application Code --

// -- Start: Our Cypress Tests --
describe('Unit test our math functions', () => {
  context('math', () => {
    it('can add numbers', () => {
      expect(add(1, 2)).to.eq(3)
    })

    it('can subtract numbers', () => {
      expect(subtract(5, 12)).to.eq(-7)
    })

    specify('can divide numbers', () => {
      expect(divide(27, 9)).to.eq(3)
    })

    specify('can multiply numbers', () => {
      expect(multiply(5, 4)).to.eq(20)
    })
  })
})
// -- End: Our Cypress Tests --
```

## Ganchos

Cypress também fornece ganchos (emprestados do [Mocha](https://docs.cypress.io/guides/references/bundled-tools.html#Mocha)).

Eles são úteis para definir as condições que você deseja executar antes de um conjunto de testes ou antes de cada teste.
Eles também são úteis para limpar as condições após um conjunto de testes ou após cada teste.

```js
beforeEach(() => {
  // root-level hook
  // runs before every test
})

describe('Hooks', () => {
  before(() => {
    // runs once before all tests in the block
  })

  beforeEach(() => {
    // runs before each test in the block
  })

  afterEach(() => {
    // runs after each test in the block
  })

  after(() => {
    // runs once after all tests in the block
  })
})
```

__A ordem de execução do gancho e do teste é a seguinte:__

- Todos os `before()` ganchos funcionam (uma vez)

- Qualquer `beforeEach()` gancho corre

- Testes executados

- Qualquer `afterEach()` gancho corre

- Todos os `after()` ganchos funcionam (uma vez)

>Antes de escrever `after()` ou fazer `afterEach()` ganchos, consulte nossos [pensamentos sobre o anti-padrão de limpeza
>do estado com `after()` ou `afterEach()`](https://docs.cypress.io/guides/references/best-practices.html#Using-after-or-afterEach-hooks).

>Desconfie de ganchos no nível de raiz, pois eles podem ser executados em uma ordem surpreendente ao clicar no botão “Executar
>todas as especificações”. Em vez disso, coloque-os dentro `describe` ou em `context` suítes para isolamento. Leia ['Tenha
>cuidado ao executar todas as especificações juntas'](https://glebbahmutov.com/blog/run-all-specs/) .

## Excluindo e Incluindo Testes

Para executar um conjunto ou teste especificado, anexe `.only` à função. Todas as suítes aninhadas também serão executadas.
Isso nos dá a capacidade de executar um teste por vez e é a maneira recomendada de escrever um conjunto de testes.

```js
// -- Start: Our Application Code --
function fizzbuzz (num) {
  if (num % 3 === 0 && num % 5 === 0) {
    return 'fizzbuzz'
  }

  if (num % 3 === 0) {
    return 'fizz'
  }

  if (num % 5 === 0) {
    return 'buzz'
  }
}
// -- End: Our Application Code --

// -- Start: Our Cypress Tests --
describe('Unit Test FizzBuzz', () => {
  function numsExpectedToEq (arr, expected) {
    // loop through the array of nums and make
    // sure they equal what is expected
    arr.forEach((num) => {
      expect(fizzbuzz(num)).to.eq(expected)
    })
  }

  it.only('returns "fizz" when number is multiple of 3', () => {
    numsExpectedToEq([9, 12, 18], 'fizz')
  })

  it('returns "buzz" when number is multiple of 5', () => {
    numsExpectedToEq([10, 20, 25], 'buzz')
  })

  it('returns "fizzbuzz" when number is multiple of both 3 and 5', () => {
    numsExpectedToEq([15, 30, 60], 'fizzbuzz')
  })
})
```

Para pular um conjunto ou teste especificado, anexe `.skip()` à função. Todas as suítes aninhadas também serão ignoradas.

```js
it.skip('returns "fizz" when number is multiple of 3', () => {
  numsExpectedToEq([9, 12, 18], 'fizz')
})
```

## Configuração de Teste

Para aplicar um valor de [configuração](https://docs.cypress.io/guides/references/configuration.html) Cypress específico
a um conjunto ou teste, passe um objeto de configuração para a função de teste ou suíte como o segundo argumento.

Essa configuração entrará em vigor durante o conjunto ou testes em que foram definidos e, em seguida, retornará aos valores
padrão anteriores depois que o conjunto ou os testes forem concluídos.

### Sintaxe

```js
describe(name, config, fn)
context(name, config, fn)
it(name, config, fn)
specify(name, config, fn)
```

### Valores de configuração permitidos

Observação: alguns valores de configuração são somente leitura e não podem ser alterados por meio da configuração de teste.
Os seguintes valores de configuração podem ser alterados por meio de configuração de teste:

- `animationDistanceThreshold`
- `baseUrl`
- `browser` nota: filtra se os testes ou um conjunto de testes são executados, dependendo do navegador atual
- `defaultCommandTimeout`
- `execTimeout`
- `env` nota: as variáveis ​​de ambiente fornecidas serão mescladas com as variáveis ​​de ambiente atuais.
- `includeShadowDom`
- `requestTimeout`
- `responseTimeout`
- `retries`
- `scrollBehavior`
- `viewportHeight`
- `viewportWidth`
- `waitForAnimations`

### Configuração de suíte

Se você deseja `browser` definir como destino um conjunto de testes a ser executado ou ser excluído ao ser executado em um
navegador específico, pode substituir a configuração dentro da configuração do conjunto. A `browser` opção aceita os mesmos
argumentos de [`Cypress.isBrowser()`](https://docs.cypress.io/api/cypress-api/isbrowser.html).

O seguinte conjunto de testes será ignorado se os testes forem executados em navegadores Chrome.

```js
describe('When NOT in Chrome', { browser: '!chrome' }, () => {
  it('Shows warning', () => {
    cy.get('.browser-warning')
      .should('contain', 'For optimal viewing, use Chrome browser')
  })

  it('Links to browser compatibility doc', () => {
    cy.get('a.browser-compat')
      .should('have.attr', 'href')
      .and('include', 'browser-compatibility')
  })
})
```

O seguinte conjunto de testes só será executado no navegador Firefox. Ele sobrescreverá a resolução da janela de visualização
em um dos testes e mesclará todas as variáveis ​​de ambiente atuais com as fornecidas.

```js
describe('When in Firefox', {
  browser: 'firefox',
  viewportWidth: 1024,
  viewportHeight: 700,
  env: {
    DEMO: true,
    API: 'http://localhost:9000'
  }
}, () => {
  it('Sets the expected viewport and API url', () => {
    expect(cy.config('viewportWidth')).to.equal(1024)
    expect(cy.config('viewportHeight')).to.equal(700)
    expect(cy.env('API')).to.equal('http://localhost:9000')
  })

  it('Uses the closest API environment variable', {
    env: {
      API: 'http://localhost:3003'
    }
  }, () => {
    expect(cy.env('API')).to.equal('http://localhost:3003')
    // other environment variables remain unchanged
    expect(cy.env('DEMO')).to.be.true
  })
})
```

### Configuração de teste único

Você pode configurar o número de tentativas de repetição durante `cypress run` ou `cypress open`. Consulte [Testar](https://docs.cypress.io/guides/guides/test-retries.html)
novas [tentativas](https://docs.cypress.io/guides/guides/test-retries.html) para obter mais informações.

```js
it('should redirect unauthenticated user to sign-in page', {
    retries: {
      runMode: 3,
      openMode: 2
    }
  } () => {
    cy.visit('/')
    // ...
  })
})
```

## Gerar testes dinamicamente

Você pode gerar testes dinamicamente usando JavaScript.

```js
describe('if your app uses jQuery', () => {
  ['mouseover', 'mouseout', 'mouseenter', 'mouseleave'].forEach((event) => {
    it('triggers event: ' + event, () => {
      // if your app uses jQuery, then we can trigger a jQuery
      // event that causes the event callback to fire
      cy
        .get('#with-jquery').invoke('trigger', event)
        .get('#messages').should('contain', 'the event ' + event + 'was fired')
    })
  })
})
```

O código acima produzirá um pacote com 4 testes:

```markdown
> if your app uses jQuery
  > triggers event: 'mouseover'
  > triggers event: 'mouseout'
  > triggers event: 'mouseenter'
  > triggers event: 'mouseleave'
  ```

## Estilos de Asserção

Cypress suporta asserções simples de estilo BDD (`expect`/ `should`) e TDD (`assert`). [Leia mais sobre afirmações simples](https://docs.cypress.io/guides/references/assertions.html).

```js
it('can add numbers', () => {
  expect(add(1, 2)).to.eq(3)
})

it('can subtract numbers', () => {
  assert.equal(subtract(5, 12), -7, 'these numbers are equal')
})
```

O [`.should()`](https://docs.cypress.io/api/commands/should.html) comando e seu alias
[`.and()`](https://docs.cypress.io/api/commands/and.html) também podem ser usados ​​para encadear mais facilmente as afirmações
dos comandos do Cypress. [Leia mais sobre afirmações](https://docs.cypress.io/guides/core-concepts/introduction-to-cypress.html#Assertions).

```js
cy.wrap(add(1, 2)).should('equal', 3)
```

## Executando testes

### Execute um único arquivo de especificação

Sugerimos executar arquivos de teste individualmente clicando no nome do arquivo de especificação para garantir o melhor
desempenho. Por exemplo, o [aplicativo Cypress RealWorld](https://github.com/cypress-io/cypress-example-conduit-app) tem
vários arquivos de teste, mas abaixo executamos um único arquivo de teste “new-transaction.spec.ts”.

![Executando uma única especificação](https://docs.cypress.io/img/guides/core-concepts/run-single-spec.175d5077.gif)

## Execute todas as especificações

Você pode executar todos os arquivos de especificações juntos clicando no botão “Executar todas as especificações”. Este
modo é equivalente a concatenar todos os arquivos de especificações juntos em uma única parte do código de teste.

![Executando todas as especificações](https://docs.cypress.io/img/guides/core-concepts/run-all-specs.f59fcccb.gif)

>Desconfie de ganchos no nível de raiz, pois eles podem ser executados em uma ordem surpreendente ao clicar no botão
>“Executar todas as especificações”. Em vez disso, coloque-os dentro `describe` ou em `context` suítes para isolamento.
>Leia ['Tenha cuidado ao executar todas as especificações juntas'](https://glebbahmutov.com/blog/run-all-specs/) .

## Executar especificações filtradas

Você também pode executar um subconjunto de todas as especificações inserindo um filtro de pesquisa de texto. Apenas as
especificações com caminhos de arquivo relativos contendo o filtro de pesquisa permanecerão e serão executadas como se
estivessem concatenando todos os arquivos de especificações ao clicar no botão “Executar N especificações”.

- O filtro de pesquisa não diferencia maiúsculas de minúsculas; o filtro “ui” corresponderá aos arquivos “UI-spec.js” e “admin-ui-spec.js”.

- O filtro de pesquisa é aplicado a todo o caminho do arquivo de especificação relativo, portanto, você pode usar nomes de
pasta para limitar as especificações; o filtro “ui” irá corresponder aos arquivos “admin-ui.spec.js” e “ui / admin.spec.js”.

![Execução de especificações que correspondem ao filtro de pesquisa](https://docs.cypress.io/img/guides/core-concepts/run-selected-specs.6bc306d6.gif)


## Assistindo testes

Ao executar em using [`cypress open`](https://docs.cypress.io/guides/guides/command-line.html#cypress-open), Cypress observa
o sistema de arquivos para mudanças em seus arquivos de especificação. Logo após adicionar ou atualizar um teste, o Cypress
o recarregará e executará todos os testes naquele arquivo de especificação.

Isso torna a experiência de desenvolvimento produtiva porque você pode adicionar e editar testes enquanto implementa um
recurso e a interface de usuário do Cypress sempre refletirá os resultados de suas edições mais recentes.

>Lembre-se de usar [`.only`](https://docs.cypress.io/guides/core-concepts/writing-and-organizing-tests.html#Excluding-and-Including-Tests)
>para limitar quais testes são executados: isso pode ser especialmente útil quando você tem muitos testes em um único arquivo
>de especificação que está constantemente editando; considere também dividir seus testes em arquivos menores, cada um lidando
>com comportamento logicamente relacionado.

## O que é assistido?

### arquivos

- [Arquivo de configuração (`cypress.json`) por padrão](https://docs.cypress.io/guides/references/configuration.html#Options)
- [`cypress.env.json`](https://docs.cypress.io/guides/guides/environment-variables.html)

## Pastas

- Diretório de integração (`cypress/integration/` por padrão)
- Diretório de suporte (`cypress/support/` por padrão)
- Diretório de plugins (`cypress/plugins/` por padrão)

A pasta, os arquivos dentro da pasta e todas as pastas filhas e seus arquivos (recursivamente) são monitorados.

>Esses caminhos de pasta referem-se aos [caminhos de pasta padrão](https://docs.cypress.io/guides/references/configuration.html#Folders-Files).
>Se você configurou o Cypress para usar caminhos de pasta diferentes, então as pastas específicas para sua configuração
>serão monitoradas.

## O que não é assistido?

Todo o resto; isso inclui, mas não se limita a:

- Seu código de aplicativo
- `node_modules`
- `cypress/fixtures/`

Se você está desenvolvendo usando uma pilha de aplicativos da web moderna baseada em JS, então provavelmente você tem suporte
para alguma forma de substituição de módulo quente que é responsável por observar o código do seu aplicativo - HTML, CSS,
JS, etc. - e recarregar de forma transparente o seu aplicação em resposta às mudanças.

## Configuração

Defina a [`watchForFileChanges`](https://docs.cypress.io/guides/references/configuration.html#Global) propriedade de configuração
`false` para desativar a observação de arquivos.

>Nada é observado durante [`cypress run`](https://docs.cypress.io/guides/guides/command-line.html#cypress-run).
>
>A `watchForFileChanges` propriedade só tem efeito ao executar o Cypress usando [`cypress open`](https://docs.cypress.io/guides/guides/command-line.html#cypress-open).

O componente responsável pelo comportamento de observação de arquivos no Cypress é o [`webpack-preprocessor`](https://github.com/cypress-io/cypress/tree/master/npm/webpack-preprocessor).
Este é o observador de arquivos padrão fornecido com o Cypress.

Se você precisar de mais controle do comportamento de observação de arquivos, pode configurar este pré-processador explicitamente:
ele expõe opções que permitem configurar o comportamento, como o que é assistido e o atraso antes de emitir um evento de
“atualização” após uma alteração.

O Cypress também fornece outros [pré-processadores de monitoramento de arquivos](https://docs.cypress.io/plugins/index.html);
você terá que configurá-los explicitamente se quiser usá-los.

- [Pré-processador Cypress Watch](https://github.com/cypress-io/cypress-watch-preprocessor)
- [Pré-processador Cypress webpack](https://github.com/cypress-io/cypress/tree/master/npm/webpack-preprocessor)
