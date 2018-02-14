---
title: 'Conhecendo, Instalando e Configurando o serviço de HTTP e Proxy Nginx'
date: 2018-02-14 16:57:12
header-img: /img/bannereginx.jpg
tags: 
- Nginx
- HTTP service
- Proxy
- AWS Amazon
- Registro de trabalho
---

Este é um registro de trabalho onde coloco passo a passo de como realizei uma determinada tarefa afim de documenentar, registrar e compartilhar o conhecimento adquirdo!

> O objetivo dessa tarefa foi entender, de forma aprofundada, os recursos que o Eginx disponibiliza e fazer a instalação para um ambiente crítico.

## Enginer X (Nginx)

Nginx é um serviço de HTPP e proxy reverso, com esse é possível controlar proxy para serviço de email e TCP/UDP. Criado inicialmente pelo [Igor Sysoev](http://sysoev.ru/en/). 

O código e distribuição está sobre o [2-clause BSD-like license.](https://nginx.org/LICENSE) e o mesmo contem uma opção para de suporte que pode ser conhecida em [Nginx, Inc.](https://www.nginx.com)

Como qualquer outro serviço de HTTP o Nginx trabalha com arquivos estáticos com recursos de auto indexação.

Para trabalhar com proxy é possível utilizar recurso de cache que acelera muito o output da requisição.

Também é possível controlar balancelamento, compactação de output (gzipping), suporte a SSL e TLS, nomes virtuais, suporte a streaming com FLV e MP4, limite de acesso, teste A/B, monitoramente de requisições entre outros recursos.

## Instalação Nginx

Para testar os recursos irei criar uma EC2 com a imagem `Ubuntu Server 16.04 LTS (HVM), SSD Volume Type - ami-bb9bd7d7` sendo uma `t2.micro`.

Conectado via SSH na EC2 com usuário `ubuntu` executei os seguintes passos:

```
sudo wget http://nginx.org/keys/nginx_signing.key
sudo apt-key add nginx_signing.key
cd /etc/apt
```

Editei o arquivo `sources.list` e adicionei essas duas linhas a seguir no final do arquivo:

```
deb http://nginx.org/packages/ubuntu xenial nginx
deb-src http://nginx.org/packages/ubuntu xenial nginx
```

Depois executei mais esses comandos:

```
sudo apt-get update
sudo apt-get install nginx
sudo service nginx start
```

Feito isso abri a porta 80 da minha EC2 via portal da AWS e o Nginx está ok:

![Nginx](/img/nginx1.jpg)

## Configuração Nginx

Para meu objetivo este seridor será apenas um proxy, servindo como um Gateway de entradas de requisição, sendo assim vamos criar um arquivo de configuração com nossas rotas.

- Primeiro eu renomiei o arquivo `/etc/nginx/conf.d/default.conf` para `/etc/nginx/conf.d/default.conf.old`
- Em seguida criei um novo arquivo com o nome `/etc/nginx/conf.d/proxyapi.conf` contendo o seguinte conteúdo:

```
server {
    root /home/ubuntu/www;

    location /cep { 
        proxy_pass https://viacep.com.br/ws;
     }
}
```

- Na pasta raiz do usuário ubuntu criei uma pasta `www` e dentro dela um arquivo `index.html` contendo o seguindo código HTML:

```HTML
<!doctype html>
<meta charset="utf8">
<title>PROXY API</title>
<style>
  body { text-align: center; padding: 150px; }
  h1 { font-size: 50px; }
  body { font: 20px Helvetica, sans-serif; color: #333; }
  article { display: block; text-align: left; width: 650px; margin: 0 auto; }
  a { color: #dc8100; text-decoration: none; }
  a:hover { color: #333; text-decoration: none; }
</style>

<article>
    <h1>PROXY API!</h1>
    <div>
        <p>Esta página é direcionada para controle de Proxy da API!</p>
    </div>
</article>
```

- Agora basta reiniciar o nginx e nossa nova configuração estará ativa:

```
sudo nginx -s reload
```

Perceba que se acessar: `http://ip_da_ec2_publica/cep/87050390/json` teremos o retorna da API que foi proxiada.

## Load Balancing

Continua...