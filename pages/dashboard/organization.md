# Organização 

As organizações são usadas para agrupar projetos e gerenciar o acesso a esses projetos.
(https://docs.cypress.io/_nuxt/img/organizations-listed-in-dashboard.eaf1de6.png).

# Funcionalidades

 * Criar projetos
 * Convidar usuários 
 * Transferir projetos
 * Pague pelo uso de todos os seus projetos

 # ID da Organização

 O ID de uma organização é um identificador único no formato UUID que pode ser encontrado na seção "Configurações da organização" do Painel. Essa ID é usada frequentemente pelas equipes de suporte, sucesso e vendas para fazer referência a uma organização.

Para localizar o ID da sua organização, vá em "Configurações da organização" e depois em ID da organização. Clique no ícone da área de transferência para copiar o ID e compartilha-lo mais facilmente com as equipes Cypress.
(https://docs.cypress.io/_nuxt/img/cypress-organization-UUID.f496262.gif).


 # Gerenciando Organizações

 ## Criar Organizações

 Você pode criar uma organização a partir do Painel de Serviços abrindo (adicionar link do Dashboard Service) o alternador de organização e clicando em + criar nova organização.
 (https://docs.cypress.io/_nuxt/img/create-org.37c5184.png).
 (https://docs.cypress.io/_nuxt/img/add-organization-dialog.6486d1c.png).


# Organização Pessoal

No passado, cada usuário do Cypress recebia uma organização pessoal - com o seu nome. Você não pode excluir ou editar o nome dessa organização pessoal, mas pode convertê-la em uma organização profissional.

Novos usuários não terão mais uma organização pessoal criada automaticamente.


# Converter para uma Organizção Pessoal

Se você já configurou seus projetos, usuários e cobrança em sua organização pessoal, pode convertê-la em uma organização profissional por meio da página de configurações da organização.
(https://docs.cypress.io/_nuxt/img/convert-to-professional-org-button.50a59c0.jpg).

Clique no botão Converter organização, forneça um nome para a organização e clique em Converter organização. Isso fará duas coisas:

1 - Ele atualizará sua organização pessoal para uma nova organização nomeada. Todos os seus projetos, usuários e informações de cobrança serão transferidos para essa nova organização.

2 - Será criada uma nova organização pessoal vazia para que você sempre tenha um lugar para guardar seus projetos e experimentos paralelos!
(https://docs.cypress.io/_nuxt/img/convert-to-professional-org-modal.12a7b19.jpg).


# Excluir Organização

Você pode excluir organizações de sua propriedade, desde que elas não tenham nenhum projeto na organização. Você deve primeiro transferir a posse de seus projetos para outra organização antes de poder excluir a organização.
(https://docs.cypress.io/_nuxt/img/remove-organization-dialog.bcf4cdf.png).


# Faturamento e Uso

## Plano Open Source

Para apoiar a comunidade, fornecemos o plano Open Source (OSS) para projetos públicos, para tirar proveito do nosso Dashboard Service com execuções de teste ilimitadas. Para se qualificar, seu projeto precisa de duas coisas:

- Seu projeto não deve ser uma entidade comercial
- O código-fonte do seu projeto está disponível em um local público com uma licença aprovada pela OSI(Colocar o link da OSI)

# Solicitando um plano OSS para organização

Siga o passos para solicitar um plano OSS para seu projeto

1-Faça login no Cypress Dashboard ou crie uma conta se você for um novo usuário.
(https://docs.cypress.io/_nuxt/img/oss-plan-1-login.8eb2ebd.png).

2-Vá para a página Organizações e selecione a organização que deseja associar a um plano OSS. Se você não tiver organizações, poderá criar uma clicando no botão + Adicionar organização.
     Nota: Organizações pessoais não podem ser usadas com um plano OSS.
(https://docs.cypress.io/_nuxt/img/oss-plan-2-select-org.875d8dc.png).

3- Vá para a página Faturamento e Uso e clique no link Aplicar para um plano de código aberto, na parte inferior da página.
(https://docs.cypress.io/_nuxt/img/oss-plan-3-billing.bda339e.png)

4- Preencha e envie o formulário de solicitação de plano OSS.
(https://docs.cypress.io/_nuxt/img/oss-plan-4-apply.570a1eb.png).

5-Você receberá um e-mail confirmando sua solicitação. A equipe Cypress analisará sua solicitação e se aprovada, uma assinatura do plano OSS será aplicada à sua organização.
Se você tiver alguma dúvida sobre o plano OSS, não hesite em nos contatar.(link do email)

# Integrações

- BitBucket (Adicionar link)
- GitHub  (Adicionar link)
- GitLab (Adicionar link)
- Jira (Adicionar link)
- Slack (Adicionar link)


# SSO Empresarial

# # Premium Dashboard Feature

O SSO Empresarial está incluido em nossos planos pagos Enterprise e Business

# Requisição de Permissões 

Todas as instruções abaixo devem ser feitas por um Proprietário da organização. Se você não for um Proprietário da Organização, entre em contato com um proprietário da organização para configurar o SSO. 

# Ativar o SSO

1- Faça login no Cypress Dashboard e navegue até a página Integrações da sua organização.
(https://docs.cypress.io/_nuxt/img/integrations-nenu-screenshot.1081176.png).

2-Role para baixo até a seção Enterprise SSO. Selecione seu provedor de SSO e anote as informações fornecidas e necessárias. Mantenha esta janela aberta e continue com as instruções de configuração para seu provedor de SSO.

# Configuração de Provedor SSO

Siga as instruções abaixo para seu provedor de SSO.

- Okta (Adicionar link)

- SAML (Adicionar link)

- Azure AD (Adicionar link)

OKTA
O Cypress Dashboard pode ser integrado ao Okta via SAML. Além da documentação abaixo, consulte a documentação oficial do Okta para configurar um novo aplicativo SAML.

1- Faça login no seu painel do Okta e vá para a seção Admin.
(https://docs.cypress.io/_nuxt/img/okta-admin-cypress-sso-setup.0e05e33.png).

2- Crie um novo aplicativo Web baseado em SAML.
(https://docs.cypress.io/_nuxt/img/okta-add-application-step1-cypress-sso.fe73ca9.png).
(https://docs.cypress.io/_nuxt/img/okta-add-application-step3-cypress-sso.87ab059.png).

3- Forneça as seguintes informações solicitadas no assistente de configuração do Okta:

 - App name: Cypress Dashboard
 - App logo: Cypress logo download (link do download)
 - Single sign on URL: A URL fornecido no Cypress Dashboard
 - Audience URI: A URL fornecido no Cypress Dashboard
 - Attribute statements: Adicione as instruções de atributo descritas no Cypress Dashboard

1- Clique em Avançar, selecione Sou um cliente Okta e clique em Concluir.
2- Clique no botão Exibir instruções de configuração no meio da página. O Cypress Dashboard precisa das informações fornecidas aqui:
 - Copie o URL de logon único do provedor de identidade para o Cypress Dashboard
 - Baixe o certificado e carregue-o no Cypress Dashboard.
 (https://docs.cypress.io/_nuxt/img/okta-download-certificate-for-cypress-dashboard.7bb7111.png).

1- Navegue até a guia Tarefas e conceda aos seus usuários acesso ao Cypress Dashboard.
2- Salve e teste a configuração.(link do test https://docs.cypress.io/guides/dashboard/organizations#Enterprise-SSO)

SAML
O Cypress Dashboard pode ser integrado ao seu provedor de identidade via SAML. Além da documentação abaixo, consulte a documentação oficial do seu provedor para configurar uma integração SAML.
(https://docs.cypress.io/_nuxt/img/enterprise-SSO-SAML.0d994e0.png).

1- Faça login na interface de administração do seu provedor de identidade.
2- Utilize o assistente de configuração fornecendo as informações solicitadas:

 - App name: Cypress Dashboard
 - App logo: Cypress logo download (link do download)
 - Single sign on URL: A URL fornecido no Cypress Dashboard
 - Audience URI: A URL fornecido no Cypress Dashboard
 - Adicione um mapeamento personalizado de atributos:
   
   * User.Email: email de usuário
   * User.FirstName: Primeiro nome do usuário  
   * User.LastName: Último nome do usuário  

1- Colete a URL de logon e o certificado do seu provedor de identidade.
    Forneça para o Cypress Dashboard.   
2- Salve e teste a configuração.(link do test https://docs.cypress.io/guides/dashboard/organizations#Enterprise-SSO)    

Azure AD
O Cypress Dashboard pode ser integrado ao seu provedor de identidade por meio do Azure AD. Além da documentação abaixo, consulte os Guias da Microsoft para configurar um aplicativo.

1- Faça logon no portal do Azure e crie um novo aplicativo.
2- Utilize o assistente de configuração fornecendo as informações solicitadas:
 - App name: Cypress Dashboard
 - App logo: Cypress logo download (link do download)
 - Login URL: A URL fornecido no Cypress Dashboard

1- Colete o ID do cliente fornecido na página Visão geral do aplicativo.
2- Vá para Certificates e Secrets em seu Aplicativo do Azure e crie um novo segredo que não expire. 
   Copie esse segredo recém-criado e cole-o no campo Segredo do Cliente do Azure no Painel do Cypress.
3- Em Permissões de API no Azure AD, verifique se o aplicativo tem acesso às permissões de User.Read 
4- Digite o domínio usado para seu Active Directory, bem como a lista de domínios SSO com os quais você deseja permitir que o usuário se autentique, no Cypress Dashboard. Isso é usado para descoberta de SSO na tela de login.
5- Salve e teste a configuração.(link do test https://docs.cypress.io/guides/dashboard/organizations#Enterprise-SSO)    

# Salvar e testar

Retorne ao Cypress Dashboard e clique em Save and test configuration. O Cypress Dashboard tentará autenticar.
Sua integração agora está completa! Você pode convidar todos os usuários em sua organização para entrar por meio de seu provedor de SSO.

# Notes

- Depois que o SSO estiver configurado no Painel, os usuários devem ser convidados por meio de seu provedor de SSO, não pelo Painel.

- Todos os usuários de SSO são adicionados inicialmente com a função de usuário de membro. Se um usuário precisar de permissões de função de usuário diferentes, isso pode ser alterado por meio do Cypress Dashboard por um membro atual com a função de proprietário ou administrador.