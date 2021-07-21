# Debugando

```markdown
O que você vai aprender

- Como o Cypress é executado no mesmo loop de evento com seu código, mantendo a depuração menos exigente e mais compreensível
- Como o Cypress adota as ferramentas de desenvolvedor padrão
- Como e quando usar debugger e o [`.debug()`] (https://docs.cypress.io/api/commands/debug)
```

## Usando o `debugger`

Seu código de teste Cypress é executado no mesmo loop de execução que seu aplicativo. Isto significa que tem acesso ao código em execução na página, bem como as coisas que o navegador faz disponível para você, como `document`, `window`, e `debugger`.

##Depure como você sempre faz

Com base nessas declarações, você pode ficar tentado a lançar um debuggerem seu teste, assim:

```markdown
it('let me debug like a fiend', () => {
  cy.visit('/my/page/path')

  cy.get('.selector-in-question')

  debugger // Doesn't work
})
```

Isso pode não funcionar exatamente como você esperava. Como você deve se lembrar da introdução ao Cypress , os cycomandos enfileiram uma ação para ser executada mais tarde. Você pode ver o que o teste acima fará, dada essa perspectiva?

Ambos [`cy.visit()`](https://docs.cypress.io/api/commands/visit)e [`cy.get()`](https://docs.cypress.io/api/commands/get) retornarão imediatamente, tendo enfileirado seu trabalho para ser feito mais tarde, e debugger serão executados antes que qualquer um dos comandos tenha realmente executado.

Vamos usar [`.then()`](https://docs.cypress.io/api/commands/then) para acessar o comando Cypress durante a execução e adicionar um debuggerno momento apropriado:

```markdown
it('let me debug when the after the command executes', () => {
  cy.visit('/my/page/path')

  cy.get('.selector-in-question').then(($selectedElement) => {
    // Debugger is hit after the cy.visit
    // and cy.get command have completed
    debugger
  })
})

```

Agora estamos no negócio! Na primeira vez, [`cy.visit()`](https://docs.cypress.io/api/commands/visit) o [`cy.get()`](https://docs.cypress.io/api/commands/get) cadeia (com seu [`.then()`](https://docs.cypress.io/api/commands/then) anexo) é enfileirada para que o Cypress execute. O `it`bloco sai e o Cypress começa seu trabalho:

1 - A página é visitada e o Cypress espera que ela carregue.

2 - O elemento é consultado e Cypress automaticamente espera e tenta novamente por alguns momentos se não for encontrado imediatamente.

3 - A função passada para [`.then()`](https://docs.cypress.io/api/commands/then) é executada, com o elemento encontrado rendido a ela.

4 - Dentro do contexto da [`.then()`](https://docs.cypress.io/api/commands/then) função, o debugger é chamado, parando o navegador e chamando o foco para as Ferramentas do Desenvolvedor.

5 - Você está dentro! Inspecione o estado do seu aplicativo como faria normalmente se tivesse debuggerinserido o no código do aplicativo.


## Usando [`.debug()`](https://docs.cypress.io/api/commands/debug)


Cypress também expõe um atalho para comandos de depuração Cypress também expõe um atalho para comandos de depuração [`.debug()`](https://docs.cypress.io/api/commands/debug). Vamos reescrever o teste acima usando este método auxiliar: 

```markdown
it('let me debug like a fiend', () => {
  cy.visit('/my/page/path')

  cy.get('.selector-in-question').debug()
})
```
O assunto atual gerado pelo [`cy.get()`](https://docs.cypress.io/api/commands/get) é exposto como a variável subjectem suas Ferramentas de Desenvolvedor para que você possa interagir com ele no console.
![Alt](https://docs.cypress.io/_nuxt/img/debugging-subject.bc0098b.png)

Use [`.debug()`](https://docs.cypress.io/api/commands/debug) para inspecionar rapidamente qualquer (ou muitas!) Parte (s) do seu aplicativo durante o teste. Você pode anexá-lo a qualquer cadeia de comandos do Cypress para dar uma olhada no estado do sistema naquele momento.

##Percorrer os comandos de teste

Você pode executar o comando de teste por comando usando o [`.pause()`](https://docs.cypress.io/api/commands/pause) comando.

```markdown
it('adds items', () => {
  cy.pause()
  cy.get('.new-todo')
  // more commands
})
```

Isso permite que você inspecione o aplicativo da web, o DOM, a rede e qualquer armazenamento após cada comando para garantir que tudo aconteça conforme o esperado.

Embora o Cypress tenha desenvolvido um [`excelente Test Runner`](https://docs.cypress.io/guides/core-concepts/test-runner) para ajudá-lo a entender o que está acontecendo em seu aplicativo e seus testes, não há como substituir todo o trabalho incrível que as equipes de navegador fizeram em suas ferramentas de desenvolvimento integradas. Mais uma vez, vemos que Cypress vai com o fluxo do ecossistema moderno, optando para alavancar essas ferramentas sempre que possível.

### Veja em ação!

Você pode ver um passo a passo de depuração de algum código de aplicativo do Cypress [`neste segmento de nossa série de tutoriais React`](https://vimeo.com/242961930#t=264s)

## Obtenha registros do console para comandos

Todos os comandos do Cypress, quando clicados no [`Log de Comandos`](https://docs.cypress.io/guides/core-concepts/test-runner#Command-Log) , imprimem informações extras sobre o comando, seu assunto e o resultado gerado. Tente clicar ao redor do Log de Comando com as Ferramentas do Desenvolvedor abertas! Você pode encontrar algumas informações úteis aqui.

Ao clicar no .type() comando, o console das Ferramentas do Desenvolvedor exibe o seguinte:
![Alt](https://docs.cypress.io/_nuxt/img/console-log-of-typing-with-entire-key-events-table-for-each-character.f235db7.png)

# Erros

Às vezes, os testes falham. Às vezes, queremos que eles falhem, apenas para sabermos que eles estão testando a coisa certa quando passam. Mas outras vezes, os testes falham involuntariamente e precisamos descobrir o porquê. O Cypress fornece algumas ferramentas para ajudar a tornar esse processo o mais fácil possível.

## Anatomia de um erro

Vamos dar uma olhada na anatomia de um erro e como ele é exibido no Cypress, por meio de um teste com falha.

```markdown

it('reroutes on users page', () => {
  cy.contains('Users').click()
  cy.url().should('include', 'users')
})
```
O centro do `<li>`Users`</li>`elemento está oculto em nosso aplicativo em teste, portanto, o teste acima falhará. No Cypress, será mostrado um erro em caso de falha que inclui as seguintes informações:

1 - **Nome do erro** : este é o tipo de erro (por exemplo, AssertionError, CypressError)
2 - **Mensagem de erro** : geralmente informa o que deu errado. O comprimento pode variar. Alguns são curtos como no exemplo, enquanto outros são longos e podem dizer exatamente como corrigir o erro.
3 - **Saiba mais**: algumas mensagens de erro contêm um link Saiba mais que o levará à documentação relevante do Cypress.
4 - **Arquivo de quadro de código** : geralmente é a linha superior do rastreamento de pilha e mostra o arquivo, o número da linha e o número da coluna realçados no quadro de código abaixo. Clicar neste link abrirá o arquivo em seu [`abridor de arquivos preferido`](https://docs.cypress.io/guides/tooling/IDE-integration#File-Opener-Preference) e destacará a linha e a coluna nos editores que o suportam.
5 - **Quadro de código** : mostra um trecho de código onde ocorreu a falha, com a linha e a coluna relevantes destacadas.
6 - **Exibir rastreamento de pilha** : Clicar aqui alterna a visibilidade do rastreamento de pilha. Os rastreamentos de pilha variam em comprimento. Clicar em um caminho de arquivo azul abrirá o arquivo em seu [`abridor de arquivos preferido`](https://docs.cypress.io/guides/tooling/IDE-integration#File-Opener-Preference).
7 - **Botão Imprimir no console** : Clique aqui para imprimir o erro completo no console do DevTools. Isso geralmente permitirá que você clique em linhas no rastreamento de pilha e abra arquivos em suas DevTools.

## Mapas de origem

O Cypress utiliza mapas de origem para aprimorar a experiência de erro. Os rastreamentos de pilha são traduzidos para que seus arquivos de origem sejam mostrados em vez do arquivo gerado que é carregado pelo navegador. Isso também permite a exibição de frames de código. Sem mapas de origem embutidos, você não verá frames de código.

Por padrão, o Cypress incluirá um mapa de origem embutido em seu arquivo de especificação, para que você obtenha o máximo da experiência de erro. Se você modificar o pré-processador , certifique-se de que os mapas de origem embutidos estejam ativados para obter a mesma experiência. Com webpack e o pré-processador webpack , por exemplo, defina a devtoolopção para inline-source-map.

Floco de depuração
Embora o Cypress seja resistente a flocos , alguns usuários experimentam flocos, especialmente quando executado em CI versus localmente. Na maioria das vezes, em casos de testes instáveis, vemos que não há asserções suficientes em torno das ações de teste ou solicitações de rede antes de passar para a próxima asserção.

Se houver alguma variação na velocidade das solicitações ou respostas da rede quando executado localmente em comparação com o CI, pode haver falhas em um sobre o outro.

Por causa disso, recomendamos executar o máximo de etapas necessárias antes de prosseguir com o teste. Isso também ajuda a isolar posteriormente onde está a falha exata durante a depuração.

Flake também pode ocorrer quando há diferenças entre os ambientes local e de CI. Você pode usar os seguintes métodos para solucionar problemas de testes que passam localmente, mas falham em CI.

Revise seu processo de construção de CI para garantir que nada esteja mudando em seu aplicativo que possa resultar em testes com falha.
Remova a variabilidade sensível ao tempo em seus testes. Por exemplo, certifique-se de que uma solicitação de rede foi concluída antes de procurar o elemento DOM que depende dos dados dessa solicitação de rede. Você pode aproveitar o aliasing para isso.
O Cypress Dashboard também oferece análises que ilustram tendências em seus testes e podem ajudar a identificar os testes que falham com mais frequência. Isso pode ajudar a restringir o que está causando o flake - por exemplo, ver o aumento das falhas após uma mudança no ambiente de teste pode indicar problemas com o novo ambiente.

Para obter mais conselhos sobre como lidar com os flocos, leia uma série de postagens do nosso blog e Identificando o cheiro do código em Cypress, do Embaixador da Cypress, Josh Justice.

Log de eventos Cypress
Cypress emite vários eventos que você pode ouvir conforme mostrado abaixo. Leia mais sobre o registro de eventos no navegador aqui .

eventos de log do console para depuração
Execute o comando Cypress fora do teste
Se você precisar executar um comando Cypress direto do console das Ferramentas do Desenvolvedor, poderá usar o comando interno cy.now('command name', ...arguments). Por exemplo, para executar o equivalente de cy.task('database', 123)fora da cadeia de comando de execução normal:

cy.now('task', 123).then(console.log)
// runs cy.task(123) and prints the resolved value
O cy.now()comando é um comando interno e pode mudar no futuro.

Cipreste violino
Enquanto estiver aprendendo o Cypress, pode ser uma boa ideia tentar pequenos testes em HTML. Nós escrevemos um plugin @ cypress / fiddle para isso. Ele pode montar rapidamente qualquer HTML fornecido e executar alguns comandos de teste do Cypress nele.

Solução de problemas do Cypress
Há momentos em que você encontrará erros ou comportamento inesperado com o próprio Cypress. Nessa situação, recomendamos verificar nosso Guia de solução de problemas .

Mais informações
Freqüentemente, depurar um teste Cypress com falha significa entender melhor como seu próprio aplicativo funciona e como o aplicativo pode competir com os comandos de teste. Recomendamos a leitura dessas postagens de blog, onde mostramos cenários de erro comuns e como resolvê-los:

- Quando o teste pode começar?
- Quando o teste pode parar?
- Quando o teste pode clicar?
- Quando o teste pode efetuar logout?
- Não se desvie demais