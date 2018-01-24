---
title: Criar certificado SSL (HTTPS) com Let's encrypt
date: 2017-01-11 20:11:11
header-img: /img/security.png
tags:
- cloud computing
- ssl
- https
---

Para criar um certificado SSL free vamos utilizar o [ZeroSSL.com](https://zerossl.com/) que registra nosso certificado no [Let’s Encrypt](https://letsencrypt.org/). A diferença para os certificados pagos (do mesmo modelo) é que este você precisa renovar, de forma manual ou criar um script, a cada 90 dias, então salve os arquivos que você vai gerar e baixar para ser utilizado na renovação.

O próprio Wizard do site ZeroSSL já gera todas as chaves necessárias, porem ele cria as chaves de 4096-bit e em muitos casos precisamos de uma chave de 2048-bit (como é o caso do API Gateway da Amazon), então vamos criar primeiro o certificado de 2048-bit e depois as demais chaves.

Primeiro vá em "CSR Generator" e digite os domínios que seu certificado irá operar. Faça download das duas chaves geradas e salve pois você irá precisar delas para renovação:

![](/img/zerossl1.png)

Agora volte no início do site e vai em "FREE SSL Certificate Wizard". No primeiro passo digite seu email e cole a chave do arquivo `csr.txt`.
Você pode validar o domínio de duas formas, uma é através de um server rodando na porta 80 com alguns arquivos para validar ou através do DNS (mais fácil).

![](/img/zerossl2.png)

Após gerar o private key faça donwload e siga ao proximo passo.

![](/img/zerossl3.png)

Agora você deve criar no gerenciador de seu DNS do domínio seguindo os dados que foram apresentado no passo de validação. Vocé precisa cadastrar cada domínio que o passo anterior apresentar. Caso esteja usando o [Route 53](/2017/01/05/Amazon-Route-53/) para gerenciar seu domínio a configuração ficaria assim:

![](/img/zerossl4.png)

Neste passo é necessário esperar o DNS popular este novo registro para seguir para o próximo passo.

Você pode, pela linha de comando, usar o `nslookup` para verificar se o DNS já populou seu domínio:

```
C:\Users\fabio>nslookup -q=TXT _acme-challenge.loremchat.com
Servidor:  Broadcom.Home
Address:  10.1.1.1

*** Broadcom.Home não encontrou _acme-challenge.loremchat.com: Non-existent domain
```

Depois de um tempo o retorno deverá ser algo parecido com isso:

```
C:\Users\fabio>nslookup -q=TXT _acme-challenge.loremchat.com
Servidor:  Broadcom.Home
Address:  10.1.1.1

Não é resposta autoritativa: _acme-challenge.loremchat.com       
text="atZsd4Dj6uDxqY49rxUoIZXKyQN8aa7eRQXyAMxBZAs"
```

Sendo assim podemos seguir ao próximo passo:

![](/img/zerossl5.png)

Faça download do domain certificate, pois depois de fechar esta página não será mais possível baixar.

No fim você gerou 4 arquivos sendo:

- **csr.txt**: CSR do domínio, você precisará deste arquivo quando for renovar seu certificado.
- **account-key.txt**: A chave da sua conta, você precisará deste arquivo tambem quando for renovar o certificado.
- **domain-key.txt**: Chave privada do seu domínio.
- **domain-crt.txt**: Certificado do domínio que contem duas divisões, em alguns casos você terá que copiar individualmente cada parte.
