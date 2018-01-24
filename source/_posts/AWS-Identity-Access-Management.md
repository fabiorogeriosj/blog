---
title: AWS Identity & Access Management
date: 2017-01-03 10:47:36
header-img: /img/security.png
tags:
- cloud computing
- aws amazon
- AWS Identity
- Access Management
- IAM Users
---

Existem duas formas de controlar os acessos aos serviços da plataforma da AWS, um é criar uma chave no usuário padrão porem este não é o melhor método recomendado pois com esta chave é possível acessar de forma ilimitada todos os serviços, e se alguma pessoa maliciosa tiver acesso a sua chave ela pode usar seus serviços e você terá uma surpresa desagradável quando sua fatura da Amazon fechar!

Outra objetivo do IAM Users é poder criar usuários com permissões de acesso ao portal console para administrar os serviços AWS com restrinções.

Para isso a AWS criou o AWS Identity & Access Management, conhecido também como IAM Users. Com ele é possível criar usuários e atribuir permissões de acesso por tipo de serviço e outras validações interessantes.

Para utilizar o IAM Users não tem custo, pois ele é um recurso que gera dependencia, ou seja, sozinho ele não faz nada, você precisa dele para usar os demais serviços que já seram cobrados.

Acessando o serviço pelo console AWS (IAM), primeiro criamos um grupo, que é útil para controlar por categoria diferentes usuários:

![](/img/iam1.jpg)

No cadastro do grupo devemos definir um nome e marcar quais políticas iremos aplicar a este grupo. Por exemplo vamos imaginar que este usuário é para acessar o serviço [Amazon S3](/2017/01/02/Amazon-S3/), sendo assim poderia procurar a Policy **AmazonS3FullAccess** onde terá todas as permissões (GET/PUT) no serviço S3.

![](/img/iam2.jpg)

Com o grupo criado vamos criar um novo usuário. No primeiro passo devemos informar o nome e o tipo de acesso que pode ser:

- **Programmatic access:** Gera as chaves necessária para acessar os demais serviços como AWS API, CLI, SDK e outras ferramentas de desenvolvimento.
- **AWS Management Console access:** Cria uma senha que permite que os usuários façam login no AWS Management Console.

Você pode marcar qual, ou quais, tipo de acesso este usuário terá. No nosso exemplo ele irá ter apenas acesso as chaves.

![](/img/iam3.jpg)

Em permissões é possível associonar o usuário a um grupo, copiar permissões de um determinado usuário já existente ou aplicar uma política existente. Como criamos um grupo já com a política aplicada vamos marca-lo.

![](/img/iam4.jpg)

Para concluir visualizamos os atributos do usuário e criamos em "Create user", com isso será possível visualizar a chave do usuário e a chave secreta que pode ser visualiza apenas no passo de criação de chave, basta clicar em "show".

Na listagem de usuário você poderá ver os usuários criados bem como suas chaves.
