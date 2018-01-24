---
title: AWS Elastic Beanstalk
date: 2017-01-16 22:26:15
header-img: /img/datacenter.jpg
tags:
- cloud computing
- aws amazon
- aws elastic beanstalk
---

Escalabilidade é algo que precisa ser pensado desde o início da aplicação pois quando um produto está na web o alcançe, dependendo do seu publico alvo, pode ser ilimitado globalmente.

O próprio serviço AWS Amazon EC2 tem vários recursos para fazer o auto scalling e o balanceamento de carga porem não é tão simples e rápido esta configuração e o provisionamento caso não esteja ativo o auto scalling. O serviço [AWS Elastic Beanstalk](https://aws.amazon.com/pt/elasticbeanstalk/) foi criado justamente para resolver este problema.

Um *case* apresentado pela Amazon, apresenta o aplicativo [Prezi de apresentações online](https://prezi.com/) que moveu dois de seus serviços críticos de backend para a AWS Elastic Beanstalk para implementar e dimensionar o serviço automaticamente, você pode ler na integra o *case* no [site oficial](https://aws.amazon.com/pt/solutions/case-studies/prezi/).

Segundo a documentação oficial do produto:

> O AWS Elastic Beanstalk é um serviço de fácil utilização para implantação e escalabilidade de aplicativos e serviços da web desenvolvidos com Java, .NET, PHP, Node.js, Python, Ruby, Go e Docker em servidores conhecidos, como Apache, Nginx, Passenger e IIS.
Basta fazer o upload de seu código e o Elastic Beanstalk se encarrega automaticamente da implementação, desde o provisionamento de capacidade, o balanceamento de carga e a escalabilidade automática até o monitoramento da saúde do aplicativo. Ao mesmo tempo, você mantém total controle sobre os recursos da AWS que possibilitam a operação do seu aplicativo e pode acessar os recursos subjacentes a qualquer momento.
Não há custos adicionais pelo Elastic Beanstalk, você só paga pelos recursos da AWS necessários para executar e armazenar seus aplicativos.

O processo é simples para criar, configurar e monitorar seu ambiente pois o serviço tem como objetivo em criar suas instâncias EC2 e toda configuração dependente para manter e escalar sua aplicação.

Aqui podemos ver o recurso de monitoramento:

![](/img/elasticbeanstalk1.png)
