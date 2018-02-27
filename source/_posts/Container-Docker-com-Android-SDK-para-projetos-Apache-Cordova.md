---
title: Container Docker com Android SDK para projetos Apache Cordova
date: 2018-02-26 17:29:24
header-img: /img/bannerandroid.jpg
tags:
- Apache Cordova
- Docker
- Container
- Ambiente Moble
- MobileUI
---

Preparar todo o ambiente de desenvolvimento do Android SDK para criar aplicativos utilizando Apache Cordova nem sempre é fácil, [neste post: Instalando e configurando Android SDK para projetos Apache Cordova](/2018/02/14/Instalando-e-configurando-Android-SDK-para-projetos-Apache-Cordova/) explico como fazer passo a passo porem se você utiliza Docker no dia a dia sem dúvida será mais fácil utilizar uma imagem para executar essa tarefa.

Utilizar o Docker com Android SDK facilita também na criação de builds automatizadas, que é o meu caso. No projeto [PlugMobile](http://plugmobile.com.br/) precisei criar uma automatizador de build pois cada cliente desta plataforma teria seu próprio aplicativo com sua cor/logo/marca.

Para utilizar a imagem basta executar o docker:

```
docker container run -it -v /home/fabiorogeriosj/workspace:/workspace fabiorogeriosj/android-sdk
``` 

No comando acima estou montando um volume para codar no ambiente físico e apenas utilizar o container para buildar.

Neste container tem instalado:

- NodeJS (v6.11.4)
- NPM (3.5.2)
- Apache Cordova (8.0.0)
- MobileUI (0.1.17)
- Android SDK (build-tools [27.0.3], platforms [26,27])
- Gradle (3.4.1)
- Open JDK 8
- Git, curl, wget, unzip e vim


Imagem em [Docker Hub](https://hub.docker.com/r/fabiorogeriosj/android-sdk/).
