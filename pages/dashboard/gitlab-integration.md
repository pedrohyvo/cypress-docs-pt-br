# Integração com o GitLab

[//]: <> (TODO - Adicionar links traduzidos Dashboard, status de commit, comentários de merge request e config para gravação)

O [Cypress Dashboard](https://on.cypress.io/dashboard) consegue integrar seus testes Cypress com o seu fluxo 
de trabalho do GitLab através do [status de commit](https://docs.cypress.io/guides/dashboard/gitlab-integration#Commit-statuses)
e de [comentários de merge request](https://docs.cypress.io/guides/dashboard/gitlab-integration#Merge-Request-comments).
Primeiramente o projeto precisa estar [configurado para gravação](https://docs.cypress.io/guides/dashboard/projects) 
para que o Cypress Dashboard utilize a integração GitLab.

> A Integração GitLab depende que o seu ambiente de Integração Contínua forneça dados SHA de 
commit de forma confiável (tipicamente via variável de ambiente). Isso não é um problema para a maioria dos usuários, 
mas se você estiver com problemas de integração GitLab na sua configuração de Integração Contínua, certifique-se de que
a informação git está sendo enviada corretamente ao seguir 
[essas diretrizes](https://docs.cypress.io/guides/continuous-integration/introduction#Git-information).
Se após isso você ainda estiver com problemas, por favor, entre em [contato conosco](mailto:hello@cypress.io).

## Instalando a integração com o GitLab

> Aplicações OAuth2 do GitLab permitem que o Cypress Dashboard autentique-se como o usuário que registrou a 
aplicação. Isso significa que o Cypress vai conseguir enxergar cada repositório GitLab que você puder acessar.
Se você quiser um controle mais rígido nos repositórios que o Cypress vai visualizar, considere criar uma conta 
de serviço com acessos mais limitados no GitLab.

1. Visite **Integrations -> GitLab** no Cypress Dashboard.
2. Siga as instruções para criar uma nova aplicação OAuth2 no GitLab. Acesse o 
[docs GitLab](https://docs.gitlab.com/ee/integration/oauth_provider.html#adding-an-application-through-the-profile) 
para mais detalhes.
3. Copie o Application ID e Secret e cole no Cypress Dashboard.
4. Conecte seus projetos em um repositório GitLab.
5. (Opicional) Configure o comportamento para cada projeto.

## Configurando a integração com o GitLab

### Status de commit

Por padrão, o Cypress postará um status de commit **cypress/run** contendo os resultados da execução do Cypress. 
Isso vai evitar que seu time faça merge de um merge request com testes do Cypress que falharam.

Adicionalmente, o Cypress pode postar um status de commit **cypress/flake** indicando se o Cypress run continha algum 
teste inconsistente. Isso vai evitar que seu time faça merge de qualquer merge request com testes inconsistentes.

Você pode gerenciar esse comportamento na página **Configurações de Projeto** do seu projeto.

## Comentários de Merge Request

Por padrão, o Cypress irá postar um comentário de Merge Request resumindo a execução quando ela terminar. Isso
inclui contagem de testes, informações da execução e um resumo de testes que falharam ou foram inconsistentes.

Você pode gerenciar esse comportamento na página **Configurações de Projeto** do seu projeto.

## Desinstalando a integração com o Gitlab

Você pode remover essa integração visitando a página **Integrations -> GitLab** da sua organização. Isso irá interromper
todos os checks de commit e comentário de Merge Request do Cypress.

## Feedback

Tem ideias sobre como podemos melhorar nossa Integração com o GitLab? [Conte-nos!](https://portal.productboard.com/cypress-io/1-cypress-dashboard/c/48-gitlab-integration?utm_medium=social&utm_source=portal_share)
