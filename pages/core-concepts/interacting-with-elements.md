# Interagindo com Elementos

```markdown
O que você vai aprender

- Como o Cypress calcula visibilidade
- Como o Cypress garante que os elementos sejam acionáveis
- Como o Cypress lida com elementos de animação
- Como você pode ignorar essas verificações e forçar eventos
```

## Acionabilidade

Alguns comandos no Cypress são para interagir com o DOM, como:

[//]: <> (TODO - Adicionar links - comandos cypress)

- [`.click()`](https://docs.cypress.io/api/commands/click.html) 
- [`.dblclick()`](https://docs.cypress.io/api/commands/dblclick.html)
- [`.rightclick()`](https://docs.cypress.io/api/commands/rightclick.html)
- [`.type()`](https://docs.cypress.io/api/commands/type.html)
- [`.clear()`](https://docs.cypress.io/api/commands/clear.html)
- [`.check()`](https://docs.cypress.io/api/commands/check.html)
- [`.uncheck()`](https://docs.cypress.io/api/commands/uncheck.html)
- [`.select()`](https://docs.cypress.io/api/commands/select.html)
- [`.trigger()`](https://docs.cypress.io/api/commands/trigger.html)

Esses comandos simulam um usuário interagindo com sua aplicação. Por debaixo dos panos, o Cypress
dispara os eventos que um navegador dispararia, fazendo com que as associações de evento de sua aplicação sejam disparadas.

Antes de emitir qualquer um dos comandos, verificamos o estado atual do DOM e executamos algumas ações para garantir que
o elemento DOM esteja “pronto” para receber a ação.

[//]: <> (TODO - Adicionar links - configuração de timeouts e o link de asserções padrão)
O Cypress esperará que o elemento passe em todas essas verificações durante o [`defaultCommandTimeout`](https://docs.cypress.io/guides/references/configuration.html#Timeouts)
(descrito em detalhes no guia de conceito principal [`Default Assertions`](https://docs.cypress.io/guides/core-concepts/introduction-to-cypress.html#Default-Assertions)).

## Verificações e Ações Realizadas

- [`Rolar elemento para exibição.`](##rolagem)
- [`Garantir que o elemento não esteja oculto.`](##visibilidade)
- [`Garantir que o elemento não esteja desabilitado.`](##inabilidade)
- [`Garantir que o elemento não esteja desconectado.`](##desconectado)
- [`Garantir que o elemento não seja somente leitura.`](##somente-leitura)
- [`Garantir que o elemento não esteja animando.`](##animações)
- [`Garantir que o elemento não está coberto.`](##cobertura)
- [`Rolar a página se ainda estiver coberto por um elemento com posição fixa.`](##rolagem)
- [`Disparar o evento nas coordenadas desejadas.`](##coordenadas)

Sempre que o Cypress não pode interagir com um elemento, ele pode falhar em qualquer uma das etapas acima. 
Normalmente, você receberá um erro explicando por que o elemento não foi considerado acionável.

## Visibilidade

Cypress verifica muitas coisas para determinar a visibilidade de um elemento. Os cálculos a seguir levam em consideração
as traduções e transformações CSS.

### Um elemento é considerado oculto se

- O `width` ou `height` é `0`.
- A propriedade CSS (ou ancestrais) é `visibility: hidden`.
- A propriedade CSS (ou ancestrais) é `display: none`.
- A propriedade CSS é `position: fixed` e está fora da tela ou encoberta.
- Qualquer um de seus ancestrais __esconde transbordamento*__
    - E esse ancestral tem um `width` ou `height` de `0`
    - E um elemento entre esse ancestral e o elemento é `position: absolute`
- Qualquer um de seus ancestrais __esconde transbordamento*__
    - E esse ancestral ou um ancestral entre ele e esse ancestral é seu pai deslocado
    - E está posicionado fora dos limites do ancestral
- Qualquer um de seus ancestrais __esconde transbordamento*__
    - E o elemento é `position: relative`
    - E está posicionado fora dos limites do ancestral

__* Esconde transbordamento__  significa que ele tem `overflow: hidden`, `overflow-x: hidden`, `overflow-y: hidden`, 
`overflow: scroll`, or `overflow: auto`

> **Opacidade**
>
> Os elementos onde a propriedade CSS (ou ancestrais) tem `opacity: 0` são considerados ocultos ao declarar a visibilidade
> do elemento diretamente.
> No entanto, os elementos onde a propriedade CSS (ou ancestrais) tem `opacity: 0` são considerados acionáveis ​​e quaisquer
> comandos usados ​​para interagir com o elemento oculto realizarão a ação.

## Inabilidade

O Cypress verifica se a propriedade `disabled` de um elemento é `true`.

## Desconectado

Quando muitos aplicativos renderizam o DOM, na verdade eles removem o elemento DOM e inserem um novo elemento
DOM em seu lugar com os atributos recém-alterados.

O Cypress verifica se um elemento que você está fazendo asserções está separado do DOM. Isso verifica se o elemento ainda
está dentro `document` da aplicação em teste.

## Somente Leitura

[//]: <> (TODO - Adicionar link do comando .type)

O Cypress verifica se a propriedade `readonly` de um elemento é definida durante [`.type()`](https://docs.cypress.io/api/commands/type.html).

## Animações

O Cypress irá automaticamente determinar se um elemento está sendo animado e esperará até que ele pare.

Para calcular se um elemento está animado, pegamos uma amostra das últimas posições em que ele estava e calculamos a 
inclinação do elemento. Você deve se lembrar disso da álgebra da 8ª série. 😉

[//]: <> (TODO - Adicionar link - configuração de acionabilidade)
Para calcular se um elemento está animado, verificamos as posições atual e anterior do próprio elemento. Se a distância
exceder o [`animationDistanceThreshold`](https://docs.cypress.io/guides/references/configuration.html#Actionability), então
consideramos o elemento como uma animação.

[//]: <> (TODO - Adicionar link - configuração de acionabilidade)
Ao chegar a esse valor, fizemos alguns experimentos para encontrar uma velocidade que “parece” muito rápida para um usuário
interagir. Você sempre pode [`aumentar ou diminuir esse limite`](https://docs.cypress.io/guides/references/configuration.html#Actionability).

[//]: <> (TODO - Adicionar link - configuração de acionabilidade)
Você também pode desligar nossas verificações de animações com a opção de configuração [`waitForAnimations`](https://docs.cypress.io/guides/references/configuration.html#Actionability).

## Cobertura

Também garantimos que o elemento com o qual estamos tentando interagir não seja coberto por um elemento pai.

Por exemplo, um elemento pode passar por todas as verificações anteriores, mas uma caixa de diálogo gigante pode estar 
cobrindo toda a tela, tornando a interação com o elemento impossível para qualquer usuário real.

> Ao verificar se o elemento está coberto, sempre verificamos suas coordenadas centrais.

Se um elemento filho o estiver cobrindo, tudo bem. Na verdade, emitiremos automaticamente os eventos que dispararmos 
para o elemento filho.

Imagine que você tem um botão:

```HTML
<button>
  <i class="fa fa-check">
  <span>Submit</span>
</button>
```

[//]: <> (TODO - Adicionar links - Log de comandos)
Muitas vezes, o elemento `<i>` ou `<span>` está cobrindo a coordenada exata com a qual estamos tentando interagir. 
Nesses casos, o evento dispara no elemento filho. Nós até anotamos isso para você no [`Log de Comandos`](https://docs.cypress.io/guides/core-concepts/test-runner.html#Command-Log).

## Rolagem

Antes de interagir com um elemento, *sempre* o rolaremos para a visualização (incluindo qualquer um de seus contêineres 
pais). Mesmo se o elemento estivesse visível sem rolagem, executamos o algoritmo de rolagem para reproduzir o mesmo 
comportamento toda vez que o comando for executado.

[//]: <> (TODO - Adicionar links dos comandos de Acionabilidade, cy.get e .find)
> Essa lógica de rolagem se aplica apenas aos [`comandos acionáveis ​​acima`](https://docs.cypress.io/guides/core-concepts/interacting-with-elements.html#Actionability).
> **Não rolamos os elementos** para a visualização ao usar comandos DOM, como [`cy.get()`](https://docs.cypress.io/api/commands/get.html)
ou [`.find()`](https://docs.cypress.io/api/commands/find.html).

Por padrão, o algoritmo de rolagem funciona rolando o ponto superior mais à esquerda do elemento no qual emitimos o 
comando para o ponto de rolagem superior e mais à esquerda de seu contêiner rolável.

Depois de rolar o elemento, se determinarmos que ele ainda está sendo encoberto, continuaremos a rolar e “empurrar” a 
página até que ela se torne visível. Isso acontece frequentemente quando você tem `position: fixed` ou 
`position: sticky` elementos de navegação que são fixados para o topo da página.

Nosso algoritmo *deve* sempre ser capaz de rolar até que o elemento não seja coberto.

[//]: <> (TODO - Adicionar links - configuração de acionabilidade)
Para alterar a posição na janela de visualização para onde rolamos um elemento, você pode usar a opção de 
configuração [`scrollBehavior`](https://docs.cypress.io/guides/references/configuration.html#Actionability). Isso pode 
ser útil se o elemento for coberto quando alinhado ao topo da janela de visualização ou se você apenas preferir que o 
elemento seja centralizado durante a rolagem dos comandos de ação. Os valores aceitos são `'center'`, `'top'`, 
`'bottom'`, `'nearest'`, e `false`, com `false` a rolagem é desativada completamente.

## Coordenadas

Depois de verificarmos se o elemento é acionável, o Cypress disparará todos os eventos apropriados e as ações padrão 
correspondentes. Normalmente, as coordenadas desses eventos são disparadas no centro do elemento, mas a maioria dos 
comandos permite que você altere a posição para a qual são disparadas.

```JS
cy.get('button').click({ position: 'topLeft' })
```

[//]: <> (TODO - Adicionar links - Log de comandos)
As coordenadas nas quais disparamos o evento geralmente estarão disponíveis ao clicar no comando no [`Log de Comandos`](https://docs.cypress.io/guides/core-concepts/test-runner.html#Command-Log).

![coords](https://docs.cypress.io/img/guides/coords.713642d1.png)

Além disso, exibiremos um “hitbox” vermelho - que é um ponto que indica as coordenadas do evento.

![cypress hitbox](https://docs.cypress.io/img/guides/hitbox.fb2d0b28.png)

## Depurando

Pode ser difícil depurar problemas quando os elementos não são considerados acionáveis ​​pelo Cypress.

Embora você *deva* ver uma bela mensagem de erro, nada se compara a inspecionar visualmente e cutucar o DOM você mesmo 
para entender o motivo.

[//]: <> (TODO - Adicionar links - Log de comandos)
Ao usar o [`Log de comandos`](https://docs.cypress.io/guides/core-concepts/test-runner.html#Command-Log) para passar o 
mouse sobre um comando, você notará que sempre rolaremos o elemento ao qual o comando foi aplicado para exibição. 
Observe que isso *NÃO* está usando os mesmos algoritmos que descrevemos acima.

[//]: <> (TODO - Adicionar links dos comandos de Acionabilidade, cy.get e .find)
Na verdade, só rolamos elementos para exibição quando comandos acionáveis ​​estão sendo executados usando os algoritmos
acima. Nós *não* rolamos elementos com os comandos DOM regulares como [`cy.get()`](https://docs.cypress.io/api/commands/get.html)
ou [`.find()`](https://docs.cypress.io/api/commands/find.html).

O motivo pelo qual rolamos um elemento para exibição ao passar o mouse sobre um comando executado é para ajudá-lo a ver quais
elementos foram encontrados por aquele comando correspondente. É um recurso puramente visual e não reflete
necessariamente a aparência de sua página quando o comando foi executado.

Em outras palavras, você não pode obter uma representação visual correta do que Cypress “viu” ao olhar para um
estado anterior.

A única maneira de você “ver” e depurar porque Cypress pensou que um elemento não estava visível é usar uma instrução `debugger`.

[//]: <> (TODO - Adicionar link - debug)
Recomendamos colocar `debugger` ou usar o comando [`.debug()`](https://docs.cypress.io/api/commands/debug.html) 
diretamente ANTES da ação.

Garantir de que suas Ferramentas de Desenvolvedor estejam abertas e você possa chegar bem perto de “ver” os cálculos
que o Cypress está realizando.

[//]: <> (TODO - Adicionar links - catálogo de eventos)
Você também pode [`vincular a eventos`](https://docs.cypress.io/api/events/catalog-of-events.html) que o Cypress dispara
enquanto trabalha com seu elemento. Usar um depurador com esses eventos lhe dará uma visão baixo nível de
como o Cypress funciona.

```JS
// break on a debugger before the action command
cy.get('button').debug().click()
```

## Forçando

Embora as verificações acima sejam muito úteis para encontrar situações que impediriam seus usuários de interagir com os
elementos - às vezes, eles podem atrapalhar!

Às vezes, não vale a pena tentar “agir como um usuário” para que um robô execute as etapas exatas que um usuário faria
para interagir com um elemento.

Imagine que você tenha uma estrutura de navegação aninhada onde o usuário deve passar o mouse e mover o mouse em um 
padrão muito específico para alcançar o link desejado.

Vale a pena tentar replicar durante o teste?

Talvez não! Para esses cenários, oferecemos uma saída de emergência para contornar todas as verificações acima e forçar 
a ocorrência de eventos!

Você pode passar `{ force: true }` para a maioria dos comandos de ação.

```JS
// force the click and all subsequent events
// to fire even if this element isn't considered 'actionable'
cy.get('button').click({ force: true })
```

> **Qual é a diferença?**
> Quando você força um evento a acontecer, nós iremos:
>
> - Continuar a realizar todas as ações padrão
> - Acionar o evento à força no elemento
>
> NÃO iremos:
>
> - Rolar o elemento para a view
> - Garantir de que está visível
> - Garantir de que não está desativado
> - Garantir de que não está desanexado
> - Garantir de que não seja somente leitura
> - Garantir de que não está animando
> - Garantir de que não está coberto
> - Dispare o evento em um descendente

Em resumo, `{ force: true }` pula as verificações e sempre dispara o evento no elemento desejado.

[//]: <> (TODO - Adicionar links - comando select e issues)
> *forçar `.select()` opções desabilitadas*
> Passar `{ force: true }` para [`.select()`](https://docs.cypress.io/api/commands/select.html) não substituirá as 
> verificações de ação para selecionar uma `<option>` desabilitada ou uma opção dentro de um `<optgroup>` desabilitado.
> Veja [`este problema`](https://github.com/cypress-io/cypress/issues/107) para mais detalhes.

[Voltar para o topo](#interagindo-com-elementos)
