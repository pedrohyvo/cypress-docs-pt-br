# Cypress Studio

```markdown
O que você vai aprender

- Como estender testes interativamente usando o Cypress Studio
- Como adicionar novos testes interativamente usando o Cypress Studio
```

## Overview

Cypress Studio fornece uma maneira visual de gerar testes dentro do Test Runner, gravando interações com o aplicação em teste.

Os comandos [`.click()`](https://docs.cypress.io/api/commands/click.html), [`.type()`](https://docs.cypress.io/api/commands/type.html), [`.check()`](https://docs.cypress.io/api/commands/check.html), [`.uncheck()`](https://docs.cypress.io/api/commands/uncheck.html), and [`.select()`](https://docs.cypress.io/api/commands/select.html) Cypress são suportados e irão gerar código de teste quando interagir com o DOM dentro do Cypress Studio. Você também pode gerar asserções clicando com o botão direito do mouse em um elemento que você gostaria de afirmar.

## Usando Cypress Studio

```markdown
Cypress Studio é uma funcionalidade experimental e pode ser habilitada adicionando o atributo
[`experimentalStudio`](https://docs.cypress.io/guides/references/experiments) no seu arquivo de configuração (`cypress.json` por padrão)
```

```HTML
{
  "experimentalStudio": true
}
```

O Cypress [`Real World App (RWA)`](https://github.com/cypress-io/cypress-realworld-app) é um projeto de código aberto que implementa um aplicativo de pagamento para demonstrar o uso real dos métodos de teste, padrões e fluxos de trabalho do Cypress. Ele será usado para demonstrar a funcionalidade do Cypress Studio abaixo.

## Estendendo um Teste

Voce pode estender qualquer teste preexistente ou começar criando um novo teste no seu [`integrationFolder`](https://docs.cypress.io/guides/references/configuration#Folders-Files) (`cypress/integration` por padrão) com o seguinte test scaffolding.

```js
// Código do Real World App (RWA)
describe("Cypress Studio Demo", () => {
  beforeEach(() => {
    // Banco de dados seed com dados de teste
    cy.task("db:seed");

    // Teste de Login com usuário
    cy.database("find", "users").then((user) => {
      cy.login(user.username, "s3cret", true);
    });
  });

  it("create new transaction", () => {
    // Teste estendido com Cypress Studio
  });
});
```

```markdown
Clone o [`Real World App (RWA)`](https://github.com/cypress-io/cypress-realworld-app) and referencie ao arquivo [`cypress/tests/demo/cypress-studio.spec.ts`](https://github.com/cypress-io/cypress-realworld-app/blob/develop/cypress/tests/demo/cypress-studio.spec.ts).
```

## Passo 1 - Rodando a especificação

Usaremos o Cypress Studio para realizar uma jornada de usuário de "Nova Transação". Primeiro, inicie o Test Runner e execute a especificação criada na etapa anterior.

![run-spec]<https://docs.cypress.io/_nuxt/img/run-spec-1.5f7b919.png>

## Passo 2 - Inicie o Cypress Studio

Assim que os testes conluírem sua execução, passe o mouse sobre o teste no Log de Comandos para revelar um botão "Adicionar Comandos ao Teste"

Clicando em "Adicionar Comandos ao Teste" iniciará o Cypress Studio

```markdown
Cypress Studio é diretamente integrado com o [`Log de Comando`](https://docs.cypress.io/guides/core-concepts/test-runner#Command-Log)
```

![extend-activate-studio]<https://docs.cypress.io/_nuxt/img/extend-activate-studio.91d9bd8.png>

> Cypress irá automaticamente executar todos os hooks e atualmente
> apresenta o código de teste e, em seguida, o teste pode ser estendido a
> partir desse ponto (ex: Estamos logados na aplicação dentro do bloco
> `beforeEach`)

Em seguida, o Test Runner executará o teste isoladamente e fará uma pausa após o último commando do teste.

![extend-ready]<https://docs.cypress.io/_nuxt/img/extend-ready.ccc2164.png>

Agora, nós podemos começar atualizando o teste e criando uma nova transação entre os usuários.
