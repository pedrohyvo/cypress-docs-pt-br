{::options parse_block_html="true" /}

# O Executor de Testes

```markdown
O que você vai aprender

- Os nomes e propósitos das partes visuais do Executor de Testes do Cypress
- Como utilizar a Área de Seletores para selecionar os elementos da sua página

```

## Visão Geral

O Cypress executa testes em um executor interativo exclusivo que permite visualizar os comandos a medida que são
executados, enquanto apresenta a aplicação em teste.

![gui-runner](https://docs.cypress.io/_nuxt/img/gui-diagram.dd71ece.png)

## Log de Comando

O lado esquerdo do Executor de Testes é uma representação visual da sua suíte de teste.
Cada bloco de teste é devidamente aninhado e cada teste, quando clicado, apresenta todo comando do Cypress e asserção
executados dentro do bloco de teste, bem como qualquer comando e asserção executados em ganchos `before`, `beforeEach`,
`afterEach` e `after` relevantes.

![command-log](https://docs.cypress.io/_nuxt/img/command-log.ca50c73.png)

### Abra arquivos na sua IDE

Há alguns locais no Log de Comando que apresentam um link para o arquivo relevante onde o código está localizado.
Clicar neste link abrirá o seu [abridor de arquivo preferido](../tooling/ide-integration.md#Preferência-do-Abridor-Arquivo).

![file-opener](https://docs.cypress.io/_nuxt/img/open-file-in-IDE.f53270c.gif)

### Pairando sobre os Comandos

Cada comando e asserção, quando pairado sobre, restaura a Aplicação em Teste (lado direito do Executor de Testes) para o
estado que ela estava quando o comando foi executado.
Isto permite que você possa "viajar no tempo" de volta para estados anteriores da sua aplicação quando está sendo testada.

> Por padrão, o Cypress mantém snapshots e dados de comando de 50 testes para viagem no tempo. Se você está percebendo
> um consumo extremamente alto de memória no seu navegador, você pode diminiuir o `numTestsKeptInMemory` na sua [configuração](../references/configuration.md#Global)

### Clicando nos comandos

Cada comando, asserção, ou erro, quando clicado, apresenta informação extra no console de ferramentas de desenvolvedor.
Clicar também fixa a Aplicação em Teste (lado direito do Executor de Testes) no seu estado anterior à execução do comando.

![pin-state](https://docs.cypress.io/_nuxt/img/clicking-commands.4293de4.png)

## Erros

O Cypress imprime vários pedaços de informação quando um erro ocorre durante o teste do Cypress.

1. **Nome do erro:** é o tipo do erro (por exemplo, AssertionError, CypressError);

2. **Mensagem do erro:** geralmente informa o que houve de errado. Ela pode variar em extensão.
Algumas são curtas como no exemplo, enquanto outras são longas, e podem dizer exatamente como corrigir o erro;

3. **Saiba mais:** algumas mensagens de erro contêm um link "Saiba mais" (*Learn more*) que irá te levar para
a documentação relevante do Cypress;

4. **Arquivo de corpo de código:** normalmente é a linha no topo da pilha de rastreamento e apresenta o arquivo, número
da linha e o número da coluna que é destacado no quadro abaixo. Clicando neste link abrirá o arquivo no seu 
[abridor de arquivo preferido](../tooling/ide-integration.md#Preferência-do-Abridor-Arquivo) e destaca a linha e coluna
nos editores que o suportam;

5. **Corpo de código:** apresenta um fragmento do código onde a falha ocorreu, destacando a linha e coluna relevantes;

6. **Visualizar pilha de rastreamento:** clicando nisto, alterna a visibilidade da pilha de rastreamento.
As pilhas de rastreamento variam em extensão. Clicando no caminho do arquivo em azul abrirá o arquivo no
seu [abridor de arquivo preferido](../tooling/ide-integration.md#Preferência-do-Abridor-Arquivo);

7. **Botão imprimir no console:** clique nele para imprimir o erro completo na sua *DevTools*.
Isto, normalmente, permitirá que você clique nas linhas da pilha de rastreamento e abra arquivos na sua *DevTools*.

![command-error](https://docs.cypress.io/_nuxt/img/command-failure-error.35fd85e.png)

## Painel de Instrumento 

Para determinados comandos como `cy.intercept()`, `cy.stup()` e `cy.spy()`, um painel de instrumento extra
é apresentado em cima do teste para dar mais informação sobre o estado dos seus testes.

### Rotas

![instrument-panel-routes](https://docs.cypress.io/_nuxt/img/instrument-panel-routes.d14b1f7.png)

### Stubs

![instrument-panel-stubs](https://docs.cypress.io/_nuxt/img/instrument-panel-stubs.ebe300b.png)

### Espiões

![instrument-panel-spies](https://docs.cypress.io/_nuxt/img/instrument-panel-spies.6e59da9.png)

## Aplicação em Teste

O lado direito do Executor de Testes é usado para apresentar a Aplicação em Teste (AeT):
a aplicação que foi navegada utilizando um `cy.visit()` ou qualquer chamada de roteamento subsequente feito
da aplicação visitada.

No exemplo abaixo, nós escrevemos o seguinte código no nosso arquivo de teste:

```js
cy.visit('https://example.cypress.io')

cy.title().should('include', 'Kitchen Sink')
```

Na Visualização da Aplicação correspondente abaixo, você pode ver que `https://example.cypress.io` está sendo apresentada
no lado direito. A aplicação não está apenas visível, mas também completamente interativa.
Você pode abrir sua ferramenta de desenvolvedor para inpecionar elementos da mesma forma que você faria na sua aplicação
normal. O DOM é completamente acessível para debugar.

![app-under-test](https://docs.cypress.io/_nuxt/img/application-under-test.94535c0.png)

A AeT também é apresentado no tamanho e orientação espcificados nos seus testes. Você pode mudar o tamanho ou orientação
com o comando `cy.viewport()` ou na sua [configuração do Cypress](../references/configuration.md).
Se a AeT não encaixar dentro da janela atual do navegador, ele é dimensionado apropriadamente para encaixar dentro da janela.

Os tamanho e dimensão atuais da AeT são apresentados no canto superior direito da janela.

A imagem abaixo mostra que nossa aplicação é exibida com `100px` de largura e `660px` de altura e dimensionado para `100%`.

![viewport-scaling](https://docs.cypress.io/_nuxt/img/viewport-scaling.3fae2b7.png)

*Nota: o lado direito do Executor de Testes também pode ser usado para exibir sintaxe*
*de erros no seu arquivo de teste, impedindo que os testes de sejam executados.*

![error-syntax](https://docs.cypress.io/_nuxt/img/errors.804ae14.png)

*Nota: internamente, a AeT renderiza dentro de um iframe.*
*Isto às vezes pode causar comportamentos inesperados, [explicado aqui](https://docs.cypress.io/api/commands/window#Cypress-uses-2-different-windows).*

## Área de Seletores

A Área de Seletores é um recurso interativo que te ajuda a:

- Determinar um seletor único para um elemento;

- Visualizar quais elementos correspondem ao seletor dado;

- Visualizar qual elemento corresponde a uma string de texto.

### Singularidade

O Cypress automaticamente calculará um **seletor único** para utilizar o elemento alvo através da execução de uma série
estratégia de seletores.

Por padrão, o Cypress irá favorecer:

1. `data-cy`;

2. `data-test`;

3. `data-testid`;

4. `id`;

5. `class`;

6. `tag`;

7. `attributes`;

8. `nth-child`.

> **Isto é configurável**
> O Cypress permite que você controle como um seletor é determinado.
> Utilize [Cypress.SelectorPlayground](https://docs.cypress.io/api/cypress-api/selector-playground-api) API para controlar
> os seletores que você quer retornados.

### Melhores Práticas

Você pode encontrar-se com dificuldade para escrever bons seletores porque:

- Sua aplicação usa ID's e nomes de classe dinâmicos;

- Seu teste quebra sempre que há mudança de CSS ou conteúdo.

Para ajudar com estes desafios corriqueiros, a Área de Seletores automaticamente dá preferência a certos atributos
`data-*` quando está determinando um seletor único.

Por favor, leia nosso [Guia de Melhores Práticas](../references/best-practices.md#Selecting-Elements) para ajudá-lo
a direcionar elementos e previnir que os testes quebrem nas mudanças de CSS ou JS.

### Encontrando Seletores

Para abrir a Área de Seletores, clique no botão <img src="./icons/crosshairs.svg" alt="crosshairs icon" width="15rem"> próximo à URL no topo do executor.
Paire sobre os elementos na sua aplicação para visualizar um seletor único para o elemento na dica de contexto.

![selector-playground](https://docs.cypress.io/_nuxt/img/open-selector-playground.0d6d17f.gif)

Clique no elemento e seu seletor aparecerá no topo. A partir daí, você pode copiar para sua área de transferência <img src="./icons/copy.svg" alt="copy icon" width="15rem">
ou imprimir no console <img src="./icons/terminal.svg" alt="terminal icon" width="15rem">.

![copy-in-selector](https://docs.cypress.io/_nuxt/img/copy-selector-in-selector-playground.fe0eeb0.gif)

### Executando Experimentos

A caixa no topo que exibe o seletor é também uma caixa de entrada de texto.

#### Editando um Seletor

Quando você edita o seletor, será exibido quantos elementos são associados e destaca estes elementos na sua aplicação.

![find-selector](https://docs.cypress.io/_nuxt/img/typing-a-selector-to-find-in-playground.1b06858.gif)

#### Mudando para *Contains*

Você também pode experimentar o que `cy.contains()` resultaria dada uma string de texto.
Clique em `cy.get` e mude para `cy.contains`.

Digite um texto para ver qual elemento ele associa.
Note que `cy.contains()` resulta apenas no primeiro elemento que combina com o texto, mesmo se múltiplos elementos
na página o contêm.

![contains-selector](https://docs.cypress.io/_nuxt/img/cy-contains-in-selector-playground.578d264.gif)

#### Desabilitando Destaques

Se você quiser interagir com sua aplicação enquanto a Área de Seletores está aberta, o destaque do elemento pode atrapalhar.
Alternando o destaque para desligado permitirá que você interaja com sua aplicação com mais facilidade.

![highlight-selector](https://docs.cypress.io/_nuxt/img/turn-off-highlight-in-selector-playground.807461b.gif)

## Atalhos do Teclado

Existe atalhos do teclado para executar com ações comuns com mais rapidez de dentro do Executor de Testes.

| Tecla | Ação |
| ---- | ---- |
| `r`| Reexecuta os testes |
| `s`| Interrompe os testes |
| `f`| Dá foco na janela "specs" |

![keyboard-shortcuts](https://docs.cypress.io/_nuxt/img/keyboard-shortcuts.f94c4ac.png)

## Histórico

| Versão | Mudanças |
| ---- | ---- |
| [3.5.0](https://docs.cypress.io/guides/references/changelog#3-5-0) | Adicionado atalhos de teclado para o Executor de Testes |
| [1.3.0](https://docs.cypress.io/guides/references/changelog#1-3-0) | Adicionada a Área de Seletores |

