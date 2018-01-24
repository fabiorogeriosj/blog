---
title: Amazon Lightsail
date: 2017-01-13 15:43:43
header-img: /img/amazonlightsail.jpg
tags:
- cloud computing
- aws amazon
- amazon lightsail
---

Para desenvolvedores iniciantes, e até mesmo experientes, nem sempre é simples e trivial trabalhar com a camada de infra das aplicações desenvolvidas. Hoje existe diversas plataformas de computação em núvem como AWS Amazon, Google Cloud, Microsoft Azure, Heroku entre outras.

Começar a trabalhar com os diferentes serviços que cada plataforma oferece pode precisar de um tempo para maturidade e práticas. Para diminuir este tempo a Amazon lançou em 2016 o serviço [Amazon Lightsail](https://amazonlightsail.com/), que é uma maneira mais fácil para começar com servidores virtuais na nuvem.

Definição do Amazon Lightsail segundo site oficial:


> Amazon Lightsail é a maneira mais fácil de começar com AWS para desenvolvedores que precisam de uma solução simples de servidor virtual privado (VPS). O Lightsail fornece aos desenvolvedores capacidade de computação, armazenamento, configurações de rede e recursos para implantar e gerenciar sites e aplicativos Web. Lightsail inclui tudo o que você precisa para lançar seu projeto rapidamente como: uma máquina virtual, armazenamento SSD, transferência de dados, gerenciamento de DNS e um IP estático - por um preço mensal baixo e previsível.

Recomendo este serviço para quem está iniciando em servidores virtuais privados, pois rápidamente você pode ter sua aplicação publicada.

## Preço

O Serviço trabalha com planos, onde cada plano tem suas limitações de memória, processador, espaço em disco SSD e tranferência. Você pode ver os planos no [site oficial](https://amazonlightsail.com/pricing/).

## Prática

Podemos fazer três coisas neste serviço sendo:
- **Criar uma nova instância**: Maquinas virtuais utilizando Linux.
- **Criar IP estático**: Para facilitar a troca de servidores sem redefinir o IP.
- **Gerenciar DNS**: Gerenciar os DNS dos domónios.

Vamos criar um servidor para rodar NodeJS e criar um domínio para responder a este servidor.

Em "Create instance" podemos escolher em instalar a maquina já com algum app instalado (exemplo wordpress) ou instalar um servidor apenas com o S.O. Vamos escolher Node.js:

![](/img/amazonlightsail1.jpg)

Em seguida definimos o plano, se necessário você pode trocar a área da região e escolher um nome para sua instancia:

> No começo de 2017 apenas a região Norte da Virginia estava em operação para este serviço.

![](/img/amazonlightsail2.jpg)

Ao clicar em "Create" aguarde alguns minutos até o processo liberar sua instância. Você pode gerenciar a maquina tanto por SSH direto pelo browser ou baixar a chave de acesso (pem) e acessar via terminal (linux) ou via putty (windows).

Se você acessar seu PUBLIC IP verá que já esta rodando um site (html apresentação bitnami) na porta 80, iremos parar este serviço para que nossa aplicação rode nesta porta.

Agora vamos criar um domínio voltando ao dashboard inicial e clicando em "Create other resources" > DNS Zone > Digite o nome do seu domínio. Vou utilizar um domínio que tenho para teste:

![](/img/amazonlightsail3.jpg)

Ao criar você poderá ver o "Nameservers" que deverá inserir no provedor do seu domínio, no meu caso preenchi os 4 nameservers no Godaddy.

Vamos também criar outros "Records" para direcionar nosso domínio a nova instância:

![](/img/amazonlightsail4.jpg)

Após aguardar alguns minutos, até o DNS ser propagado você já poderá ver seu servidor rodando no domínio configurado (página html demo).

Nosso objetivo neste post é criar um servidor em NodeJS utilizando o express com SSL (https). Todos os meus posts focam em utilizar SSL pois hoje já é possível criar SSL de graça e a segurança de sua aplicação é tão importante quanto a regra de negócio da mesma.

Para criar um certificado SSL free veja este post [(Criar certificado SSL (HTTPS) com Let's encrypt)](/2017/01/11/Criar-certificado-SSL-HTTPS-com-Let-s-encrypt/), em seguida vamos conectar via SSH em nossa instância:

![](/img/amazonlightsail5.jpg)

Ao conectar você verá o terminal, vamos então logar com usuário root para dar um stop do servidor que está rodando na porta 80, para isso digite os comandos abaixo:

```
sudo su -
cd /opt/bitnami/
./ctlscript.sh stop apache
```

Se você testar o domínio configurado, no meu caso http://bugph.com, vai ver que não está mais funcionando, vamos então criar nosso simples app com NodeJS. Vá até a pasta `/opt` e crie uma pasta chamada `appweb` em seguida entre nesta pasta e instale o módulo `express` do NodeJS:

```
cd /opt
mkdir appweb
cd appweb
npm install express
```

Depois de instalado crie um arquivo `app.js` com o seguinte código:

``` javascript
var fs = require('fs');
var http = require('http');
var https = require('https');
var privateKey  = fs.readFileSync('./domain-key.txt', 'utf8');
var certificate = fs.readFileSync('./domain-crt.txt', 'utf8');

var credentials = {key: privateKey, cert: certificate};
var express = require('express');
var app = express();

function ensureSecure(req, res, next){
  if(req.secure){
    return next();
  };
  res.redirect('https://' + req.hostname + req.url);
};

app.all('*', ensureSecure);

app.get('/', function(req, res){
  res.send('<h2>Bugph rodando com SSL</h2>');
})

var httpServer = http.createServer(app);
var httpsServer = https.createServer(credentials, app);

httpServer.listen(80);
httpsServer.listen(443);
```

Na linha 4 e 5 estamos carregando o arquivo do certificado SSL, se você seguiu [este post](/2017/01/11/Criar-certificado-SSL-HTTPS-com-Let-s-encrypt/) sabe o que estou falando :). Os certificados devem estar na mesma pasta do `app.js`.

O método `ensureSecure()` foi criado para servir de redirecionador caso o usuário acessou nosso site usando `http://`.

Na linha 18 está fornçando que todas as requisições primeiro executa a função de validação.

Na linha 20 até 22 temos uma rota simples que quando acessado (`bugph.com/`) irá retornar um simples `h2` com uma mensagem.

As demais linhas são para criar o server e rodar tanto na porta 80 quando na porta 443 (https).

Feito isso você já pode testar rodando o script:

```
node app.js
```

Se tudo deu certo e nada deu errado :) você deve ter visto o resultado parecido com o meu:

![](/img/amazonlightsail6.jpg)

---

Em sua instância, No Amazon Lightsail, você também pode verificar alguns monitoramentos como uso de CPU, rede e status:

![](/img/amazonlightsail7.jpg)

Pode verificar dados de seu IP e as regras do Firewall, por exemplo se você for rodar uma aplicação na porta 8080 será necessário adicionar uma nova regra no firewall:

![](/img/amazonlightsail8.jpg)

E também pode criar snapshot, que é um backup de sua instância que poderá ser recuperado caso necessário. Este serviço gera custo conforme descrito no [site oficial do Amazon Lightsail](https://amazonlightsail.com/features/):

> Proteja seus dados, clone seu servidor com Snapshot Lightsail. Crie e gerencie Snapshot por $0,05/GB por mês.

E concluindo os recursos da instância você pode ver o histórico de alterações na mesma e excluir difinitivamente sua maquina.
