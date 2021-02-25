# Introdução

O [Cypress Dashboard](https://on.cypress.io/dashboard) é um serviço que permite que você tenha acesso a 
testes gravados - normalmente ao executar os testes do Cypress de seu provedor de integração contínua (CI).

[![Visão Geral do Cypress Dashboard.](https://img.youtube.com/vi/ezp60FUnjGg/0.jpg)](https://youtu.be/ezp60FUnjGg)

>  Real World Example

> O [Cypress Real World App (RWA)](https://github.com/cypress-io/cypress-realworld-app) potencializa o 
[Cypress Dashboard na CI](https://dashboard.cypress.io/projects/7s5okt) para testar mais de 300 casos de teste
em paralelo em 25 máquinas, vários navegadores, vários tamanhos de dispositivos e vários sistemas operacionais.

> Verifique o [Real World App Dashboard](https://dashboard.cypress.io/projects/7s5okt).

## Funcionalidades

### Organizar projetos

No dashboard você pode:

- Configurar um projeto para registrar no dashboard
- Redefinir ou adicionar mais chaves de registro
- Alterar quem pode acessar seu projeto Cypress
- Transferir propriedade de projetos
- Deletar projetos

### Ver os resultados do teste

No dashboard você pode:

[//]: <> (TODO - Adicionar link cy.request)

- Ver o número de testes reprovados, aprovados, pendentes e ignorados.
- Obter todo o rastreamento da pilha de testes com falha.
- Visualizar as capturas de tela feitas quando os testes falham ou 
ao usar [cy.screenshot()](https://docs.cypress.io/api/commands/screenshot.html).
- Assistir a um vídeo de todo teste executado ou um videoclipe no ponto de falha do teste.
- Ver a rapidez com que seus arquivos de especificação são executados no CI, incluindo se eles foram executados em paralelo.
- Ver agrupamentos de testes relacionados. 

![Latest runs](https://docs.cypress.io/img/dashboard/dashboard-runs-list.38bf0e41.png)

### Gerenciar execuções

No dashboard você pode:

- Cancelar execuções em andamento.
- Arquivar execuções canceladas ou com erro.

### Gerenciar organizações

No dashboard você pode:

- Criar, editar e deletar organizações.
- Ver os detalhes de uso de cada organização.
- Pagar o plano de pagamento selecionado.

### Gerenciar usuários

No dashboard você pode:

- Convidar e editar as funções do usuário para organizações.
- Aceitar ou rejeitar pedidos de adesão à sua organização. 

### Integrar com Github

No dashboard você pode:

[//]: <> (TODO - Adicionar links verificação de status e pull requests)

- Integrar seus testes Cypress com seu fluxo de trabalho GitHub por meio de 
[verificação de status](https://docs.cypress.io/guides/dashboard/github-integration.html#Status-checks) 
do commit.
- Integrar o Cypress no GitHub por meio de 
[pull requests](https://docs.cypress.io/guides/dashboard/github-integration.html#Pull-request-comments).

### Integrar com Slack

No dashboard você pode:

- Integrar o Cypress no Slack em cada execução de teste salvo.

### **Veja os testes executados no Test Runner**

[//]: <> (TODO - Adicionar link Test Runner)
Além disso integramos os testes executados no Cypress 
[Test Runner](https://docs.cypress.io/guides/core-concepts/test-runner.html). 
Isso significa que você pode ver os testes executados na guia *Execuções* de cada projeto.

![Test runs](https://docs.cypress.io/img/dashboard/runs-list-in-desktop-gui.c0a8a677.png)

> **Tem uma pergunta que não está respondida aqui?**

> [Respondemos a algumas perguntas comuns sobre o serviço do Dashboard em nosso FAQ.](https://docs.cypress.io/faq/questions/dashboard-faq.html)

## Projetos exemplo

[//]: <> (TODO - Adicionar links Dashboard e projeto público)
Depois de fazer login no [serviço Dashboard](https://on.cypress.io/dashboard), 
você pode visualizar qualquer 
[projeto público](https://docs.cypress.io/guides/dashboard/projects.html#Public-vs-Private).

Aqui estão alguns de nossos próprios projetos públicos que você pode ver:

- [cypress-realworld-app](https://dashboard.cypress.io/projects/7s5okt)
- [cypress-example-recipes](https://dashboard.cypress.io/#/projects/6p53jw)
- [cypress-example-kitchensink](https://dashboard.cypress.io/#/projects/4b7344)
- [cypress-example-todomvc](https://dashboard.cypress.io/#/projects/245obj)
- [cypress-example-piechopper](https://dashboard.cypress.io/#/projects/fuduzp)

[Voltar para o topo](#introdução)
