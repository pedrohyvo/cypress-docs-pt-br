# Por que Cypress?

```markdown
O que você vai aprender

- O que é o Cypress e porque você deveria usá-lo
- Nossa missão e no que acreditamos
- Funcionalidades-chave do Cypress
```

[![Cypress em poucas palavras](http://img.youtube.com/vi/LcGHiFnBh3Y/0.jpg)](http://www.youtube.com/watch?v=LcGHiFnBh3Y")

## Em poucas palavras

O Cypress é uma ferramenta de teste de front end de última geração, criada para
a web moderna. Abordamos os principais pontos problemáticos que desenvolvedores
e engenheiros de QA enfrentam ao testar aplicações modernas.

Nós simplificamos:

- [Configurando os testes](#configurando-os-testes)
- [Escrevendo os testes](#escrevendo-os-testes)
- [Executando os testes](#executando-os-testes)
- [Depurando os testes](#depurando-os-testes)

O Cypress é mais frequentemente comparado ao Selenium; no entanto, o Cypress
é fundamentalmente e arquiteturalmente diferente. O Cypress não é restringido
pelas mesmas restrições que o Selenium.

Isso permite que você escreva testes **mais rápidos**, **fáceis**
e **confiáveis**.

## Quem usa o Cypress?

Nossos usuários normalmente são desenvolvedores ou engenheiros de QA que criam
aplicações web usando modernos frameworks JavaScript.

O Cypress permite que você escreva todos os tipos de testes:

- Testes end to end (ou testes de ponta-a-ponta)
- Testes de integração
- Testes de unidade

O Cypress pode testar tudo que é executado em um navegador.

## O ecossistema do Cypress

O Cypress consiste de um Test Runner (executor de testes) grátis, [open source](https://github.com/cypress-io/cypress),
[instalado localmente](../getting-started/installing-cypress.md) **e** um painel como serviço, 
para gravar seus testes.

- ***Primeiro:*** O Cypress torna fácil configurar e começar a escrever testes
todos os dias enquanto você cria sua aplicação localmente. *TDD no seu melhor!*
- ***Então:*** Depois de criar um conjunto de testes e integrar o Cypress ao
seu provedor de integração contínua (CI), nosso Painel como serviço pode gravar
suas execuções de teste. 
Você nunca terá que se perguntar: *por que isso falhou?*

## Nossa missão

Nossa missão é construir um ecossistema próspero e de código aberto que aumente
a produtividade, torne os testes uma experiência agradável e gere felicidade
para os desenvolvedores. Nós nos responsabilizamos por defender um 
processo de teste **que realmente funcione**.

Acreditamos que nossa documentação deve ser simples e acessível. 
Isso significa permitir que nossos leitores entendam plenamente não apenas
**o que**, mas o **porquê** também.

Queremos ajudar os desenvolvedores a criar uma nova geração de aplicações 
modernas com mais rapidez, melhor e sem o estresse e a ansiedade associados
ao gerenciamento de testes.

Sabemos que, para que tenhamos sucesso, precisamos capacitar, fomentar e 
promover um ecossistema que tenha sucesso com o código aberto. Cada linha
de código de teste é um investimento **em sua base de código** e nunca será
ligado a nós como um serviço pago ou empresa. Os testes poderão ser executados
e funcionar independentemente, *sempre*.

Acreditamos que os testes requerem muito 💙 e estamos aqui para construir
uma ferramenta, um serviço e uma comunidade em que todos possam aprender e 
se beneficiar. Estamos resolvendo os pontos mais difíceis compartilhados 
por todos os desenvolvedores que trabalham na Web. Acreditamos nesta missão e 
esperamos que você se junte a nós para tornar o Cypress um ecossistema 
duradouro que ajuda a todos serem felizes.

## Funcionalidades

O Cypress vem totalmente pronto, com tudo que você precisa. Aqui está uma lista 
de coisas que ele pode fazer que nenhum outro framework de teste pode:

[//]: <> (TODO - Adicionar link log de comandos)

- **Viagem no tempo:** O Cypress tira *snapshots* enquanto seus testes 
são executados. Basta passar o mouse sobre os comandos no [log de comandos](https://docs.cypress.io/guides/core-concepts/test-runner.html#Command-Log)
para ver exatamente o que aconteceu em cada etapa.

[//]: <> (TODO - Adicionar link depure diretamente)

- **Depuração:** Pare de adivinhar por que seus testes estão falhando. 
[Depure diretamente](https://docs.cypress.io/guides/overview/why-cypress.html#Features) através de
ferramentas familiares como o Chrome DevTools. 
Nossos erros legíveis e *stack trace* tornam a depuração rápida.

[//]: <> (TODO - Adicionar link espera automaticamente)

- **Espera automatica:** Nunca adicione esperas ou sleeps aos seus testes. 
O Cypress [espera automaticamente](https://docs.cypress.io/guides/core-concepts/introduction-to-cypress.html#Cypress-is-Not-Like-jQuery)
por comandos e asserções antes de prosseguir. Chega de *async hell*.

[//]: <> (TODO - Adicionar link controle o comportamento)

- **Spies, Stubs, e Clocks:** Verifique e
[controle o comportamento](https://docs.cypress.io/guides/guides/stubs-spies-and-clocks.html) de funções,
respostas do servidor ou timers. A mesma funcionalidade que você adora no teste
de unidade está na ponta dos seus dedos.

[//]: <> (TODO - Adicionar link edge cases)

- **Controle de tráfego de rede:** Facilmente [controle, limite e teste 
*edge cases*](https://docs.cypress.io/guides/guides/network-requests.html)
sem envolver seu servidor. Você pode limitar o tráfego de rede como quiser.

- **Resultados consistentes:** Nossa arquitetura não usa o Selenium ou 
o WebDriver. Diga olá aos testes rápidos, consistentes e confiáveis que
são *flaky free* (livres de "fraquesas").

- **Screenshots e vídeos:** Veja capturas de tela tiradas automaticamente
em caso de falha ou vídeos de todo o seu conjunto de testes quando executado
a partir da interfáce de linha de comando.

- **Suporte entre navegadores:** Rode testes no Firefox e navegadores da família Chrome
(incluindo Edge e Electron) localmente e otimizado na *pipeline* de integração contínua.

## Configurando os testes

Não há servidores, drivers ou quaisquer outras dependências para instalar ou
configurar. Você pode escrever seu primeiro teste passando em 60 segundos.

[Vídeo de exemplo](https://docs.cypress.io/img/snippets/installing-cli.mp4)

## Escrevendo os testes

Testes escritos com o Cypress são fáceis de ler e entender. 
Nossa API vem totalmente pronta, usando ferramentas com as quais 
você já está familiarizado.

[Vídeo de exemplo](https://docs.cypress.io/img/snippets/installing-cli.mp4)

## Executando os testes

O Cypress é executado tão rápido quanto seu navegador pode renderizar conteúdo.
Você pode assistir a testes executarem em tempo real enquanto desenvolve 
suas aplicações. TDD para a vitória!

[Vídeo de exemplo](https://docs.cypress.io/img/snippets/running-tests.mp4)

## Depurando os testes

Mensagens de erro legíveis ajudam você a depurar rapidamente. 
Você também tem acesso a todas as ferramentas de desenvolvedor 
que conhece e adora.

[Vídeo de exemplo](https://docs.cypress.io/img/snippets/debugging.mp4)

## Cypress no Mundo Real

![Cypress no Mundo Real](https://docs.cypress.io/img/guides/real-world-app.df1de4ad.png)

O Cypress torna rápido e fácil iniciar os testes e, quando você começa a testar seu aplicativo,
**muitas vezes se pergunta se está usando as melhores práticas ou estratégias escaláveis.**

Para guiar esse caminho, o time do Cypress criou a 
[Aplicação Mundo Real (RWA)](https://github.com/cypress-io/cypress-realworld-app),
uma aplicação exemplo completa que demonstra testes com o **Cypress em cenários práticos e realistas.**

O RWA alcança cobertura de código total com testes ponta a ponta em vários navegadores e
tamanhos de dispositivo, mas também inclui testes de regressão visual, testes de API,
testes de unidade e os executa em um pipeline de CI eficiente. Use o RWA para **aprender,**
**experimentar, mexer e praticar** testes de aplicativos da web com o Cypress.

O aplicativo vem com tudo que você precisa, 
[basta clonar o repositório](https://github.com/cypress-io/cypress-realworld-app)
e iniciar os testes. 

[Voltar para o topo](#por-que-cypress)
