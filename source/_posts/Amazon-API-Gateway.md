---
title: Amazon API Gateway
date: 2017-01-06 09:22:20
header-img: /img/devloper-api-background3.jpg
tags:
- cloud computing
- aws amazon
- amazon api gateway
---

Quando se trabalha com micro serviços, algo comum hoje em dia, uma dúvida frequente é como gerenciar as diferentes chamadas e integrar diferentes serviços para uma aplicação. Uma forma simples é ter uma camada conhecida como API Gateway onde todos os processos de entrada são roteados e gerenciado, veja o exemplo abaixo:

![](/img/microservices-architecture.png)

O [Amazon API Gateway](https://aws.amazon.com/pt/api-gateway/) é um serviço totalmente gerenciado que permite que desenvolvedores criem, publiquem, mantenham, monitorem e protejam APIs em qualquer escala. Com apenas alguns cliques no Console de Gerenciamento da AWS, você pode criar uma API que atua como uma "porta de entrada" para que aplicativos acessem dados, lógica de negócios ou funcionalidades a partir de serviços de back-end, como cargas de trabalho executadas no Amazon Elastic Compute Cloud (Amazon EC2), código executado no AWS Lambda ou qualquer aplicação web. O Amazon API Gateway processa todas as tarefas relacionadas à aceitação e ao processamento de até centenas de milhares de chamadas simultâneas de APIs, incluindo gerenciamento de tráfego, autorização e controle de acesso, monitoramento e gerenciamento de versões de APIs. O Amazon API Gateway não tem taxas mínimas ou custos antecipados. Você paga apenas pelas chamadas de API recebidas e pela quantidade transferida de dados de saída.

Os principais benefícios são o custo baixo e a escalabilidade que é automática.

## Preço

Amazon API Gateway, cobra apenas quando suas APIs são usadas. Não há taxas mínimas e nem compromissos iniciais. Você paga apenas pelas chamadas de API recebidas e pela quantidade transferida de dados de saída. O Amazon API Gateway também oferece a opção de armazenar dados em cache mediante uma taxa horária que varia de acordo com o tamanho de cache selecionado. O nível gratuito do API Gateway inclui **um milhão de chamadas** de API por mês durante até 12 meses.

Como os demais serviços o preço leva em consideração a localização onde foi criado sua API, como comentado no [post sobre AWS](/2016/12/15/Amazon-Web-Services-AWS/#Regioes), então vamos exemplificar que foi criado uma nova API nos EUA (Norte da Virginia) sendo assim o custo será:

**Chamadas de APIs**
3,50 USD por milhão de chamadas de API recebidas, mais o custo de transferência de dados de saída em gigabytes.

**Custos de transferência de dados**
Se você usar o Amazon API Gateway, será cobrado pelas chamadas de APIs e pelas transferências de dados para a Internet da forma descrita a seguir.

Taxas de transferência de dados de saída do Amazon API Gateway

0,09 USD/GB para os primeiros 10 TB
0,085 USD/GB para os próximos 40 TB
0,07 USD/GB para os próximos 100 TB
0,05 USD/GB para os próximos 350 TB

## Prática

Acesasndo o serviço via Console e clicando em Create new API você deverá informar se vai criar uma API nova, se vai clonar alguma API já existente, se vai importar de uma estrutura JSON do Swagger ou se vai criar com base em um exemplo disponibilizado pela AWS.

Para praticar vou criar um do zero para fazer algumas consultas a aplicação exemplo que venho usando nos demais post [(https://loremchat.com)](https://loremchat.com):

![](/img/apigateway2.jpg)

API Gateway é composta por conjuntos de recursos "Resources" e métodos "Method". Um Resource é uma entidade lógica que pode ser acessada através de um caminho de API.Um recurso pode ter uma ou mais operações que são definidas por operações HTTP, como GET, POST e DELETE. Uma combinação de Resource e uma operação identifica um método na API. Cada método corresponde a uma solicitação de API REST.
Em cada método você pode configurar e mapear os dados de entreda para um destino como uma aplicação de backend rodando em uma instância EC2 ou executar uma função no AWS Lambda.

Por padrão já temos um resource criado para responder ao link raiz "/", sendo assim vamos criar um método GET para retornar uma HTML de boas vindas. Clique em Actions > Create Method:

![](/img/apigateway3.jpg)

Ao criar o método precisamos definir a forma de integração deste método podendo ser uma função lambda, uma requisição HTTP, um mock ou um serviço específico da AWS, iremos testar todos as opções mas para esta rota vamos escolher Mock pois iremos retornar algo estático (HTML):

![](/img/apigateway4.jpg)

Todos os métodos tem as ações de:
- **Method Request**: Informações sobre as configurações de autorização deste método e os parâmetros que ele pode receber.
- **Integration Request**: Fornecer informações sobre o backend de destino que esse método chamará e se os dados de solicitação de entrada devem ser modificados.
- **Integration Response**: Primeiro, declare tipos de resposta usando Method Response. Em seguida, mapeie as possíveis respostas do backend para os tipos de resposta deste método.
- **Method Response**: Fornecer informações sobre os tipos de resposta deste método, seus cabeçalhos e tipos de conteúdo.

![](/img/apigateway5.jpg)

Para nossa rota de Mock com boas vindas não precisamos configurar nenhum parametro do Request e sim apenas o Response para retornar o html, para isso clique em "Integration Response" > Expanda Expanda "Body Mapping Templates" e exclua o Content-Type existente e crie um com o tipo text/html. No template vamos usar colocar um html simples.

![](/img/apigateway7.jpg)

Agora precisamos espeficicar que o tipo do retorno é html e excluir o modelo do corpo, pois o próprio navegador irá interpletar o body com base no tipo do cabeçalho. Para isso clique em "Method Response" > Expanda o status 200 > exclua o "Response Body for 200" e adicione o Content-Type do tipo text/html para o "Response Headers for 200":

![](/img/apigateway8.jpg)

Por fim volte ao "Integration Response" para editar o "Header Mappings", pois acabamos de definir um parametro no passo anterior, para isso precisamos espeficicar que é um text/html. Expanda o "Header Mappings" e edite o Content-Type para 'text/html' (com aspas simples):

![](/img/apigateway9.jpg)

Se clicar em "Test" você poderá ver o HTML retornado porem ainda não esta acessível por link nossa API. Para tornar visível nossa API precisamos fazer o Deploy, ou seja, liberar para produção. Vá em Actions > Deploy API. Neste momento será exigido um "stage", isto é útil para deferentes estágil da API, como por bla.com/producao, bla.com/homologacao, etc. Crie um novo stage e clique em Deploy:

![](/img/apigateway10.jpg)

Acessando o link veremos o resultado do nosso método GET na raiz "/" da API.

### Customizando domínio

Podemos ver que a url gerada para nossa API não é amigável, podemos resolver isso facilmente customizando um nome de domínio, ou seja, nossa API irá responder ao domínio api.loremchat.com.

Primeiro passo importante é saber que o serviço API Gateway só responde a domínio com certificado (https) e para isso precisamos do certificado para o domínio que queremos responder.

> No momento em que foi escrito este post o AWS Certificate Manager não dava suporte ao API Gateway então por este motivos vamos usar um certificado SSL gratuito.

Crie um certificado SSL através do Let's encrypt seguindo o post sobre [Criar certificado SSL (HTTPS) com Let's encrypt](/2017/01/11/Criar-certificado-SSL-HTTPS-com-Let-s-encrypt/) em seguida em "Custom Domain Names" preencha com as informações do certificado gerado.

**Certificate Body** é a primeira parte do `domain-crt.txt`:

![](/img/apigateway11.jpg)

**Certificate chain** é a segunda parte do `domain-crt.txt`:

![](/img/apigateway14.jpg)

E **Certificate private key** é o `domain-key.txt`.
Com todos os dados preenchidos basta clicar em "Save":

![](/img/apigateway12.jpg)

Ao ser criado você poderá ver o link que será gerado e uma mensagem dizendo que pode demorar até 40 minutos para que seu nome de domínio seja criado.

![](/img/apigateway13.jpg)

Em seguida precisamos criar uma mapeamento do domínio para um "stage" da nossa API. Clique em "Create API mapping" > deixe em branco o "Base path" e selecione a API e o stage. O Base Path é interessante quando você quer controlar a API por versão, por exemplo se informar "v1" em Base path o domínio final ficaria: api.loremchat.com/v1, mas neste caso vamos deixar em branco.

![](/img/apigateway15.jpg)

Agora precisamos direcionar em nosso DNS o domínio api.loremchat.com para o domínio criado, no meu caso: `d3h243lydymgjd.cloudfront.net`. Se você estiver utilizando o Route 53 crie um novo registro da seguinte forma:

![](/img/apigateway16.jpg)

Agora só aguardar o DNS ser propagado e você poderá ver sua API rodando com seu dominio e com SSL:

![](/img/apigateway17.jpg)

Agora vamos criar um método GET que irá ser responsável por fazer uma requisição HTTP para outro endpoint, sendo assim nossa API só irá fazer a rota, porem com isso podemos controlar o acesso entre outros recursos que o API Gateway fornece:

![](/img/apigateway18.jpg)

Após criado é necessário fazer o deploy para a API ser disponível, no fim teremos a nova rota funcionando:

![](/img/apigateway19.jpg)

Outra método bem utilizado no API Gateway é criar uma rota para executar uma função do [AWS Lambda](/2017/01/12/AWS-Lambda), no exemplo abaixo criei uma rota para consumir uma função lambda que faz uma consulta ao banco [DynamoDB](/2017/01/03/Amazon-DynamoDB/):

API Gateway - Criando o método:

![](/img/apigateway20.jpg)

API Gateway - "Integration Request" > Mapeando os paramens de envio. Veja a documentação oficial dos [possíveis mapeamento para mais detalhes](http://docs.aws.amazon.com/pt_br/apigateway/latest/developerguide/api-gateway-mapping-template-reference.html).

![](/img/apigateway21.jpg)

AWS Lambda - Recebe o parametro da API e retorna o resultado da consulta do DynamoDB:

![](/img/apigateway22.jpg)


Também é possível executar chamdas para os demais serviços da AWS, cada serviço tem uma fluxo de configuração diferente, você pode encontrar outros posts relacionados ao mesmo facilmente na web!
