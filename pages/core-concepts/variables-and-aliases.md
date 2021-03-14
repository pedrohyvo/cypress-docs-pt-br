# Variáveis e Apelidos

```markdown
O que você vai aprender

- Como lidar com comandos assíncronos
- O que são apelidos e como eles simplificam seu código
- Por que você raramente precisa usar variáveis com Cypress
- Como usar apelidos para objetos, elementos e rotas
```

## Valores Retornados

Novos usuários do Cypress podem inicialmente achar desafiador trabalhar com a natureza assíncrona de nossas APIs.

> **Não se preocupe!**
>
> Existem muitas maneiras de referenciar, comparar e utilizar os objetos que os comandos do Cypress fornecem para você.
>
> Depois de pegar o jeito do código assíncrono, você perceberá que pode fazer tudo o que pode fazer de forma síncrona,
> sem que seu código faça backflips.
>
> Este guia explora muitos padrões comuns para escrever um bom código Cypress que pode lidar até mesmo com as situações 
> mais complexas.

As APIs assíncronas vieram para ficar no JavaScript. Elas são encontradas em todo o código moderno. Na verdade, a 
maioria das novas APIs dos navegadores são assíncronas e muitos módulos Node principais também são assíncronos.

Os padrões que exploraremos abaixo são úteis dentro e fora do Cypress.

O primeiro e mais importante conceito que você deve reconhecer é...

> **Valores Retornados**
>
> Você não pode atribuir ou trabalhar com os valores de retorno de qualquer comando Cypress. Os comandos são 
> enfileirados e executados de forma assíncrona.

```JS
// this won't work the way you think it does
const button = cy.get('button')
const form = cy.get('form')

button.click()
```

## Encerramentos

