# integração do GitHub

O [Cypress Dashboard](https://on.cypress.io/dashboard) consegue integrar seus testes Cypress
 com seu Workflow do GitHub através de  [verificações de status](#verificacoes-de-status)
 e comentários de [pull request](#comentarios-de-pull-request).

![pull-request-cypress-integration-comments-github-checks](https://docs.cypress.io/_nuxt/img/pull-request-cypress-integration-comments-github-checks.8fd68f7.jpg)

>A plataforma local GitHub Enterprise atualmente não é suportada.

>A Integração com o GitHub depende do seu ambiente de CI, fornecendo dados de commit SHA
(normalmente por meio de uma variável de ambiente).
>Isso não é um problema para a maioria dos usuários, mas se você estiver enfrentando problemas de integração com o GitHub
>com sua configuração de CI, por favor, certifique-se que as informações do git sejam enviadas corretanmente
>seguindo essas diretrizes. Se após isso você ainda estiver enfrentando problemas, por favor, entre em contato conosco.

## Instale o app Cypress GitHub

Antes de você estar apto a habilitar a integração com o GitHub para seus projetos Cypress, você primeiro deve instalar
o app Cypress GitHub. Você pode iniciar o processo de instalação do aplicativo através da página de configuração
da sua organização, ou a partir de uma página de configuração de um projeto na [Dashboard Cypress](https://on.cypress.io/dashboard).

### Instalar via configurações de integração da organização

1. Vá para a página de Dashboard da organização, ou abra o seletor de organizações.
1. Selecione a organização que você deseja integrarcom uma conta do GitHub ou organização do GitHub.
    ![Docs Cypress](https://docs.cypress.io/_nuxt/img/select-cypress-organization.41ec503.png)
1. Visite a página de **integração** da organização selecionada na sidebar.
    ![Docs Cypress](https://docs.cypress.io/_nuxt/img/navigate-to-organization-integrations.0c43d75.png)
1. CLique no botão **Instalar Integração com o GitHub**.

### Instalar via configurações do projeto

1. Selecione sua organização no seletor.
    ![Docs Cypress](https://docs.cypress.io/_nuxt/img/select-cypress-organization.41ec503.png)
1. Selecione o projeto que você deseja integrar com um repositório do GitHub.
    [Docs Cypress](https://docs.cypress.io/_nuxt/img/select-cypress-project.fe3b44b.png)
1. Vá para a página de configurações do projeto.
    [Docs Cypress](https://docs.cypress.io/_nuxt/img/visit-project-settings.43a21a4.png)
1. Role até a seção de **Integração com o GitHub**.
1. Clique no botão **Instalar Cypress GitHub App**.

### Processo de instalação do App Cypress GitHub

Assim que você iniciar o processo de instalação do app do GitHub por meio de 
 página de configurações Cypress da organização
ou uma página de configurações do projeto, você será redirecionado(a) ao GitHub.com para completar a instalação:

1. Selecione a conta ou Organização GitHub que você deseja integrar com sea organização Cypress Dashboard.
    ![Docs Cypress](https://docs.cypress.io/_nuxt/img/select-gh-org.c083d7b.jpg)
2. Escolha associar **todos os repositórios** ou apenas selecionar repositórios do GitHub
com a instalação do seu app Cypress GitHub.
    ![Docs Cypress](https://docs.cypress.io/_nuxt/img/select-all-gh-repos.d0b1835.jpg)
    > Todos os Atuais e futuros repositórios serão inclusos com esta instalação, caso selecione **Todos os repositórios**.
3. a
