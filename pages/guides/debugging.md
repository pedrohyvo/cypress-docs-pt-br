# Debugando

```markdown
O que você vai aprender

- Como o Cypress é executado no mesmo loop de evento com seu código, mantendo a depuração menos exigente e mais compreensível
- Como o Cypress adota as ferramentas de desenvolvedor padrão
- Como e quando usar debugger e o <https://docs.cypress.io/api/commands/debug>


## Usando o `debugger`

Seu código de teste Cypress é executado no mesmo loop de execução que seu aplicativo. Isto significa que tem acesso ao código em execução na página, bem como as coisas que o navegador faz disponível para você, como `document`, `window`, e `debugger`.
