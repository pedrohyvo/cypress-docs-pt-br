# TypeScript

O Cypress vem com [declarações oficiais de tipo](https://github.com/cypress-io/cypress/tree/develop/cli/types) 
para [TypeScript](https://www.typescriptlang.org/). Isso te permite escrever seus testes em TypeScript.

## Instalar TypeScript

Você precisará ter instalado no mínimo a versão 3.4 do TypeScript no seu projeto para ter suporte TypeScript dentro do Cypress.

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
O suporte ao TypeScript já vem embutido no [Visual Studio Code](https://code.visualstudio.com/), 
[Visual Studio](https://visualstudio.microsoft.com/pt-br/) e [WebStorm](https://www.jetbrains.com/webstorm/) - os demais
editores requerem configuração extra.

## Configurar tsconfig.json

Recomendamos a seguinte configuração em um arquivo 
[`tsconfig.json`](http://www.typescriptlang.org/docs/handbook/tsconfig-json.html) 
dentro da sua pasta do [`Cypress`](https://docs.cypress.io/guides/core-concepts/writing-and-organizing-tests.html#Folder-Structure)
