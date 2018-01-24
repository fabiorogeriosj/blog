---
title: Amazon EC2
date: 2016-12-16 13:52:34
header-img: /img/circuito.jpg
tags:
- cloud computing
- aws amazon
- amazon ec2
---


O [Amazon Elastic Compute Cloud (Amazon EC2)](https://aws.amazon.com/pt/ec2/) é um produto para criar maquinas virtuais utilizando recursos compartilhados. Com uma interface bem intuitiva e simples, é possível criar maquinas em minutos e disponibilizar no ambiente web.

Você pode a qualquer momento melhorar a performance da maquina, chamada de instancia, alterando o tipo dela.

O processo é bem simples e não irei entrar em detalhes tendo em vista que existem inúmeros tutoriais passo-a-passo de como criar uma instancia e gerenciala, mas de forma resumida podemos dizer que no passo 1 você seleciona qual tipo de instancia vai utilizar: linux ou windows! Lembrando que o tipo influencia no valor que será cobrado por horas.

![Passo 1](/img/ec2_1.jpg)

Você pode fazer simulações de quando irá pagar por mês de cada serviço utilizando a [calculadora da AWS Amazon](http://calculator.s3.amazonaws.com/index.html).

Em seguida, você irá selecionar o tipo da instancia, quanto mais memória e virtual CPU tiver maior o valor da hora da máquina em execução. Veja os tipos na tabela abaixo (esta mesma tabela é exibida quando você está no passo 2):

> Lembrando que se você é um usuário novo, você pode usar a **t2.micro** por 12 meses de graça.

| Family | Type | vCPUs  | Memory (GiB) | Instance Storage (GB) | EBS-Optimized Available | Network Performance |
| --- | --- | --- | --- | --- | --- | --- |
| General purpose | t2.nano | 1 | 0.5 | EBS only | - | Low to Moderate |
| General purpose| **t2.micro** | 1 | 1 | EBS only | - | Low to Moderate |
| General purpose| t2.small | 1 | 2 | EBS only | - | Low to Moderate |
| General purpose| t2.medium | 2 | 4 | EBS only | - | Low to Moderate |
| General purpose| t2.large | 2 | 8 | EBS only | - | Low to Moderate |
| General purpose| t2.xlarge | 4 | 16 | EBS only | - | Moderate |
| General purpose| t2.2xlarge | 8 | 32 | EBS only | - | Moderate |
| General purpose| m4.large | 2 | 8 | EBS only | Yes | Moderate |
| General purpose| m4.xlarge | 4 | 16 | EBS only | Yes | High |
| General purpose| m4.2xlarge | 8 | 32 | EBS only | Yes | High |
| General purpose| m4.4xlarge | 16 | 64 | EBS only | Yes | High |
| General purpose| m4.10xlarge | 40 | 160 | EBS only | Yes | 10 Gigabit |
| General purpose| m4.16xlarge | 64 | 256 | EBS only | Yes | 20 Gigabit |
| General purpose| m3.medium | 1 | 3.75 | 1 x 4 (SSD) | - | Moderate |
| General purpose| m3.large | 2 | 7.5 | 1 x 32 (SSD) | - | Moderate |
| General purpose| m3.xlarge | 4 | 15 | 2 x 40 (SSD) | Yes | High |
| General purpose| m3.2xlarge | 8 | 30 | 2 x 80 (SSD) | Yes | High |
| Compute optimized| c4.large | 2 | 3.75 | EBS only | Yes | Moderate |
| Compute optimized| c4.xlarge | 4 | 7.5 | EBS only | Yes | High |
| Compute optimized| c4.2xlarge | 8 | 15 | EBS only | Yes | High |
| Compute optimized| c4.4xlarge | 16 | 30 | EBS only | Yes | High |
| Compute optimized| c4.8xlarge | 36 | 60 | EBS only | Yes | 10 Gigabit |
| Compute optimized| c3.large | 2 | 3.75 | 2 x 16 (SSD) | - | Moderate |
| Compute optimized| c3.xlarge | 4 | 7.5 | 2 x 40 (SSD) | Yes | Moderate |
| Compute optimized| c3.2xlarge | 8 | 15 | 2 x 80 (SSD) | Yes | High |
| Compute optimized| c3.4xlarge | 16 | 30 | 2 x 160 (SSD) | Yes | High |
| Compute optimized| c3.8xlarge | 32 | 60 | 2 x 320 (SSD) | - | 10 Gigabit |
| Memory optimized| r3.large | 2 | 15 | 1 x 32 (SSD) | - | Moderate |
| Memory optimized| r3.xlarge | 4 | 30.5 | 1 x 80 (SSD) | Yes | Moderate |
| Memory optimized| r3.2xlarge | 8 | 61 | 1 x 160 (SSD) | Yes | High |
| Memory optimized| r3.4xlarge | 16 | 122 | 1 x 320 (SSD) | Yes | High |
| Memory optimized| r3.8xlarge | 32 | 244 | 2 x 320 (SSD) | - | 10 Gigabit |

Após escolher o tipo da instancia você irá para o passo 3 onde deve definir algumas configurações como:

**Number of instances**: Você pode criar várias instâncias com a mesma configuração, isso é utilizado em casos de escalonamento automático onde sua aplicação tem um escalamento horizontal e distribui os acessos em diferentes máquinas. A Escala Automática também substituirá automaticamente as instâncias não saudáveis. Saiba mais sobre [Auto Scaling](https://docs.aws.amazon.com/autoscaling/latest/userguide/AutoScalingGroup.html).

**Purchasing option**: As instâncias spot do Amazon EC2 permitem que você faça propostas para capacidade computacional de reserva do Amazon EC2, leia mais sobre [Request Spot instances](https://aws.amazon.com/pt/ec2/spot/).

**Network, Subnet e Auto-assign Public IP**: Configure os dados de sua rede.

**IAM role**: Você pode definir as credeciais do proprio sistema de usuários da AWS, iremos discutir isto em outro mmomento.

**Shutdown behavior**: Define qual comportamento a instancia deve sofrer quando uma maquina virtual é desligada.

Os demais itens são para monitoramente e segurança para possíveis queda da instancia. Importante saber que caso você não queira utilizar uma instancia que compartilha hardware, você pode configurar a propriedade **Tenancy** para executar de fato uma maquina física.

No passo 4 você define o armazenamento da instância como quantidade de gigas, tipo do volume (SSD ou maginético) e outras propriedades do contexto.

> Para a instancia gratuíta você pode usar até 30 gigas de armazenamento no SSD ou maginético.

No passo 5 você pode difinir um nome para sua instancia e outras tags para utilizar em outras configurações que veremos em outro momento.

Passo 6 é muito importante pois define o tipo de segurança que será utilizado e quais portas estaram abertas pela firewall antes de chegar a sua instancia. Depois de configurado você irá baixar um arquivo que é sua chave de acesso a instancia por SSH, você deve quardar bem esta chave pois sem ela, caso for linux, você não consegue acessar a maquina!

Feito estas configurações é só clicar em launch e sua maquina estará online em minutos.
