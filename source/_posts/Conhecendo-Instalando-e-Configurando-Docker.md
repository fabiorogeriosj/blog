---
title: 'Conhecendo, Instalando e Configurando Docker'
date: 2018-02-15 17:02:49
header-img: /img/bannercontainers.jpg
tags: 
- Docker
- Containers
- DevOps
- Registro de trabalho
---

Este é um registro de trabalho onde coloco passo a passo de como realizei uma determinada tarefa afim de documenentar, registrar e compartilhar o conhecimento adquirdo!

> O objetivo dessa tarefa foi entender, de forma aprofundada, os recursos que trabalhar com containers pode proporcionar para uso mais eficiente de máquinas em uma aplicação de missão crítica.

É comum quem está iniciando os estudos com Docker se confundir com maquinas virtuais, porem o Docker é bem diferente no quisito de virtualização.

Docker pode ser considerado como uma plataforma aberta que foi criado com o principal objetivo de facilitar o desenvolvimento, implantação e execução de aplicações em ambientes isolados, (existe uma familiariedade com maquinas virtuais aqui) isso traz uma garantia mais segura que os ambientes que serão executando, tanto em desenvolvimento quanto em produção, serão os mesmos.

Imagina que você está desenvolvendo localhost sua aplicação e precisa colocar isso em produção, com o Docker você tem a segurança de colocar sua maquina para rodar em produção (analogia), pois o mesmo ambiente que você usa para testar sua aplicação será a mesma que estará em produção.

Outro ponto que o Docker melhorou foi a conversa entre os desenvolvedores e administradores de servidor (DevOps) pois o desenvolvedor consegue, sem muito esforço, validar e implementar sua proprioa inframestrutura de servidor.

O diferencial do Docker é o modelo de empacotar sua aplicação conhecida como "container" e virar uma imagem Docker, vamos entender melhor olhando para imagem abaixo:

![Docker](/img/docker1.png)

No modelo tradicional de virtualização (VMs) podemos perceber que cada nova aplicação rodadando dentro deuma máquina fisica é replicada o ambiente como um todo, ou seja, dentro de cada maquina VM existe um sistema operacional e todo o sistema de arquivos base, isso consome muito recurso e poder de processamento.

Ja no modelo de container perceba que os recursos são compartilhados entre os containers pelo seu hospedeiro, ou seja, os container usa o proprio recurso operacional da maquina física e com isso aumenta a eficiencia no uso dos mesmos. Os containers são isolados a nível disco, memória, processamento e rede.

Outro ponto importante é que o tempo para disponibilizar esses containers são calculados em seguindo, uma vez que ele não precisa levantar todo o sistema de aquivo mas sim apenas a aplicação desejada.

Essas imagens podem ser customizadas conforme desejar ou pode ser utilizado uma já instalada com a aplicação necessário, por exemplo se você precisar de uma imagem que tenha o NodeJS instalado, você pode baixar a mesma de um ambiente público e utilizar em seu ambiente, e esta mesma imagem pode ir para produçao garantindo assim que os ambientes estaram sempre identicos.

Em resumo o Docker isola uma virtualização em nível do sistema operacional, ou seja, ele utiliza apenas o kernel de seu hospedeira e com isso você pode ter um ou mais containers que estão trabalhando com recursos compartilhados porem isolados. É possível liberar para que os containers compartilhem recursos de cada um entre outros.

![Docker](/img/docker2.jpg)

O armazenamento de arquivos de cada container também é isolado por padrão, ou seja, se você salva arquivos estátivos na sua aplicação eles serão perdidos quando um container morrer, diferente de uma maquina virtual.

Quando você precisa persistir arquivos existem três opções:

- Mapeamento de pasta esfecifica do host:
-- Neste modelo é feito um mapeamento de uma deterinada pasta que existe no host para dentro do container.
- Mapeamento via contêiner de dados:
-- É possível criar um container com a principal função de ter um volume que pode ser utilizado por outros container, um problema nessa abordagem é que esse container não pode ser destruido pois os arquivos estáticos seriam perdidos.
- Mapeamento de volumes:
-- A partir da versão 1.9 do Docker é possível criar um volume isuloado de um container, facilitando assim a portabilidade do volume com demas containers.

Para saber mais sobre o sistema de armazenamento do Docker recomendo ler [esse post](http://techfree.com.br/2015/12/entendendo-armazenamentos-de-dados-no-docker/).

Continua...