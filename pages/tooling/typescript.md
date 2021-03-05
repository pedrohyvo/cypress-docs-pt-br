# TypeScript

O Cypress já vem com [declarações oficiais de tipo](https://github.com/cypress-io/cypress/tree/develop/cli/types) 
para [TypeScript](https://www.typescriptlang.org/). Isso permite que você escreva seus testes em TypeScript.

## Instalar TypeScript

Você precisará ter, no mínimo, a versão 3.4 do TypeScript instalada no seu projeto para ter suporte ao TypeScript no Cypress.

### Com npm

```bash
npm install --save-dev typescript
```

### Com yarn

```bash
yarn add --dev typescript
```

## Configurar seu ambiente de desenvolvimento

Encontre o seu editor de código no 
[documento de Suporte do Editor ao TypeScript](https://github.com/Microsoft/TypeScript/wiki/TypeScript-Editor-Support) 
e siga as instruções para o seu IDE ter o suporte e o 
[preenchimento inteligente de código](https://docs.cypress.io/guides/tooling/IDE-integration.html#Writing-Tests) 
do TypeScript configurados no seu ambiente de desenvolvimento antes de continuar. 
O suporte ao TypeScript já vem configurado no [Visual Studio Code](https://code.visualstudio.com/), 
[Visual Studio](https://visualstudio.microsoft.com/pt-br/) e [WebStorm](https://www.jetbrains.com/webstorm/) - os demais
editores requerem configuração extra.

## Configurar tsconfig.json

Recomendamos a seguinte configuração em um arquivo 
[`tsconfig.json`](http://www.typescriptlang.org/docs/handbook/tsconfig-json.html) 
dentro da sua pasta do [`Cypress`](https://docs.cypress.io/guides/core-concepts/writing-and-organizing-tests.html#Folder-Structure):

```bash
{
  "compilerOptions": {
    "target": "es5",
    "lib": ["es5", "dom"],
    "types": ["cypress"]
  },
  "include": [
    "**/*.ts"
  ]
}
```

O `"types"` informa para o compilador TypeScript incluir apenas definições de tipo do Cypress. Ao fazer isso, instâncias vão ser endereçadas 
onde o projeto utilizar @types/chai ou @types/jquery. Já que o Chai e jQuery são namespaces (globais), 
versões incompatíveis farão o gerenciador de pacotes (yarn ou npm) juntar e incluir múltiplas definições, causando confiltos.

> Você pode encontrar um exemplo de Jest e Cypress instalado no mesmo projeto usando um arquivo `tsconfig.json` 
separado no repositório [cyperss-io/cypress-and-jest-typescript-example](https://github.com/cypress-io/cypress-and-jest-typescript-example).

> Talvez seja necessário reiniciar o servidor TypeScript do seu IDE se a configuração acima não funcionar. Por exemplo:
>
> VS Code (dentro de um arquivo .ts ou .js):
>
>- Abra a paleta de comandos (Mac: cmd+shift+p, Windows: ctrl+shift+p)   
>- Digite "restart ts" e selecione a opção "TypeScript: Restart TS server."
>
> Se não funcionar, tente reiniciar o IDE.

## Tipos de comandos personalizados

Ao adicionar comandos personalizados no objeto `cy`, você pode adicionar manualmente seus tipos para evitar erros TypeScript.

Por exemplo, se você adicionar o comando `cy.dataCy` dentro do seu 
[`supportFile`](https://docs.cypress.io/guides/references/configuration.html#Folders-Files) assim:

```bash
// cypress/support/index.ts
Cypress.Commands.add('dataCy', (value) => {
  return cy.get(`[data-cy=${value}]`)
})
```

Então você pode adicionar o comando `dataCy` na interface global Chainable do Cypress (chamada assim pois os comandos 
são encadeados) ao criar um novo arquivo de definições TypeScript ao lado do seu 
[`supportFile`](https://docs.cypress.io/guides/references/configuration.html#Folders-Files), 
nesse caso em `cypress/support/index.d.ts`.

```bash
// em cypress/support/index.d.ts
// carrega as definições de tipo que vêm junto com o módulo Cypress
/// <reference types="cypress" />

declare namespace Cypress {
  interface Chainable {
    /**
     * Custom command to select DOM element by data-cy attribute.
     * @example cy.dataCy('greeting')
    */
    dataCy(value: string): Chainable<Element>
  }
}
```

> Um comentário JSDoc bem detalhado acima do tipo de método será muito útil para qualquer um que usar 
o seu comando personalizado.

Se os seus arquivos specs estiverem escritos em TypeScript, você deve incluir o arquivo de definição TypeScript,
`cypress/support/index.d.ts`, com o resto dos arquivos fonte.

Mesmo que o seu projeto esteja somente em JavaScript, os specs JavaScript podem saber sobre o novo comando ao 
referenciar o arquivo usando o comentário especial de `reference path` com três barras.

```bash
// a partir do seu cypress/integration/spec.ts
/// <reference path="../support/index.d.ts" />
it('works', () => {
  cy.visit('/')
  // IntelliSense e compilador TS não
  // deve reclamar sobre método desconhecido
  cy.dataCy('greeting')
})
```

**Exemplos:**

- Veja a receita de exemplo em [Adicionando Comandos Personalizados](https://github.com/cypress-io/cypress-example-recipes#fundamentals).

- Você pode encontrar um exemplo de comando personalizado escrito em TypeScript no repositório 
    [omerose/cypress-support](https://github.com/omerose/cypress-support).

- O projeto de exemplo [cypress-example-todomvc](https://github.com/cypress-io/cypress-example-todomvc#custom-commands)
    utiliza comandos personalizados para evitar código boilerplate.

## Tipos de asserções personalizadas

Se você estender asserções Cypress, você pode estender os tipos de asserções para o compilador TypeScript entender 
os novos métodos. Veja a 
[Receita: Adicionando Asserções Chai](https://docs.cypress.io/examples/examples/recipes.html#Fundamentals)
para instruções.

## Tipos de plugins

Você pode utilizar as declarações de tipo do Cypress no seu 
[arquivo de plugins](https://docs.cypress.io/guides/tooling/plugins-guide.html#Use-Cases) anotando da seguinte forma:

```bash
// cypress/plugins/index.ts

/// <reference types="cypress" />

/**
 * @type {Cypress.PluginConfig}
 */
module.exports = (on, config) => {

}
```

## Histórico

| Versão | Mudanças                                                                                                   |
|--------|------------------------------------------------------------------------------------------------------------|
| [5.0.0](https://docs.cypress.io/guides/references/changelog.html#5-0-0)  | Atualização da versão mínima do TypeScript de 2.9+ para 3.4+                                               |
| [4.4.0](https://docs.cypress.io/guides/references/changelog.html#4-4-0)  | Adicionado suporte para o TypeScript sem precisar de sua própria transpilação através de pré-processadores |

## Veja também

- [Integração do IDE](https://docs.cypress.io/guides/tooling/IDE-integration.html)

[Voltar para o topo](#TypeScript)
