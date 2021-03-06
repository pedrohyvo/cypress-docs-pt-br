# Guia de Como Contribuir

## Para os novos contribuidores

1. Faça um fork do projeto.
2. Crie uma nova branch com as suas alterações: 
3. `git checkout -b nome-pagina-nome-secao` (altere nome-pagina-nome-secao de acordo com o 
nome da página ou seção que você está traduzindo).
4. Salve as alterações e crie uma mensagem de commit contando o que você fez:
`git commit -m "doc: Add Example page"`.
5. Envie as suas alterações: `git push origin nome-pagina-nome-secao`.
6. Verifique se a sua branch está atualizada com base no repositório `upstream` e submeta seu pull request!
Ex:

```bash
git fetch upstream
git checkout master
git reset --hard upstream/master  
git push origin master --force
```

> Quando estiver traduzindo qualquer conteúdo, tenha certeza que está seguindo as regras
do `markdown lint rules`


&nbsp;

## Como rodar o markdown lint localmente

**Pré-requisito: ter o node instalado na sua máquina.**

Após fazer o fork do projeto no seu repositório local você deve rodar:

```bash
npm install
node_modules/.bin/markdownlint ./ --ignore node_modules
```

Obs: para os usuários do VSCode, existe uma extensão [markdownlint](https://marketplace.visualstudio.com/items?itemName=DavidAnson.vscode-markdownlint).

## Guia de Estilos Universal

Esta seção descreve as regras que devem ser aplicadas para todas as páginas. 
Quando estiver se referindo ao próprio `Cypress`, use `o Cypress`.

### Texto em Blocos de Código

Mantenha o texto em blocos de código sem tradução, exceto para os comentários. 
Você pode optar por traduzir o texto em strings, mas tenha cuidado para não traduzir 
strings que se refiram ao código!

Exemplo:

```js
//Example
cy
  .get('#login-field')
  .should('be.visible')
// Checking if login field is visible
```

✅ FAÇA:

```js
//Exemplo
cy
  .get('#login-field')
  .should('be.visible')
// Checking if login field is visible
```

✅ PERMITIDO:

```js
//Exemplo
cy
  .get('#login-field')
  .should('be.visible')
// Checando se o campo de login é visível
```

❌ NÃO FAÇA:

```js
//Exemplo
cy
  .get('#login-campo')
// "login-field" se refere a um ID de um elemento.
// NÃO TRADUZA  
  .should('be.visible')
// Checando se o campo de login é visível
```

❌ DEFINITIVAMENTE NÃO FAÇA:

```js
//Exemplo
cy
  .get('#login-campo')
  .should('seja.visivel')
// Checando se o campo de login é visível
```
