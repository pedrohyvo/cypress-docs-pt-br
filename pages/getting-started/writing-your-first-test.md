# Escrevendo o Primeiro Teste
```
O que você vai aprender

- Como começar a testar um novo projeto com Cypress
- Como são reportados testes com sucesso e falhas
- Testando a navegação web, buscando por elementos no DOM e escrevendo asserções
```

[Vídeo de exemplo.](https://vimeo.com/237115455)

# Adicione um arquivo de teste

Assumindo que você tenha [instalado](pages/getting-started/installing-cypress.md#instalando) e [aberto](pages/getting-started/installing-cypress.md#abrindo-o-cypress) o Cypress Test Runner, chegou a hora de escrever o primeiro teste. Vamos então:

1. Criar um arquivo `sample_spec.js`.
2. Checar que o Cypress atualizou a lista de arquivos de teste.
3. Executar o Cypress Test Runner.

Vamos criar um novo arquivo na pasta `cypress/integration`, a qual já foi criada:

```touch {your_project}/cypress/integration/sample_spec.js```

Uma vez que o arquivo tenha sido criado, o Cypress Test Runner deve exibi-lo na lista de testes de integração. O Cypress monitora mudanças em seus arquivos de teste e os exibe imediatamente.

Mesmo que ainda não tenha escrito nenhum teste - tudo bem - clique em `sample_spec.js` e veja o Cypress iniciar o navegador.

> O Cypress abre o teste em um navegador previamente instalado em seu sistema operacional. Você pode ler mais sobre como isso é feito em Iniciando Navegadores.

[Vídeo de exemplo](https://docs.cypress.io/img/snippets/empty-file-30fps.mp4)

[//]: <> (TODO - Adicionar link Test Runner quando for traduzido)
Agora estamos oficialmente no [Test Runner](https://docs.cypress.io/guides/core-concepts/test-runner.html). É nele onde vamos gastar a maioria do tempo testando.

> Note que o Cypress exibe a mensagem que não pode encontrar nenhum teste. Isso é normal - testes ainda não foram escritos! Você verá essa mensagem também caso exista algum erro ao analisar os arquivos de teste. Você pode abrir o Dev Tools para inspecionar o console e buscar por erros de sintaxe que impediu o Cypress de ler seus arquivos de testes.

# Escreva seu primeiro teste

Chegou a hora de escrever o nosso primeiro teste. Vamos então:

1. Escrever nosso primeiro teste com sucesso.
2. Escrever nosso primeiro teste com falha.
3. Ver o Cypress recarregar o teste em tempo real.

A medida que salvamos nosso novo teste, veremos que o navegador recarrega automaticamente em tempo real.

Abra sua IDE favorita e adicione o código abaixo no arquivo de teste `sample_spec.js`.

```
describe('My First Test', () => {
  it('Does not do much!', () => {
    expect(true).to.equal(true)
  })
})
```

Quando salvar esse arquivo você deve ver o navegador recarregar.

Embora não faça nada, esse é nosso primeiro teste com sucesso!

[//]: <> (TODO - Adicionar link Test Runner quando for traduzido)
No [log de comandos](https://docs.cypress.io/guides/core-concepts/test-runner.html#Command-Log) você vai ver que o Cypress exibe a suíte de teste, o teste e a primeira asserção (que deve estar em verde - sucesso).

![Primeiro teste com sucesso](https://docs.cypress.io/img/guides/first-test.88031830.png)

[//]: <> (TODO - Adicionar links Test Runner e Visit Command quando forem traduzidos)
> Observe que o Cypress exibe uma mensagem sobre essa ser a página padrão [ao lado direito](https://docs.cypress.io/guides/core-concepts/test-runner.html#Application-Under-Test). Cypress assume que você irá sair e [visitar](https://docs.cypress.io/api/commands/visit.html) outra página na internet - mas também pode funcionar bem sem isso.

Agora vamos escrever nosso primeiro teste com falha.

```
describe('My First Test', () => {
  it('Does not do much!', () => {
    expect(true).to.equal(false)
  })
})
```

Quando você salvar, poderá observar que o Cypress exibe o teste com falha em vermelho, uma vez que `true` não é igual a `false`.

[//]: <> (TODO - Adicionar links File Opener Preference e Debugging Errors quando forem traduzidos)
Cypress também exibe o log do erro e o pedaço do código onde a asserção falhou (quando disponível). Você pode clicar no link do arquivo em azul para abrir o arquivo em que o erro ocorreu [em sua IDE preferida](https://docs.cypress.io/guides/tooling/IDE-integration.html#File-Opener-Preference). Para ler mais sobre exibição de erros, leia sobre [Debugando Erros](https://docs.cypress.io/guides/guides/debugging.html#Errors).

![Teste com falha](https://docs.cypress.io/img/guides/failing-test.971461e3.png)

[//]: <> (TODO - Adicionar link Test Runner quando for traduzido)
Cypress fornece um ótimo [Test Runner](https://docs.cypress.io/guides/core-concepts/test-runner.html) que exibe uma estrutura visual das suítes, testes e asserções. Em breve você verá também comandos, eventos, requisições de rede e mais.

[Vídeo do primeiro teste com falha](https://docs.cypress.io/img/snippets/first-test-30fps.mp4)

[//]: <> (TODO - Adicionar link Bundled Tools quando for traduzido)
> O que é describe, it e expect?
Todas essas funções vem das [Bundled Tools](https://docs.cypress.io/guides/references/bundled-tools.html) que fazem parte do Cypress. Describe e it vem do [Mocha](https://mochajs.org/). Expect vem do [Chai](http://www.chaijs.com/).

> Cypress utiliza dessas ferramentas e frameworks populares, os quais é esperado que você já tenha alguma familiaridade e conhecimento prévio. Mas se não tiver, tudo bem.

# Escreva um teste real