[//]: <> (TODO - Adicionar links - comando cypress .then)
Para acessar o que cada comando Cypress fornece, você usa [`.then()`](https://docs.cypress.io/api/commands/then.html).

```JS
cy.get('button').then(($btn) => {
  // $btn is the object that the previous
  // command yielded us
})
```

Se você estiver familiarizado com [`Promises nativas`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Using_promises),
o Cypress `.then()` funciona da mesma maneira. Você pode continuar a aninhar mais comandos Cypress dentro do `.then()`.

Cada comando aninhado tem acesso ao trabalho realizado nos comandos anteriores. Isso acaba deixando a leitura muito boa.

```JS
cy.get('button').then(($btn) => {

  // store the button's text
  const txt = $btn.text()

  // submit a form
  cy.get('form').submit()

  // compare the two buttons' text
  // and make sure they are different
  cy.get('button').should(($btn2) => {
    expect($btn2.text()).not.to.eq(txt)
  })
})

// these commands run after all of the
// other previous commands have finished
cy.get(...).find(...).should(...)
```

Os comandos fora de `.then()` não serão executados até que todos os comandos aninhados sejam concluídos.

> Usando funções de callback, criamos um [`encerramento`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures).
> Os encerramentos nos permitem manter referências para fazer referência ao trabalho realizado em comandos anteriores.

## Depuração

Usar funções `.then()` é uma excelente oportunidade para usar o [`depurador`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/debugger).
Isso pode ajudá-lo a entender a ordem em que os comandos são executados. Isso também permite que você inspecione os 
objetos que o Cypress fornece a você em cada comando.

```JS
cy.get('button').then(($btn) => {
  // inspect $btn <object>
  debugger

  cy.get('#countries').select('USA').then(($select) => {
    // inspect $select <object>
    debugger

    cy.url().should((url) => {
      // inspect the url <string>
      debugger

      $btn    // is still available
      $select // is still available too
    })
  })
})
```

## Variáveis

Normalmente no Cypress você dificilmente precisa usar `const`, `let` ou `var`. Ao usar encerramentos, você sempre
terá acesso aos objetos que foram fornecidos a você sem atribuí-los.

A única exceção a essa regra é quando você está lidando com objetos mutáveis (que mudam de estado). Quando as coisas 
mudam de estado, você geralmente deseja comparar o valor anterior de um objeto com o próximo valor.

Este é um ótimo caso de uso para um `const`.

```HTML
<button>increment</button>

you clicked button <span id='num'>0</span> times
```

```JS
// app code
let count = 0

$('button').on('click', () => {
  $('#num').text(count += 1)
})
```

```JS
// cypress test code
cy.get('#num').then(($span) => {
  // capture what num is right now
  const num1 = parseFloat($span.text())

  cy.get('button').click().then(() => {
    // now capture it again
    const num2 = parseFloat($span.text())

    // make sure it's what we expected
    expect(num2).to.eq(num1 + 1)
  })
})
```

A razão para usar `const` é porque o objeto `$span` é mutável. Sempre que você tiver objetos mutáveis e tentar compará-los,
precisará armazenar seus valores. Usar `const` é uma maneira perfeita de fazer isso.

## Apelidos

Usar funções de callback `.then()` para acessar os valores de comando anteriores é ótimo, mas o que acontece quando você
está executando o código em ganchos como `before` ou `beforeEach`?

```JS
beforeEach(() => {
  cy.button().then(($btn) => {
    const text = $btn.text()
  })
})

it('does not have access to text', () => {
  // how do we get access to text ?!?!
})
```

Como teremos acesso ao `text`?

Poderíamos fazer nosso código fazer alguns backflips feios usando let para obter acesso a ele.

> **Não faça isso**
>
> Este código abaixo é apenas para demonstração.

```JS
describe('a suite', () => {
  // this creates a closure around
  // 'text' so we can access it
  let text

  beforeEach(() => {
    cy.button().then(($btn) => {
      // redefine text reference
      text = $btn.text()
    })
  })

  it('does have access to text', () => {
    // now text is available to us
    // but this is not a great solution :(
    text
  })
})
```

Felizmente, você não precisa fazer seu código dar backflips. Com o Cypress, podemos lidar melhor com essas situações.

> **Apresentando Apelidos**
>
> Os Apelidos são uma construção poderosa no Cypress que tem muitos usos. Exploraremos cada um de seus recursos a seguir.
>
> A princípio, vamos usá-los para compartilhar objetos entre seus ganchos e seus testes.

## Contexto de compartilhamento

O contexto de compartilhamento é a maneira mais simples de usar apelidos.

[//]: <> (TODO - Adicionar links - comandos cypress .as)
Para criar um apelido de algo que você gostaria de compartilhar, use o comando [`.as()`](https://docs.cypress.io/api/commands/as.html).

Vejamos nosso exemplo anterior com apelidos.

```JS
beforeEach(() => {
  // alias the $btn.text() as 'text'
  cy.get('button').invoke('text').as('text')
})

it('has access to text', function () {
  this.text // is now available
})
```

Nos bastidores, ao apelidar objetos básicos e primitivos é utilizado o objeto de [`contexto`](https://github.com/mochajs/mocha/wiki/Shared-Behaviours)
compartilhado do Mocha: isto é, apelidos estão disponíveis como `this.*`.

O Mocha compartilha contextos automaticamente para nós em todos os ganchos aplicáveis para cada teste. Além disso, esses
apelidos e propriedades são limpos automaticamente após cada teste.

```JS
describe('parent', () => {
  beforeEach(() => {
    cy.wrap('one').as('a')
  })

  context('child', () => {
    beforeEach(() => {
      cy.wrap('two').as('b')
    })

    describe('grandchild', () => {
      beforeEach(() => {
        cy.wrap('three').as('c')
      })

      it('can access all aliases as properties', function () {
        expect(this.a).to.eq('one')   // true
        expect(this.b).to.eq('two')   // true
        expect(this.c).to.eq('three') // true
      })
    })
  })
})
```

**Acessando Fixtures:**

[//]: <> (TODO - Adicionar links - comando cy.fixture)
O caso de uso mais comum para compartilhar contexto é ao lidar com [`cy.fixture()`](https://docs.cypress.io/api/commands/fixture.html).

Frequentemente, você pode carregar um acessório em um gancho `beforeEach`, mas deseja utilizar os valores em seus testes.

```JS
beforeEach(() => {
  // alias the users fixtures
  cy.fixture('users.json').as('users')
})

it('utilize users in some way', function () {
  // access the users property
  const user = this.users[0]

  // make sure the header contains the first
  // user's name
  cy.get('header').should('contain', user.name)
})
```

> **Cuidado com os comandos assíncronos** 
>
> Não se esqueça de que os comandos do Cypress são assíncronos!
>
> Você não pode usar uma referência `this.*` Até que o comando `.as()` seja executado.

```JS
it('is not using aliases correctly', function () {
  cy.fixture('users.json').as('users')

  // nope this won't work
  //
  // this.users is not defined
  // because the 'as' command has only
  // been enqueued - it has not run yet
  const user = this.users[0]
})
```

[//]: <> (TODO - Adicionar links - comando cypress .then)
Os mesmos princípios que introduzimos muitas vezes antes se aplicam a essa situação. Se você deseja acessar o que um
comando produz, você deve fazê-lo em um encerramento usando um [`.then()`](https://docs.cypress.io/api/commands/then.html).

```JS
// yup all good
cy.fixture('users.json').then((users) => {
  // now we can avoid the alias altogether
  // and use a callback function
  const user = users[0]

  // passes
  cy.get('header').should('contain', user.name)
})
```

**Evitando o uso do `this`**

> **Arrow Functions**
>
> Acessar apelidos como propriedades com `this.*` não irá funcionar se você usar [`arrow functions`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/>Reference/Functions/Arrow_functions)
> para seus testes ou ganchos.
>
> É por isso que todos os nossos exemplos usam a sintaxe de regular `function () {}` ao invés da sintaxe de
> “seta gorda” lambda `() => {}`.

Em vez de usar a sintaxe `this.*`, Há outra maneira de acessar apelidos.

[//]: <> (TODO - Adicionar links - comando cy.get)
O comando [`cy.get()`](https://docs.cypress.io/api/commands/get.html) é capaz de acessar apelidos com uma sintaxe 
especial usando o caractere `@`:

```JS
beforeEach(() => {
  // alias the users fixtures
  cy.fixture('users.json').as('users')
})

it('utilize users in some way', function () {
  // use the special '@' syntax to access aliases
  // which avoids the use of 'this'
  cy.get('@users').then((users) => {
    // access the users argument
    const user = users[0]

    // make sure the header contains the first
    // user's name
    cy.get('header').should('contain', user.name)
  })
})
```

[//]: <> (TODO - Adicionar links - comando cy.get)
Ao usar [`cy.get()`](https://docs.cypress.io/api/commands/get.html), evitamos o uso do `this`.

Lembre-se de que há casos de uso para as duas abordagens porque elas têm ergonomias diferentes.

Ao usar `this.users`, temos acesso a ele de forma síncrona, enquanto que ao usar `cy.get('@users')` ele se torna um 
comando assíncrono.

[//]: <> (TODO - Adicionar links - comando wrap)
Você pode pensar em `cy.get('@users')` como fazendo a mesma coisa que [`cy.(this.users)`](https://docs.cypress.io/api/commands/wrap.html).

## Elementos

Os Apelidos têm outras características especiais quando usados com elementos DOM.

Depois de criar um apelido para os elementos DOM, você pode acessá-los posteriormente para reutilização.

```JS
// alias all of the tr's found in the table as 'rows'
cy.get('table').find('tr').as('rows')
```

[//]: <> (TODO - Adicionar links - comando cy.get)
Internamente, o Cypress fez uma referência à coleção `<tr>` retornada como o apelido “rows”. Para fazer referência a essas
mesmas “linhas” posteriormente, você pode usar o comando [`cy.get()`](https://docs.cypress.io/api/commands/get.html).

```JS
// Cypress returns the reference to the <tr>'s
// which allows us to continue to chain commands
// finding the 1st row.
cy.get('@rows').first().click()
```

[//]: <> (TODO - Adicionar links - comando cy.get)
Como usamos o caractere `@` em [`cy.get()`](https://docs.cypress.io/api/commands/get.html), em vez de consultar o DOM em
busca de elementos, [`cy.get()`](https://docs.cypress.io/api/commands/get.html) procura por um apelido existente chamado
de `rows` e retorna a referência (se encontrar).

**Elementos obsoletos:**

[//]: <> (TODO - Adicionar links - comando cy.get)
Em muitos aplicativos single-page JavaScript, o DOM re-renderiza partes do aplicativo constantemente. Se você criar
um apelido para os elementos DOM que foram removidos do DOM no momento em que você chamar [`cy.get()`](https://docs.cypress.io/api/commands/get.html)
com o apelido, o Cypress consultará automaticamente o DOM para encontrar esses elementos novamente.

```HTML
<ul id="todos">
  <li>
    Walk the dog
    <button class="edit">edit</button>
  </li>
  <li>
    Feed the cat
    <button class="edit">edit</button>
  </li>
</ul>
```

Vamos imaginar que, quando clicamos no botão `.edit`, nosso `<li>` é renderizado novamente no DOM. Em vez de exibir o botão
de edição, ele exibe um campo de texto `<input />` permitindo que você edite o *todo*. O `<li>` anterior foi *completamente*
removido do DOM e um novo `<li>` é renderizado em seu lugar.

```JS
cy.get('#todos li').first().as('firstTodo')
cy.get('@firstTodo').find('.edit').click()
cy.get('@firstTodo').should('have.class', 'editing')
  .find('input').type('Clean the kitchen')
```

Quando referenciamos `@firstTodo`, Cypress verifica se todos os elementos que está referenciando ainda estão no DOM. 
Se estiverem, ele retorna os elementos existentes. Se não forem, o Cypress reproduz os comandos que levam à definição do
apelido.

Em nosso caso, ele emitiria novamente os comandos: `cy.get('# todos li').First()`. Tudo funciona porque o novo `<li>` foi
encontrado.

[//]: <> (TODO - Adicionar links - comando cy.get)
> Normalmente, a repetição dos comandos anteriores retornará o que você espera, mas nem sempre. É recomendável que você 
> crie um apelido para os elementos o mais rápido possível, em vez de seguir adiante na cadeia de comandos.
> 
> - `cy.get('#nav header .user').as('user')` ✅ (bom)
> - `cy.get('#nav').find('header').find('.user').as('user')` ❌ (ruim)
>
> Em caso de dúvida, você sempre pode executar [`cy.get()`](https://docs.cypress.io/api/commands/get.html) para 
> consultar os elementos novamente.

## Rotas

[//]: <> (TODO - Adicionar links - rotas)
Apelidos também podem ser usados com [rotas](https://docs.cypress.io/api/commands/route.html). Apelidar suas rotas 
permite que você:

- garanta que seu aplicativo faça as solicitações pretendidas
- espere que o seu servidor envie a resposta
- acesse o objeto de solicitação real para asserções

![aliasing-routes](https://docs.cypress.io/img/guides/aliasing-routes.b16f3c04.jpg)

Aqui está um exemplo de como apelidar uma rota e aguardar sua conclusão.

```JS
cy.intercept('POST', '/users', { id: 123 }).as('postUser')

cy.get('form').submit()

cy.wait('@postUser').then(({ request }) => {
  expect(request.body).to.have.property('name', 'Brian')
})

cy.contains('Successfully created user: Brian')
```

> Novo no Cypress?
>
> [Temos um guia muito mais detalhado e abrangente sobre o roteamento de requisições de rede.](https://docs.cypress.io/guides/guides/network-requests.html)

## Requisições

Apelidos também podem ser usados com [requisições](https://docs.cypress.io/api/commands/request.html).

Aqui está um exemplo de como apelidar uma requisição e acessar suas propriedades posteriormente.

```JS
cy.request('https://jsonplaceholder.cypress.io/comments').as('comments')

// other test code here

cy.get('@comments').should((response) => {
  if (response.status === 200) {
      expect(response).to.have.property('duration')
    } else {
      // whatever you want to check here
    }
  })
})
```

[Voltar para o topo](#variáveis-e-apelidos)
