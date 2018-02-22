---
title: Trabalhando com Docker container de Windows
date: 2018-02-20 13:58:02
header-img: /img/bannerwindocker.jpg
tags:
- Docker
- Windows
- Containers
- DevOps
- Registro de trabalho
---

Este é um registro de trabalho onde coloco passo a passo de como realizei uma determinada tarefa a fim de documentar, registrar e compartilhar o conhecimento adquirido!

> O objetivo dessa tarefa foi rodar uma aplicação, criada em Delphi, em um container. Devido a natureza do projeto a imagem mais limpa e pequena do windows `nanoserve` não foi bem sucedida na aplicação em questão, pois algumas dependências do windows não foram encontradas.

Caso você não conheça o básico sobre Docker e ainda não tem o ambiente preparado para trabalhar com ele, recomendo você ler o post **[Conhecendo, Instalando e Configurando Docker](/2018/02/15/Conhecendo-Instalando-e-Configurando-Docker/)**.

Um detalhe é que os container windows só roda em um host windows, pois diferentemente de uma VM o Docker utiliza os containers em camada, sendo assim ele usa o kernel do windows.

Primeira coisa precisamos alterar o Docker para utilizar os containers em windows, para isso altere o parametro seguinte:

![WinDocker](/img/windocker1.jpg)

Agora podemos fazer pull de novas imagens do Windows. Antes de rodar o projeto em Delphi (apenas console) primeiro eu criei um projeto em NodeJS e, utilizando o módulo [pkg](https://github.com/zeit/pkg), criei um executavel para poder testar a imagem do windows antes de fazer de fato o projeto final. Este projeto demo é uma simples API.

Para esse projeto demo utilizei o seguinte Dockerfile:

```
FROM microsoft/windowsservercore
WORKDIR "C:\\"
COPY app.exe .
CMD app.exe
```

E então criar a nova imagem:

```
docker image build -t app:api .
```

E por fim rodar essa imagem em um container:

```
docker container run -it --rm -p 80:8080 app:api
```

![WinDocker](/img/windocker2.jpg)

Testado e sabendo que funciona bem a imagem vou criar outra agora com o projeto definitivo, que por fim ficou assim do Dockerfile:

```
FROM microsoft/windowsservercore
COPY setupapp.exe .
RUN setupapp.exe
WORKDIR "C:\\Program Fles (x86\\App)"
COPY config.ini .
RUN PowerShell -Command "& {Set-TimeZone -Name 'E. South America Standard Time'}"
CMD appRunConsole.exe
```

Veja que é possível execurtar comandos com o PowerShell, isso é muito interessante pois no meu caso precisei alterar registros do windows, assinar certificados, etc. E então criar a nova imagem:

```
docker image build -t appwin:saas .
```

E por fim rodar essa imagem em um novo container:

```
docker container run -it --rm appwin:saas
```