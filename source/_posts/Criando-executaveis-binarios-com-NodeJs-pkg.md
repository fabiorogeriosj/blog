---
title: Criando executáveis binários com NodeJs + pkg
date: 2018-03-01 10:29:03
header-img: /img/63361735-binary-wallpapers.png
tags:
- NodeJS
- pkg
- bin
---

No momento deste post precisei executar uma rotina que interagisse com os servidores da receita federal e essa implementação já estava ok em uma aplicação escrita em NodeJS, porem como minha infra estava em Docker com Windows, devido a natureza do projeto, não queria deixar como dependência também a instalação do NodeJS dentro do container, sendo assim criei um executável binário com o projeto anulando a dependência do NodeJS.

## pkg

[pkg](https://github.com/zeit/pkg) é um módulo escrito em NodeJS que permite empacotar seu projeto NodeJS para um executável que pode ser executado sem o dependência do NodeJS instalado.

Alguns casos de uso do módulo:
- Faça uma versão comercial da sua aplicação sem fontes
- Faça uma demo / avaliação / versão de teste do seu aplicativo sem fontes
- Execute instantaneamente sua aplicação para outras plataformas
- Faça algum tipo de arquivo ou instalador auto-extraível
- Não é necessário instalar NodeJS e npm para executar o aplicativo empacotado
- Não é necessário baixar centenas de arquivos por meio da instalação npm para implantar seu aplicativo. Implemente-o como um único arquivo
- Coloque seus recursos dentro do executável para torná-lo ainda mais portátil

## Utilizando

Primeiro precisamos instalar o módulo globalmente:

```
npm install -g pkg
```

Meu package.json:

```
{
  "name": "app",
  "version": "1.0.0",
  "description": "",
  "main": "app.js",
  "scripts": {
    "start": "app"
  },
  "author": "",
  "license": "ISC",
  "dependencies": {
    "request": "^2.83.0"
  }
}
```

Tendo meu app.js implementado basta eu executar o pkg para empacotar:

```
pkg -t node6-win-x64 app.js
```

Com o comando acima será gerado um executável `app.exe` com o resultado da minha aplicação. Outros parametros que podem ser utilizados com o pkg:

– Gera um executável para Linux, macOS e Windows
    `$ pkg index.js`
– Lê o package.json do local segue instruções do 'bin'
    `$ pkg .`
– Gera um executável de uma versão espefícia
    `$ pkg -t node6-alpine-x64 index.js`
– Gera executáveis com base nas versões escolhidas e SO
    `$ pkg -t node4-linux,node6-linux,node6-win index.js`
– Incorpora '--expose-gc' dentro do executável
    `$ pkg --options expose-gc index.js`


