# Integração com o Slack

[//]: <> (TODO - Adicionar links traduzidos Dashboard, status de commit, comentários de merge request e config para gravação)

A integração com o Slack permite que você veja os resultados do teste diretamente no canal de slack do seu time.

![Docs Cypress](https://docs.cypress.io/_nuxt/img/cypress-slack-integration-channel-feed.e077328.png)

## Instalar o app Cypress Slack

> **Requisitos de Propriedade**

> Para instalar a integração com o slack, você deve ser o administrador ou dono da sua estrutura do Cypress Dashboard
> e da sua área de trabalho do Slack.

### Para instalar a integração com o Slack

1. Vá para o Dashboard [Organizations page](https://dashboard.cypress.io/organizations) ou abra a troca de organização.

2. Selecione a organização que você deseja integrar com o Slack.
![Docs Cypress](https://docs.cypress.io/_nuxt/img/select-cypress-organization.41ec503.png)

3. Entre na organização selecionada na página **Integrations** pela barra de navegação lateral.
![Docs Cypress](https://docs.cypress.io/_nuxt/img/navigate-to-organization-integrations.0c43d75.png)

4. Clique no botão **Install Slack Integration**.

5. Você verá uma janela pop-up que solicita permissão para o Cypress acessar a área de trabalho e permite que você 
escolha a área de trabalho do Slack e o canal para associar à instalação. Depois de selecionar um canal e permitir o 
acesso, a instalação está concluída! O Cypress Dashboard postará resultados de execução para todos os projetos em sua 
organização para o canal Slack especificado.

## Configuração por organização

### Adicione um canal de Slack adicional

Você pode postar os resultados da execução em um canal adicional. Para adicionar um canal:

1. Navegue para a página **Integrations** da sua organização com a integração do Slack instalada.

2. Dentro da integração do Slack, clique em **Configure**.

3. Clique em **Add Slack Channel**.

4. Você verá uma janela pop-up que solicita que você escolha um canal para associar a sua organização. 
O Cypress Dashboard postará os resultados das execuções para todos os projetos da sua organização no novo canal do Slack.


### Setar preferências de notificação

Por padrão, o Cypress Dashboard postará uma mensagem do Slack para cada canal configurado somente para execuções que reprovam.
Se você quiser alterar essas preferências:

1. Navegue para a página **Integrations** da sua organização com a integração do Slack instalada.

2. Dentro da integração do Slack, clique em **Configure**.

3. Em **Notifications**, selecione o canal do Slack de sua preferência.

- **Todas as execuções**: notificará em todas as execuções (incluindo as aprovadas).

- **Execuções reprovadas somente**: notificará somente as execuções com status de reprovado.


### Silenciar um canal

Se você quer que o Cypress Dashboard pare temporariamente de postar mensagens do Slack para um determinado canal, 
você pode silenciar esse canal. Isso te permite pausar e resumir as notificações facilmente para um canal específico sem
perder a configuração que você definiu.

1. Navegue para a página **Integrations** da sua organização com a integração do Slack instalada.

2. Dentro da integração do Slack, clique em **Configure**.

3. Em **Actions**, selecione **Mute** para cada canal que você quer silenciar.


### Remover um canal do Slack

Você pode fazer com que o Cypress Dashboard pare de postar notificações de um canal. Você pode remover todos os canais 
do Slack se preferir desabilitar completamente as notificações globais em favor das notificações por projeto.

1. Navegue para a página **Integrations** da sua organização com a integração do Slack instalada.

2. Dentro da integração do Slack, clique em **Configure**.

3. Em **Actions**, selecione **Delete** para cada canal que você quer apagar.


## Configuração por projeto

Se a sua organização tem várias equipes trabalhando em projetos separados, você pode adaptar as notificações do Slack de
cada projeto para atender às necessidades das suas equipes.

### Adicione um novo canal de Slack

Você pode fazer com que o Cypress Dashboard publique os resultados de execução para um projeto específico em um canal adicional.

1. Selecione sua organização na troca de organização.
![Docs Cypress](https://docs.cypress.io/_nuxt/img/select-cypress-organization.41ec503.png)

2. Selecione o projeto que você deseja integrar com o Slack.
![Docs Cypress](https://docs.cypress.io/_nuxt/img/select-cypress-project.fe3b44b.png)

3. Vá para a página de configurações do projeto.
![Docs Cypress](https://docs.cypress.io/_nuxt/img/visit-project-settings.43a21a4.png)

4. Role para baixo até a seção **Slack Integration**.

5. Clique em **Add Slack Channel**.

6. Você verá uma janela pop-up que permite você escolher o canal para associar com o projeto.


### Setar preferências de notificação

Por padrão, o Cypress Dashboard postará uma mensagem para cada canal configurado somente para execuções reprovadas. 
Você não pode substituir as preferências de notificação para os canais globais da organização. 
Se você quiser alterar essas preferências:

1. Navegue até a página **Integrations** do projeto com a integração do Slack instalada.

2. Role para baixo até a seção **Slack Integration**.

3. Em **Notifications**, selecione sua preferência para cada canal do Slack:

- **Todas as execuções**: notificará em todas as execuções (incluindo as aprovadas).

- **Execuções reprovadas somente**: notificará somente as execuções com status de reprovado.


### Silenciar um canal

Se você quer que o Cypress Dashboard pare temporariamente de postar mensagens do Slack para um determinado canal, 
você pode silenciar esse canal. Isso te permite pausar e resumir as notificações facilmente para um canal específico sem
perder a configuração que você definiu. Você pode ainda silenciar as mensagens para os canais de sua organização global.

1. Navegue para a página **Integrations** do seu projeto com a integração do Slack instalada.

2. Role para baixo até a seção **Slack Integration**.

3. Em **Actions**, selecione **Mute** para cada canal que você quer silenciar.


### Remover um canal

Você pode fazer com que o Cypress Dashboard pare de publicar as notificações para um canal. Você não pode apagar o canal
de notificação global de um projeto.

1. Navegue para a página **Integrations** do seu projeto com a integração do Slack instalada.

2. Role para baixo até a seção **Slack Integration**.

3. Em **Actions**, selecione **Delete** para cada canal que você quer apagar.


## Remover a integração

Você pode remover completamente a integração com o Slack de seu espaço de trabalho. Isso irá remover o @cypress bot de seu
espaço de trabalho e apagará todas as configurações do Slack que você setou no Cypress Dashboard. Você não pode desfazer,
mas você poderá instalar a integração com o Slack de novo futuramente.

1. Navegue para a página **Integrations** do seu projeto com a integração do Slack instalada.

2. Dentro da integração com o Slack, clique em **Configure**.

3. Clique **Uninstall Slack Integration** para desinstalar a integração com o Slack.

[Voltar para o topo](#integração-com-o-slack)
