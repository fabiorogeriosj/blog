---
title: AWS Certificate Manager
date: 2017-01-11 13:47:38
header-img: /img/securitynumbers.jpeg
tags:
- cloud computing
- aws amazon
- amazon certificate manager
---

Com o crescimento de diferentes tipos de aplicações presentes na web, a discução sobre proteger os dados dos usuários está sempre em alta! Google por exemplo em seus eventos de 2016 falou muito sobre a segurança das informações e anunciou algumas funcionalidades em seu nevegador Chrome para impulsionar o uso de um certificado SSL.

Nas versões mais nova do Chrome é apresentando uma frase que o site não é seguro caso ele não esteja com um certificado SSL (HTTPS) quando é clicado em informações do site:

![](/img/semssl.jpg)

Quando é utilizado um certificado o destaque é maior:

![](/img/comssl.jpg)

Existe várias formas e provedores de SSL, onde a maioria é pago, alguns free e outros free caso uso um determinado serviço do provedor.

Na série de posts sobre AWS que estou escrevendo em alguns casos irei explicar como usar um certificado gerado pelo [Let’s Encrypt](https://letsencrypt.org/) pois o certificado da AWS não é suportado para todos os serviços.

O [AWS Certificate Manager](https://aws.amazon.com/pt/certificate-manager) é um serviço que permite provisionar, gerenciar e implantar facilmente certificados SSL/TLS (Secure Sockets Layer/Transport Layer Security) **para uso com os serviços da AWS**. Algo comum que existe no processo de gerar um certificado SSL é a renovação, usando o AWS Certificate Manager este processo não é necessário, pois o proprio serviço faz a renovação.

## Preço

Não é cobrado pelos certificados pois os mesmos só funcionam dentro do ambiente AWS, ou seja, apenas para os serviços da Amazon então você já irá pagar pelos serviços relacionados.

## Na prática

O processo é simples, você deve preencher o dominio, ou os domínios (por exemplo loremchat.com, www.loremchat.com) e clicar em "Request a certificate". Em seguida você irá receber um email para autorizar o uso do certificado lincado com sua conta, basta clicar no link e clicar no botão aprovar. Feito isso o certificado está pronto para ser usado pelos serviços (nem todos) da AWS.
