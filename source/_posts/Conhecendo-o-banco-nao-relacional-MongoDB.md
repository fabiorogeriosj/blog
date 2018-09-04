---
title: Conhecendo o banco não relacional MongoDB
date: 2018-08-10 09:21:25
header-img: /img/nosql.jpg
tags: 
- NoSQL
- MongoDB
- Introdução
---

> Com o objetivo de praticar e compartilhar meu conhecimento adquirido com MongoDB, irei escrever uma série de posts baseado no livro: MongoDB: The Definitive Guide, Second Edition. Espero que gostem e ajude :)

O MongoDB é um banco de dados extremamente poderoso, flexível e escalável. Com ele podemos combinar a capacidade de expansão como índices secundários, consultas de intervalo, classificação, agregações e índices geoespaciais. 

## Fácil de usar
O MongoDB é um banco de dados orientado a documentos, não um banco de dados relacional. O principal motivo para não usar o modelo relacional é facilitar o dimensionamento, mas existem algumas outras vantagens também.
Um banco de dados orientado a documentos substitui o conceito de “linha” por um mais flexível, o “documento”. Ao permitir documentos e matrizes incorporados, o documento permite representar relações hierárquicas complexas com um único registro. Isso se encaixa naturalmente na forma como os desenvolvedores de sistemas modernos pensam sobre seus dados.
Também não há esquemas pré definidos: as chaves e os valores de um documento não são fixos. Sem um esquema fixo, adicionar ou remover campos conforme necessário se torna
Mais fácil. Geralmente, isso torna o desenvolvimento mais rápido, pois os desenvolvedores podem fazer iterações rapidamente. 

## Escala Fácil
Os tamanhos dos conjuntos de dados para aplicativos estão crescendo em um ritmo incrível. É comum aplicações, até mesmo simples, necessitar armazenar terabytes de dados. À medida que aumenta a quantidade de dados que os desenvolvedores precisam armazenar, os desenvolvedores enfrentam dificuldades de decisão: como devem escalar seus bancos de dados? Escalar um banco de dados se reduz ao escolha entre aumentar a escala (obter uma máquina maior) ou dimensionar (particionar dados em mais máquinas). O escalonamento é frequentemente o caminho de menor resistência, mas caras: máquinas grandes são frequentemente muito caras, e eventualmente um limite físico é atingido onde uma máquina mais potente não pode ser comprada a qualquer custo. A alternativa é scale out: para adicionar espaço de armazenamento ou aumentar o desempenho, compre outro servidor de commodity e adicione-o ao seu cluster. Isso é mais barato e mais escalável; no entanto, é mais difícil administrar mil máquinas do que cuidar de uma.

O MongoDB foi projetado para expandir. Seu modelo de dados orientado a documentos facilita para dividir os dados em vários servidores. O MongoDB cuida automaticamente de
balanceamento de dados e carga em um cluster, redistribuindo documentos automaticamente e encaminhando solicitações de usuários para as máquinas corretas. Isso permite que os desenvolvedores se concentrem em regras do aplicativo. Quando um cluster precisa de mais capacidade, novos “nós” podem ser adicionados e o MongoDB descobrirá como os dados existentes devem ser espalhar para eles.

## Muitos recursos
O MongoDB é destinado a ser um banco de dados de propósito geral, portanto, além de criar, ler, atualizando e excluindo dados, ele fornece uma lista cada vez maior de recursos exclusivos:

*Indexação*
O MongoDB suporta índices secundários genéricos, permitindo uma variedade de consultas rápidas, e fornece recursos de indexação exclusivos, compostos, geoespaciais e de texto completo.

*Agregação*
O MongoDB suporta um “pipeline de agregação” que permite construir agregações de peças simples e permitir que o banco de dados otimize-o.

*Tipos de coleção especial*
O MongoDB suporta coleções time-to-live para dados que devem expirar em um certo
tempo, como sessões. Ele também suporta coleções de tamanho fixo, que são úteis para
mantendo dados recentes, como logs.

*Armazenamento de arquivo*
O MongoDB suporta um protocolo fácil de usar para armazenar arquivos grandes e metadados de arquivos.


Alguns recursos comuns aos bancos de dados relacionais não estão presentes no MongoDB, especialmente junções e transações multirrisco complexas. Omitir estes foi uma decisão arquitetônica para permitir maior escalabilidade, já que ambos os recursos são difíceis de fornecer com eficiência em um sistema distribuído.


O desempenho incrível é uma meta importante para o MongoDB e moldou grande parte de seu design. O MongoDB adiciona preenchimento dinâmico a documentos e pré-aloca arquivos de dados para transações extras.  Ele usa o máximo de RAM que puder, assim como seu cache e tenta escolher automaticamente os índices corretos para as consultas. Em resumo, quase todos os aspectos do MongoDB foram projetados para manter o alto desempenho.

Embora o MongoDB seja poderoso e tente manter muitos recursos de sistemas relacionamentos, não se destina a fazer tudo o que um banco de dados relacional faz. Sempre que possível, o servidor de banco de dados transfere o processamento e a lógica para o lado do cliente pelos drivers ou pelo código do aplicativo de um usuário. Mantendo este design simplificado é uma das razões pelas quais o MongoDB pode alcançar um desempenho tão alto.



