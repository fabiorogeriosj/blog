---
title: Primeiros passos com MongoDB
date: 2018-09-05 09:27:37
header-img: /img/nosql.jpg
tags: 
- NoSQL
- MongoDB
- Primeiros passos
---

> O MongoDB é um banco de dados não relacional poderoso e fácil de começar, tanto em nívem de instalação quanto em nível de utilização.

Podemos começar a entender o  MongoDB com seus documentos que são: um conjunto ordenado de chaves com valores associados. A representação de um documento varia de acordo com a linguagem de programação, mas a maioria das linguagens tem uma estrutura de dados que é um ajuste natural, como um mapa, hash ou dicionário. Em JavaScript, por exemplo, documentos são representados como objetos:

`{"nome" : "Fábio Rogério"}`

Este documento simples contém uma única chave, "nome", com um valor de "Fábio Rogério". A maioria dos documentos serão mais complexos do que esta simples e
irão conter vários pares de chave/valor:

`{"nome" : "Fábio Rogério", "ano_nascimento" : 1986}`

As chaves em um documento são seqüências de caracteres. Qualquer caractere UTF-8 é permitido em uma chave, com algumas exceções:

- As chaves não devem conter o caractere \0 (caractere nulo). Este é usado para significar o fim de uma chave.
- O . e $ têm algumas propriedades especiais e devem ser usados apenas em
certas circunstâncias. Em geral, eles devem ser considerado reservado, e os drives irão reclamar se forem usados de forma inadequada.

O MongoDB é sensível a maiúsculas e minúsculas. Uma última observação importante é que os documentos no MongoDB não podem conter chaves duplicadas. Por exemplo, o seguinte não é um documento legal:

`{"nome" : "Fábio Rogério", "nome" : "Silva José"}`

Os pares chave/valor nos documentos são ordenados: `{"x": 1, "y": 2}` e não é o mesmo que
`{"y": 2, "x": 1}`. Ordem de campo geralmente não importa e você não deve projetar seu esquema depende de uma certa ordenação de campos (o MongoDB pode reordená-los). 

Em desenvolvimento...

