# Testando sua aplicação

```markdown
O que você vai aprender

- A relação entre Cypress e seu back-end
- Como configurar o Cypress para sua aplicação
- Trabalhando com (ou sem) seu mecanismo de autenticação
- Alavancando a eficácia dos dados de teste
```

[Vídeo do Brian Mann na AssertJS(2018)](https://youtu.be/5XQOK0v_YRE)

## Passo 1: Rodar o servidor

[//]: <> (TODO - Adicionar links Test Runner)
Assumindo que você tenha 
[instalado com sucesso o Test Runner](https://docs.cypress.io/guides/getting-started/installing-cypress.html#Installing)
e [aberto o Test Runner](https://docs.cypress.io/guides/getting-started/installing-cypress.html#Opening-Cypress)
no seu projeto, a primeira coisa que você precisa fazer é iniciar o servidor de desenvolvimento local 
que hospeda a aplicação.

Deve ser parecido com **<http://localhost:8080>**.

[//]: <> (TODO - Adicionar link boas práticas)
> Anti-padrão
> Não tente iniciar um servidor da web a partir de scripts Cypress. Leia sobre as 
[boas práticas](https://docs.cypress.io/guides/references/best-practices.html#Web-Servers) aqui.

> Por quê rodar um servidor de desenvolvimento local?

Você pode estar se perguntando - por que não posso simplesmente visitar minha aplicação que está em produção?

Embora você certamente possa testar uma aplicação que já está implantada,
esse não é realmente o **ponto ideal** do Cypress.

O Cypress é construído e otimizado para ser uma ferramenta para o seu desenvolvimento local diário.
Na verdade, depois de começar a usar o Cypress por um tempo, acreditamos que você pode achar útil 
até mesmo fazer **todo o seu desenvolvimento** nele.

No final das contas, você não só poderá **testar e desenvolver** ao mesmo tempo, mas também será capaz de construir
sua aplicação web **rápida** enquanto obtém testes “gratuitos”.

Além do mais - como o Cypress permite que você faça coisas como **stub de solicitações de rede**,
você pode construir sua aplicação web sem precisar de um servidor para fornecer respostas JSON válidas.

Por último, mas não menos importante - tentar encaixar testes em uma aplicação já construída é muito mais difícil
do que construí-la enquanto você escreve os testes. Você provavelmente encontrará uma série de 
desafios / obstáculos iniciais que, de outra forma, teriam sido evitados ao escrever testes desde o início.

Por fim, e provavelmente o mais importante motivo pelo qual você deseja testar em servidores locais,
é a capacidade de **controlá-los**. Quando sua aplicação está sendo executada em produção, você não pode controlá-la. 

Quando está em desenvolvimento, você pode:

- Pegar atalhos
- Pré-popular dados executando scripts
- Expor rotas específicas do ambiente de teste
- Desativar recursos de segurança que tornam a automação difícil
- Redefinir estado no servidor / banco de dados 

Com isso dito - você ainda tem a opção de ter os **dois caminhos**. 

Muitos de nossos usuários executam a maioria de seus testes de integração em um servidor de desenvolvimento local,
mas reservam um conjunto menor de **testes de fumaça** que são executados apenas em uma aplicação de produção implantada.

## Passo 2: Acesse seu servidor

Assim que seu servidor estiver funcionando, é hora de acessá-lo.

Vamos deletar o arquivo `sample_spec.js` criado no tutorial anterior que agora não é mais necessário. 

```bash
rm cypress/integration/sample_spec.js
```

Agora vamos criar nosso próprio arquivo de especificações chamado `home_page_spec.js`.

```bash
touch cypress/integration/home_page_spec.js
```

Assim que o arquivo for criado, você deve vê-lo na lista de arquivos de teste.

![home page spec](https://docs.cypress.io/img/guides/testing-your-app-home-page-spec.049e17f2.png)

Agora você precisará adicionar o seguinte código em seu arquivo de teste para acessar seu servidor: 

```javascript
describe('The Home Page', () => {
  it('successfully loads', () => {
    cy.visit('http://localhost:8080') // change URL to match your dev URL
  })
})
```

Agora clique no arquivo `home_page_spec.js` e observe o Cypress abrir seu navegador.

Se você se esqueceu de iniciar o servidor, verá o erro abaixo: 

![visit fail](https://docs.cypress.io/img/guides/testing-your-app-visit-fail.10d4b941.png)

Se você iniciou seu servidor, deverá ver sua aplicação carregada e funcionando. 

## Passo 3: Configure Cypress

[//]: <> (TODO - Adicionar link opção de configuração)
Se você pensar no futuro, perceberá rapidamente que você digitará muito essa URL, 
já que todo teste precisará visitar alguma página da sua aplicação. 
Felizmente, o Cypress fornece uma 
[opção de configuração](https://docs.cypress.io/guides/references/configuration.html) para isso. 
Vamos aproveitar isso agora.

Abra seu [arquivo de configuração](https://docs.cypress.io/guides/references/configuration.html) 
(`cypress.json` no seu diretório do projeto, por padrão) ele começa vazio:

```javascript
{}
```

Vamos adicionar a opção `baseUrl`.

```javascript
{
  "baseUrl": "http://localhost:8080"
}
```

[//]: <> (TODO - Adicionar links dos comandos)
Isso irá prefixar automaticamente os comandos [cy.visit()](https://docs.cypress.io/api/commands/visit.html) 
e [cy.request()](https://docs.cypress.io/api/commands/request.html) com esta baseUrl. 

> Sempre que você modificar seu arquivo de configuração, o Cypress irá reiniciar-se automaticamente e 
encerrar todos os navegadores abertos. Isto é normal. Clique no arquivo de testes novamente para 
reiniciar o navegador.

Nós agora podemos visitar o caminho relativo, e omitir o nome do host e a porta.

```javascript
describe('The Home Page', () => {
  it('successfully loads', () => {
    cy.visit('/')
  })
})
```

Excelente! Tudo ainda deve estar verde.

> Opções de Configuração

[//]: <> (TODO - Adicionar link configuração)
O Cypress tem muito mais opções de configuração que você pode usar para personalizar seu comportamento. 
Coisas como onde seus testes residem, períodos de tempo limite padrão, variáveis de ambiente, qual relatório usar, etc.
Verifique-os em [Configuração](https://docs.cypress.io/guides/references/configuration.html)!

## Estratégias de Teste

Você está prestes a embarcar na escrita de testes para sua aplicação e somente você conhece sua aplicação, 
então não temos muitos conselhos específicos para lhe dar.

**O que testar, onde estão os casos extremos e as integrações, quais regressões você provavelmente encontrará, etc.,** 
**depende inteiramente de você, da sua aplicação e de sua equipe.**

Dito isso, o teste da web moderna tem algumas dificuldades que toda equipe experimenta, então aqui estão 
algumas dicas rápidas sobre situações comuns que você provavelmente encontrará.

## Inserindo dados

Dependendo de como sua aplicação é construída - é provável que sua aplicação web seja afetada e controlada pelo servidor.

Normalmente, nos dias de hoje, os servidores se comunicam com aplicações front-end via JSON, 
mas você também pode estar executando uma aplicação web HTML tradicional renderizada no lado do servidor.

Geralmente, o servidor é responsável por enviar respostas que refletem algum tipo de **estado** que ele mantém - 
geralmente em um banco de dados.

Tradicionalmente, ao escrever testes `e2e` usando Selenium, antes de automatizar o navegador você faz algum 
tipo de **configuração e destruição** no servidor.

Talvez você precise gerar um usuário, e inseri-lo com associações e registros. 
Você pode estar familiarizado com o uso de coisas como *fixtures* ou *factories*.

Para testar vários estados de página - como uma visualização vazia ou uma visualização de paginação, 
você precisa propagar o servidor para que esse estado possa ser testado.

**Embora haja muito mais nessa estratégia, você geralmente tem três maneiras de facilitar isso com o Cypress:**

[//]: <> (TODO - Adicionar links para os comandos abaixo)

- [cy.exec()](https://docs.cypress.io/api/commands/exec.html) - para executar comandos do sistema
- [cy.task()](https://docs.cypress.io/api/commands/task.html) - para executar o código no Node através do 
[pluginsFile](https://docs.cypress.io/guides/references/configuration.html#Folders-Files)
- [cy.request()](https://docs.cypress.io/api/commands/request.html) - para fazer requisições HTTP

Se você estiver executando o `node.js` em seu servidor, pode adicionar uma função 
`before` ou` beforeEach` que executa uma tarefa `npm`.

```javascript
describe('The Home Page', () => {
  beforeEach(() => {
    // reset and seed the database prior to every test
    cy.exec('npm run db:reset && npm run db:seed')
  })

  it('successfully loads', () => {
    cy.visit('/')
  })
})
```

Em vez de apenas executar um comando do sistema, você pode desejar mais flexibilidade e expor uma série de rotas 
apenas ao executar em um ambiente de teste.

**Por exemplo, você pode compor várias solicitações juntas para informar ao servidor exatamente o estado que deseja criar.**

```javascript
describe('The Home Page', () => {
  beforeEach(() => {
    // reset and seed the database prior to every test
    cy.exec('npm run db:reset && npm run db:seed')

    // seed a post in the DB that we control from our tests
    cy.request('POST', '/test/seed/post', {
      title: 'First Post',
      authorId: 1,
      body: '...'
    })

    // seed a user in the DB that we can control from our tests
    cy.request('POST', '/test/seed/user', { name: 'Jane' }).its('body').as('currentUser')
  })

  it('successfully loads', () => {
    // this.currentUser will now point to the response
    // body of the cy.request() that we could use
    // to log in or work with in some way

    cy.visit('/')
  })
})
```

Embora não haja nada realmente errado com essa abordagem, ela adiciona muita complexidade. 
Você lutará para sincronizar o estado entre o servidor e o navegador - e sempre precisará 
configurar / desativar esse estado antes dos testes (que é lento).

A boa notícia é que não somos Selenium, nem uma ferramenta de teste tradicional e2e. 
Isso significa que não estamos sujeitos às mesmas restrições.

**Com o Cypress, existem várias outras abordagens que podem oferecer uma experiência indiscutivelmente melhor e mais rápida.**

## Fazendo stub no servidor

Outra abordagem válida que se opõe à propagação e comunicação com o servidor é ignorá-la completamente.

[//]: <> (TODO - Adicionar link cy.visit)
Embora você ainda receba todos os recursos HTML / JS / CSS regulares de seu servidor e continue a utilizar 
[cy.visit()](https://docs.cypress.io/api/commands/visit.html) da mesma maneira - você pode 
copiar as respostas JSON provenientes dele.

Isso significa que em vez de redefinir o banco de dados ou propagá-lo com o estado desejado, você pode forçar 
o servidor a responder com o que você quiser. Dessa forma, não apenas evitamos a necessidade de sincronizar 
o estado entre o servidor e o navegador, mas também evitamos a mutação do estado de nossos testes. 
Isso significa que os testes não criarão um estado que possa afetar outros testes.

Outra vantagem é que isso permite que você construa sua aplicação sem a necessidade do contrato 
do servidor para existir. Você pode construí-la da maneira que deseja que os dados sejam estruturados 
e até mesmo testar todos os casos extremos, sem precisar de um servidor.

No entanto - provavelmente ainda há um equilíbrio aqui onde ambas as estratégias são válidas 
(e você provavelmente deve fazê-las).

Embora o stub seja ótimo, isso significa que você não tem as garantias de que essas cargas de resposta 
realmente correspondam ao que o servidor enviará. No entanto, ainda existem muitas maneiras válidas 
de contornar isso:

### Gere os fixture stubs com antecedência

Você pode fazer com que o servidor gere todos os fixture stubs para você com antecedência. 
Isso significa que seus dados refletirão o que o servidor realmente enviará.

### Escreva um único teste e2e sem stubs e, em seguida use stub para o restante

Outra abordagem mais equilibrada é integrar as duas estratégias. Provavelmente, você deseja ter um 
único teste que adote uma abordagem `e2e` verdadeira e não faça stubs. Ele usará o recurso para 
valer - incluindo a propagação do banco de dados e configuração do estado.

Depois de estabelecer que está funcionando, você pode usar stubs para testar todos os casos extremos 
e cenários adicionais. Não há benefícios em usar dados reais na grande maioria dos casos. 
Recomendamos que a grande maioria dos testes use dados de stub. Eles serão ordens de magnitude 
mais rápidos e muito menos complexos.

> Guia: Requisições de Rede 

[//]: <> (TODO - Adicionar link guia sobre solicitações de rede)
> Por favor, leia nosso [Guia sobre Solicitações de Rede](https://docs.cypress.io/guides/guides/network-requests.html) 
para uma análise e abordagem muito mais completas para fazer isso.

## Logando

Um dos primeiros (e indiscutivelmente um dos mais difíceis) obstáculos que você terá que superar é testar 
o login em sua aplicação.

Nada retarda uma suíte de teste como ter que fazer login, mas todas as partes boas da sua aplicação 
provavelmente requerem um usuário autenticado! Aqui estão algumas dicas.

## Teste totalmente o fluxo de login - mas apenas uma vez

É uma ótima ideia ter seu fluxo de inscrição e login sob cobertura de teste, pois é muito importante 
para todos os seus usuários e você nunca quer quebrar.

O login é um dos recursos de missão crítica e provavelmente deve envolver o servidor. 
Recomendamos que você teste a inscrição e o login usando sua interface do usuário (UI) como um usuário real faria:

Aqui está um exemplo ao lado da propagação de seu banco de dados:

```javascript
describe('The Login Page', () => {
  beforeEach(() => {
    // reset and seed the database prior to every test
    cy.exec('npm run db:reset && npm run db:seed')

    // seed a user in the DB that we can control from our tests
    // assuming it generates a random password for us
    cy.request('POST', '/test/seed/user', { username: 'jane.lane' })
      .its('body')
      .as('currentUser')
  })

  it('sets auth cookie when logging in via form submission', function () {
    // destructuring assignment of the this.currentUser object
    const { username, password } = this.currentUser

    cy.visit('/login')

    cy.get('input[name=username]').type(username)

    // {enter} causes the form to submit
    cy.get('input[name=password]').type(`${password}{enter}`)

    // we should be redirected to /dashboard
    cy.url().should('include', '/dashboard')

    // our auth cookie should be present
    cy.getCookie('your-session-cookie').should('exist')

    // UI should reflect this user being logged in
    cy.get('h1').should('contain', 'jane.lane')
  })
})
```

Provavelmente, você também desejará testar sua UI de login para:

- Nome de usuário / senha inválidos
- Nome de usuário já utilizado
- Requisitos de complexidade de senha
- Casos extremos, como contas bloqueadas / excluídas

Cada um deles provavelmente requer um teste e2e completo.

Agora, depois de ter seu login completamente testado - você pode ficar tentado a pensar:

> "…certo, ótimo! Vamos repetir este processo de login antes de cada teste!”

> **Não! Por favor, não.**

> Não use **sua UI** para fazer login antes de cada teste.

Vamos investigar e desvendar o porquê.

### Ignorando sua UI

Ao escrever testes para um recurso muito específico, você deve usar sua UI para testá-lo.

Mas quando você estiver testando outra área do sistema que depende de um estado de um recurso anterior: 
**não use sua UI para configurar esse estado.**

Aqui está um exemplo mais robusto:

Imagine que você esteja testando a funcionalidade de um **carrinho de compras**. Para testar isso, 
você precisa da capacidade de adicionar produtos a esse carrinho. Bem, de onde vêm os produtos? 
Você deve usar sua UI para fazer login na área de administração e, em seguida, criar todos os produtos, 
incluindo suas descrições, categorias e imagens? Depois de fazer isso, você deve visitar cada produto e 
adicionar cada um ao carrinho de compras?

Não. Você não deveria fazer isso.

> Antipadrão

> Não use sua UI para criar um estado! É extremamente lento, complicado e desnecessário.

[//]: <> (TODO - Adicionar link boas práticas)
> Leia sobre as [boas práticas](https://docs.cypress.io/guides/references/best-practices.html) aqui.

Usar sua UI para fazer login é exatamente o mesmo cenário que descrevemos anteriormente. 
O login é um pré-requisito de estado que vem antes de todos os outros testes.

[//]: <> (TODO - Adicionar link cy.request) 
Como o Cypress não é Selenium, podemos realmente pegar um grande atalho aqui e pular a necessidade 
de usar nossa UI usando [cy.request()](https://docs.cypress.io/api/commands/request.html).

[//]: <> (TODO - Adicionar link cy.request)
Porque [cy.request()](https://docs.cypress.io/api/commands/request.html) obtém e define cookies automaticamente 
nos bastidores, podemos realmente usá-lo para construir o estado sem usar a UI do seu navegador, mas ainda assim 
ter um desempenho exatamente como se viesse do navegador!

Vamos revisitar o exemplo acima, mas suponha que estamos testando alguma outra parte do sistema.

```javascript
describe('The Dashboard Page', () => {
  beforeEach(() => {
    // reset and seed the database prior to every test
    cy.exec('npm run db:reset && npm run db:seed')

    // seed a user in the DB that we can control from our tests
    // assuming it generates a random password for us
    cy.request('POST', '/test/seed/user', { username: 'jane.lane' })
      .its('body')
      .as('currentUser')
  })

  it('logs in programmatically without using the UI', function () {
    // destructuring assignment of the this.currentUser object
    const { username, password } = this.currentUser

    // programmatically log us in without needing the UI
    cy.request('POST', '/login', {
      username,
      password
    })

    // now that we're logged in, we can visit
    // any kind of restricted route!
    cy.visit('/dashboard')

    // our auth cookie should be present
    cy.getCookie('your-session-cookie').should('exist')

    // UI should reflect this user being logged in
    cy.get('h1').should('contain', 'jane.lane')
  })
})
```

Você vê a diferença? Conseguimos fazer o login sem realmente precisar usar nossa UI. Isso economiza 
muito tempo visitando a página de login, preenchendo o nome de usuário, senha e esperando que o servidor 
nos redirecione antes de cada teste.

Como testamos anteriormente o sistema de login de ponta a ponta sem usar nenhum atalho, já temos 100% de 
confiança de que ele está funcionando corretamente.

Use a metodologia acima ao trabalhar com qualquer área do sistema que requeira que o estado seja 
configurado em outro lugar. Lembre-se: não use sua UI!

> Instruções de autenticação

> Fazer login pode ser mais complexo do que o que acabamos de ver.

> Criamos várias instruções que abrangem cenários adicionais, como lidar com tokens CSRF ou 
testar formulários de login baseados em XHR.

[//]: <> (TODO - Adicionar link recipes) 
> Sinta-se à vontade para 
[explorar estas instruções em logins adicionais](https://docs.cypress.io/examples/examples/recipes.html).

## Primeiros Passos

Ok, terminamos de conversar. Agora mergulhe e comece a testar sua aplicação!

A partir daqui, você pode querer explorar mais alguns de nossos guias:

[//]: <> (TODO - Adicionar links aqui abaixo)
 
- [Vídeos Tutoriais](https://docs.cypress.io/examples/examples/tutorials.html) 
para assistir a vídeos tutoriais passo a passo
- [API do Cypress](https://docs.cypress.io/api/api/table-of-contents.html) 
para aprender quais comandos estão disponíveis enquanto você trabalha
- [Introdução ao Cypress](https://docs.cypress.io/guides/core-concepts/introduction-to-cypress.html) 
explica como o Cypress realmente funciona
- [Linha de comando](https://docs.cypress.io/guides/guides/command-line.html) 
para executar todos os seus testes externos via cypress run
- [Integração contínua](https://docs.cypress.io/guides/guides/continuous-integration.html) 
para executar o Cypress em CI
- [Teste entre navegadores](https://docs.cypress.io/guides/guides/cross-browser-testing.html) 
para a execução ideal de testes em CI nos navegadores da família Chrome e Firefox
- [Cypress Real World App(RWA)](https://github.com/cypress-io/cypress-realworld-app) 
para demonstrações de práticas de teste no Cypress, configuração e estratégias em um projeto do mundo real.

[Voltar para o topo](#testando-sua-aplicação)
