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

## Documentos

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

## Coleções

Uma coleção é um grupo de documentos. Sobre um documento podemos fazer uma analogia com um linha em um banco de dados relacional, logo uma coleção pode ser considerada como uma analogia a uma tabela.

### Esquemas Dinâmicos

Coleções têm esquemas dinâmicos. Isso significa que os documentos em uma única coleção podem ter qualquer número de diferentes "formas". Por exemplo, Vamos analisar os dois documentos (linhas) abaixo e perceba que os dois podem ser armazenados na mesma coleção (tabela) e não iremos ter problema de esquema:

```JSON
{"nome": "Fábio Rogério", "ano_nascimento": 1986}
{"nome": "Silva José", "fone": "4498989898"}
```

Note que os documentos anteriores não só têm tipos diferentes para seus valores (string e integer) mas também tem chaves completamente diferentes. Porque qualquer documento pode ser colocado em qualquer coleção, muitas vezes surge a pergunta: "Por que precisamos de coleções separadas em tudo?" É uma boa pergunta - sem necessidade de esquemas separados para diferentes tipos de documentos, por que devemos usar mais de uma coleção? Existem vários bons razões:

- Manter diferentes tipos de documentos na mesma coleção pode ser um pesadelo para desenvolvedores e administradores. Os desenvolvedores precisam ter certeza de que cada consulta irá retornar apenas os documentos de um determinado tipo ou que o código do aplicativo de consulta pode manipular documentos de diferentes formas. Se estivermos perguntando por postagens de blog, é um incômodo eliminar documentos que contêm dados do autor.
- É muito mais rápido obter uma lista de coleções do que extrair uma lista dos tipos em um
coleção. Por exemplo, se tivéssemos um campo "tipo" em cada documento especificado
se o documento era um "usuario", "cliente" ou "forneceder", seria muito mais lento para encontrar esses três valores em uma única coleção do que ter três separados coleções e consulta a coleção correta.
- Agrupar documentos do mesmo tipo juntos na mesma coleção permite localidade dos dados. Obter várias postagens de blog de uma coleção contendo apenas postagens provavelmente exigem menos buscas de disco do que obter os mesmos posts de uma coleção de mensagens e dados do autor.
- Começamos a impor alguma estrutura em nossos documentos quando criamos índices.
(Isto é especialmente verdadeiro no caso de índices únicos.) Estes índices são definidos por
coleção. Colocando apenas documentos de um único tipo na mesma coleção, pode indexar nossas coleções de forma mais eficiente. Como você pode ver, existem razões sólidas para criar um esquema e para agrupar tipos de documentos juntos, mesmo que o MongoDB não o imponha.

### Nomeação
Uma coleção é identificada por seu nome. Os nomes das coleções podem ser qualquer string UTF-8, com algumas restrições:
- Uma string vazia ("") não é um nome de coleção válido.
- Os nomes das coleções não podem conter caractere \0 (caractere nulo) porque isso delineia o final do nome de uma coleção.
- Você não deve criar nenhuma coleção que comece com system.algum_none ou um prefixo reservado para coleções internas. Por exemplo, a coleção system.users contém os usuários do banco de dados e a coleção system.namespaces contém informações sobre todos as coleções do banco de dados.
- As coleções criadas pelo usuário não devem conter o caractere reservado $ no nome.
Os vários drivers disponíveis para o banco de dados suportam usando $ em nomes de coleção porque algumas coleções geradas pelo sistema contêm isso. Você não deve usar $ em um nome, a menos que você esteja acessando uma dessas coleções.

#### Subcoleções
Uma convenção para organizar coleções é usar subcoleções separadas por namespace
junto com o ponto (.). Por exemplo, um aplicativo contendo um blog pode ter um coleção chamada blog.posts e uma coleção separada chamada blog.authors. Isso é para apenas fins organizacionais - não há relação entre a coleção de blogs e não precisa nem existir) e seus "filhos". Embora as subcoleções não tenham propriedades especiais, elas são úteis e incorpora/dividido em muitas ferramentas do MongoDB:
- GridFS, um protocolo para armazenar arquivos grandes, usa subcoleções para armazenar metadados de arquivos separadamente dos blocos de conteúdo.

### Bancos de dados
Além de agrupar documentos por coleção, o MongoDB agrupa coleções em bases de dados. Uma única instância do MongoDB pode hospedar vários bancos de dados. Um banco de dados tem suas próprias permissões e cada banco de dados é armazenado em arquivos separados no disco. Uma boa regra é armazenar todos os dados para uma única
aplicação na mesma base de dados. Bancos de dados separados são úteis ao armazenar dados para vários aplicativos ou usuários no mesmo servidor MongoDB.

Como coleções, os bancos de dados são identificados pelo nome. Nomes de banco de dados podem ser qualquer UTF-8 string, com as seguintes restrições:

- String vazia ("") não é um nome de banco de dados válido.
- Um nome de banco de dados não pode conter nenhum desses caracteres: /, \,., ", *, <,>,:, |,?, $, (espaço único) ou \0 (caractere nulo). Basicamente, fique com ASCII alfanumérico.
- Os nomes dos bancos de dados fazem distinção entre maiúsculas e minúsculas, mesmo em sistemas de arquivos que não diferenciam maiúsculas de minúsculas. Manter coisas simples, tente usar apenas caracteres minúsculos.
- Os nomes dos bancos de dados estão limitados a um máximo de 64 bytes. Uma coisa a lembrar sobre nomes de banco de dados é que eles realmente acabam como arquivos no seu sistema de arquivos. Isso explica porque muitas das restrições anteriores existem.
Existem também vários nomes de bancos de dados reservados, que você pode acessar, mas que semântica especial. Estes são os seguintes:

*admin*
Este é o banco de dados "root", em termos de autenticação. Se um usuário é adicionado ao administrador banco de dados, o usuário herda automaticamente as permissões para todos os bancos de dados. tem também determinados comandos do servidor que podem ser executados somente a partir do banco de dados de administração, como listar todos os bancos de dados ou desligar o servidor.

*local*
Este banco de dados nunca será replicado e pode ser usado para armazenar quaisquer coleções local para um único servidor.

*config*
Quando o MongoDB está sendo usado em uma configuração compartilhada ele usa a configuração de banco de dados para armazenar informações sobre os fragmentos.

Ao concatenar um nome de banco de dados com uma coleção nesse banco de dados, você pode obter um nome de coleção qualificado chamado namespace. Por exemplo, se você estiver usando o blog.posts no banco de dados "cms", o namespace dessa coleção seria
cms.blog.posts. Os namespaces são limitados a 121 bytes de comprimento e, na prática,
ter menos de 100 bytes de comprimento.

No próximo passo vamos instalar, configurar e iniciar o MongoDB.

