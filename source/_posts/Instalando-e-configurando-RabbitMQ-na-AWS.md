---
title: Instalando e configurando RabbitMQ na AWS
date: 2018-01-23 10:46:33
header-img: /img/63361735-binary-wallpapers.png
tags: 
- AWS Amazon
- RabbitMQ
- Registro de trabalho
---

Este é um registro de trabalho onde coloco passo a passo de como realizei uma determinada tarefa afim de documenentar, registrar e compartilhar o conhecimento adquirdo!

> O objetivo dessa tarefa foi entender melhor os recursos do RabbitMQ e preparar um ambiente para teste com o mesmo, ou seja, instalar e configurar o RabbitMQ em uma máquina na AWS Amazon.

## Criando uma EC2

Para disponibilizar um ambiente aberta a outras equipes, ou seja, acessível de onde eles estiverem, decidi instalar o RabbitMQ em uma [instância EC2]((http://fabiorogeriosj.com.br/2016/12/16/Amazon-EC2/)) da Amazon e deixei o IP público. Após os testes ser finalizados irei excluir essa máquina.

Nos meus projetos de testes sempre escolho a distro que a Amazon trabalhou para minha instância, nesse caso não obtive sucesso com esta versão e optei por usar o CentOS 7!

Como irei fazer um teste de carga moderado preferi utilizar o tipo da EC2 `Linux on t2.small` (2GB memória) com 30GB de volume (HD) e, neste momento, penso ser suficiente para o trabalho.

## Instalação

Conectado via SSH em minha instância, primeiro instalo o `erlang`, pois o RabbitMQ é desenvolvido nesta linguagem. No repositório padrão do `yum` sobre o `erlang` não estava compatível com a versão que o RabbitMQ precisava, pois neste momento irei instalar a versão 3.7.x do `erlang` e o mesmo precisa no mínimo da versão 19.3. Você pode ver [isso aqui](https://www.rabbitmq.com/which-erlang.html).

### Instalação do erlang

Olhando para as versões do erlang baixei o pacote para Centos 7 (64-bit) listado [aqui](https://www.erlang-solutions.com/resources/download.html).

```
sudo yum groupinstall 'Development Tools'
wget https://packages.erlang-solutions.com/erlang-solutions-1.0-1.noarch.rpm
sudo rpm -Uvh erlang-solutions-1.0-1.noarch.rpm
sudo yum install erlang
```

### Instalação do RabbitMQ

Uma vez instalado a versão correta do `erlang` podemos instalar a RabbitMQ. Neste passo precisei entrar com usuário `root` pois apenas com sudo não funcionou.

```
sudo su -
rpm --import https://dl.bintray.com/rabbitmq/Keys/rabbitmq-release-signing-key.asc
yum install rabbitmq-server-3.7.2-1.el7.noarch.rpm
```

Desta forma o RabbitMQ foi instaldo com sucesso, agora vamos habilitar um [plugin](https://www.rabbitmq.com/management.html) que disponibiliza, via HTTP, uma painel de gerenciamento e monitoramento.

```
rabbitmq-plugins enable rabbitmq_management
```

Uma vez feito isso vamos definir que sempre quando ligar a máquina o RabbitMQ já será startado e vamos iniciar o serviço do RabbitMQ estando com usuário `root`.

```
chkconfig rabbitmq-server on
/sbin/service rabbitmq-server start
```

### Configurações

Agora vamos habilitar o acesso publico, para o meu caso que irei fazer testes, do RabbitMQ, pois da forma que está eu irei conseguir acessar, via usuário de gerenciamento, apenas localhost.

Neste passo irei cria um usuário `admin`, sempre lembrando que esse é um ambiente para testar o serviço então para produção irei adotar recursos mais seguros, e definir uma senha que também será `admin`. Em seguida irei setar permissões de acesso publico e definir o mesmo como administrador.

```
rabbitmqctl add_user admin admin
rabbitmqctl set_permissions -p / admin ".*" ".*" ".*"
rabbitmqctl set_user_tags admin administrator
```

Sendo assim agora consigo acessar o portal de gerenciamento via browser, passando o ip da minha máquina e a porta: `15672`

![visão portal](/img/visaogeralportalrabbitmq.jpg)

### Exemplo de uso

Para testar o RabbitMQ implementei uma simples aplicação em NodeJS.

#### send.js

```javascript
const amqp = require('amqplib/callback_api')

amqp.connect('amqp://154.45.12.170', (err, conn) => {
    if(err) return console.error(err)    

    conn.createChannel((err, ch) => {
        var q = 'hello'

        ch.assertQueue(q, {durable: false})

        ch.sendToQueue(q, new Buffer('Hello world!'))
        console.log(' [x] Sent message.')

        setTimeout(() => {
            conn.close()
            process.exit(0)
        }, 500)
    })

})
```

#### receive.js

```javascript
const amqp = require('amqplib/callback_api')

amqp.connect('amqp://154.45.12.170', (err, conn) => {
    if(err) return console.error(err)    

    conn.createChannel((err, ch) => {
        var q = 'hello'

        ch.assertQueue(q, {durable: false})

        console.log(" [*] Waiting for messages in %s. To exit press CTRL+C", q)
        ch.consume(q, (msg) => {
            console.log(" [x] Received %s", msg.content.toString());
        }, {noAck: true})
        
    })

})
```