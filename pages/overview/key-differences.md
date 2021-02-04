# Diferenças-chave

```markdown
O que você vai aprender

- O que torna o Cypress unico
- Como sua arquitetura se diferencia do Selenium
- Novas abordagens de testes que antes não eram possíveis
```

## Arquitetura

A maioria das ferramentas de teste (como o Selenium) opera executando 
fora do navegador e executando comandos remotos pela rede. 
*Cypress é exatamente o oposto.* O Cypress é executado no mesmo ciclo
 de execução (*run loop*) que sua aplicação.

Por de trás do Cypress está um processo do servidor Node.js. O Cypress e o 
processo Node.js se comunicam, sincronizam e executam tarefas constantemente
em nome um do outro. Ter acesso a ambas as partes (*front-end* e *back-end*)
nos dá a capacidade de responder aos eventos da sua aplicação em tempo real,
enquanto, ao mesmo tempo, trabalhar fora do navegador para tarefas que exigem
um privilégio mais alto.

O Cypress também opera na camada de rede, lendo e alterando o tráfego da web 
em tempo real. Isso permite que o Cypress não apenas modifique tudo que entra
e saia do navegador, mas também altere algum código que possa interferir em 
sua capacidade de automatizar o navegador.

O Cypress finalmente controla todo o processo de automação de cima para baixo,
o que o coloca na posição única de ser capaz de entender tudo o que acontece 
dentro e fora do navegador. Isso significa que o Cypress é capaz de fornecer 
resultados mais consistentes do que qualquer outra ferramenta de teste.

Visto que o Cypress é instalado localmente em sua máquina, ele também pode 
acessar o sistema operacional para tarefas de automação. Isso torna possível
a execução de tarefas como tirar screenshots, gravar vídeos, operações gerais
do sistema de arquivos e perações de rede.

## Acesso nativo

Já o Cypress opera em sua aplicação, isso significa que ele tem acesso nativo
a todos os objetos. Quer seja a `window`, o` document`, um elemento DOM, sua 
instância de aplicação, uma função, um temporizador, um *service worker* ou 
qualquer outra coisa - você tem acesso a tudo isso nos testes do Cypress. 
Não há serialização de objetos, não há protocolo *over-the-wire* - você tem 
acesso a tudo. Seu código de teste pode acessar todos os mesmos objetos que
o código de sua aplicação.

## Novos tipos de teste

Ter controle total sobre sua aplicação, o tráfego de rede e o acesso nativo 
a todos os objetos do *host* revelam uma nova maneira de testar que nunca foi 
possível antes. Em vez de ser 'bloqueado' por sua aplicação e não ser capaz de
controlá-la facilmente - o Cypress permite que você altere qualquer aspecto de 
como a sua aplicação funciona. Em vez de testes lentos e caros, como testes que 
necessitam a criação do estado necessário para uma determinada situação, 
você pode simplesmente criar esses estados artificialmente como faria em um 
teste de unidade. Você pode, por exemplo:

- Fazer um Stub do navegador ou das funções da sua aplicação e forçá-los a se 
comportar conforme necessário no seu caso de teste.
- Exponha *data stores*, ou armazenamentos de dados (como no Redux) para que 
você possa alterar programaticamente o estado de sua aplicação diretamente de 
seu código de teste.
- Teste *edge cases* como 'páginas vazias', forçando seu servidor a 
enviar respostas vazias.
- Teste como sua aplicação responde a erros em seu servidor modificando os 
códigos de status de resposta para 500.
- Modifique elementos do DOM diretamente - como forçar a exibição de 
elementos ocultos.
- Use plugins de terceiros programaticamente. Em vez de se preocupar com widgets
complexos da interface do usuário, como seleções múltiplas, 
preenchimentos automáticos, menus suspensos, visualizações em árvore ou 
calendários, basta chamar os métodos diretamente do seu código de teste 
para controlá-los.
- Impedir que o Google Analytics carregue *antes* que qualquer código 
de sua aplicação execut durante o teste.
- Receber notificações síncronas sempre que sua aplicação fizer a transição 
para uma nova página ou quando começar a descarregar.
- Controle o tempo movendo-se para frente ou para trás para que os 
temporizadores ou *polls* disparem automaticamente sem ter que esperar pelo 
tempo necessário em seus testes.
- Adicione seus próprios ouvintes de eventos (*event listeners*) para responder 
à sua aplicação. Você pode atualizar o código da aplicação para se comportar de 
maneira diferente quando estiver em testes no Cypress. Você pode controlar as 
mensagens do WebSocket de dentro do Cypress, carregar condicionalmente scripts 
de terceiros ou chamar funções diretamente na sua aplicação.

