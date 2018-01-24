---
title: Amazon S3
date: 2017-01-02 10:58:15
header-img: /img/storage.jpg
tags:
- cloud computing
- aws amazon
- amazon s3
---

O [Amazon Simple Storage Service](https://aws.amazon.com/pt/s3/), conhecido como [Amazon S3](https://aws.amazon.com/pt/s3/), é um armazenamento de objetos baseado em web service para armazenar e recuperar qualquer volume de dados.

Um caso bem conhecido é o uso do Amazon S3 pela Netflix, onde eles utilizam como um ecosistema para armazenar dados de backup e ferramentas de sua estrutura.

No evento AWS re:Invent de 2016 a diretora de big data da Netflix, Eva Tse, apresentou sua arquitetura utilizando o Amazon S3 e relatou alguns números interessantes, por exemplo ela apresentou que é trafegado mais de 100 Terabyte para o Amazon S3 por dia 😱😱😱😱!
Você pode ver esta [palestra na integra aqui](https://www.youtube.com/watch?v=o52vMQ4Ey9I).

Realmente é bem simples armazenar arquivos estáticos no Amazon S3, iremos ver de forma prática como fazer isso criando uma aplicação simples, em JavaScript utilizando NodeJS, para fazer upload de fotos! Antes vamos ver algumas vantagens destacadas pela AWS sobre o S3 que é importante você saber:

- **Simples:** O Amazon S3 é simples de usar e tem um console de gerenciamento e uma aplicação móvel, ambos baseados na web. O S3 também disponibiliza APIs REST e SDKs completos para a integração fácil com tecnologias de terceiros.
- **Resiliente:** O Amazon S3 proporciona infraestrutura durável para armazenar dados importantes e foi projetado para oferecer durabilidade de objetos de 99,999999999%. Seus dados são armazenados com redundância em várias instalações e diversos dispositivos em cada instalação.
- **Escalável:** Com o Amazon S3, você pode armazenar a quantidade de dados desejada e acessá-los quando precisar. Não é mais necessário adivinhar as necessidades futuras de armazenamento. Você pode aumentar e diminuir a escala de acordo com a necessidade, aumentando consideravelmente a agilidade empresarial.
- **Seguro:** O Amazon S3 oferece suporte à transferência de dados usando SSL e criptografia automática dos dados após o upload. Você também pode configurar políticas de bucket para gerenciar permissões de objetos e controlar o acesso a seus dados usando o AWS Identity e Access Management (IAM).
- **Disponível:** O Amazon S3 Standard foi concebido para proporcionar uma disponibilidade de objetos de até 99,99% durante um determinado ano e tem o respaldo do Acordo de nível de serviço do Amazon S3, garantindo que você possa contar com o serviço sempre que precisar. E você também pode escolher a região da AWS para otimizar a latência, minimizar os custos ou cumprir requisitos normativos.
- **Baixo custo:** O Amazon S3 permite armazenar grandes quantidades de dados com custo muito baixo. Com o uso de políticas de ciclo de vida, você pode definir políticas para migrar automaticamente dados para a categoria Padrão – Acesso ocasional e para o Amazon Glacier, conforme se aproximarem do fim da vida útil, para reduzir ainda mais os custos.
- **Transferência de dados simples:** A Amazon oferece várias opções de migração de dados para a nuvem e possibilita a movimentação de grandes volumes de dados de e para o Amazon S3 com simplicidade e economia. Os clientes podem escolher entre métodos de conector otimizados para rede, baseados em disco físico ou de terceiros para importar ou exportar dados do S3.
- **Integrado:** O Amazon S3 está altamente integrado a outros Serviços da AWS para facilitar a criação de soluções que usam vários Serviços da AWS. As integrações incluem os seguintes serviços: Amazon CloudFront, Amazon CloudWatch, Amazon Kinesis, Amazon RDS, Amazon Glacier, Amazon EBS, Amazon DynamoDB, Amazon Redshift, Amazon Route 53, Amazon EMR, Amazon VPC, Amazon KMS e AWS Lambda.
- **Fácil de gerenciar:** Os recursos de gerenciamento de armazenamento do S3 permitem a adoção de uma abordagem controlada por dados à otimização do armazenamento, à segurança de dados e à eficiência do gerenciamento. Esses recursos de nível empresarial disponibilizam dados sobre os seus dados, para que você possa gerenciar o armazenamento com base nestes metadados personalizados.

Existem vários *cases* conhecidos que utilizam o Amazon S3 para realizar backup e arquivamento, armazenamento e distribuição de conteúdo, análise de big data, hospedagem de sites estáticos entre outros.


### Preço

A definição de preço do Amazon S3 é calculado pelo tanto de dados que você armazena e trafega levando em consideração a localização dos dados, como comentado no [post sobre AWS](/2016/12/15/Amazon-Web-Services-AWS/#Regioes). Existe também um nível gratuíto para novos cadastros onde você recebe 5 GB de armazenamento padrão do Amazon S3, 20.000 solicitações de Get, 2.000 solicitações de Put, 15 GB de transferência de dados para fora a cada mês, por um ano.

Por exemplo vamos exemplificar que você não tem prioridade de latência em seu armazenamento, ou seja, não é necessário uma resposta rápida dos dados armazenados quando solicitados, sendo assim vamos escolher a região [Leste dos EUA (Norte da Virgínia)](https://aws.amazon.com/pt/about-aws/global-infrastructure/). Estimando que vamos armazenar 800GB por mês e ter 10 mil request de PUT (upload de arquivos), 1 milhão de request de GET (visualização de arquivos) e o mesmo nível de transferência por mês então pagariamos $24 Dolares mês!
Se a aplicação for regional, ou seja para usuários brasileiros, e a latência é uma preocupação, podemos utilizar a região de São Paulo, sendo assim o custo por mês ficaria $57 Dolares.

Para aplicações que armazena e trafega muitos dados devem ser analisadas com cuidado pois dependendo do objetivo de armazenamento existem outras opções dentro da plataforma AWS que podem ter um custo bem menor. Por exemplo caso sua aplicação é realizar backups e que o tempo de acesso pode ser entre 1 a 5 minutos você pagaria para a mesma estimativa descrita acima $3 Dolares na região dos EUA e $7 Dolares em São Paulo.


## Na prática

Para nossa prática vamos criar uma aplicação web que tem por objetivo postar fotos de forma pública, ou seja, todos podem ver as fotos postadas sem restrições. Se essa fosse uma aplicação real precisariamos pelo menos garantir um login, por questões de direitos e responsabilidades sobre as imagens publicadas, mas como não é o caso vamos deixar de forma pública!

irei utilizar como linguagem de programação o JavaScript utilizando a plataforma NodeJS. Como o objetivo é praticar o serviço Amazon S3 não vou me preocupar com segurança e outros detalhes primários como por exemplo registrar um domínio e utilizar um certificado SSL.

Iremos utilizar apenas uma máquina em [Linux (Amazon EC2)](/2016/12/16/Amazon-EC2/) e o serviço Amazon S3.

Para utilizar os serviços da Amazon de forma programada, ou seja, não utilizar o console da Amazon mas sim, por exemplo, uma linguagem de programação, precisamos ter uma chave de acesso e uma chave secreta. A forma mais segura é utilizar o serviço [IAM Users](/2017/01/03/AWS-Identity-Access-Management/) para criar nossa chave.

Com as chaves anotadas crie um arquivo de credenciais em ~/.aws/credentials no Mac/Linux ou em C:\Users\USERNAME\.aws\credentials no Windows com a seguinte estrutura:

```
[default]
aws_access_key_id = sua_chave_de_acesso
aws_secret_access_key = sua_chave_secreta
```

Veja mais detalhes de como começar usando o [AWS no Node aqui](https://aws.amazon.com/pt/sdk-for-node-js/).

Em Amazon S3 temos que criar um repositório, conhecido como Bucket, para fazer upload e realizar consultas de objetos, comumente chamado de objects.

Ao entrar em Amazon S3 clique em "Create Bucket", você deve informar um nome, este nome deve ser único, ou seja, não deve estar em uso, e depois selecionar a região onde será armazenado seus objetos.

![](/img/s31.jpg)

A definição da região influencia diretamente o valor que você irá pagar por mês e também a latência dos objetos, irei fazer um teste da forma mais lenta até a forma mais rápida.

Vamos ao código e em seguida a explicação das linhas. Você também pode ver o projeto completo no [meu GitHub](https://github.com/fabiorogeriosj/example-amazon-s3).

Existe um módulo em NPM para acessar de forma simples os métodos da API do Amazon S3, então nosso trabalho aqui fica simples :)

Iremos utilizar apenas 2 métodos da API, porem existe outros métodos que você precisa conhecer, para isso veja a documentação do [AWS SDK S3](http://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/S3.html).

app.js:
``` javascript
var fs = require('fs');
var path = require('path');
var AWS = require('aws-sdk');
var express = require('express');
var multer  = require('multer');
var app = express();
var upload = multer({ dest: './upload/'});

//instanciando classe S3
var s3 = new AWS.S3();
var bucketFotos = 'appfotos';
var urlBucketFotos = 'https://s3-us-west-2.amazonaws.com/appfotos/';

//config express
app.use(express.static(__dirname));
app.set('views', __dirname);
app.set('view engine', 'jade');

//rotas
app.get('/', function (req, res) {
  var params = {
    Bucket: bucketFotos,
    EncodingType: 'url'
  };
  s3.listObjects(params, function(err, data) {
    if (err){
        console.log(err, err.stack);
        res.render('index', { erro: err });
    } else {
        res.render('index', { dados: data, url: urlBucketFotos });
    }
  })
})

app.post('/', upload.single('foto'), function(req, res){
  var stream = fs.createReadStream(req.file.path);
  var ext = path.extname(req.file.originalname);
  var params = {
    Bucket: bucketFotos,
    Key: req.file.filename + ext,
    Body: stream,
    ACL: 'public-read-write'
  };
  s3.upload(params, function(err, data) {
    if(err){
      res.render('index', { erro: err });
    } else {
      res.redirect('/');
    }
  })
})

app.listen(80, function () {
  console.log('appfotos rodando na porta 80!')
});

```

Da linha 1 à 7 está sendo carregando os módulos do NodeJS.
Na linha 11 foi criado uma variável para informar o nome do Bucket que vamos utilizar.
Na linha 12 também foi criado uma variável para informar a url do Bucket pois o objetivo é acessar as imagens direto do S3.
Da linha 15 à 17 é configurado algumas propriedades do Express (modulo para prover as views e rotas da aplicação).
Da linha 20 à linha 33 temos uma rota, que aceita request do tipo GET, que primeiro executa o método `listObjects` do módulo aws-sdk e após receber seu retorno responde para o browser enviando os dados recebidos e a variável url.
Da linha 35 à 51 temos a rota de POST onde será recebido um arquivo, em seguida enviamos ao S3 pelo método `upload`.

index.jade:
``` html
html
  head
    meta(charset='utf-8')
    title Exemplo Amazon S3
  body
    p= erro
    form(action="/", method="post", enctype="multipart/form-data")
      <input type="file" name="foto">
      <input type="submit" value="Enviar foto">
    h1 Fotos
    each foto in dados.Contents
      img(src= url + foto.Key, style="height: 200px;")
```

Na view apenas criamos um form simples e iteramos as fotos recebidas do S3!

Vejamos o resultado:

![](/img/amazons3_1.gif)

Existe um recurso para acelerar as transferências de objetos aos Buckets deixando 300% mais rápido, porem isso acarreta em um custo adicional por GB transferido.

![](/img/s32.jpg)

Vamos testar novamente com o Transfer Acceleration habilitado:

![](/img/amazons3_2.gif)

É notável o aumento da velocidade na hora de ser listado as imagens do Bucket. Podemos melhorar ainda mais esta performance criando um Bucket em São Paulo para diminuir a latência.

![](/img/s33.jpg)

Em seguida alteramos a url de acesso para o novo Bucket e o nome do Bucket:

``` javascript
var bucketFotos = 'appfotosp';
var urlBucketFotos = 'https://s3-sa-east-1.amazonaws.com/appfotosp/';
```

Testando agora com o Bucket em São Paulo:

![](/img/amazons3_3.gif)

Sem o acelerador já melhorou significativamente na visualização. Vamos habilitar o acelerador para o Bucket de São Paulo e alterar o url.

``` javascript
var urlBucketFotos = 'appfotosp.s3-accelerate.amazonaws.com';
```

O resultado:

![](/img/amazons3_4.gif)

Podemos ver uma pequena melhora. Como a foto primeiro é enviado para a instância do EC2 e depois enviado ao Amazon S3, seria possível melhor ainda mais criando a instância EC2 em São Paulo, lembrando que no exemplo das imagens a instância estava no EUA.
