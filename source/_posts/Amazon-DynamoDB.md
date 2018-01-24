---
title: Amazon DynamoDB
date: 2017-01-03 17:15:06
header-img: /img/pixelrelevo.jpg
tags:
- cloud computing
- aws amazon
- amazon dynamoDB
---

Quando se fala em aplicações que trafegam uma grande quantidade de dados por minuto é comum pensar que os bancos de dados relacionais não é a melhor escolha. Isto não é uma afirmação, tendo em vista que aplicações como Facebook, Twitter, Netflix usam banco de dados relacionais e também banco de dados não relacionais, conhecido como NoSQL, para resolver diferentes problemas em diferentes camadas de sua aplicação.

Por exemplo o aplicativo [Duolingo (aprender inglês de graça)](https://www.duolingo.com/) usa o Amazon DynamoDB para armazenar 31 bilhões de itens (lições, exercícios, comentários, usuários, etc). Ela atinge 24.000 unidades de leitura por segundo e 3.300 unidades de gravação por segundo. Além disso, o Duolingo usa uma série de outros serviços da AWS, como o [Amazon EC2](/2016/12/16/Amazon-EC2/) para computação, o Amazon ElastiCache para aumentar o desempenho, o [Amazon S3](/2017/01/02/Amazon-S3/) para armazenamento de dados relacionados à imagem eo Amazon Rational Database Service (Amazon RDS) para armazenamento permanente de dados. [Saiba mais sobre este *case* aqui](https://aws.amazon.com/pt/solutions/case-studies/duolingo-case-study-dynamodb/).

DynamoDB é um banco de dados de esquema livre que requer apenas um nome de tabela e chave primária. A chave primária da tabela é composta de um ou dois atributos que identificam itens de forma exclusiva, particionam os dados e classificam os dados dentro de cada partição.

Este serviço tem um desempenho bem rápido e estável, normalmente a latência do serviço é inferior a 10 milissegundos. Outro ponto que vale destacar é a escalabilidade, onde você gerencia de forma bem simples e fácil a capacidade que escrita/leitura por segundos em sua tabela, com isso o proprio serviço se preocupa em escalar seus dados.


### Preço

O Amazon DynamoDB permite especificar a taxa de transferência de solicitações que você deseja que sua tabela forneça (a "capacidade de taxa de transferência" da sua tabela). Nos bastidores, o serviço controla o provisionamento de recursos para alcançar o valor da taxa de transferência de solicitações. Em vez de perguntar sobre instâncias, hardware, memória e outros fatores que podem afetar o valor da taxa de transferência, simplesmente você forneçe o nível de taxa de transferência que deseja alcançar e a AWS fará o resto.

Ao criar ou atualizar sua tabela do Amazon DynamoDB, você especifica quanto de capacidade deseja reservar para leituras e gravações. O Amazon DynamoDB irá reservar os recursos de máquina necessários para atender suas necessidades de taxa de transferência, por meio de um desempenho de baixa latência e consistente.

Exite um nível gratuito para novos usuários onde os clientes recebem 25 GB de armazenamento gratuito, bem como uma capacidade de taxa de transferência contínua gratuita de até 25 unidades de capacidade de gravação e 25 unidades de capacidade de leitura (taxa de transferência suficiente para processar até 200 milhões de solicitações por mês) e 2,5 milhões de solicitações de leitura gratuitas de Streams do DynamoDB.

Veja um exemplo de custo retirado da propria documentação da AWS Amazon:

Suponhamos que sua aplicação precise executar 1 milhão de gravações e 1 milhão de leituras por dia em uma tabela do DynamoDB, 50 mil solicitações de leitura dos Streams do DynamoDB por dia e armazenar 1 GB de dados.

Para simplificar, vamos supor também que a sua carga de trabalho é relativamente constante ao longo do dia e que seus itens têm tamanho inferior a 1 KB. (Você pode facilmente aumentar e diminuir a escala para atender a cargas de trabalho variáveis e ajustar para itens maiores, mas, neste exemplo, vamos manter a simplicidade).

Primeiro, você precisa calcular quantas gravações e leituras por segundo são necessárias. Em um dia, 1 milhão de gravações distribuídas uniformemente equivalem a 1.000.000 (gravações) / 24 (horas) / 60 (minutos) / 60 (segundos) = 11,6 gravações por segundo. Uma unidade de capacidade de gravação do DynamoDB pode atender a 1 gravação por segundo, portanto, você precisa de 12 unidades de capacidade de gravação. Da mesma forma, para atender a 1 milhão de leituras fortemente consistentes por dia, você precisa de 12 unidades de capacidade de leitura.

Usando a definição de preços de taxa de transferência provisionada na região Leste dos EUA (Norte da Virgínia), 12 unidades de capacidade de gravação custam 0,1872 USD por dia e 12 unidades de capacidade de leitura custam 0,0374 USD por dia. Assim, o custo total de capacidade de taxa de transferência provisionada é 0,1872 USD + 0,0374 USD = 0,2246 USD por dia. O custo de 50 mil solicitações de leitura dos Streams do DynamoDB Streams por dia é 50 mil/100 mil x 0,02 USD = 0,01 USD. O armazenamento custa 0,25 USD por GB por mês. Considerando um mês de 30 dias, 1 GB custaria 1 x 0,25 USD/30 = 0,0083 USD por dia. Combinando esses números, o custo do DynamoDB (capacidade de taxa de transferência provisionada + solicitações de leitura de streams + armazenamento) é 0,2246 USD (para a capacidade de taxa de transferência provisionada) + 0,01 USD (para solicitações de leitura de streams) + 0,0083 USD (para armazenamento) = 0,2429 USD por dia.

Por menos de 0,25 USD/dia (7,50 USD/mês), você pode oferecer suporte a uma aplicação que executa 1 milhão de gravações e leituras por dia, 100 mil solicitações de leitura de streams e armazena 1 GB de dados.

Se você não usou todos os recursos disponibilizados pelo nível gratuito (25 unidades de capacidade de gravação, 25 unidades de capacidade de leitura, 2,5 milhões de solicitações de leitura de streams e 25 GB de armazenamento), pode executar essa aplicação gratuitamente no DynamoDB.


## Na prática

Para nosso exemplo vamos criar uma aplicação de chat que pode ser testada aqui: [http://loremchat.com](http://loremchat.com). Como dependência utilizei uma [instância EC2](/2016/12/16/Amazon-EC2/) para rodar a aplicação utilizando a linguagem de programação JavaScript (NodeJS), registrei um domínio (loremchat.com) e gerenciei o DNS do domínio pelo [Route 53](/2017/01/05/Amazon-Route-53/).

Para esta prática no momento em que foi escrito este post a aplicação estava assim:

![](/img/dynamodb2.jpg)

Acessando o serviço do DynamoDB criei uma tabela com uma chave de Partition key chamado "sala" e um sort key chamado "timestamp". Como a aplicação é um chat dividido por sala, ou seja qualquer um pode criar uma sala e compartilhar o link da mesma, então este modelo irá facilitar a indexação:

![](/img/dynamodb1.jpg)

Como deixei marcado **Use default settings** então nosso provisionamento ficou com capacidade de 5 unidades de leitura e escrita, porem estes parametros podem ser alterado posteriormente.

O código desta aplicação esta no [meu GitHub](https://github.com/fabiorogeriosj/example-amazon-dynamoDB) e você pode testá-la, caso eu não tenha desligado a maquina EC2 :), acessando o site [http://loremchat.com](http://loremchat.com). Vamos ao código:

Na aplicação de backend, responsável por receber as solicitações do browser, iremos utilizar o módulo `aws-sdk` que já facilita nossa vida para consumir os serviços da AWS. Para que sua aplicação de backend tenha acesso ao serviço é necessário criar uma chave no serviço [IAM Users](/2017/01/03/AWS-Identity-Access-Management/) com permissão de acesso ao DynamoDB.

Com as chaves anotadas crie um arquivo de credenciais em ~/.aws/credentials no Mac/Linux ou em C:\Users\USERNAME\.aws\credentials no Windows com a seguinte estrutura:

```
[default]
aws_access_key_id = sua_chave_de_acesso
aws_secret_access_key = sua_chave_secreta
```

Veja mais detalhes de como começar usando o [AWS no Node aqui](https://aws.amazon.com/pt/sdk-for-node-js/).

Você pode ver mais detalhes de como utilizar o serviço DynamoDB com Nodejs lendo a [documentação oficial](http://docs.aws.amazon.com/pt_br/amazondynamodb/latest/gettingstartedguide/GettingStarted.NodeJs.html).

Primeiro passo é carregar os módulos, configura-los e conectar ao DynamoDB:

``` javascript
var AWS = require('aws-sdk');
var express = require('express');
var bodyParser = require('body-parser');
var app = express();
var server = require('http').Server(app);
var io = require('socket.io')(server);
app.locals.moment = require('moment');
app.locals.moment.locale('pt-br');

//config express
app.use(express.static(__dirname+'/www'));
app.set('views', __dirname+'/www');
app.set('view engine', 'jade');
app.use(bodyParser.urlencoded({extended: true}));
app.use(bodyParser.json());

AWS.config.update({region: 'sa-east-1'});
var docClient = new AWS.DynamoDB.DocumentClient();
```

Veja que na linha 17 é informado qual região está nossa tabela. É possível criar replicar de dados em outras regiões!

Para que o usuário já tenha algumas salas criadas vou criar um array para listar na interface:

``` javascript
var salas = [
  {
    nome: 'geral',
    icone: 'megaphone.png',
    descricao: 'Assunto geral que rola no mundo!'
  },
  {
    nome: 'música',
    icone: 'headphones.png',
    descricao: 'Trocar ideias no estilo musical ;)'
  },
  {
    nome: 'filme',
    icone: 'youtube.png',
    descricao: 'O que ta rolando sobre filmes, séries e novelas?'
  },
  {
    nome: 'festa',
    icone: 'theater.png',
    descricao: 'Aquela festa animal! Onde? Quando? #bora!'
  },
  {
    nome: 'viagem',
    icone: 'airplane.png',
    descricao: 'O mundo é grande de mais para ficar apenas em um lugar!'
  },
  {
    nome: 'universidade',
    icone: 'mortarboard.png',
    descricao: 'Duas canetas um papel e bora pro papo de universitários :)'
  },
];
```

Por questões de performance irei utilizar conexão via socket à nossa aplicação, pois se a cada nova mensagem ou a cada segundo fosse necessário dar um reload na página para carregar as novas mensagens isso aumentaria nosso custo de acesso ao DynamoDB. Primeiro vamos avisar todas as páginas que estão logadas quando um novo usuário se conectar enviando o número de clientes conectados ao socket:

``` javascript
io.on('connection', function (socket) {
  io.emit('userConnections', io.engine.clientsCount);
});
io.on('disconnect', function (socket) {
  io.emit('userConnections', io.engine.clientsCount);
});
```

Vamos a rota GET:

```javascript
app.get('/*', function (req, res) {
  var sala = "geral";
  var salasPorLink = [];
  if(req.url === "/"){
    salas[0].principal=true;
  } else {
    sala = decodeURI(req.url.replace("/",""));
    var heSalaOficial=false;
    for(i in salas){
      if(salas[i].nome === sala){
        salas[i].principal=true;
        heSalaOficial=true;
      } else {
        salas[i].principal=false;
      }
    }
    if(!heSalaOficial){
      salasPorLink.push({
        nome: sala,
        icone: 'target.png',
        descricao: 'Apenas os usuários que tiverem o link da sala poderam enviar mensagens.',
        principal:true
      })
    }
  }
  var params = {
      TableName : "mensagens",
      Limit: 100,
      KeyConditionExpression: "#sala = :sala",
      ExpressionAttributeNames:{
          "#sala": "sala"
      },
      ExpressionAttributeValues: {
          ":sala":sala
      },
      ScanIndexForward: false
  };
  docClient.query(params, function(err, data){
    //console.log(err, data);
    if(err){
      res.render('index', {error:err});
    } else {
      res.render('index', {mensagens:data.Items, salas:salas, salasPorLink: salasPorLink, sala:sala});
    }
  });
})
```

Da linha 2 a linha 25 existe um algoritmo para verificar a url do usuário e identificar a sala e se a sala é uma das criadas alteriormente, chamadas de salas oficiais.
Da linha 26 a linha 37 criamos o objetivo em JSON que será enviado ao DynamoDB contendo o nome da tabela, limite da resposta e filtro inicial.
Da linha 38 em diante executamos o método `query` enviando o objetivo criado anteriormente, com a resposta do serviço renderizamos a view enviando os dados recebidos.

Vamos ao método POST responsável por receber as novas mensagens:

```javascript
app.post('/*', function(req, res){
  if(!req.body.usuario || !req.body.usuario.name || !req.body.usuario.photo || !req.body.mensagem){
    res.jsonp({isValid:false, msg:"Você precisa estar logado e digitar uma mensagem para conversar com a galera!"});
  } else if(req.body.mensagem.length > 10000){
    res.jsonp({isValid:false, msg:"Eita! Sua mensagem é muito grande, não foi possível enviar :("});
  } else {
    var sala = "geral";
    if(req.url != "/"){
      sala = decodeURI(req.url.replace("/",""));
    }
    var table = "mensagens";
    var params = {
        TableName:table,
        Item: {
            "sala": sala,
            "timestamp": new Date().getTime(),
            "usuario": req.body.usuario,
            "mensagem": req.body.mensagem
        }
    };
    docClient.put(params, function(err, data) {
        if (err) {
            res.jsonp({isValid:false, data:err});
        } else {
            res.jsonp({isValid:true, data:params.Item});
            io.emit('sala:'+sala+':message', params.Item);
        }
    });
  }
});

server.listen(80, function () {
  console.log('appdynamodb rodando na porta 80!')
});
```
O algoritmo é simples, primeiro validamos se existe mesmo uma nova mensagem sendo enviada, validamos a sala pela url, criamos o objeto de envio e executamos o método `put` do dynamoDB.

Simples assim :)

Como o objetivo deste post não é explicar os algoritimos da aplicão que são inrelevantes para o teste com DynamoDB, você pode ver o código da interface no GitHub.
