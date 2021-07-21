# Debugando

```markdown
O que você vai aprender

- Como o Cypress é executado no mesmo loop de evento com seu código, mantendo a depuração menos exigente e mais compreensível
- Como o Cypress adota as ferramentas de desenvolvedor padrão
- Como e quando usar debugger e o [`.debug()`] (https://docs.cypress.io/api/commands/debug)


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

Vamos usar [`.then()`] (https://docs.cypress.io/api/commands/then) para acessar o comando Cypress durante a execução e adicionar um debuggerno momento apropriado:

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

Agora estamos no negócio! Na primeira vez, [`cy.visit()`](https://docs.cypress.io/api/commands/visit) o [`cy.get()`](https://docs.cypress.io/api/commands/get) cadeia (com seu [`.then()`] (https://docs.cypress.io/api/commands/then) anexo) é enfileirada para que o Cypress execute. O `it`bloco sai e o Cypress começa seu trabalho:

1 - A página é visitada e o Cypress espera que ela carregue.
2 - O elemento é consultado e Cypress automaticamente espera e tenta novamente por alguns momentos se não for encontrado imediatamente.
3 - A função passada para [`.then()`] (https://docs.cypress.io/api/commands/then) é executada, com o elemento encontrado rendido a ela.
4 - Dentro do contexto da [`.then()`] (https://docs.cypress.io/api/commands/then) função, o debugger é chamado, parando o navegador e chamando o foco para as Ferramentas do Desenvolvedor.
5 - Você está dentro! Inspecione o estado do seu aplicativo como faria normalmente se tivesse debuggerinserido o no código do aplicativo.


## Usando [`.debug()`] (https://docs.cypress.io/api/commands/debug)


Cypress também expõe um atalho para comandos de depuração Cypress também expõe um atalho para comandos de depuração [`.debug()`] (https://docs.cypress.io/api/commands/debug). Vamos reescrever o teste acima usando este método auxiliar: 

```markdown
it('let me debug like a fiend', () => {
  cy.visit('/my/page/path')

  cy.get('.selector-in-question').debug()
})
```
O assunto atual gerado pelo [`cy.get()`](https://docs.cypress.io/api/commands/get) é exposto como a variável subjectem suas Ferramentas de Desenvolvedor para que você possa interagir com ele no console.
![Alt](https://docs.cypress.io/_nuxt/img/debugging-subject.bc0098b.png)

Use [`.debug()`] (https://docs.cypress.io/api/commands/debug) para inspecionar rapidamente qualquer (ou muitas!) Parte (s) do seu aplicativo durante o teste. Você pode anexá-lo a qualquer cadeia de comandos do Cypress para dar uma olhada no estado do sistema naquele momento.

##Percorrer os comandos de teste

Você pode executar o comando de teste por comando usando o [`.pause()`] (https://docs.cypress.io/api/commands/pause) comando.

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