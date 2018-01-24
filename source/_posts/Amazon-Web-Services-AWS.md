---
title: Amazon Web Services (AWS)
date: 2016-12-15 10:11:46
header-img: /img/datacenter.jpg
tags:
- cloud computing
- aws amazon
---

Amazon Web Services, ou apenas AWS Amazon, é uma plataforma de serviços em nuvem onde disponibiliza inúmeros produtos para resolver diferentes problemas de aplicações em nuvem.

Irei explicar todos os produtos dessa plataforma com exemplos de implementação, a fim de documentar de forma introdutória as possibilidades que temos com ela. Outro motivo de escrever sobre os produtos é praticar e aprender um pouco mais sobre os benefícios e recursos que esta plataforma oferece. Pretendo escrever também sobre outras plataformas com Azure, Heroku, Google, IBM, etc!

Segundo o site da AWS:

> A Amazon Web Services (AWS) é uma plataforma de serviços em nuvem segura, oferecendo poder computacional, armazenamento de banco de dados, distribuição de conteúdo e outras funcionalidades para ajudar as empresas em seu dimensionamento e crescimento. Explore como milhões de clientes estão utilizando os produtos e as soluções na Nuvem AWS e soluções para construir aplicações sofisticadas com mais flexibilidade, escalabilidade e confiabilidade.

## Preço

AWS cobra por hora em alguns produtos e por consumo em outros. A AWS também oferece um nível gratuito que é de 12 meses após o cadastro na plataforma, porém existem limitações. Por exemplo o produto EC2, máquina virtual, você tem no plano gratuito 750 horas de instância de 1 giga de memória com Linux ou Windows.

## Produtos

Conforme vou escrevendo sobre os produtos irei linkando os posts nos títulos abaixo:

AWS divide seus produtos em categorias como:

#### Computação
| Produto | Descrição |
| --- | --- |
| [Amazon EC2](/2016/12/16/Amazon-EC2) | *Servidores virtuais na nuvem* |
| Amazon EC2 Container Registry | *Armazenar e recuperar imagens do Docker* |
| Amazon EC2 Container Service | *Executar e gerenciar contêineres do Docker* |
| [Amazon Lightsail](/2017/01/13/Amazon-Lightsail/) | *Execute e gerencie servidores privados virtuais* |
| Amazon VPC | *Recursos de nuvem isolados* |
| AWS Batch | *Execute trabalhos em lote em qualquer escala* |
| AWS Elastic Beanstalk | *Executar e gerenciar aplicativos da Web* |
| [AWS Lambda](/2017/01/12/AWS-Lambda) | *Execute seu código em resposta a eventos* |
| Auto Scaling | *Elasticidade automática* |


#### Armazenamento
| Produto | Descrição |
| --- | --- |
| [Amazon S3](/2017/01/02/Amazon-S3/) | *Armazenamento escalável em nuvem* |
| Amazon EBS | *Block Storage para EC2* |
| Amazon Elastic File System | *Armazenamento gerenciado de arquivos do EC2* |
| Amazon Glacier | *Armazenamento de arquivos com baixo custo na nuvem* |
| AWS Storage Gateway | *Integração de armazenamento híbrido* |
| AWS Snowball | *Transporte de dados na escala de petabytes* |
| AWS Snowball Edge | *Transporte de dados na escala de petabytes com computação integrada* |
| AWS Snowmobile | *Transporte de dados na escala de exabytes* |


#### Banco de dados
| Produto | Descrição |
| --- | --- |
| Amazon Aurora | *Banco de dados relacional gerenciado de alto desempenho* |
| Amazon RDS | *Serviço de banco de dados relacional gerenciado para MySQL, PostgreSQL, Oracle, SQL Server e MariaDB* |
| [Amazon DynamoDB](/2017/01/03/Amazon-DynamoDB/) | *Banco de dados NoSQL gerenciado* |
| Amazon ElastiCache | *Sistema de cache de memória* |
| Amazon Redshift | *Data warehousing rápido, simples e econômico* |


#### Migração
| Produto | Descrição |
| --- | --- |
| AWS Database Migration Service | *Migre bancos de dados com tempo de inatividade mínimo* |
| AWS Server Migration Service | *Migre servidores locais para a AWS* |


#### Redes e entrega de conteúdo
| Produto | Descrição |
| --- | --- |
| Amazon CloudFront | *Rede de entrega de conteúdo global* |
| [Amazon Route 53](/2017/01/05/Amazon-Route-53/) | *Sistema de nomes de domínio escalável* |
| AWS Direct Connect | *Conexão de rede dedicada à AWS* |
| Elastic Load Balancing | *Balanceamento de carga em alta escala* |


#### Ferramentas de desenvolvedor
| Produto | Descrição |
| --- | --- |
| AWS CodeCommit | *Armazene código em repositórios Git privados* |
| AWS CodeBuild | *Crie e teste código* |
| AWS CodeDeploy | *Automatize a implantação de códigos* |
| AWS CodePipeline | *Faça o lançamento de softwares usando a distribuição contínua* |
| AWS X-Ray | *Analise e depure suas aplicações* |
| Interface da linha de comando da AWS | *Ferramenta unificada para gerenciar Serviços da AWS* |


