---
title: Extraindo cores de imagens com Ionic + Color Thief
date: 2015-12-21 22:12:00
header-img: /img/colors.jpg
tags:
- ionic
- color thief
- imagem
---

O design de interfaces é um ponto crucial para aplicações web e mobile! Neste pequeno post vou mostrar um simples código, usando uma lib em JS, para extrair as cores de uma determinada imagem.

## Demonstração:

![](/img/demoappgetcolors.gif)

Para testar em seu device com Android [baixe o .apk aqui](https://github.com/fabiorogeriosj/ionic-get-colors-image/raw/master/appGetColors/appGetColorsDemo.apk).

Estou usando uma Lib chamada [Color Thief](https://github.com/lokesh/color-thief/) escrita em JavaScript!

Para instalar esta lib basta fazer o donwload do [color-thief.min.js](https://github.com/lokesh/color-thief/raw/master/dist/color-thief.min.js) e carregar no index.html:

``` html
...
    <script src="js/ng-cordova.min.js"></script>
    <script src="cordova.js"></script>

    <!-- your app's js -->
    <script src="js/app.js"></script>
    <script src="js/color-thief.min.js"></script>
  </head>
  <body ng-app="starter">
  ...
```

Veja o trecho de código abaixo onde capturo as cores de uma determinada imagem:

``` javascript
$scope.getColors = function (){

  var colorThief = new ColorThief();
  var a = document.getElementById("imgDemo");
  if(a.src){
    var p = colorThief.getPalette(a, 5);
    $scope.palette = p;
  } else {
    alert("Take a picture first!");
  }
}
```

Você pode ver o código completo deste app [neste repositório](https://github.com/fabiorogeriosj/ionic-get-colors-image).

Dúvidas só deixar comentários! Espero que isso ajude alguem ;)
