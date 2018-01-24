---
title: AWS Lambda
date: 2017-01-12 14:11:54
header-img: /img/microservices.jpg
tags:
- cloud computing
- aws amazon
- aws lambda
---

Um dos produtos atuais mais interessante, na minha opnião, que a Amazon lançou é o [AWS Lambda](https://aws.amazon.com/pt/lambda/), pois com ele você pode executar código em resposta a eventos e não precisa se preocupar com a infraestrutura para provisionar o recurso necessário. Você pode usar o AWS Lambda para estender outros serviços da AWS com lógica personalizada ou criar seus próprios serviços de back-end que operam na escala, desempenho e segurança da AWS. O AWS Lambda pode executar automaticamente códigos em resposta a múltiplos eventos, como modificações a objetos em buckets do [Amazon S3](/2017/01/02/Amazon-S3/) ou atualizações de tabela no [Amazon DynamoDB](/2017/01/03/Amazon-DynamoDB/).

> O Lambda executa seu código em infraestrutura de computação de alta disponibilidade e realiza toda a administração dos recursos de computação, incluindo manutenção de servidor e de sistema operacional, provisionamento de capacidade e escalabilidade automática, implementação de código e de patch de segurança e monitoramento e registro de código. Tudo o que você precisa fazer é fornecer o código.

### Preço

A precificação também é interessante pois você é cobrado de acordo com o número de solicitações de funções e o tempo de execução do código.

O nível gratuito do Lambda inclui 1 milhão de solicitações gratuitas por mês e 400.000 GB/segundo de tempo de computação por mês.

#### Solicitações
Você é cobrado pelo número total de solicitações em todas as suas funções. O Lambda conta uma solicitação cada vez que começa a executar em resposta a uma notificação de evento ou chamada de invocação, incluindo invocações de teste do console.

O primeiro milhão de solicitações do por mês é gratuito
0,20  USD por 1 milhão de solicitações depois disso (0,0000002  USD por solicitação)

#### Duração
A duração é calculada a partir do momento em que seu código começa a ser executado até ele retornar ou encerrar, arredondando para os 100 m mais próximos. O preço depende da quantidade de memória que você alocar para sua função. Você é cobrado em 0,00001667  USD a cada GB/segundo usado.

#### Nível gratuito
O nível gratuito do Lambda inclui 1 milhão de solicitações gratuitas por mês e 400.000 GB/segundo de tempo de computação por mês. O tamanho da memória que você escolhe para suas funções do Lambda determina por quanto tempo elas podem ser executadas no nível gratuito. O nível gratuito do Lambda não expira automaticamente ao final do período de 12 meses do nível gratuito da AWS, ele fica disponível para os clientes novos e existentes indefinidamente.

Você pode ver a tabela de preço e mais detalhes na [página oficial do produto](https://aws.amazon.com/pt/lambda/pricing/), bem como simulações de cobranças.


## Prática

Você pode executar uma função lambda apartir de alguns serviços específicos da plataforma como mostrado abaixo:

![](/img/lambda1.png)

Vamos criar uma função para ser executada quando uma rota do serviço [API Gateway](/2017/01/06/Amazon-API-Gateway/) for requisitado. Vamos seguir o projeto que já vem sendo criado nos demais posts sobre a plataforma AWS, o aplicativo demo [https://loremchat.com](https://loremchat.com).

Primeiro passo quando for criar uma nova função é escolher um modelo caso necessário, no nosso caso vamos escolher o modelo "Blank Function" pois não queremos exemplos de código:

![](/img/lambda2.png)

No segundo passo podemos escolher o serviço que irá disparar o evento pra executar a função no Lambda, se for escolhido, por exemplo o API Gateway, ele irá criar uma nova rota no serviço de origem, mas para este posto não vamos escolher o serviço de origem, pois em outro momento vamos escolher esta função. Então no passo dois basta clicar em Next.

No próximo passo temos que definir um nome para nossa função e o "Runtime" que iremos utilizar, ou seja a tecnologia/linguagem de programação. No Meu exemplo escolhi Node.js e meu código ficou assim:

``` javascript
const doc = require('dynamodb-doc');

const dynamo = new doc.DynamoDB();

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

exports.handler = (event, context, callback) => {
    var sala = event.url;
    if(!sala){
        sala = "geral";
    }
    var heSalaOficial = false;
    var salasPorLink = [];
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
    dynamo.query(params, function(err, data) {
        console.log("dados>>>>", data);
        var html = "";
        html += "<html> ";
        html += "   <head> ";
        html += "      <meta charset='utf-8'> ";
        html += "      <meta name='viewport' content='width=device-width, initial-scale=1.0'> ";
        html += "      <title>LoremChat</title> ";
        html += "      <link href='https://fonts.googleapis.com/css?family=Lato:400,900' rel='stylesheet'> ";
        html += "      <link href='https://assets.loremchat.com/css/estilo.css' rel='stylesheet'> ";
        html += "   </head> ";
        html += "   <body> ";
        html += "      <div id='fb-root'></div> ";
        html += "      <script src='https://connect.facebook.net/pt_BR/all.js'></script> ";
        html += "      <div id='notificacao'><img src='https://assets.loremchat.com/img/key-up.png'> <span>x novas mensagens não lidas</span></div> ";
        html += "      <div class='editor'> ";
        html += "        <button id='lodin-fb' type='button'>Logar com Facebook para enviar mensagens na sala <strong>"+sala+"</strong></button> ";
        html += "        <span id='aviso'>Para ver as mensagens em tempo real você precisa estar logado!</span> ";
        html += "        <input type='text' id='editor' disabled='true' placeholder='Entrando na sala "+sala+"...' autofocus='true'> ";
        html += "      </div> ";
        html += "      <div class='chat'> ";
        for(i in data.Items){
            var mensagem = data.Items[i];
            html += "<div class='mensagem' id='"+mensagem.timestamp+"'> ";
            html += "<img src='"+mensagem.usuario.photo+"'><h1>"+mensagem.usuario.name+" <span></span><p>"+mensagem.mensagem+"</p></h1>";
            html += "</div> ";
        }
        html += "</div> ";
        html += "      <div class='salas'> ";
        html += "         <img src='https://assets.loremchat.com/img/menu-button-of-three-horizontal-lines.png' id='iconmenu'> ";
        html += "         <h2>Salas oficiais</h2> ";
        for(i in salas){
            html += "         <a href='/"+salas[i].nome+"'><div class='sala ";
            if(salas[i].principal){
                html += "principal";
            }
            html += "'> ";
            html += "               <img src='https://assets.loremchat.com/img/"+salas[i].icone+"'> ";
            html += "               <h1> "+salas[i].nome+" <p>"+salas[i].descricao+"</p> </h1> ";
            html += "            </div> ";
            html += "         </a> ";
        }
        if(!heSalaOficial){
            html += " <h2>Salas por link</h2> ";   
            for(i in salasPorLink){
                html += " <a href='/"+salasPorLink[i].nome+"'><div class='sala ";
                if(salasPorLink[i].principal){
                    html += "principal";
                }
                html += "'> ";
                html += "               <img src='https://assets.loremchat.com/img/"+salasPorLink[i].icone+"'> ";
                html += "               <h1> "+salasPorLink[i].nome+" <p>"+salasPorLink[i].descricao+"</p> </h1> ";
                html += "            </div> ";
                html += "         </a> ";
            }
        }
        html += "         <a onclick='criarNovaSala()' id='linkNovaSala' class='new'>Criar uma sala</a> ";
        html += "         <h2 class='form'>Crie sua sala</h2> ";
        html += "         <form id='formnovasala' method='GET' action='/' class='form'> ";
        html += "            <input id='novasala' placeholder='Digite o nome da sala'> ";
        html += "            <p>Apenas os usuários com o link desta sala poderam participar da conversa!</p> ";
        html += "            <button type='submit'>Criar nova sala</button> ";
        html += "         </form> ";
        html += "         <div id='footer'></div> ";
        html += "      </div> ";
        html += "      <script src='https://assets.loremchat.com/js/jquery-3.1.1.min.js'></script> ";
        html += "      <script src='https://assets.loremchat.com/js/moment.min.js'></script> ";
        html += "      <script src='https://assets.loremchat.com/js/moment-with-locales.min.js'></script> ";
        html += "      <script src='https://loremchat.herokuapp.com/socket.io/socket.io.js'></script> ";
        html += "      <script>var SOCKET_URL = 'https://loremchat.herokuapp.com'; </script> ";
        html += "      <script src='https://assets.loremchat.com/js/script.js'></script> ";
        html += "   </body> ";
        html += "</html> ";
        callback(null, html);
    });
};
```

O código, embora ficou grande, é simples, apenas valida um parametro de entrada e faz uma consulta no banco DynamoDB e monta um HTML de retorno.

Rolando a página você verá alguns parametros que devem ser revisados. Os mais importantes são o **Role** que é a permissão que será dada para que esta função seja executado por outros serviços, e o tamanho de memória que será utilizado para executar a função e o tempo máximo que pode demorar pra executar:

![](/img/lambda3.png)

Após este passo basta confirmar e sua função está pronta para ser executada. Você pode testa-la passando parametros para analisar o retorno clicando em "Test".

Para ser invodado esta função pelo API gateway basta seguir o post [sobre o serviço específico](http://localhost:4000/2017/01/06/Amazon-API-Gateway/).
