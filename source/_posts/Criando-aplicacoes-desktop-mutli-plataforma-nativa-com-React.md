---
title: Criando aplicações desktop mutli-plataforma nativa com React
date: 2019-03-29 10:38:20
header-img: /img/react2.jpg
tags:
- Proton Native
- React
- desktop
- Cross platform
---

Assim como o ElectronJS, o Proton JS cria aplicações para desktop utilizando tecnologia web, neste caso é utilizado apenas o Javascript, através do React, pois os elementos de interface são nativos, um diferecial comparando com ElectronJS.

Neste artigo fiz um pequeno experimento para testar os principais componentes de interface.

> Resultado: Não gostei muito do processo de build e do resultado final, mas para aplicações simples ele se faz mais útil do que Electron, pelo fato de ser mais leve e não precisar do Chomium para Linux, ja para Windows essa dependencia ainda é necessária.

Projeto demo em: [https://github.com/fabiorogeriosj/react-desktop-proton](https://github.com/fabiorogeriosj/react-desktop-proton)

![No Linux](/img/print-proton-linux.png)
![No Windows](/img/print-proton-win.png)

## Como fiz essa demo

No Linux (Fedora):

```
sudo dnf install gtk3-devel gstreamer-devel clutter-devel webkitgtk3-devel libgda-devel gobject-introspection-devel
sudo dnf install "gtk+"
sudo dnf install gcc-c++
```

No Windows: 

```
npm install --global --production windows-build-tools
```

No Mac:

*-- Não precisa de nada, mac já tem tudo que precisa! --*


```
npm install -g create-proton-app
create-proton-app my-app
cd my-app
npm run start
```

## Para quem usa StandarJS

Para quem usa [StandarJS](https://standardjs.com) instale o `babel-eslint` para não ficar acusando erros das novas features do JS (ES Next).

```
npm install babel-eslint --save-dev
```

E adicione em seu package.json:

```
{
  "standard": {
    "parser": "babel-eslint"
  }
}
```