#### Ferramentas de gerenciamento
| Produto | Descrição |
| --- | --- |
| Amazon CloudWatch | *Monitore recursos e aplicações* |
| Amazon EC2 Systems Manager | *Configure e gerencie instâncias EC2 e servidores locais* |
| AWS CloudFormation | *Crie e gerencie recursos com modelos* |
| AWS CloudTrail | *Rastreie atividades de usuário e uso de APIs* |
| AWS Config | *Rastreie inventário e alterações de recursos* |
| AWS OpsWorks | *Automatize operações usando o Chef* |
| AWS Service Catalog | *Crie e use produtos padronizados* |
| AWS Trusted Advisor | *Otimize o desempenho e a segurança* |
| AWS Personal Health Dashboard | *Visualização personalizada da saúde de Serviços da AWS* |

#### Segurança, identidade e conformidade
| Produto | Descrição |
| --- | --- |
| [AWS Identity & Access Management](/2017/01/03/AWS-Identity-Access-Management/) | *Gerencie o acesso de usuário e as chaves de criptografia* |
| Amazon Inspector | *Analise a segurança do aplicativo* |
| [AWS Certificate Manager](/2017/01/11/AWS-Certificate-Manager/) | *Provisione, gerencie e implante certificados SSL/TLS* |
| AWS CloudHSM | *Armazenamento de chaves baseado em hardware para conformidade normativa* |
| AWS Directory Service | *Hospede e gerencie o Active Directory* |
| AWS Key Management Service | *Criação e controle gerenciados de chaves de criptografia* |
| AWS Organizations | *Gerencie as definições de várias contas* |
| AWS Shield | *Proteção contra DDoS* |
| AWS WAF | *Filtre tráfego da web mal-intencionado* |


#### Análise
| Produto | Descrição |
| --- | --- |
| Amazon Athena | *Consulte dados no S3 usando SQL* |
| Amazon EMR | *Estrutura de Hadoop hospedada* |
| Amazon CloudSearch | *Serviço de pesquisa gerenciada* |
| Amazon Elasticsearch Service | *Execute e escale clusters do Elasticsearch* |
| Amazon Kinesis | *Trabalhe com dados de streaming em tempo real* |
| Amazon Quicksight | *Serviço rápido de análise empresarial* |
| AWS Data Pipeline | *Serviço de orquestração para fluxos de trabalho periódicos e direcionados a dados* |
| AWS Glue | *Prepare e carregue dados* |


#### Inteligência Artificial
| Produto | Descrição |
| --- | --- |
| Amazon Lex | *Crie chatbots de voz e texto* |
| Amazon Polly | *Transforme texto em falas realistas* |
| Amazon Rekognition | Pesquise e analise imagens* |
| Amazon Machine Learning | *Aprendizagem de máquina para desenvolvedores* |


#### Serviços móveis
| Produto | Descrição |
| --- | --- |
| AWS Mobile Hub | *Crie, teste e monitore aplicações* |
| [Amazon API Gateway](/2017/01/06/Amazon-API-Gateway/) | *Crie, implante e gerencie APIs* |
| Amazon Cognito | *Sincronização de identidades de usuário e dados de aplicativos* |
| Amazon Pinpoint | *Notificações por push para aplicações móveis* |
| AWS Device Farm | *Teste aplicativos Android, FireOS e iOS em dispositivos reais na nuvem* |
| AWS Mobile SDK | *Kit de desenvolvimento de software móvel* |


#### Serviços de aplicação
| Produto | Descrição |
| --- | --- |
| AWS Step Functions | *Coordene aplicações distribuídas* |
| Amazon Elastic Transcoder | *Transcodificação de mídia escalável e fácil de usar* |


#### Sistema de mensagens
| Produto | Descrição |
| --- | --- |
| Amazon SQS | *Serviço de enfileiramento de mensagens* |
| Amazon SNS | *Serviço de envio de notificações* |
| Amazon SES | *Serviço de envio e recebimento de e-mails* |


#### Produtividade empresarial
| Produto | Descrição |
| --- | --- |
| Amazon WorkDocs | *Serviço de armazenamento e compartilhamento empresarial* |
| Amazon WorkMail | *E-mail e calendário empresarial seguro e gerenciado* |


#### Streaming de desktop e aplicações
| Produto | Descrição |
| --- | --- |
| Amazon WorkSpaces | *Serviço de computação de desktop* |
| Amazon AppStream 2.0 | *Faça o streaming de aplicações de desktop de modo seguro para o navegador* |


#### Internet das Coisas
| Produto | Descrição |
| --- | --- |
| Plataforma do AWS IoT | *Conecte dispositivos à nuvem* |
| AWS Greengrass | *Computação, sistema de mensagens e sincronização locais para dispositivos* |
| Botão do AWS IoT | *Versão do Dash Button programável na nuvem* |


#### Desenvolvimento de jogos
| Produto | Descrição |
| --- | --- |
| Amazon Lumberyard | *Um mecanismo de jogos 3D gratuito para várias plataformas, com código-fonte completo e integrado à AWS e ao Twitch* |

## Regiões

AWS tem servidores em várias regiões pelo mundo, e isso influencia o preço dos produtos, pois geralmente você escolhe a região onde seu produto irá rodar, influenciando também a latência do acesso. No dia que foi escrito este post as regiões eram:

- ***Oeste dos EUA*** (Oregon, Califórnia)
- ***Leste dos EUA*** (Virgínia, Ohio)
- ***Canadá*** (Central)
- ***América do Sul*** (São Paulo)
- ***Europa*** (Dublin, Frankfurt, Londres)
- ***Ásia-Pacífico*** (Seul, Cingapura, Sydney, Tóquio)
- ***China*** (Pequim)
