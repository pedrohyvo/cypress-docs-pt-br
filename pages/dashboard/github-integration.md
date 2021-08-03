# integração do GitHub

O [Cypress Dashboard](https://on.cypress.io/dashboard) consegue integrar seus testes Cypress
 com seu fluxo de trabalho do GitHub através de  [verificações de status](#verificacoes-de-status)
 e comentários de [pull request](#comentarios-de-pull-request).

![pull-request-cypress-integration-comments-github-checks](https://docs.cypress.io/_nuxt/img/pull-request-cypress-integration-comments-github-checks.8fd68f7.jpg)

>A plataforma local GitHub Enterprise atualmente não é suportada.

>A Integração do GitHub depende do seu ambiente de CI, fornecendo dados de commit SHA
(normalmente por meio de uma variável de ambiente).
>Isso não é um problema para a maioria dos usuários, mas se você estiver enfrentando problemas de integração do GitHub
>com sua configuração de CI, por favor, certifique-se que as informações do git sejam enviadas corretanmente
>seguindo essas diretrizes. Se após isso você ainda estiver enfrentando problemas, por favor, entre em contato conosco.
