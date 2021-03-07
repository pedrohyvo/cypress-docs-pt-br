# Projetos

Com o Cypress, você pode gravar os testes do seu projeto.

[//]: <> (TODO - Adicionar link integração contínua)

Normalmente, você deseja gravar ao executar testes em 
[integração contínua](https://docs.cypress.io/guides/continuous-integration/introduction.html), mas 
também pode gravar seus testes ao executar localmente.

## Configuração

> Para configurar seu projeto para gravar, você deve usar o Test Runner.

> Certifique-se de [instalar](../getting-started/installing-cypress.md) e 
[abri-lo](../getting-started/installing-cypress.md#abrindo-o-cypress) primeiro!

## Configure um projeto para gravar

[//]: <> (TODO - Adicionar links - verificar todos os items da lista)

1. Clique na guia **Runs** de seu projeto no 
[Test Runner](https://docs.cypress.io/guides/core-concepts/test-runner.html).
2. Clique em **Connect to Dashboard**.
3. Você precisará fazer login para gravar seus testes, portanto, será solicitado que você faça login 
no Cypress Dashboard aqui, caso ainda não tenha feito isso.

    ![Setup a project](https://docs.cypress.io/img/dashboard/projects/setup-a-project-1.2e5fcf2c.png)
 
4. Escolha quem é o dono do projeto. Você pode ser o dono ou selecionar uma organização que criou. 
Se você não tiver nenhuma organização, clique em **Create organization**. As organizações funcionam 
exatamente como no GitHub. Eles permitem que você separe seus projetos pessoais e de trabalho. 
[Leia mais sobre organizações](https://docs.cypress.io/guides/dashboard/organizations.html).

    ![Setup a project](https://docs.cypress.io/img/dashboard/projects/setup-a-project-2.36fe8b32.png)

5. Se você não tiver nenhum projeto existente, terá a oportunidade de criar um novo aqui. 
Se você tem projetos existentes e deseja criar um novo, pode clicar em **Create new project** para fazer um novo.

    - Preencha o nome do seu projeto (isso é apenas para fins de exibição e pode ser alterado posteriormente).
    - Escolha se este projeto é Público ou Privado.
      - **Um projeto público** pode ter suas gravações e execuções vistas por qualquer pessoa. 
Normalmente, esses são projetos de código aberto.
      - **Um projeto privado** restringe seu acesso 
[apenas aos usuários que você convida](https://docs.cypress.io/guides/dashboard/users.html). 

    ![Setup a project](https://docs.cypress.io/img/dashboard/projects/setup-a-project-3.1e1b3934.png)

6. Como alternativa, se você já criou um projeto no Dashboard, você pode linkar seu projeto 
selecionando no menu *dropdown*. Certifique-se de selecionar um projeto limpo que não tenha sido 
vinculado anteriormente a um projeto existente.

    ![Setup a project](https://docs.cypress.io/img/dashboard/projects/setup-a-project-4.66a081bd.png)

7. Clique em **Setup Project**.
8. Agora você deve ver uma tela explicando como registrar sua primeira execução com sua chave de registro. 

    ![Setup a project](https://docs.cypress.io/img/dashboard/projects/setup-a-project-5.15b5679e.png)

9. Depois de configurar seu projeto, o Cypress insere um 
[projectId](###id-do-projeto) exclusivo em seu arquivo de configuração, 
`cypress.json` por padrão. Se você estiver usando o controle de origem, recomendamos que você 
verifique seu arquivo de configuração, incluindo o `projectId`, no controle de origem.

10. Dentro da [integração contínua](https://docs.cypress.io/guides/continuous-integration/introduction.html), 
ou a partir do terminal do seu computador local, passe a [Chave de Registro](###chave-de-registro) exibida 
enquanto executa o comando [cypress run](https://docs.cypress.io/guides/guides/command-line.html#cypress-run).

    - Fornecer a chave de registro diretamente:

        ```bash
        cypress run --record --key <record key>
        ```        

    - Ou setar a chave de registro como variável de ambiente

        ```bash
        export CYPRESS_RECORD_KEY=<record key>
        cypress run --record
        ``` 

[//]: <> (TODO - Adicionar link - Test Runner)
🎉 Seus testes agora estão sendo gravados! Assim que a execução dos testes terminar, você os verá no 
[Dashboard](https://on.cypress.io/dashboard) e na guia `Runs` do 
[Test Runner](https://docs.cypress.io/guides/core-concepts/test-runner.html).

![Dashboard runs list](https://docs.cypress.io/img/dashboard/dashboard-runs-list.38bf0e41.png)

![runs list in desktop gui](https://docs.cypress.io/img/dashboard/runs-list-in-desktop-gui.c0a8a677.png)

## Identificação

O Cypress usa seu `projectId` e [Chave de Registro](###chave-de-registro) juntos para identificar projetos únicos.

### ID do Projeto

Depois de configurar seu projeto para gravar, geramos um `projectId` único para seu projeto e 
o inserimos automaticamente em seu arquivo `cypress.json`.

O `projectId` **é uma string de 6 caracteres no seu** `cypress.json`

```json
{
  "projectId": "a7bq2k"
}
```

Isso nos ajuda a identificar o seu projeto de maneira única. Se você alterar isso manualmente, **o Cypress** 
**não será mais capaz de identificar seu projeto ou encontrar as *builds* registradas para ele.**

Se você estiver usando o controle de origem, recomendamos que você verifique seu `cypress.json`, 
incluindo o `projectId`, no controle de origem. Se você não quiser que seu `projectId` fique visível 
em seu código-fonte, você pode defini-lo como uma variável de ambiente usando o nome `CYPRESS_PROJECT_ID`. 
O mecanismo exato para fazer isso depende do seu sistema, mas pode ser algo como:

```bash
export CYPRESS_PROJECT_ID={projectId}
```

### Chave de Registro

A chave de registro é usada para autenticar que seu projeto tem permissão para gravar testes no serviço de Dashboard.
Contanto que sua chave de registro permaneça privada, ninguém será capaz de registrar execuções de teste 
para seu projeto - mesmo se eles tiverem seu `projectId`.

Pense em sua chave de registro como a chave que permite *escrever e criar execuções*. No entanto, 
não tem nada a ver com a capacidade de *ler ou ver* as execuções depois de gravadas.

> **Expondo uma chave de registro**

> Qualquer pessoa que tenha acesso ao `projectId` e a `chave de registro` de um projeto pode gravar 
execuções no projeto dessa organização no Dashboard.

> Você não gostaria que pessoas fora de sua equipe realizassem testes porque:

> 1. **Isso pode aumentar o número de testes executados**. Já que o Cypress cobra com base 
no número de testes registrados - isso significa que eles podem usar todos os testes atribuídos e 
haveria consequências para isso.
> 2. Eles podem gravar quaisquer dados que quiserem em seu Cypress Dashboard. 
Eles poderiam editar o conjunto de testes para que os testes gravados registrassem coisas diferentes do que 
a intenção original do projeto. Isso pode incluir visitas a diferentes sites e geração de vídeos de visitas 
a esses sites, por exemplo.

> Se uma chave de registro for exposta, você deve [excluí-la](###deletar-chave-de-registro) e 
[criar uma nova chave de registro](###criar-uma-nova-chave-de-registro). 
As chaves excluídas serão inválidas; Se um projeto for executado com uma chave excluída, não será possível gravar.

[//]: <> (TODO - Adicionar link - environment variables)
> Você pode definir sua chave de registro como uma variável de ambiente para ajudar a protegê-la. 
Saiba mais [aqui](https://docs.cypress.io/guides/continuous-integration/introduction.html#Environment-variables).

Assim que você estiver configurado para registrar execuções de teste, 
geramos automaticamente uma *chave de registro* para o projeto.

### **Uma chave de registro é um GUID semelhante a este**

```bash
f4466038-70c2-4688-9ed9-106bf013cd73
```

Você pode criar várias chaves de registro para um projeto ou excluir as existentes do nosso 
[Dashboard](https://on.cypress.io/dashboard).

![Record keys in project settings dashboard](https://docs.cypress.io/img/dashboard/record-keys-in-project-settings-dashboard.fcaeea13.png)

Você também pode encontrar sua chave de registro dentro da guia **Settings** no Test Runner.

![Record key shown in desktop gui](https://docs.cypress.io/img/dashboard/record-key-shown-in-desktop-gui-configuration.15896828.jpg)

## Chaves de Registro

Veja [Chave de registro](###chave-de-registro) para uma descrição completa de como as chaves de registro são usadas.

### Criar uma nova chave de registro

1. Acesse a página de projetos da sua organização.
2. Selecione o projeto para o qual deseja alterar o acesso.
    
    ![Select cypress project](https://docs.cypress.io/img/dashboard/select-cypress-project.c8d65a99.png)

3. Acesse a página de **configuração** do projeto.

    ![Visit project settings](https://docs.cypress.io/img/dashboard/visit-project-settings.f345bc8b.png)

4. Aqui você verá uma seção **Record Keys**.

    ![Record keys in project settings dashboard](https://docs.cypress.io/img/dashboard/record-keys-in-project-settings-dashboard.fcaeea13.png)

5. Clique em **Create new key**. Uma nova chave será gerada automaticamente para seu projeto.

### Deletar chave de registro

1. Acesse a página de projetos da sua organização.
2. Selecione o projeto para o qual deseja alterar o acesso.

    ![Select cypress project](https://docs.cypress.io/img/dashboard/select-cypress-project.c8d65a99.png) 

3. Acesse a página de **configuração** do projeto.

    ![Visit project settings](https://docs.cypress.io/img/dashboard/visit-project-settings.f345bc8b.png)

4. Aqui você verá uma seção **Record Keys**.

    ![Record keys in project settings dashboard](https://docs.cypress.io/img/dashboard/record-keys-in-project-settings-dashboard.fcaeea13.png)

5. Clique em **Delete** ao lado da chave de registro que deseja excluir.

## Configurações de paralelização

### Atraso de conclusão de execução

[//]: <> (TODO - Adicionar link - guia de paralelização)
Você pode editar o número de segundos que uma execução aguardará para que novos grupos se juntem antes de 
fazer a transição para "concluída". Veja nosso 
[guia de paralelização](https://docs.cypress.io/guides/guides/parallelization.html#Run-completion-delay) para saber mais.

![Run completion delay](https://docs.cypress.io/img/dashboard/run-completion-delay.dc152f75.jpg)

## Integração Github

Você pode integrar seu projeto ao GitHub e editar suas configurações na página de configurações do projeto.

![Visit project settings](https://docs.cypress.io/img/dashboard/visit-project-settings.f345bc8b.png)

[//]: <> (TODO - Adicionar link - integração github)
Veja nosso guia de [Integração Github](https://docs.cypress.io/guides/dashboard/github-integration.html) para saber mais.

## Integração Slack

Você pode integrar seu projeto ao Slack e editar suas configurações na página de configurações do projeto.

![Visit project settings](https://docs.cypress.io/img/dashboard/visit-project-settings.f345bc8b.png)

[//]: <> (TODO - Adicionar link - integração slack)
Veja nosso guia de [Integração Slack](https://docs.cypress.io/guides/dashboard/slack-integration.html) para saber mais.

## Selos do README

Os selos do README permitem que você aumente a visibilidade do status de teste do seu projeto e a 
contagem de testes para outros desenvolvedores visualizarem o arquivo README do seu projeto.

### Criar um distintivo do README

1. Em sua conta do Cypress Dashboard, selecione o projeto para o qual você gostaria de criar um selo no README.
2. Na página de configurações do projeto, role para baixo até a seção "README Badges" e clique em “Configure Badge”.

    ![Dashboard badge configure button](https://docs.cypress.io/img/dashboard/badges/dashboard-badge-configure-button.eb79365a.png)

    Observação: Selos do README atualmente estão disponíveis apenas para projetos públicos.

3. Um modal de configuração aparecerá. O ID do projeto será pré-preenchido com o ID associado ao projeto 
que você selecionou. Você pode escolher designar uma *branch* específica ou deixar este campo em branco 
para sempre usar a *build* mais recente do projeto.
4. Em seguida, estilize seu selo. *Flat* é o estilo padrão e é mais comumente usado, 
mas 5 opções de estilo estão disponíveis.
5. Selecione o tipo de selo para alterar a quantidade e o tipo de informação que é exibida. 
O simples status mostrará apenas se os testes estão passando ou falhando. O status detalhado mostrará 
o número de testes que foram aprovados, reprovados ou ignorados. A contagem de teste mostrará quantos testes 
estão incluídos em seu projeto.
6. Depois de selecionar todas as suas configurações, verifique a visualização e certifique-se de que 
tudo esteja como você gosta.
7. 🎉 Seu selo está pronto para ser incorporado. Copie o *markdown* na parte inferior 
do modal "Configure Badge" e incorpore-o no arquivo README do seu projeto para que todos possam ver!

    ![Bashboard badge configuration](https://docs.cypress.io/img/dashboard/badges/dashboard-badge-configuration.65e97c17.png)

Veja também 
[Destaque o status de teste do seu projeto com o dos selos do Cypress README](https://www.cypress.io/blog/2020/09/02/highlight-your-projects-test-status-with-cypress-readme-badges/).

## Acesso às execuções

Visite as configurações do seu projeto para ver quem tem acesso às execuções do seu projeto.

![Visit project settings](https://docs.cypress.io/img/dashboard/visit-project-settings.f345bc8b.png)

### Público x Privado

- **Público** significa que qualquer pessoa pode ver as execuções de teste registradas para o projeto. 
É semelhante a como os projetos públicos no GitHub, Travis CI ou CircleCI são tratados. 
Qualquer pessoa que conheça seu `projectId` poderá ver as execuções registradas para projetos públicos.

[//]: <> (TODO - Adicionar links - usuários e organizações)

- **Privado** significa que apenas os 
[usuários](https://docs.cypress.io/guides/dashboard/users.html) que você convida para sua 
[organização](https://docs.cypress.io/guides/dashboard/organizations.html) podem ver suas 
execuções registradas. Mesmo que alguém conheça seu `projectId`, essa pessoa não terá acesso 
às suas execuções, a menos que você o tenha convidado.

### Alterar acesso do projeto

1. Acesse a página de projetos da sua organização.
2. Selecione o projeto para o qual deseja alterar o acesso.

    ![Select cypress project](https://docs.cypress.io/img/dashboard/select-cypress-project.c8d65a99.png)

3. Acesse a página de **configuração** do projeto.

    ![Visit project settings](https://docs.cypress.io/img/dashboard/visit-project-settings.f345bc8b.png)

4. Aqui você verá uma seção exibindo o acesso às execuções. Escolha o acesso apropriado que você 
gostaria de atribuir ao projeto aqui.

![Access to runs](https://docs.cypress.io/img/dashboard/access-to-runs.b5ca0210.png)

## Transferir propriedade

### Transferir projeto para outro usuário ou organização

[//]: <> (TODO - Adicionar link - organização)
Você pode transferir projetos de sua propriedade para outra 
[organização](https://docs.cypress.io/guides/dashboard/organizations.html) da qual faça parte ou 
para outro usuário na sua organização. Os projetos só podem ser transferidos do [serviço Dashboard](https://on.cypress.io/dashboard).

1. Selecione sua organização no *switcher* de organização.
2. Selecione o projeto que deseja transferir.

    ![Select cypress project](https://docs.cypress.io/img/dashboard/select-cypress-project.c8d65a99.png)

3. Acesse a página de **configuração** do projeto.

    ![Visit project settings](https://docs.cypress.io/img/dashboard/visit-project-settings.f345bc8b.png)

4. Role para baixo até a seção **Transferir Propriedade** e clique em **Transferir Propriedade**.

    ![Transfer ownership button](https://docs.cypress.io/img/dashboard/transfer-ownership-button.22d66cf1.png)

5. Selecione o usuário ou organização e clique em **Transferir**.

    ![Transfer Ownership of project](https://docs.cypress.io/img/dashboard/transfer-ownership-of-project-dialog.eceefeaa.png)

### Cancelar transferência de projeto

Após a transferência, você pode cancelar a transferência a qualquer momento visitando os projetos da organização 
e clicando em **Cancelar transferência**.

![Cancel transfer of project](https://docs.cypress.io/img/dashboard/cancel-transfer-of-project.751b3383.png)

### Aceitar ou rejeitar projeto transferido

Quando um projeto é transferido para você, você receberá um e-mail com uma notificação. 
Você poderá aceitar ou rejeitar o projeto transferido clicando na notificação na barra lateral e 
clicando em ‘Aceitar’ ou ‘Rejeitar’.

![See pending transfer](https://docs.cypress.io/img/dashboard/see-pending-transfer.dba986c1.png)

![Accept or reject transfer project](https://docs.cypress.io/img/dashboard/accept-or-reject-transfer-of-project.ae0ae78c.png)

## Deletar projeto

Você pode excluir projetos de sua propriedade. Isso também excluirá todas as execuções de teste registradas. 
A exclusão de projetos só pode ser feita a partir do [serviço Dashboard](https://on.cypress.io/dashboard).

1. Selecione sua organização no *switcher* de organização.
2. Selecione o projeto que deseja remover.

    ![Select cypress project](https://docs.cypress.io/img/dashboard/select-cypress-project.c8d65a99.png)

3. Acesse a página de **configuração** do projeto.

    ![Visit project settings](https://docs.cypress.io/img/dashboard/visit-project-settings.f345bc8b.png)

4. Na parte inferior da página Configurações, clique no botão **Yes, remove project**.

    ![Remove project dialog](https://docs.cypress.io/img/dashboard/remove-project-dialog.a84d3195.png)

5. Confirme que deseja excluir o projeto clicando em **Yes, remove project**.

[Voltar para o topo](#projetos)
