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
