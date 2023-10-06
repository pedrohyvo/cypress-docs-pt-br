# Asserções

O Cypress empacota a biblioteca popular de asserções [Chai](#chai), assim como
extensões úteis para [Sinon](#sinon-chai) e [jQuery](#chai-jquery), trazendo
dezenas de asserções poderosas gratuitamente.

> **Novo no Cypress?**
>
> Este documento é apenas uma referência a todas as asserções que o Cypress
> suporta.
>
> Se você está procurando entender **como** usar essas asserções, leia sobre
> asserções em nosso guia de
> [Introdução ao Cypress](../core-concepts/introduction-to-cypress.md#Asserções)
> guide.

## Chai

[https://github.com/chaijs/chai](https://github.com/chaijs/chai)

### Asserções BDD

Esses encadeadores estão disponíveis para asserções BDD (`expect`/`should`). Os
pseudônimos listados podem ser usados de forma intercambiável com seu encadeador
original. Você pode ver a lista completa das asserções Chai BDD
[aqui](http://chaijs.com/api/bdd/).

| Encadeador                                                                                                           | Exemplo                                                                                                                               |
| -------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------- |
| not                                                                                                                  | `expect(name).to.not.equal('Jane')`                                                                                                   |
| deep                                                                                                                 | `expect(obj).to.deep.equal({ name: 'Jane' })`                                                                                         |
| nested                                                                                                               | `expect({a: {b: ['x', 'y']}}).to.have.nested.property('a.b[1]')`<br>`expect({a: {b: ['x', 'y']}}).to.nested.include({'a.b[1]': 'y'})` |
| ordered                                                                                                              | `expect([1, 2]).to.have.ordered.members([1, 2]).but.not.have.ordered.members([2, 1])`                                                 |
| any                                                                                                                  | `expect(arr).to.have.any.keys('age')`                                                                                                 |
| all                                                                                                                  | `expect(arr).to.have.all.keys('name', 'age')`                                                                                         |
| a(_type_) <br><small class="aliases"><strong>Aliases: </strong>an</small>                                            | `expect('test').to.be.a('string')`                                                                                                    |
| include(_value_) <br><small class="aliases"><strong>Aliases: </strong>contain, includes, contains</small>            | `expect([1,2,3]).to.include(2)`                                                                                                       |
| ok                                                                                                                   | `expect(undefined).to.not.be.ok`                                                                                                      |
| true                                                                                                                 | `expect(true).to.be.true`                                                                                                             |
| false                                                                                                                | `expect(false).to.be.false`                                                                                                           |
| null                                                                                                                 | `expect(null).to.be.null`                                                                                                             |
| undefined                                                                                                            | `expect(undefined).to.be.undefined`                                                                                                   |
| exist                                                                                                                | `expect(myVar).to.exist`                                                                                                              |
| empty                                                                                                                | `expect([]).to.be.empty`                                                                                                              |
| arguments <br><small class="aliases"><strong>Aliases: </strong>Arguments</small>                                     | `expect(arguments).to.be.arguments`                                                                                                   |
| equal(_value_) <br><small class="aliases"><strong>Aliases: </strong>equals, eq</small>                               | `expect(42).to.equal(42)`                                                                                                             |
| deep.equal(_value_)                                                                                                  | `expect({ name: 'Jane' }).to.deep.equal({ name: 'Jane' })`                                                                            |
| eql(_value_) <br><small class="aliases"><strong>Aliases: </strong>eqls</small>                                       | `expect({ name: 'Jane' }).to.eql({ name: 'Jane' })`                                                                                   |
| greaterThan(_value_) <br><small class="aliases"><strong>Aliases: </strong>gt, above</small>                          | `expect(10).to.be.greaterThan(5)`                                                                                                     |
| least(_value_)<br><small class="aliases"><strong>Aliases: </strong>gte</small>                                       | `expect(10).to.be.at.least(10)`                                                                                                       |
| lessThan(_value_) <br><small class="aliases"><strong>Aliases: </strong>lt, below</small>                             | `expect(5).to.be.lessThan(10)`                                                                                                        |
| most(_value_) <br><small class="aliases"><strong>Aliases: </strong>lte</small>                                       | `expect('test').to.have.length.of.at.most(4)`                                                                                         |
| within(_start_, _finish_)                                                                                            | `expect(7).to.be.within(5,10)`                                                                                                        |
| instanceOf(_constructor_) <br><small class="aliases"><strong>Aliases: </strong>instanceof</small>                    | `expect([1, 2, 3]).to.be.instanceOf(Array)`                                                                                           |
| property(_name_, _[value]_)                                                                                          | `expect(obj).to.have.property('name')`                                                                                                |
| deep.property(_name_, _[value]_)                                                                                     | `expect(deepObj).to.have.deep.property('tests[1]', 'e2e')`                                                                            |
| ownProperty(_name_) <br><small class="aliases"><strong>Aliases: </strong>haveOwnProperty, own.property</small>       | `expect('test').to.have.ownProperty('length')`                                                                                        |
| ownPropertyDescriptor(_name_) <br><small class="aliases"><strong>Aliases: </strong>haveOwnPropertyDescriptor</small> | `expect({a: 1}).to.have.ownPropertyDescriptor('a')`                                                                                   |
| lengthOf(_value_)                                                                                                    | `expect('test').to.have.lengthOf(3)`                                                                                                  |
| match(_RegExp_) <br><small class="aliases"><strong>Aliases: </strong>matches</small>                                 | `expect('testing').to.match(/^test/)`                                                                                                 |
| string(_string_)                                                                                                     | `expect('testing').to.have.string('test')`                                                                                            |
| keys(_key1_, _[key2]_, _[...]_) <br><small class="aliases"><strong>Aliases: </strong>key</small>                     | `expect({ pass: 1, fail: 2 }).to.have.keys('pass', 'fail')`                                                                           |
| throw(_constructor_) <br><small class="aliases"><strong>Aliases: </strong>throws, Throw</small>                      | `expect(fn).to.throw(Error)`                                                                                                          |
| respondTo(_method_) <br><small class="aliases"><strong>Aliases: </strong>respondsTo</small>                          | `expect(obj).to.respondTo('getName')`                                                                                                 |
| itself                                                                                                               | `expect(Foo).itself.to.respondTo('bar')`                                                                                              |
| satisfy(_method_) <br><small class="aliases"><strong>Aliases: </strong>satisfies</small>                             | `expect(1).to.satisfy((num) => { return num > 0 })`                                                                                   |
| closeTo(_expected_, _delta_) <br><small class="aliases"><strong>Aliases: </strong>approximately</small>              | `expect(1.5).to.be.closeTo(1, 0.5)`                                                                                                   |
| members(_set_)                                                                                                       | `expect([1, 2, 3]).to.include.members([3, 2])`                                                                                        |
| oneOf(_values_)                                                                                                      | `expect(2).to.be.oneOf([1,2,3])`                                                                                                      |
| change(_function_) <br><small class="aliases"><strong>Aliases: </strong>changes</small>                              | `expect(fn).to.change(obj, 'val')`                                                                                                    |
| increase(_function_) <br><small class="aliases"><strong>Aliases: </strong>increases</small>                          | `expect(fn).to.increase(obj, 'val')`                                                                                                  |
| decrease(_function_) <br><small class="aliases"><strong>Aliases: </strong>decreases</small>                          | `expect(fn).to.decrease(obj, 'val')`                                                                                                  |

Esses obtentores (getters) também estão disponíveis para asserções BDD. Eles
não fazem nada, mas permitem que se escreva frases claras em inglês.

| Obtentores encadeáveis (Chainable getters)                                                  |
| ------------------------------------------------------------------------------------------- |
| `to`, `be`, `been`, `is`, `that`, `which`, `and`, `has`, `have`, `with`, `at`, `of`, `same` |

### Asserções TDD

Essas asserções estão disponíveis para asserções TDD (`assert`). Você pode ver
a lista completa de asserções Chai disponíveis
[aqui](http://chaijs.com/api/assert/).

| Assertion                                                   | Example                                                |
| ----------------------------------------------------------- | ------------------------------------------------------ |
| .isOk(_object_, _[message]_)                                | `assert.isOk('everything', 'everything is ok')`        |
| .isNotOk(_object_, _[message]_)                             | `assert.isNotOk(false, 'this will pass')`              |
| .equal(_actual_, _expected_, _[message]_)                   | `assert.equal(3, 3, 'vals equal')`                     |
| .notEqual(_actual_, _expected_, _[message]_)                | `assert.notEqual(3, 4, 'vals not equal')`              |
| .strictEqual(_actual_, _expected_, _[message]_)             | `assert.strictEqual(true, true, 'bools strict eq')`    |
| .notStrictEqual(_actual_, _expected_, _[message]_)          | `assert.notStrictEqual(5, '5', 'not strict eq')`       |
| .deepEqual(_actual_, _expected_, _[message]_)               | `assert.deepEqual({ id: '1' }, { id: '1' })`           |
| .notDeepEqual(_actual_, _expected_, _[message]_)            | `assert.notDeepEqual({ id: '1' }, { id: '2' })`        |
| .isAbove(_valueToCheck_, _valueToBeAbove_, _[message]_)     | `assert.isAbove(6, 1, '6 greater than 1')`             |
| .isAtLeast(_valueToCheck_, _valueToBeAtLeast_, _[message]_) | `assert.isAtLeast(5, 2, '5 gt or eq to 2')`            |
| .isBelow(_valueToCheck_, _valueToBeBelow_, _[message]_)     | `assert.isBelow(3, 6, '3 strict lt 6')`                |
| .isAtMost(_valueToCheck_, _valueToBeAtMost_, _[message]_)   | `assert.isAtMost(4, 4, '4 lt or eq to 4')`             |
| .isTrue(_value_, _[message]_)                               | `assert.isTrue(true, 'this val is true')`              |
| .isNotTrue(_value_, _[message]_)                            | `assert.isNotTrue('tests are no fun', 'val not true')` |
| .isFalse(_value_, _[message]_)                              | `assert.isFalse(false, 'val is false')`                |
| .isNotFalse(_value_, _[message]_)                           | `assert.isNotFalse('tests are fun', 'val not false')`  |
| .isNull(_value_, _[message]_)                               | `assert.isNull(err, 'there was no error')`             |
| .isNotNull(_value_, _[message]_)                            | `assert.isNotNull('hello', 'is not null')`             |
| .isNaN(_value_, _[message]_)                                | `assert.isNaN(NaN, 'NaN is NaN')`                      |
| .isNotNaN(_value_, _[message]_)                             | `assert.isNotNaN(5, '5 is not NaN')`                   |
| .exists(_value_, _[message]_)                               | `assert.exists(5, '5 is not null or undefined')`       |
| .notExists(_value_, _[message]_)                            | `assert.notExists(null, 'val is null or undefined')`   |
| .isUndefined(_value_, _[message]_)                          | `assert.isUndefined(undefined, 'val is undefined')`    |
| .isDefined(_value_, _[message]_)                            | `assert.isDefined('hello', 'val has been defined')`    |
| .isFunction(_value_, _[message]_)                           | `assert.isFunction(x => x * x, 'val is func')`         |
| .isNotFunction(_value_, _[message]_)                        | `assert.isNotFunction(5, 'val not funct')`             |
| .isObject(_value_, _[message]_)                             | `assert.isObject({num: 5}, 'val is object')`           |
| .isNotObject(_value_, _[message]_)                          | `assert.isNotObject(3, 'val not object')`              |
| .isArray(_value_, _[message]_)                              | `assert.isArray(['unit', 'e2e'], 'val is array')`      |
| .isNotArray(_value_, _[message]_)                           | `assert.isNotArray('e2e', 'val not array')`            |
| .isString(_value_, _[message]_)                             | `assert.isString('e2e', 'val is string')`              |
| .isNotString(_value_, _[message]_)                          | `assert.isNotString(2, 'val not string')`              |
| .isNumber(_value_, _[message]_)                             | `assert.isNumber(2, 'val is number')`                  |
| .isNotNumber(_value_, _[message]_)                          | `assert.isNotNumber('e2e', 'val not number')`          |
| .isFinite(_value_, _[message]_)                             | `assert.isFinite('e2e', 'val is finite')`              |
| .isBoolean(_value_, _[message]_)                            | `assert.isBoolean(true, 'val is bool')`                |
| .isNotBoolean(_value_, _[message]_)                         | `assert.isNotBoolean('true', 'val not bool')`          |
| .typeOf(_value_, _name_, _[message]_)                       | `assert.typeOf('e2e', 'string', 'val is string')`      |
| .notTypeOf(_value_, _name_, _[message]_)                    | `assert.notTypeOf('e2e', 'number', 'val not number')`  |

## Chai-jQuery

[https://github.com/chaijs/chai-jquery](https://github.com/chaijs/chai-jquery)

Esses encadeadores estão disponíveis ao assertar sobre um objeto do DOM.

[//]: <> (TODO - Adicionar links - cy.get, cy.contains)

Normalmente, você usará esses encadeadores depois de usar os comandos do DOM
como: [`cy.get()`](https://docs.cypress.io/api/commands/get),
[`cy.contains()`](https://docs.cypress.io/api/commands/contains), etc.

| Encadeadores            | Asserção                                                             |
| ----------------------- | -------------------------------------------------------------------- |
| attr(_name_, _[value]_) | `expect($el).to.have.attr('foo', 'bar')`                             |
| prop(_name_, _[value]_) | `expect($el).to.have.prop('disabled', false)`                        |
| css(_name_, _[value]_)  | `expect($el).to.have.css('background-color', 'rgb(0, 0, 0)')`        |
| data(_name_, _[value]_) | `expect($el).to.have.data('foo', 'bar')`                             |
| class(_className_)      | `expect($el).to.have.class('foo')`                                   |
| id(_id_)                | `expect($el).to.have.id('foo')`                                      |
| html(_html_)            | `expect($el).to.have.html('I love testing')`                         |
| text(_text_)            | `expect($el).to.have.text('I love testing')`                         |
| value(_value_)          | `expect($el).to.have.value('test@dev.com')`                          |
| visible                 | `expect($el).to.be.visible`                                          |
| hidden                  | `expect($el).to.be.hidden`                                           |
| selected                | `expect($option).not.to.be.selected`                                 |
| checked                 | `expect($input).not.to.be.checked`                                   |
| focus[ed]               | `expect($input).not.to.be.focused`<br>`expect($input).to.have.focus` |
| enabled                 | `expect($input).to.be.enabled`                                       |
| disabled                | `expect($input).to.be.disabled`                                      |
| empty                   | `expect($el).not.to.be.empty`                                        |
| exist                   | `expect($nonexistent).not.to.exist`                                  |
| match(_selector_)       | `expect($emptyEl).to.match(':empty')`                                |
| contain(_text_)         | `expect($el).to.contain('text')`                                     |
| descendants(_selector_) | `expect($el).to.have.descendants('div')`                             |

## Sinon-Chai

[https://github.com/domenic/sinon-chai](https://github.com/domenic/sinon-chai)

[//]: <> (TODO - Adicionar links - stub, spy)

Esses encadeadores são usados em asserções com
[`cy.stub()`](https://docs.cypress.io/api/commands/stub) e
[`cy.spy()`](https://docs.cypress.io/api/commands/spy).

| Sinon.JS propriedade/método | Asserção                                                                |
| --------------------------- | ----------------------------------------------------------------------- |
| called                      | `expect(spy).to.be.called`                                              |
| callCount                   | `expect(spy).to.have.callCount(n)`                                      |
| calledOnce                  | `expect(spy).to.be.calledOnce`                                          |
| calledTwice                 | `expect(spy).to.be.calledTwice`                                         |
| calledThrice                | `expect(spy).to.be.calledThrice`                                        |
| calledBefore                | `expect(spy1).to.be.calledBefore(spy2)`                                 |
| calledAfter                 | `expect(spy1).to.be.calledAfter(spy2)`                                  |
| calledWithNew               | `expect(spy).to.be.calledWithNew`                                       |
| alwaysCalledWithNew         | `expect(spy).to.always.be.calledWithNew`                                |
| calledOn                    | `expect(spy).to.be.calledOn(context)`                                   |
| alwaysCalledOn              | `expect(spy).to.always.be.calledOn(context)`                            |
| calledWith                  | `expect(spy).to.be.calledWith(...args)`                                 |
| alwaysCalledWith            | `expect(spy).to.always.be.calledWith(...args)`                          |
| calledWithExactly           | `expect(spy).to.be.calledWithExactly(...args)`                          |
| alwaysCalledWithExactly     | `expect(spy).to.always.be.calledWithExactly(...args)`                   |
| calledWithMatch             | `expect(spy).to.be.calledWithMatch(...args)`                            |
| alwaysCalledWithMatch       | `expect(spy).to.always.be.calledWithMatch(...args)`                     |
| returned                    | `expect(spy).to.have.returned(returnVal)`                               |
| alwaysReturned              | `expect(spy).to.have.always.returned(returnVal)`                        |
| threw                       | `expect(spy).to.have.thrown(errorObjOrErrorTypeStringOrNothing)`        |
| alwaysThrew                 | `expect(spy).to.have.always.thrown(errorObjOrErrorTypeStringOrNothing)` |

## Adicionando Novas Asserções

Como estamos usando `chai`, isso significa que você pode estendê-lo como quiser.
O Cypress "simplesmente funcionará" com novas asserções adicionadas ao `chai`.
Você pode:

- Escrever suas próprias asserções `chai` como
  [documentado aqui](http://chaijs.com/api/plugins/).
- Instalar qualquer biblioteca `chai` existente com `npm` e importar em seu
  arquivo de teste ou de suporte.

[//]: <> (TODO - Adicionar link - exemplos)

> [Confira nosso exemplo estendendo chai com novas asserções.](https://docs.cypress.io/examples/examples/recipes#Fundamentals)

## Asserções Comuns

[//]: <> (TODO - Adicionar links - should, retry)

Aqui está uma lista de asserções de elementos comuns. Observe como usamos essas
asserções (listadas acima) com
[`.should()`](https://docs.cypress.io/api/commands/should).
Você também pode querer ler sobre como o Cypress
[tenta novamente](https://docs.cypress.io/guides/core-concepts/retry-ability)
as asserções.

### Tamanho

```javascript
// tenta até encontrar 3 <li.selected> correspondentes
cy.get('li.selected').should('have.length', 3);
```

### Classe

```javascript
// tenta até esse input não tenha a classe disabled
cy.get('form').find('input').should('not.have.class', 'disabled');
```

### Valor

```javascript
// tenta até esse textarea contenha o valor correto
cy.get('textarea').should('have.value', 'foo bar baz');
```

### Conteúdo do Texto

```javascript
// alega que o conteúdo do texto do elemento é exatamente o texto fornecido
cy.get('#user-name').should('have.text', 'Joe Smith');
// alega que o texto do elemento inclui a substring fornecida
cy.get('#address').should('include.text', 'Atlanta');
// tenta até que este span não contenha 'click me'
cy.get('a').parent('span.help').should('not.contain', 'click me');
// o texto do elemento deve começar com "Hello"
cy.get('#greeting')
  .invoke('text')
  .should('match', /^Hello/);
// dica: use cy.contains para encontrar o elemento com seu texto
// correspondendo com a expressão regular fornecida
cy.contains('#a-greeting', /^Hello/);
```

[//]: <> (TODO - Adicionar link - faq)

**Dica:** leia sobre asserções em um texto com entidades de espaçamentos
ininterruptos em
[Como obtenho o conteúdo de texto de um elemento?](https://docs.cypress.io/faq/questions/using-cypress-faq#How-do-I-get-an-element-s-text-contents)

### Visibilidade

```javascript
// tenta até que o button com id "form-submit" esteja visível
cy.get('button#form-submit').should('be.visible');
// tenta até que o item da lista com
// o texto "write tests" esteja visível
cy.contains('.todo li', 'write tests').should('be.visible');
```

**Nota:** se houverem múltiplos elementos, as asserções `be.visible` e
`not.be.visible` agem de forma diferente:

```javascript
// tenta até que ALGUNS elementos estejam visíveis
cy.get('li').should('be.visible');
// tenta até que TODOS elementos estejam invisíveis
cy.get('li.hidden').should('not.be.visible');
```

Assista ao pequeno vídeo
["Múltiplos elementos e asserções should('be.visible')"](https://www.youtube.com/watch?v=LxkrhUEE2Qk)
que mostra como verificar corretamente a visibilidade dos elementos.

### Existência

```javascript
// tenta até que o elemento com id loading não exista mais
cy.get('#loading').should('not.exist');
```

### State

```javascript
// tenta até que o elemento de type radio esteja selecionado
cy.get(':radio').should('be.checked');
```

### CSS

```javascript
// tenta até que .completed esteja com o css correspondente
cy.get('.completed').should('have.css', 'text-decoration', 'line-through');
```

```javascript
// tenta enquanto .accordion o css não tenha a propriedade "display: none"
cy.get('#accordion').should('not.have.css', 'display', 'none');
```

### Propriedade disabled

```html
<input type="text" id="example-input" disabled />
```

```javascript
cy.get('#example-input')
  .should('be.disabled')
  // vamos habilitar este elemento do teste
  .invoke('prop', 'disabled', false);

cy.get('#example-input')
  // podemos usar a asserção "enabled"
  .should('be.enabled')
  // ou negar a asserção "disabled"
  .and('not.be.disabled');
```

## Asserções negativas

Existem asserções que são positivas e negativas. Exemplos de asserções
positivas:

```javascript
cy.get('.todo-item').should('have.length', 2).and('have.class', 'completed');
```

As asserções negativas têm o encadeador _"not"_ prefixado à asserção.
Exemplos de asserções negativas:

```javascript
cy.contains('first todo').should('not.have.class', 'completed');
cy.get('#loading').should('not.be.visible');
```

### ⚠️ Falsa aprovação em testes

Asserções negativas podem passar por motivos inesperados. Digamos que queremos
testar que uma aplicação de lista de tarefas adiciona um novo item à lista após
digitar a tarefa e pressionar enter.

#### Asserções positivas

Ao adicionar um elemento à lista de tarefas e usando uma **asserção positiva**,
o teste verifica o número específico de itens na lista em nosso aplicativo.

O teste abaixo ainda pode passar erroneamente se o aplicativo se comportar de
maneira inesperada, como adicionar uma tarefa em branco, ao invés de adicionar
a nova tarefa com o texto "Write tests".

```javascript
cy.get('li.todo').should('have.length', 2);
cy.get('input#new-todo').type('Write tests{enter}');

// usando uma asserção positiva para verificar o número exato de itens
cy.get('li.todo').should('have.length', 3);
```

#### Asserções negativas

Porém, ao usar uma **asserção negativa** no teste abaixo, o teste pode passar
erroneamente quando o aplicativo se comporta de diversas maneiras inesperadas:

- O aplicativo exclui toda a lista de itens de tarefas em vez de inserir a
  terceira tarefa
- O aplicativo exclui uma tarefa ao invés de adicionar uma nova tarefa
- O aplicativo adiciona uma tarefa em branco
- Uma variedade infinita de erros de aplicação possíveis

```javascript
cy.get('li.todo').should('have.length', 2);
cy.get('input#new-todo').type('Write tests{enter}');

// usando asserção negativa para verificar se não tem o número de itens
cy.get('li.todo').should('not.have.length', 2);
```

#### Recomendação

Recomendamos o uso de asserções negativas para verificar se uma condição
específica não está mais presente depois que o aplicativo executa uma ação.
Por exemplo, quando um item concluído previamente é desmarcado, podemos
verificar se uma classe CSS foi removida.

```javascript
// primeiramente, o item é marcado como concluído
cy.contains('li.todo', 'Write tests')
  .should('have.class', 'completed')
  .find('.toggle')
  .click();

// a classe CSS foi removida
cy.contains('li.todo', 'Write tests').should('not.have.class', 'completed');
```

Para obter mais exemplos, leia a postagem do blog
[Cuidado com as afirmações negativas.](https://glebbahmutov.com/blog/negative-assertions/).

## Should callback

[//]: <> (TODO - Adicionar links - retry, should)

Se as asserções construídas não forem suficientes, você pode escrever sua
própria função de asserção e passá-la como um callback para o comando
`.should()`. O Cypress irá automaticamente fazer uma
[nova tentativa](https://docs.cypress.io/guides/core-concepts/retry-ability) da
função de callback até que ela passe ou o tempo limite do comando se esgote.
Veja a documentação
[`.should()`](https://docs.cypress.io/api/commands/should#Function).

```html
<div class="main-abc123 heading-xyz987">Introduction</div>
```

```javascript
cy.get('div').should(($div) => {
  expect($div).to.have.length(1);

  const className = $div[0].className;

  // className será uma string como "main-abc123 heading-xyz987"
  expect(className).to.match(/heading-/);
});
```

## Asserções múltiplas

Você pode anexar várias asserções em um mesmo comando.

```html
<a class="assertions-link active" href="https://on.cypress.io" target="_blank"
  >Cypress Docs</a
>
```

```js
cy.get('.assertions-link')
  .should('have.class', 'active')
  .and('have.attr', 'href')
  .and('include', 'cypress.io');
```

Observe que todas as asserções encadeadas usarão a mesma referência ao elemento
original. Por exemplo, se você quiser testar um elemento de loading que
primeiro aparece e depois desaparece, o posterior NÃO FUNCIONARÁ porque o mesmo
elemento não pode ser visível e invisível ao mesmo tempo:

```js
// ⛔️ NÃO FUNCIONA
cy.get('#loading').should('be.visible').and('not.be.visible');
```

Em vez disso, você deve dividir as asserções e consultar novamente o elemento:

```js
// ✅ A MANEIRA CORRETA
cy.get('#loading').should('be.visible');
cy.get('#loading').should('not.be.visible');
```

## Veja também

[//]: <> (TODO - Adicionar link - introduction-to-cypress)

- [Guia: Introdução ao Cypress](https://docs.cypress.io/guides/core-concepts/introduction-to-cypress#Assertions)
- [Asserções cypress-example-kitchensink](https://example.cypress.io/commands/assertions)
- Postagem do blog [Cypress should callback](https://glebbahmutov.com/blog/cypress-should-callback/)

[Voltar para o topo](#Asserções)
