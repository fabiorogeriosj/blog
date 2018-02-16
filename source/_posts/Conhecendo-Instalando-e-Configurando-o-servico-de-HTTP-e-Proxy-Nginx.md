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

## Configuração básica Nginx

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

Para aplicações que precisam de uma performance alta geralmente utilizamos recursos de balanceamento de carga conhecido como `load balancing`. Essa tarefa no eginx é simples e fácil de configurar.

No meu cenário tenho uma API em NodeJS rodando em outro servidor em duas portas diferentes, sendo assim meu config ficou assim:

```
upstream backend {
    server 34.238.82.235:8080 weight=1;
    server 34.238.82.235:8090 weight=2;
}

server {
    root /home/ubuntu/www;
    location / {
        proxy_pass http://backend;
    }

    location /cep {
        proxy_pass https://viacep.com.br/ws;
    }
}

```

O parametro `wight` define um peso para cada servidor, ou seja, o servidor `34.238.82.235:8090` irá receber duas vezes a mais a quantidade de requisição.

É possível fazer a mesma coisa com conexões TCP, por exemplo, banco de dados. No meu cenário tenho três máquinas com o banco `MySQL` e os mesmo sincronizando os dados entre eles.

```
stream {
    upstream mysql_read {
        server 34.238.82.235:3306 weight=5;
        server 34.238.82.222:3306;
        server 34.238.82.154:3306 backup;
    }
    server {
        listen 3306;
        proxy_pass mysql_read;
    }
}
```

Perceba que o primeiro servidor `34.238.82.235` irá ter 5 vezes mais de acesso, e o servidor `34.238.82.154` foi marcado como `backup`, isso significa que ele só será acessível pelo balanciamento caso os outros dois servidores estiver offline.

## Métodos de balanceamento 

No nginx existem cinco métodos de balanceamentos cada um com seu algortimo específico:

- `Round robin`
-- Este é o método padrão quando não definido outro, seu objetivo é distribuir por igual, lenvando em consideração o parametro `weight`, entre os servidores.
- `Least connections`
-- Este método leva em consideração a numero de conexões em aberto com os servidores e assim direciona as requisições para aquele que tiver o menor número de conexões em aberto.
- `Least time`
-- O melhor na minha opnião porem este método só existente na versão Plus do nginx (versão comercial paga), seu objetivo é semelhante o `Least conections` porem leva em consideração o tempo de resposta dos servidores.
- `Generic hash`
-- Bem útil para conhecer de onde vem as conexões, pois para cada ciclo de servidores é criado uma hash que pode ser definido por você ou gerada pelo nginx.
- `IP hash`
-- Apenas para HTTP este método tem contorle para todas as requisições vindas do mesmo ip de entrada ser servido pelo mesmo servidor.

## Limite de conexões

Este recurso está ativo apenas na versão Plus do nginx, com ele é possível adicionar o `max_conns` em um ou  mais servidores e controlar o limite de requisição para o mesmo, exemplo:

```
upstream api {
    server 34.238.82.235:8080 max_conns=25;
    server 34.238.82.125:8080 max_conns=15;
}
```

## Autenticação básica

Nginx vem com alguns recursos de autenticação, desde o básico até OpneID. Neste cenário estarei utilizando o modelo Basic Authorization que consiste em um usuário e senha.

Primeiro preciso ecriptografar uma senha, para isso utilizo o openssl, que após executado exibe a senha no terminal:

```
openssl passwd senhatemp123
```

![nginx](/img/nginx2.jpg)

Em seguida criei o arquivo `/etc/nginx/conf.d/passwd` com o seguinte conteudo:

```
fabio:7nBTuyXqNB/Vk
```

Agora nas configurações do egnix `/etc/nginx/conf.d/proxyapi.conf` adicione as duas linhas sobre autenticação:

```
...

location / {
    auth_basic "Private site";
    auth_basic_user_file conf.d/passwd;
    proxy_pass http://backend;
}

...
```

Algora se eu tentar acessar via browser ou via requisição é preciso enviar a autenticação:

![nginx](/img/nginx3.jpg)

![nginx](/img/nginx4.jpg)

Observe que o Value para Authorization é a contactenação de `Basic` + Base64 do `usuario:senha`.