## Atalhos

Tentando testar áreas difíceis de alcaçar em sua aplicação? Não gosta dos 
efeitos colaterais que uma ação cria? Cansado de repetir as mesmas ações lentas
 toda hora? Você pode simplesmente ignorá-las para a maioria dos casos de teste.

O Cypress impede que você seja forçado a sempre "agir como um usuário" para 
gerar o estado de uma determinada situação. Com o Cypress você pode interagir 
programaticamente e controlar sua aplicação. Você não precisa mais usar sua 
interface de usuário para criar estados.

Isso significa que você não precisa visitar uma página de login, digitar um nome
de usuário e senha e esperar que a página seja carregada e/ou redirecionada para
cada teste executado. O Cypress dá a você a habilidade de pegar atalhos e logar
programaticamente. Com comandos como `cy.request ()`, você pode enviar
solicitações HTTP diretamente, e ainda ter essas requisições sincronizadas
com o navegador. Os cookies são enviados e aplicados automaticamente. 
Preocupado com o CORS? Não fique, isso é completamente ignorado. 
O poder de escolher quando testar como um usuário e quando pular partes lentas
e repetitivas é seu.

## *Flake resistant* (resistentes à "fraquesas")

O Cypress conhece e compreende tudo o que acontece em sua aplicação de forma 
síncrona. Ele é notificado no momento em que a página é carregada e no momento
em que a página é descarregada. É impossível para o Cypress perder elementos
quando ele aciona eventos. O Cypress sabe até mesmo o quão rápida a animação
de um elemento leva e esperará que ele pare de animar. Além disso, ele espera
automaticamente que os elementos se tornem visíveis, se tornem ativados e 
deixem de ser cobertos. Quando as páginas começarem a transição, o Cypress 
pausará a execução do comando até que a página seguinte seja totalmente 
carregada. Você pode até mesmo dizer ao Cypress para esperar por solicitações
de rede específicas até que sejam terminadas.

O Cypress executa a grande maioria dos seus comandos dentro do navegador, 
portanto, não há atrasos devido à rede. Comandos executam e conduzem sua 
aplicação tão rápido quanto ela é capaz de renderizar. Para lidar com 
estruturas JavaScript modernas com UI complexas, você usa asserções para 
informar ao Cypress qual deve ser o estado desejado de sua aplica'ao. 
O Cypress esperará automaticamente que sua aplicação atinja esse estado 
antes de prosseguir. Você está completamente isolado de se preocupar com 
esperas manuais ou novas tentativas. O Cypress espera automaticamente pela
existência de elementos e nunca irá lhe render por elementos antigos que 
foram desanexados do DOM.

## Depuração

Acima de tudo, o Cypress foi construído para a usabilidade.

Existem centenas de mensagens de erro personalizadas descrevendo o motivo
exato pelo qual seu teste do Cypress falhou.

Há uma interface do usuário rica que mostra visualmente a execução dos comandos,
asserções, solicitações de rede, *spies*, *stubs*, carregamentos de página ou 
alterações de URL.

O Cypress tira *snapshots* de sua aplicação e permite que você viaje no tempo
de volta ao estado em que ela estava quando os comandos foram executados.

Você pode usar o DevTools enquanto seus testes são executados, você pode ver 
todas as mensagens do console, e todas as solicitações de rede. Você pode 
inspecionar elementos, e você pode até mesmo usar instruções de depurador em 
seu código de teste ou no código de sua aplicação. Não há perda de 
fidelidade - você pode usar todas as ferramentas com as quais já 
está familiarizado. Isso permite que você teste e desenvolva, 
tudo ao mesmo tempo.

## *Trade offs*

Embora o Cypress possua muitas novas e poderosas capacidades - 
também existem importantes *trade-offs* que fizemos para tornar isso possível.

Se você estiver interessado em entender mais, 
escrevemos um guia completo sobre esse tópico.

[Voltar para o topo](#diferenças-chave)
