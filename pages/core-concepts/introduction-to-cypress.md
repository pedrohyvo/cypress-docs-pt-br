# Introdução ao Cypress

```markdown
O que você vai aprender

- Como o Cypress faz consultas ao DOM
- Como o Cypress gerencia sujeitos e cadeias de comandos
- O que são e como funcionam as asserções
- Como timeouts são aplicados aos comandos
```

[//]: <> (TODO - Adicionar link - tutoriais em vídeo)

> **Importante**
>
> Este é o guia mais importante para entender como testar com o Cypress.
> É importante que você leia e entenda este guia. Faça perguntas sobre
> ele para que possamos melhorá-lo.
>
> Quando você terminar, recomendamos assistir aos
> [Tutoriais em Vídeo](https://docs.cypress.io/examples/examples/tutorials.html).

## O Cypress pode ser simples (às vezes)

Simplicidade tem a ver com fazer mais digitando menos. Vejamos um exemplo:

```JS
describe('Recurso do Artigo', () => {
  it('Criar um novo artigo', () => {
    cy.visit('/posts/new')           // 1.

    cy.get('input.post-title')       // 2.
      .type('Meu Primeiro Artigo')   // 3.

    cy.get('input.post-body')        // 4.
      .type('Olá, mundo!')           // 5.

    cy.contains('Enviar')            // 6.
      .click()                       // 7.

    cy.url()                         // 8.
      .should('include', '/posts/my-first-post')

    cy.get('h1')                     // 9.
      .should('contain', 'Meu Primeiro Artigo')
  })
})
```

Você consegue ler isso? Se conseguiu, é mais ou menos assim:

1. Visite a página em `/posts/new`.
2. Encontre o `<input>` com a classe `post-title`.
3. Digite "Meu Primeiro Artigo" nele.
4. Encontre o `<input>` com a classe `post-body`.
5. Digite "Olá, mundo!" nele.
6. Encontre o elemento que contém o texto "Enviar".
7. Clique nele.
8. Pegue a URL do navegador e verifique se ela inclui `/posts/my-first-post`.
9. Encontre a tag `h1` e verifique se ela contém o texto "Meu Primeiro Artigo".

Esse é um teste relativamente simples, mas observe quanto
código ele abrange, tanto no cliente quanto no servidor!

No restante deste guia, vamos explorar os conceitos básicos do Cypress
que fazem este exemplo funcionar. Vamos desmistificar as regras que o
Cypress segue para que você possa testar sua aplicação com eficiência,
de forma que ela reflita ao máximo as ações do usuário. Além disso,
explicaremos como usar alguns atalhos, quando apropriado.

## Consultando elementos

### O Cypress é como o jQuery

Se você já usou o [jQuery](https://jquery.com/), provavelmente está acostumado a consultar elementos assim:

```JS
$('.my-selector')
```

Na Cypress, consultamos elementos da mesma forma:

```JS
cy.get('.my-selector')
```

[//]: <> (TODO - Adicionar link - incorpora o jQuery)

Na verdade, o Cypress [incorpora o jQuery](https://docs.cypress.io/guides/references/bundled-tools.html#Other-Library-Utilities)
e expõe muitos de seus métodos de travessia do DOM para que você possa trabalhar com estruturas HTML complexas
mais facilmente usando APIs que já conhece.

```JS
// Cada método tem seu equivalente no jQuery. Reaproveite o que você já sabe!
cy.get('#main-content')
  .find('.article')
  .children('img[src^="/static"]')
  .first()
```

[//]: <> (TODO - Adicionar link - integração github)

> **Conceito importante**
>
> O Cypress aproveita o sofisticado mecanismo de seletores do jQuery para tornar
> os testes mais simples e legíveis para desenvolvedores Web modernos.
>
> Gostaria de conhecer melhores práticas sobre como selecionar elementos?
> [Leia esta página](https://docs.cypress.io/guides/references/best-practices.html#Selecting-Elements).

No entanto, há uma diferença na forma de acessar os elementos do DOM retornados pela consulta:

```JS
// O código a seguir funciona, pois o jQuery retorna o elemento de forma síncrona.
const $jqElement = $('.element')

// O código a seguir não vai funcionar! O Cypress não retorna o elemento de forma síncrona.
const $cyElement = cy.get('.element')
```

Vamos entender por quê...

### O Cypress _não_ é como o jQuery

Pergunta: O que acontece quando o jQuery não consegue encontrar
nenhum elemento do DOM correspondente ao seletor?

Resposta: _Ops!_ Ele retorna uma coleção do jQuery vazia. Há um objeto real
com o qual podemos trabalhar, porém ele não contém o elemento que queríamos.
Por isso, começamos a adicionar verificações condicionais e a repetir nossas
consultas manualmente.

```JS
// $() retorna imediatamente com uma coleção vazia.
const $myElement = $('.element').first()

// Isso gera verificações condicionais deselegantes
// e, ainda pior: testes cheios de falhas!
if ($myElement.length) {
  doSomething($myElement)
}
```

Pergunta: O que acontece quando o Cypress não consegue encontrar nenhum elemento do
DOM correspondente ao seletor?

Resposta: _Não tem problema!_ O Cypress repete automaticamente a consulta até que:

#### 1. O elemento seja encontrado

```JS
cy
  // cy.get() procura '#element', repetindo a consulta até...
  .get('#element')

  // ...encontrar o elemento!
  // Agora é possível trabalhar com ele usando .then
  .then(($myElement) => {
    doSomething($myElement)
  })
```

#### 2. Um timeout definido seja atingido

```JS
cy
// cy.get() procura '#element-does-not-exist', repetindo a consulta até...
// ...o timeout ser atingido sem o elemento ter sido encontrado.
// O Cypress para e o teste falha.
.get('#element-does-not-exist')

// ...O código abaixo nunca será executado...
.then(($myElement) => {
    doSomething($myElement)
})
```

Isso torna o Cypress robusto e imune a dezenas de problemas
comuns que ocorrem em outras ferramentas de teste. Considere
todas as situações que poderiam fazer uma consulta do DOM falhar:

- O DOM ainda não foi carregado.
- O framework que você está usando não terminou de ser inicializado.
- Uma requisição XHR não recebeu a resposta.
- Uma animação ainda não terminou.
- E por aí vai...

[//]: <> (TODO - Adicionar link - integração github)

Antes, você seria obrigado a escrever código personalizado para lidar com cada um desses problemas:
uma combinação desastrosa de esperas aleatórias, retentativas condicionais e verificações de nulo
que sobrecarregam seus testes. Mas não no Cypress! Com retentativas automáticas e
[timeouts personalizáveis](https://docs.cypress.io/guides/references/configuration.html#Timeouts),
o Cypress acaba com todos esses problemas.

> **Conceito importante**
>
> O Cypress encapsula todas as consultas ao DOM com uma lógica robusta de "retentativa e timeout" que reflete melhor
> o funcionamento de aplicações Web reais. Com essa pequena adaptação na forma como localizamos elementos do DOM,
> temos uma grande melhoria na estabilidade de todos os nossos testes. Os testes com falhas estão com os dias contados!

[//]: <> (TODO - Adicionar links - integração github)

> No Cypress, quando você quiser interagir com um elemento do DOM diretamente, chame
> [`.then()`](https://docs.cypress.io/api/commands/then.html) com uma função
> callback que recebe o elemento como primeiro argumento. Quando quiser pular
> completamente a funcionalidade de retentativa e timeout e trabalhar de forma
> síncrona tradicional, use [`Cypress.$`](https://docs.cypress.io/api/utilities/$.html).

### Consultando pelo conteúdo do texto

[//]: <> (TODO - Adicionar link - integração github)

Outra maneira de encontrar algo (uma maneira mais humana) é procurar pelo conteúdo,
ou seja, pelo que o usuário vê na página. Para isso, existe o prático comando
[`cy.contains()`](https://docs.cypress.io/api/commands/contains.html). Por exemplo:

```JS
// Encontrar um elemento no documento que contém o texto "Novo Artigo".
cy.contains('Novo Artigo')

// Encontrar um elemento dentro de '.main' que contêm o texto "Novo Artigo".
cy.get('.main').contains('Novo Artigo')
```

Isso é útil ao escrever testes do ponto de vista do usuário que está interagindo com sua
aplicação. Ele só sabe que quer clicar no botão "Enviar" e não faz ideia de que esse
botão tem um atributo `type` igual a `submit` ou a classe CSS `my-submit-button`.

> **Internacionalização**
>
> Se a sua aplicação for traduzido para vários idiomas para i18n, considere as
> implicações de encontrar elementos do DOM usando o texto que o usuário vê!

### Quando elementos estão faltando

Como mostramos acima, o Cypress leva em consideração a natureza assíncrona das aplicações Web e não
emite uma falha imediatamente logo na primeira vez que um elemento não é encontrado. Em vez disso, o Cypress
espera um tempo para que a sua aplicação possa terminar seja lá o que ela esteja fazendo!

[//]: <> (TODO - Adicionar link - integração github)

Isso é conhecido como um `timeout`, e é possível personalizar a maioria dos comandos com tempos de timeout
específicos ([o timeout padrão é de 4 segundos](https://docs.cypress.io/guides/references/configuration.html#Timeouts)).
Esses comandos indicam a opção `timeout` em sua documentação da API, detalhando como definir o número de milissegundos
durante os quais você deseja continuar tentando encontrar o elemento.

```JS
// Espere 10 segundos até este elemento aparecer
cy.get('.my-slow-selector', { timeout: 10000 })
```

[//]: <> (TODO - Adicionar link - integração github)

Você também pode definir o timeout globalmente através da
[configuração: `defaultCommandTimeout`](https://docs.cypress.io/guides/references/configuration.html#Timeouts).

> **Conceito importante**
>
> Para refletir o comportamento das aplicações Web, o Cypress é assíncrono e usa timeouts
> para saber quando deve parar de esperar que uma aplicação entre no estado esperado.
> Os timeouts podem ser configurados globalmente ou por comando.

> **Timeouts e desempenho**
>
> Neste caso, há uma consideração de desempenho importante: testes com tempos de timeout mais longos levam mais tempo
> para falhar. Os comandos sempre continuam assim que as condições esperadas são atendidas, portanto, os testes funcionais
> serão executados tão rápido quanto sua aplicação permitir. Por padrão, um teste que falha devido a um timeout
> consumirá todo o tempo de timeout. Isso significa que, embora você _possa_ aumentar o tempo de timeout para
> partes específicas da sua aplicação, _não deve_ usar um timeout extra longo "apenas por precaução".

[//]: <> (TODO - Adicionar links - integração github)

Mais adiante neste guia entraremos em muito mais detalhes sobre
[Asserções Padrão](https://docs.cypress.io/guides/core-concepts/introduction-to-cypress.html#Default-Assertions) e
[Timeouts](https://docs.cypress.io/guides/core-concepts/introduction-to-cypress.html#Timeouts).

## Cadeias de Comandos

É muito importante entender o mecanismo que o Cypress usa para encadear comandos.
Ele gerencia uma cadeia de Promises para você, e cada comando gera um "sujeito"
para o próximo comando, até que a cadeia termine ou seja encontrado um erro.
O desenvolvedor geralmente não precisa usar Promises diretamente, mas é útil entender como elas funcionam!

### Interagindo com elementos

[//]: <> (TODO - Adicionar links - integração github)

Como vimos no exemplo inicial, o Cypress permite clicar e digitar em elementos da página usando os comandos
[`.click()`](https://docs.cypress.io/api/commands/click.html) e
[`.type()`](https://docs.cypress.io/api/commands/type.html) com um comando
[`cy.get()`](https://docs.cypress.io/api/commands/get.html) ou
[`cy.contains()`](https://docs.cypress.io/api/commands/contains.html).
Este é um ótimo exemplo de encadeamento em ação. Vamos vê-lo novamente:

```JS
cy.get("textarea.post-body")
  .type("Este é um ótimo artigo");
```

[//]: <> (TODO - Adicionar links - integração github)

Estamos encadeando [`.type()`](https://docs.cypress.io/api/commands/type.html) no comando
[`cy.get()`](https://docs.cypress.io/api/commands/get.html),
solicitando que ele digite no sujeito gerado pelo comando
[`cy.get()`](https://docs.cypress.io/api/commands/get.html), que será um elemento do DOM.

Veja a seguir outros comandos de ação que o Cypress oferece para interagir com seu aplicativo:

[//]: <> (TODO - Adicionar links - integração github)

- [`.blur()`](https://docs.cypress.io/api/commands/blur.html) - Tira o foco de um elemento do DOM focalizado.
- [`.focus()`](https://docs.cypress.io/api/commands/focus.html) - Focaliza um elemento do DOM.
- [`.clear()`](https://docs.cypress.io/api/commands/clear.html) - Limpa o valor de um input ou textarea.
- [`.check()`](https://docs.cypress.io/api/commands/check.html) - Marca caixas de seleção ou botões de opção.
- [`.uncheck()`](https://docs.cypress.io/api/commands/uncheck.html) - Desmarcas caixas de seleção.
- [`.select()`](https://docs.cypress.io/api/commands/select.html) - Seleciona um `<option>`
  dentro de um `<selection>`.
- [`.dblclick()`](https://docs.cypress.io/api/commands/dblclick.html) - Clica duas vezes em um elemento do DOM.
- [`.rightclick()`](https://docs.cypress.io/api/commands/rightclick.html) - Clica com o botão direito do mouse
  em um elemento do DOM.

[//]: <> (TODO - Adicionar link - integração github)

Esses comandos oferecem [algumas garantias](https://docs.cypress.io/guides/core-concepts/interacting-with-elements.html)
em relação a qual deve ser o estado dos elementos antes de eles executarem suas ações.

[//]: <> (TODO - Adicionar link - integração github)

Por exemplo, quando você escreve um comando [`.click()`](https://docs.cypress.io/api/commands/click.html),
o Cypress garante que é possível interagir com o elemento (da mesma forma que um usuário real faria).
Ele irá esperar automaticamente até que o elemento tenha um estado "acionável" confirmando que o elemento:

- Não está oculto
- Não está coberto
- Não está desativado
- Não está sendo animado

Isso também ajuda a evitar erros ao interagir com sua aplicação nos testes.
Normalmente você pode substituir esse comportamento com a opção `force`.

[//]: <> (TODO - Adicionar link - integração github)

> **Conceito importante**
>
> O Cypress oferece um algoritmo simples mas poderoso para [interagir com elementos](https://docs.cypress.io/guides/core-concepts/interacting-with-elements.html).

### Asserções sobre elementos

As asserções permitem fazer coisas como confirmar que um elemento está visível
ou tem determinado atributo, classe CSS ou estado.
As asserções são comandos que permitem descrever o estado _desejado_ da sua aplicação.
O Cypress irá esperar automaticamente até que seus elementos tenham esse estado ou reprovará
o teste se as asserções não forem aprovadas.
Veja a seguir uma breve demonstração das asserções em ação:

```JS
cy.get(':checkbox').should('be.disabled')

cy.get('form').should('have.class', 'form-horizontal')

cy.get('input').should('not.have.value', 'US')
```

Em cada um dos exemplos, é importante observar que o Cypress _espera_ automaticamente
até que essas asserções sejam aprovadas.
Isso evita a necessidade de saber ou definir o momento exato em que seus elementos terão esse estado.

[//]: <> (TODO - Adicionar link - integração github)

Aprenderemos mais sobre as
[asserções](https://docs.cypress.io/guides/core-concepts/introduction-to-cypress.html#Assertions) posteriormente neste guia.

### Gerenciamento de sujeitos

Uma nova cadeia do Cypress sempre começa com `cy.[comando]`, e o resultado gerado pelo `comando`
determina quais outros comandos podem ser chamados em seguida (encadeados).

[//]: <> (TODO - Adicionar link - integração github)

Alguns métodos geram `null` e, portanto, não podem ser encadeados, tais como [`cy.clearCookies()`](https://docs.cypress.io/api/commands/clearcookies.html).

[//]: <> (TODO - Adicionar links - integração github)

Alguns métodos, tais como [`cy.get()`](https://docs.cypress.io/api/commands/get.html) ou
[`cy.contains()`](https://docs.cypress.io/api/commands/contains.html), geram um elemento do DOM, permitindo que outros
comandos sejam encadeados neles (pressupondo que eles esperam um sujeito do DOM), tais como [`.click()`](https://docs.cypress.io/api/commands/click.html)
ou até mesmo [`cy.contains()`](https://docs.cypress.io/api/commands/contains.html) novamente.

#### Alguns comandos podem ser encadeados

[//]: <> (TODO - Adicionar link - integração github)

- apenas em `cy`, ou seja, eles não operam sobre um sujeito: [`cy.clearCookies()`](https://docs.cypress.io/api/commands/clearcookies.html).

[//]: <> (TODO - Adicionar link - integração github)

- em comandos que geram determinados tipos de sujeitos (como elementos do DOM): [`.type()`](https://docs.cypress.io/api/commands/type.html).

[//]: <> (TODO - Adicionar link - integração github)

- em `cy` _e também_ em um comando que gera um sujeito: [`cy.contains()`](https://docs.cypress.io/api/commands/contains.html).

#### Alguns comandos geram

[//]: <> (TODO - Adicionar link - integração github)

- `null`, ou seja, nenhum comando pode ser encadeado em seguida: [`cy.clearCookies()`](https://docs.cypress.io/api/commands/clearcookies.html).

[//]: <> (TODO - Adicionar link - integração github)

- o mesmo sujeito que foi gerado originalmente: [`.click()`](https://docs.cypress.io/api/commands/click.html).

[//]: <> (TODO - Adicionar link - integração github)

- um novo sujeito, conforme apropriado para o comando [`.wait()`](https://docs.cypress.io/api/commands/wait.html).

Esse processo na verdade é muito mais intuitivo do que parece.

#### Exemplos

```JS
cy.clearCookies() // Terminou: 'null' foi gerado, não há possibilidade de encadeamento

cy.get('.container-principal') // Gera um array de elementos do DOM correspondentes
  .contains('Headlines')       // Gera o primeiro elemento do DOM que inclui o conteúdo em questão
  .click()                     // Gera o mesmo elemento do DOM do comando anterior
```

> **Conceito importante**
>
> Os comandos do Cypress não retornam sujeitos, apenas os geram. Lembre-se: os comandos do Cypress são assíncronos e são
> enfileirados para execução posterior. Durante a execução, os sujeitos são gerados de um comando para o seguinte, e muito
> código útil do Cypress é executado entre cada comando para garantir que tudo funcione corretamente.

[//]: <> (TODO - Adicionar link - integração github)

> Para contornar a necessidade de referenciar elementos, o Cypress tem um recurso
> [conhecido como aliasing](https://docs.cypress.io/guides/core-concepts/variables-and-aliases.html). O aliasing ajuda
> a armazenar e salvar referências de elementos para uso futuro.

[//]: <> (TODO - Adicionar link - integração github)

#### Usando [`.then()`](https://docs.cypress.io/api/commands/then.html) para manipular um sujeito

Quer assumir o controle do fluxo de comandos e manipular diretamente o sujeito?
Sem problema, basta adicionar `.then()` à cadeia de comandos.
Quando o comando anterior for resolvido, ele chamará sua função callback usando o sujeito gerado como o primeiro argumento.

[//]: <> (TODO - Adicionar link - integração github)

Se você quiser continuar encadeando comandos após seu [`.then()`](https://docs.cypress.io/api/commands/then.html),
precisará especificar o sujeito que deseja gerar para esses comandos,
o que pode ser feito com um valor de retorno diferente de `null` ou `undefined`.
O Cypress irá gerá-lo para o próximo comando para você.

#### Vejamos um exemplo

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

[//]: <> (TODO - Adicionar link - integração github)

> **Conceito importante**
>
> Temos muito mais exemplos e casos de uso de `cy.then()` em nosso
> [Guia de Conceitos Básicos](https://docs.cypress.io/guides/core-concepts/variables-and-aliases.html),
> que ensina como lidar corretamente com código assíncrono, quando usar variáveis e o que é aliasing.

#### Usando aliases para fazer referências a sujeitos anteriores

[//]: <> (TODO - Adicionar link - integração github)

O Cypress tem recursos adicionais para rapidamente fazer referência a sujeitos anteriores chamados
[aliases](https://docs.cypress.io/guides/core-concepts/variables-and-aliases.html). É mais ou menos assim:

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

Isso nos permite reutilizar nossas consultas ao DOM para tornar os testes mais rápidos quando o elemento ainda está no DOM,
processando automaticamente novas consultas ao DOM quando o elemento não é imediatamente encontrado no DOM.
Isso é particularmente útil quando estamos trabalhando com frameworks de front-end que fazem muitas renderizações consecutivas!

### Os comandos são assíncronos

É muito importante entender que os comandos do Cypress não fazem nada no momento em que são chamados,
mas são enfileirados para serem executados mais tarde.
Isso é o que queremos dizer quando falamos que os comandos do Cypress são assíncronos.

#### Veja este pequeno teste, por exemplo

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

#### Misturando código assíncrono e síncrono

É importante lembrar que os comandos do Cypress são executados de forma
assíncrona se você for tentar misturar os comandos do Cypress com código síncrono.
O código síncrono é executado imediatamente e não espera que os comandos do Cypress anteriores sejam executados.

⚠️ Uso incorreto

No exemplo abaixo, `el` é avaliado imediatamente, antes que `cy.visit()` tenha sido executado,
portanto, sempre será avaliado como um array vazio.

```JS
it('não funciona como esperamos', () => {
  cy.visit('/my/resource/path') // Nada acontece ainda

  cy.get('.awesome-selector')   // Nada acontecendo ainda
    .click()                    // Não, nada

  // Cypress.$ é síncrono, portanto, é avaliado imediatamente
  // Ainda não há nenhum elemento a ser encontrado porque
  // cy.visit() foi apenas enfileirado para visit
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

No exemplo abaixo, a verificação do valor de `username` é avaliada imediatamente, antes que `cy.visit()` seja executado,
portanto sempre será avaliada como `undefined`.

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
> Cada comando (e cadeia de comandos) do Cypress retorna imediatamente,
> e foi simplesmente adicionado a uma fila de comandos a serem executados posteriormente.
>
> Não é possível fazer nada de útil diretamente com o valor de retorno de um comando.
> Os comandos são enfileirados e gerenciados totalmente por baixo dos panos.
>
> Projetamos nossa API assim porque o DOM é um objeto altamente mutável que fica desatualizado constantemente.
> Para que o Cypress evite falhas e saiba quando prosseguir, gerenciamos os comandos de uma forma altamente controlada
> e determinística.

> **Por que não posso usar async/await?**
>
> Se você é um programador JS moderno, talvez ouça a palavra "assíncrono" e pense:
> por que não posso simplesmente usar `async/wait` em vez de aprender uma API especializada?
>
> As APIs do Cypress são desenvolvidas de forma muito diferente das práticas com as quais você provavelmente está acostumado:
> porém, esses padrões de projeto são totalmente intencionais. Entraremos em mais detalhes mais adiante neste guia.

### Os comandos são executados em série

Depois que uma função de teste termina de ser executada, o Cypress executa os comandos
que foram enfileirados usando as cadeias de comandos `cy.*`.

#### Vamos analisar outro exemplo

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
5. Afirmar que a URL deve incluir uma _string_ específica.

Essas ações sempre acontecerão em série (uma após a outra), nunca em paralelo (ao mesmo tempo). Por quê?

Para ilustrar isso, vamos revisitar essa lista de ações e expor alguns dos ✨ truques ✨ ocultos que o Cypress faz a cada passo:

1. Visitar uma URL
   ✨ e esperar que o evento `load` da página seja disparado depois que todos os recursos externos tiverem sido carregados✨

   [//]: <> (TODO - Adicionar link - integração github)

2. Encontrar um elemento pelo seletor
   ✨ e [tentar novamente](https://docs.cypress.io/guides/core-concepts/retry-ability.html)
   até que ele seja encontrado no DOM ✨

   [//]: <> (TODO - Adicionar link - integração github)

3. Executar uma ação de clique nesse elemento
   ✨ depois de esperar que o elemento tenha um
   [estado acionável](https://docs.cypress.io/guides/core-concepts/interacting-with-elements.html) ✨
4. Pegar a URL e...

   [//]: <> (TODO - Adicionar link - integração github)

5. Afirmar que a URL deve incluir uma _string_ específica
   ✨ e [tentar novamente](https://docs.cypress.io/guides/core-concepts/retry-ability.html)
   até que a asserção seja aprovada ✨

Como você pode ver, o Cypress trabalha muito para garantir que o estado da aplicação seja
exatamente o estado que nossos comandos esperam.
Cada comando pode ser resolvido rapidamente (tão rápido que você não irá vê-los em um estado pendente),
mas outros podem levar segundos ou até mesmo dezenas de segundos para serem resolvidos.

[//]: <> (TODO - Adicionar link - integração github)

Embora a maioria dos comandos expire após alguns segundos, outros comandos especializados que
esperam que determinados processos demorem muito mais tempo, como o
[`cy.visit()`](https://docs.cypress.io/api/commands/visit.html), naturalmente aguardarão mais tempo antes de expirar.

[//]: <> (TODO - Adicionar link - integração github)

Estes comandos têm seus próprios valores de timeout específicos que estão documentados em nossa [configuração](https://docs.cypress.io/guides/references/configuration.html).

> Conceito importante
>
> Qualquer espera ou nova tentativa que seja necessária para garantir que
> uma etapa executada foi bem-sucedida deve terminar antes do início da etapa seguinte.
> Se ela não terminar com sucesso antes de o timeout ser atingido, o teste será reprovado.

### Os comandos são Promises

Este é o grande segredo da Cypress: pegamos nosso padrão favorito de composição de código JavaScript, as Promises,
e as incorporamos diretamente na essência do Cypress. Acima, quando dizemos que estamos enfileirando ações
para serem tomadas mais tarde, poderíamos reformular isso como "adicionar Promises a uma cadeia de Promises".

Vamos comparar o exemplo anterior com uma versão fictícia do mesmo como código bruto, mas em forma de Promises:

#### Demonstração cheia de Promises (não é um código válido)

```JS
it('muda a URL ao clicar em "awesome"', () => {
  // ESTE CÓDIGO NÃO É VÁLIDO E
  // SERVE APENAS PARA DEMONSTRAÇÃO
  return cy.visit('/my/resource/path')
  .then(() => {
    return cy.get('.awesome-selector')
  })
  .then(($element) => {
    // não análogo
    return cy.click($element)
  })
  .then(() => {
    return cy.url()
  })
  .then((url) => {
    expect(url).to.eq('/my/resource/path#awesomeness')
  })
})
```

#### Como realmente fica no Cypress com as Promises encapsuladas e escondidas

```JS
it('muda a URL ao clicar em "awesome"', () => {
  cy.visit('/my/resource/path')

  cy.get('.awesome-selector')
    .click()

  cy.url()
    .should('include', '/my/resource/path#awesomeness')
})
```

Uma grande diferença! Além de ser muito mais legível, o Cypress oferece outros benefícios,
pois as Promises em si não têm o conceito de novas tentativas.

Sem a possibilidade de fazer novas tentativas, as asserções falhariam aleatoriamente.
Isso levaria a resultados inconsistentes e com falhas. É também por isso que não podemos usar novos recursos do JS como `async/await`.

O Cypress não pode gerar valores primitivos isolados de outros comandos.
Isso porque os comandos do Cypress agem internamente como um fluxo assíncrono de dados
que só é resolvido depois de ser afetado e modificado por outros comandos.
Isso significa que não podemos gerar valores parciais, porque temos que saber tudo que você espera
antes de devolvermos um valor.

Esses padrões de projeto garantem que possamos criar testes previsíveis, repetíveis e consistentes que não apresentem falhas.

[//]: <> (TODO - Adicionar link - integração github)

> O Cypress é construído com Promises que vêm do [Bluebird](http://bluebirdjs.com/).
> No entanto, os comandos do Cypress não retornam essas típicas instâncias de Promises.
> Em vez disso, retornamos algo chamado `Chainer`, que funciona como uma camada
> que fica por cima das instâncias das Promises internas.
>
> Por esse motivo, você nunca pode atribuir ou retornar algo útil dos comandos do Cypress.
>
> Se você deseja saber mais sobre como lidar com comandos assíncronos do Cypress, leia nosso [Guia de Conceitos Básicos](https://docs.cypress.io/guides/core-concepts/variables-and-aliases.html).

### Comandos não são Promises

A API do Cypress não é uma implementação exata das Promises.
Ela têm qualidades semelhantes a Promises, porém, há diferenças importantes das quais você deve estar ciente.

1. Você não pode fazer uma "corrida" de comandos ou executar vários comandos ao mesmo tempo (em paralelo).

2. Você não pode esquecer "acidentalmente" de retornar ou encadear um comando.

3. Você não pode adicionar um manipulador de erro `.catch` a um comando que falhou.

Existem razões _muito_ específicas pelas quais essas limitações foram incorporadas na API do Cypress.

Todo o propósito do Cypress (e o que o torna muito diferente de outras ferramentas de teste) é
criar testes consistentes sem falhas, que são executados de forma idêntica de uma execução para a próxima.
Tornar isso possível tem seu preço: há algumas restrições que à primeira vista podem parecer estranhas
para desenvolvedores acostumados a trabalhar com Promises.

Vejamos em detalhes cada um dessas restrições:

#### Você não pode fazer uma "corrida" de comandos ou executar vários comandos ao mesmo tempo

O Cypress garante que executará todos os seus comandos de forma determinística e idêntica sempre que eles forem executados.

Vários comandos do Cypress _modificam_ do estado do navegador de alguma forma.

[//]: <> (TODO - Adicionar link - integração github)

- [`cy.request()`](https://docs.cypress.io/api/commands/request.html) automaticamente obtém/define cookies no servidor remoto.

[//]: <> (TODO - Adicionar link - integração github)

- [`cy.clearCookies()`](https://docs.cypress.io/api/commands/clearcookies.html) apaga todos os cookies do navegador.

[//]: <> (TODO - Adicionar link - integração github)

- [`.click()`](https://docs.cypress.io/api/commands/click.html) faz sua aplicação reagir a eventos de clique.

Nenhum dos comandos acima é _idempotente_: todos eles causam efeitos colaterais.
Uma corrida de comandos não é possível porque os comandos devem ser executados
de forma controlada e em série a fim de garantir a consistência.
Como os testes de integração e e2e geralmente imitam as ações de um usuário real,
o Cypress baseia seu modelo de execução de comandos em um usuário real agindo passo a passo.

#### Você não pode esquecer acidentalmente de retornar ou encadear um comando

Em Promises reais, é muito fácil "perder" uma Promise aninhada se você não a retornar ou encadear corretamente.

Vamos imaginar o seguinte código do Node:

```JS
// Supondo que estamos usando Promises com o módulo fs
return fs.readFile('/foo.txt', 'utf8')
.then((txt) => {
  // Ops, esquecemos de encadear/retornar essa Promise,
  // então ela é basicamente "perdida".
  // Isso pode criar condições de corrida bizarras e
  // bugs difíceis de rastrear.
  fs.writeFile('/foo.txt', txt.replace('foo', 'bar'))

  return fs.readFile('/bar.json')
  .then((json) => {
    // ...
  })
})
```

A razão pela qual isso é possível no mundo das Promises é o fato de você
poder executar várias ações assíncronas em paralelo.
Por baixo dos panos, cada "cadeia" de Promises retorna uma instância da Promise
que rastreia a relação entre as instâncias pai e filho vinculadas.

Como o Cypress garante que os comandos sejam executados _somente_ em série,
você não precisa se preocupar com isso quando usa o Cypress.
Enfileiramos todos os comandos em um singleton _global_.
Como sempre há apenas uma única instância da fila de comandos, é impossível que os comandos sejam "_perdidos_".

É como se o Cypress "enfileirasse" todos os comandos.
Posteriormente, eles serão executados na ordem exata em que foram usados, 100% do tempo.

Nunca há necessidade de `retornar` comandos do Cypress.

#### Você não pode adicionar um manipulador de erro `.catch` a um comando que falhou

No Cypress, não há recuperação de erro predefinida para comandos que falham.
Um comando e todas as suas asserções _sempre_ serão aprovados ou, se um comando falhar,
nenhum dos comandos restantes será executado e, portanto, o teste será reprovado.

Você pode estar se perguntando:

> Como crio um fluxo de controle condicional usando if/else,
> de forma que, se um elemento existir (ou não existir), eu possa escolher o que fazer?

O problema com essa pergunta é que esse tipo de fluxo de controle condicional acaba sendo não determinístico.
Isso significa que é impossível para um script (ou robô) segui-lo de forma 100% consistente.

Geralmente, existe apenas um número limitado de situações muito específicas em que você _pode_ criar um fluxo de controle.
Pedir uma recuperação de erros é basicamente o mesmo que pedir outro fluxo de controle `if/else`.

Dito isso, desde que você esteja ciente das possíveis armadilhas relacionadas ao fluxo de controle,
é possível fazer isso no Cypress!

[//]: <> (TODO - Adicionar link - integração github)

Leia tudo sobre como fazer [testes condicionais](https://docs.cypress.io/guides/core-concepts/conditional-testing.html) aqui.

[Voltar para o topo](#introdução-ao-cypress)
