# Integração de IDE

O que você aprenderá

• Como abrir arquivos diretamente na sua IDE
• Extensões e plug-ins para IDEs populares
• Como configurar o preenchimento inteligente de código no seu ambiente de desenvolvimento

Preferência de abertura de arquivos

Ao clicar em um caminho de arquivo ou em um erro no log de comando, o Cypress tentará abrir o arquivo no seu sistema. Se o editor suportar o realce em linha do arquivo, o arquivo será aberto com o cursor posicionado exatamente na linha e coluna de interesse.

A primeira vez que você clicar em um caminho de arquivo, o Cypress solicitará que você selecione a localização preferida para abrir o arquivo. Você pode optar por abri-lo em:

• Sistema de arquivos (por exemplo, Finder no MacOS, File Explorer no Windows)
• Um IDE instalado no seu sistema
• Um caminho de aplicação personalizado

O Cypress tentará encontrar editores de arquivos disponíveis no seu sistema e os exibirá como opções. Se o seu editor preferido não estiver listado, você pode especificar o caminho completo para ele selecionando "Outro". O Cypress fará todos os esforços para abrir o arquivo, mas não há garantia de que funcione com todas as aplicações.

Depois de definir sua preferência de abertura de arquivos, todos os arquivos serão automaticamente abertos na aplicação selecionada sem necessidade de escolher novamente. Se você quiser alterar sua seleção, pode fazê-lo no separador "Definições" do Cypress, clicando em "Preferência do Abridor de Arquivos".

# Extensões e Plug-ins

Existem muitas extensões e plug-ins de IDE de terceiros que podem ajudar a integrar seu IDE com o Cypress.

Visual Studio Code

• Cypress Fixture-IntelliSense: Fornece suporte ao seu cy.fixture() com intellisense para fixtures existentes.
• Cypress Helper: Oferece vários auxiliares e comandos para integração com o Cypress.
• Cypress Snippets: Trechos de código úteis do Cypress.
• Cypress Snippets (atualizado): Esta extensão inclui os snippets mais recentes e mais comuns do Cypress.
• Open Cypress: Permite abrir especificações Cypress e blocos it() simples diretamente a partir do VS Code.
• Test Utils: Adicione ou remova facilmente os modificadores .only e .skip com atalhos de teclado ou com a paleta de comandos.
• Cypress Test Explorer: Ajuda a descobrir, navegar e executar testes Cypress diretamente do editor.

Plataforma IntelliJ

• JetBrains Aqua: Um IDE para automação de testes de interface do usuário com suporte abrangente ao Cypress. Os recursos incluem preenchimento automático, depuração in-IDE, pesquisa de teste e muito mais.

• Plug-in de Automação de Teste: O plug-in oficial, desenvolvido e mantido pela JetBrains, oferece suporte robusto para Cypress e é compatível com IntelliJ IDEA, CLion, GoLand, PhpStorm, PyCharm, Rider, RubyMine e WebStorm. Ele engloba todos os recursos encontrados no JetBrains Aqua.

• Suporte ao Cypress: Integra o Cypress à estrutura de teste comum do IntelliJ.

Preenchimento inteligente de código

# Escrever testes

Funcionalidades

O IntelliSense está disponível para o Cypress. Ele oferece sugestões de código inteligentes diretamente no seu IDE enquanto escreve testes. Um pop-up típico do IntelliSense mostra a definição do comando, um exemplo de código e um link para a página de documentação completa.

Preenchimento automático ao digitar comandos do Cypress

Ajuda da Assinatura ao Escrever e ao Passar o Mouse sobre os Comandos Cypress

Preenchimento automático durante a digitação de cadeias de asserção, incluindo a exibição apenas de asserções DOM se o teste for feito em um elemento DOM.

Configurar no seu ambiente de desenvolvimento

Este documento assume que você instalou o Cypress.

O Cypress vem com declarações de tipo TypeScript incluídas. Os editores de texto modernos podem usar essas declarações de tipo para mostrar o IntelliSense dentro dos arquivos de especificação.

Diretivas de barra tripla

A maneira mais simples de ver o IntelliSense ao digitar um comando ou asserção do Cypress é adicionar uma diretiva de barra tripla ao cabeçalho do seu arquivo de teste JavaScript ou TypeScript. Isso ativará o IntelliSense em uma base por arquivo. Copie a linha de comentário abaixo e cole-a no seu arquivo de especificações.

/// <reference types=“Cypress” />

Se você escrever comandos personalizados e fornecer definições TypeScript para eles, poderá usar as diretivas de barra tripla para mostrar o IntelliSense, mesmo que seu projeto use apenas JavaScript. Por exemplo, se os seus comandos personalizados estão escritos em cypress/support/commands.js e os descreve em cypress/support/index.d.ts use:

// definições de tipo para o objeto Cypress “cy”
/// <reference types=“cypress” />

// definições de tipo para comandos personalizados como “createDefaultTodos”
/// <reference types=“../support” />

Veja o repositório cypress-example-todomvc para um exemplo prático.

Se a diretiva de barra tripla não funcionar, consulte o seu editor de código no documento Suporte ao editor do TypeScript e siga as instruções do seu IDE para obter o suporte ao TypeScript e o recurso autocompletar código inteligente configurados no seu ambiente de desenvolvimento. O suporte ao TypeScript está incorporado no Visual Studio Code, Visual Studio e WebStorm - todos os outros editores exigem configuração adicional.
Declarações de tipo de referência via jsconfig

Em vez de adicionar diretivas de barra tripla a cada arquivo de especificação JavaScript, alguns IDEs (como o VS Code) entendem um arquivo jsconfig.json comum na raiz do projeto. Nesse arquivo, é possível incluir o módulo Cypress e suas pastas de teste.

{
  “include": [“./node_modules/cypress”, “cypress/**/*.js”]
}

O preenchimento inteligente de código deve agora mostrar ajuda para comandos cy dentro de arquivos de especificação JavaScript regulares.

Declarações de tipos de referência através do tsconfig

Adicionar um tsconfig.json dentro da pasta do cypress com a seguinte configuração deve fazer com que o preenchimento inteligente de código funcione.

{
  “compilerOptions": {
    “allowJs": true,
    “types": [“cypress”]
  },
  “include": [“**/*.*”]
}
