# Integração com o GitHub

O [Cypress Dashboard](https://on.cypress.io/dashboard) consegue integrar seus testes Cypress
com seu Workflow do GitHub através de [verificações de status](#verificacoes-de-status)
e comentários de [pull requests](#comentarios-de-pull-requests). 

Um projeto primeiramente precisa estar [configurado para gravação](https://docs.cypress.io/guides/dashboard/projects)
para que o Cypress Dashboard possa utilizar a integração com o GitHub.

![pull-request-cypress-integration-comments-github-checks](https://docs.cypress.io/_nuxt/img/pull-request-cypress-integration-comments-github-checks.8fd68f7.jpg)

>A plataforma local GitHub Enterprise atualmente não é suportada.

>A Integração com o GitHub depende que o seu ambiente de integração contínua forneça dados SHA de commit
(tipicamente via variável de ambiente). Isso não é um problema para a maioria dos usuários,
mas se você estiver com problemas de integração com o GitHub com sua configuração de integração contínua,
por favor, certifique-se que a informação git está sendo enviada corretamente
seguindo [essas diretrizes](https://docs.cypress.io/guides/continuous-integration/introduction#Git-information).
Se após isso você ainda estiver enfrentando problemas, por favor, entre em [contato conosco](mailto:hello@cypress.io).

## Instalando o app Cypress GitHub

Antes que você possa habilitar a integração com o GitHub em seus projetos Cypress, você primeiro deve instalar
o app Cypress GitHub. Você pode iniciar o processo de instalação do aplicativo através da página de configuração
da sua organização, ou a partir de uma página de configuração de um projeto no [Cypress Dashboard](https://on.cypress.io/dashboard).

### Instalando via configurações de integração da organização

1. Vá para a [Página de organizações](https://dashboard.cypress.io/organizations) no dashboard, ou abra a lista de organizações.
2. Selecione a organização que você deseja integrar com uma conta do GitHub ou organização do GitHub.
![Docs Cypress](https://docs.cypress.io/_nuxt/img/select-cypress-organization.41ec503.png)
3. Visite a página de **integrações** da organização selecionada, na navegação lateral.
![Docs Cypress](https://docs.cypress.io/_nuxt/img/navigate-to-organization-integrations.0c43d75.png)
4. CLique no botão **Instalar Integração com o GitHub**.

### Instalando via configurações do projeto

1. Selecione sua organização na lista de organizações.
![Docs Cypress](https://docs.cypress.io/_nuxt/img/select-cypress-organization.41ec503.png)
2. Selecione o projeto que você deseja integrar com um repositório GitHub.
[Docs Cypress](https://docs.cypress.io/_nuxt/img/select-cypress-project.fe3b44b.png)
3. Vá para a página de configurações do projeto.
[Docs Cypress](https://docs.cypress.io/_nuxt/img/visit-project-settings.43a21a4.png)
4. Role até a seção **Integração com o GitHub**.
5. Clique no botão **Instalar Cypress GitHub App**.

### Processo de instalação do aplicativo Cypress GitHub

Assim que você iniciar o processo de instalação do app do GitHub
[por meio de  uma página de configurações Cypress da organização](#instalando-via-cypress),
 ou uma [página de configurações do projeto](#instalando-via-projeto),
você será redirecionado(a) ao GitHub.com para completar a instalação:

1. Selecione a conta ou organização GitHub que você deseja integrar com seu Cypress Dashboard da organização.
![Docs Cypress](https://docs.cypress.io/_nuxt/img/select-gh-org.c083d7b.jpg)
2. Escolha associar **todos os repositórios** ou apenas selecione repositórios GitHub
com a instalação do seu app Cypress GitHub.
![Docs Cypress](https://docs.cypress.io/_nuxt/img/select-all-gh-repos.d0b1835.jpg)

> Todos os atuais e *futuros* repositórios serão inclusos com esta instalação, caso selecionar **Todos os repositórios**.

![Docs Cypress](https://docs.cypress.io/_nuxt/img/select-gh-repos.3c71946.jpg)

3. Clique no botão **instalar** para completar a instalação.

## Habilitando a integração com o GitHub para um projeto

Após concluir a instalação do app Cypress Github para a sua organização, você agora pode habilitar
a Integração com o Github para *qualquer* projeto Cypress.

1. Vá para a página de configurações do projeto.
![Docs Cypress](https://docs.cypress.io/_nuxt/img/visit-project-settings.43a21a4.png)
2. role até a seção integração com o GitHub.
Você pode rapidamente acessar as configurações de Integração do GitHub de um projeto, clicando no link **configurar**
do projeto desejado na página de Integrações de uma organização:
![Docs Cypress](https://docs.cypress.io/_nuxt/img/org-settings-with-no-enabled-projects.bdbf46e.png)
3. Selecione um repositório GitHub a ser associado com o projeto.
![Docs Cypress](https://docs.cypress.io/_nuxt/img/project-settings-repo-selection.449e45a.png)

Assim que um repositório GitHub for associado a um projeto Cypress,
a integração com o GitHub será habilitada imediatamente:

![Docs Cypress](https://docs.cypress.io/_nuxt/img/project-settings-selected-repo.9fc3a58.png)

Você também pode ver todas as Integrações com o Github habilitadas em projetos Cypress
na sua página de **integrações** das suas organizações:

![Docs Cypress](https://docs.cypress.io/_nuxt/img/org-settings-with-projects.57df6b0.png)

## Verificações de status

Se as verificações de status estão habilitadas nas configurações de integração com o Github de um projeto,
O Cypress Dashboard irá reportar o status de teste Cypress ao GitHub para commits relacionados.

[Verificações de status](https://help.github.com/en/articles/about-status-checks)
ajudam a previnir a fusão de um commit ou pull-request no resto da sua base de código até que todos os seus testes Cypress
tenham passado.

O app Cypress GitHub reporta as verificações de status de commit em dois estilos separados:

- Uma verificação por [grupo de execução](#agrupando-execucoes-de-teste).
![Docs Cypress](https://docs.cypress.io/_nuxt/img/status-checks-per-group-failed.d1bc049.png)
- Ou uma por arquivo spec.
![Docs Cypress](https://docs.cypress.io/_nuxt/img/status-checks-per-spec.338a4ba.png)

Cada verificação de status irá reportar o número de falhas ou aprovações no teste, e o link de **detalhes** associados
irá lhe direcionar à página de execução de testes no Cypress Dashboard para lhe ajudar a ir mais fundo
no problema via mensagens de erro, stack traces, screenshots, e gravações de vídeo:

![Docs Cypress](https://docs.cypress.io/_nuxt/img/dashboard-fail-tab.bbc59ae.png)

### Desabilitando verificações de status

As verificações de status do GitHub são opcionais, e podem ser desabilitadas nas configurações de Integração com o Github:

![Docs Cypress](https://docs.cypress.io/_nuxt/img/status-check-settings.6d4b23e.png)

## Comentários de pull-requests

O app Cypress Github pode fornecer informações de teste detalhadas nos pull-requests por meio de comentários, que incluem:

- Estatísticas de execução, tais como: testes aprovados, reprovados, pulados e acima do limite.
- Detalhes do contexto de execução:
    - O projeto Cypress associado
    - Status final de execução (aprovado, reprovado, etc)
    - Commit SHA vinculado ao GitHub commit
    - A hora em que a execução começou e terminou, bem como a duração.
    - Sistema operacional e versão do navegador.
- Falhas de execução:
    - As 10 primeiras falhas são exibidas com um link para ver mais.
    - Cada teste reprovado é vinculado à falha associada no painel Cypress.
    - Miniaturas de capturas de tela também são fornecidas com cada falha para fornecer contexto de forma conveniente.

Um exemplo de um comentário de pull-request Cypress pode ser visto abaixo:

![Docs Cypress](https://docs.cypress.io/_nuxt/img/pr-comment-fail.047ae7e.jpg)

### Desabilitando comentários de pull-requests

Comentários de pull-requests e miniaturas de capturas de telas em falhas são opcionais, e podem ser desabilitados se não
forem necessários nas configurações de Integração com o GitHub de um projeto:

![Docs Cypress](https://docs.cypress.io/_nuxt/img/pr-comments-settings.0482e9b.png)

## Desinstalando o app Cypress GitHub

Você pode desinstalar o app Cypress GitHub ao seguir os seguintes passos:

1. Acesse as ***configurações** da sua organização no GitHub.
1. Clique nos **apps instalados no GitHub**.
1. Clique em **configurar** ao lado do aplicativo Cypress.
1. CLique em **desinstalar** abaixo da seção "desinstalar Cypress".

[Voltar para o topo](#integração-com-o-github)
