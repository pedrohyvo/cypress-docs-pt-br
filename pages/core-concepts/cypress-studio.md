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

# Step 3 - Interagindo com a Aplicação

Para registrar ações, começe a interagir com a aplicação. Aqui clicaremos no botão "New" à direita do cabeçalho e como resultado veremos nosso clique registrado no Log de Comandos.

![extend-click-new-transaction](https://docs.cypress.io/_nuxt/img/extend-click-new-transaction.8ea6668.png)

Em seguida, podemos começar a digitar o nome de um usuário que queremos pagar.

![extend-type-user-name](https://docs.cypress.io/_nuxt/img/extend-type-user-name.4ba1bd7.png)

Depois de vermos o nome aparecer nos resultados, queremos adicionar uma asserção para garantir que nossa função de pessquisa funcione corretamente. Clicando com o botão direito do mouse no nome do usuário abrirá um menu a partir do qual podemos adicionar uma asserção para verificar se o elemento contém o texto correto (o nome do usuário).

![extend-assert-user-name](https://docs.cypress.io/_nuxt/img/extend-assert-user-name.c215a18.png)

Podemos então clicar naquele usuário para avançar para a próxima tela. Completaremos o formulário de transação clicando e digitando o valor e a descrição.

![extend-type-transaction-form](https://docs.cypress.io/_nuxt/img/extend-type-transaction-form.51b7c1e.png)

> Observe os comandos gerados no Log de Comandos.

Agora é hora de completar a transação. Vode talvez tenha percebido que o botão "PAY" foi desabilitado antes de digitarmos nos campos. Para ter certeza que nossa validação do formulário funcione corretamente, vamos adicionar uma asserção para garantir que o botão "PAY" esteja habilitado.

![extend-assert-button-enabled](https://docs.cypress.io/_nuxt/img/extend-assert-button-enabled.ec54800.png)

Por fim, clicamos no botão "PAY" e veremos uma página de confirmação de nossa nova transação.

![extend-save-test](https://docs.cypress.io/_nuxt/img/extend-save-test.2007e93.png)

Para descartar as interações, clique no botão "Cancelar" para sair do Cypress Studio. Se estiver satisfeito com as interações com a aplicação, clique em "Save commands" e o código de teste será salvo em seu arquivo de especificações. Alternativamente, você pode escolher o botão "copy" para copiar os comandos gerados para sua área de transferência.

