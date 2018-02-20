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

Este é um registro de trabalho onde coloco passo a passo de como realizei uma determinada tarefa a fim de documentar, registrar e compartilhar o conhecimento adquirido!

> O objetivo dessa tarefa foi entender, de forma aprofundada, os recursos que trabalhar com containers pode proporcionar para uso mais eficiente de máquinas em uma aplicação de missão crítica.

É comum quem está iniciando os estudos com Docker se confundir com máquinas virtuais, porém o Docker é bem diferente no quesito de virtualização.

Docker pode ser considerado como uma plataforma aberta que foi criado com o principal objetivo de facilitar o desenvolvimento, implantação e execução de aplicações em ambientes isolados, (existe uma familiaridade com máquinas virtuais aqui) isso traz uma garantia mais segura que os ambientes que serão executando, tanto em desenvolvimento quanto em produção, serão os mesmos.

Imagina que você está desenvolvendo localhost sua aplicação e precisa colocar isso em produção, com o Docker você tem a segurança de colocar sua máquina para rodar em produção (analogia), pois o mesmo ambiente que você usa para testar sua aplicação será a mesma que estará em produção.

Outro ponto que o Docker melhorou foi a conversa entre os desenvolvedores e administradores de servidor (DevOps) pois o desenvolvedor consegue, sem muito esforço, validar e implementar sua própria infraestrutura de servidor.

O diferencial do Docker é o modelo de empacotar sua aplicação conhecida como "container" e virar uma imagem Docker, vamos entender melhor olhando para imagem abaixo:

![Docker](/img/docker1.png)

No modelo tradicional de virtualização (VMs) podemos perceber que cada nova aplicação rodando dentro de uma máquina física é replicada o ambiente como um todo, ou seja, dentro de cada máquina VM existe um sistema operacional e todo o sistema de arquivos base, isso consome muito recurso e poder de processamento.

Já no modelo de container perceba que os recursos são compartilhados entre os containers pelo seu hospedeiro, ou seja, os container usa o próprio recurso operacional da máquina física e com isso aumenta a eficiência no uso dos mesmos. Os containers são isolados a nível disco, memória, processamento e rede.

Outro ponto importante é que o tempo para disponibilizar esses containers são calculados em seguindo, uma vez que ele não precisa levantar todo o sistema de arquivo mas sim apenas a aplicação desejada.

Essas imagens podem ser customizadas conforme desejar ou pode ser utilizado uma já instalada com a aplicação necessário, por exemplo se você precisar de uma imagem que tenha o NodeJS instalado, você pode baixar a mesma de um ambiente público e utilizar em seu ambiente, e esta mesma imagem pode ir para produção garantindo assim que os ambientes estarão sempre idênticos.

Em resumo o Docker isola uma virtualização em nível do sistema operacional, ou seja, ele utiliza apenas o kernel de seu hospedeira e com isso você pode ter um ou mais containers que estão trabalhando com recursos compartilhados porém isolados. É possível liberar para que os containers compartilhem recursos de cada um entre outros.

![Docker](/img/docker2.jpg)

O armazenamento de arquivos de cada container também é isolado por padrão, ou seja, se você salva arquivos estáticos na sua aplicação eles serão perdidos quando um container morrer, diferente de uma máquina virtual.

Quando você precisa persistir arquivos existem três opções:

- Mapeamento de pasta específica do host:
-- Neste modelo é feito um mapeamento de uma determinada pasta que existe no host para dentro do container.
- Mapeamento via contêiner de dados:
-- É possível criar um container com a principal função de ter um volume que pode ser utilizado por outros container, um problema nessa abordagem é que esse container não pode ser destruído pois os arquivos estáticos seriam perdidos.
- Mapeamento de volumes:
-- A partir da versão 1.9 do Docker é possível criar um volume isolado de um container, facilitando assim a portabilidade do volume com demas containers.

Para saber mais sobre o sistema de armazenamento do Docker recomendo ler [esse post](http://techfree.com.br/2015/12/entendendo-armazenamentos-de-dados-no-docker/).

## Instalação

No momento que este post foi criado a Docker estava dividido em duas versões de produto, uma sendo `Community` e outra sendo `Enterprise`. No meu caso utilizei a CE, sendo assim baixe a versão para seu sistema operacional direto do [site do docker](https://www.docker.com/community-edition) e next, next, next.

Para o docker funcionar você precisa habilitar a virtualização em seu sistema, abaixo podemos ver no windows:

![Docker](/img/docker3.jpg)

Após a conclusão precisamos iniciar o serviço de daemon do docker, para isso, no meu caso que estou testando em ambiente Windows, abra como administrador o "Docker for Windows".

> A primeira vez que essa aplicação é executada o Docker irá analisar toda sua estrutura de máquina e verificar se pode executar as aplicações, no meu caso tive que adicionar meu usuário no grupo `docker-users` para resolver esse problema apontado [no GitHub](https://github.com/docker/for-win/issues/868#issuecomment-312639000).

Após o docker estar em execução, vamos abrir nosso terminal e rodar um container que retorna apenas uma mensagem no terminal, isso será nosso teste final para ver se a instalação foi bem sucedida de fato. No terminal digite:

```
docker container run hello-world
```

Este comando irá rodar uma imagem chamada `hello-world` mas antes de ser exibida a mensagem no terminal ele faz alguns passos para essa tarefa ser concluída, tais como:
- Primeiro o docker verifica se essa imagem já foi baixada localmente, se não foi ele baixa do [Docker Hub](https://hub.docker.com/).
- O docker cria um novo container com a imagem `hello-world` e executa a mesma.
- O docker exibe a mensagem de retorno `Hello from Docker!` vinda do container e finaliza o processo do container.

Pronto, o Docker está instalado com sucesso!

## Comandos essenciais

O comando de rodar um container já vimos anteriormente mas basicamente ele funciona nessa ordem:

```
docker container run <parâmetros> <imagem> <CMD> <argumentos>
```

Vamos executar um container com a imagem do ubuntu e aberto o bash em nosso terminal com interação, isso mesmo vamos abrir um terminal bash do linux no CMD do windows :)

Para isso execute o comando:

```
docker container run -it ubuntu bash
```

Passamos como parâmetro o `-it`, que habilita o container pra ser interativo e aloca um pseudo TTY, algo como um terminal interativo. O  comando `bash` passado depois da imagem é o primeiro programa que será executado após o container estar rodando.

Para sair e finalizar o container digite `exit`.

Os parâmetros mais utilizados na execução de um container porém ser:

| Parâmetro | Descrição |
|---|---|
| `-d` | Execução do container em background |
| `-i` | Modo interativo. Mantém o STDIN aberto mesmo sem console anexado |
| `-t` | Aloca uma pseudo TTY |
| `--rm` | Automaticamente remove o container após finalização (Não funciona com -d)
| `--name` | Nomear o container |
| `-v` | Mapeamento de volume |
| `-p` | Mapeamento de porta |
| `-m` | Limitar o uso de memória RAM |
| `-c` | Balancear o uso de CPU |

Outro comando útil é o `image list` que exibe as imagens baixadas em nosso sistema (host):

```
docker image list
```

É possível baixar uma nova versão para casa imagem executando o comando `docker image pull <imagem>`.

Para testarmos outros parâmetros vamos criar um outro container, utilizando a imagem `node`, com o objetivo de criar uma API simples com NodeJS. Nosso comando ficará:

```
docker container run -it --rm -p 80:8080 -m 512M node bash
```

O comando acima cria um novo container utilizando no máximo 512M de memória do host e será removido após ser finalizado, e também irá expor a porta 80 para o host mapeando a porta 8080 do container.

Agora vamos instalar um módulo binário para o NodeJS que cria uma API a partir de um arquivo JSON:

```
npm install -g json-server
cd /opt
echo '{ "users":[] }' > api.json
json-server --watch api.json --port 8080
```

Você pode testar essa sua API fazendo GET/POST/DELETE no IP da sua máquina física (host) na porta 80:

![Docker](/img/docker4.jpg)

Com o container ainda em execução, abra outro terminal e vamos verificar todos os containers executados no host:

```
docker container list -a
```

O parâmetro `-a` informa que geremos ver todos os containers já executados no host. Outros possíveis parâmetros são:

| Parâmetro | Descrição
| --- | --- |
| `-a` | Lista todos os containers, inclusive os desligados |
| `-l` | Lista os últimos containers, inclusive os desligados |
| `-n` | Lista os últimos N containers, inclusive os desligados |
| `-q` | Lista apenas os ids dos containers, ótimo para utilização em scripts |

> É possível utilizar o comando abreviado como: `docker ps`.

Podemos também parar um container em execução com o comando `stop` passando como parâmetro o id do container ou o nome:

```
docker container stop 484939aw342c
```

Se observar no outro terminal que estava em execução o mesmo terá aplicado o comando `exit`, e neste caso nossa API com os dados também foram removidos, então para manter os dados gerados dentro do container e mantê lo salvo vamos adicionar um volume apontando uma pasta do host:

Para finalizar nossos comandos essenciais vamos fazer com que nosso container execute de forma que os dados sejam salvos em um volume e a execução do servidor seja automática. Na máquina física vamos criar uma pasta no c: com o nome api e vamos mapear esta pasta para dentro do container no caminho /api:

```
docker container run -it --rm -p 80:8080 -m 512M -v c:\api\:/api node bash
```

Agora dentro do container podemos entrar no diretório `/api` e criar nosso arquivo de banco da API e um executável `.sh` para iniciar o processo da API quando o container for levantado:

```
cd /api
echo '{ "users":[] }' > db.json
echo '#!/bin/bash' > server.sh
echo 'npm install -g json-server' >> server.sh
echo 'json-server --watch db.json --port 8080' >> server.sh
```

Se você observar pelo windows explorer, irá ver que em sua pasta `c:\api` terá os arquivos que criamos no container.

Agora vamos sair/matar o container e executar passando como executável o arquivo `.sh`:

```
docker container run -it --rm -p 80:8080 -m 512M -v c:\api\:/api node /api/server.sh
```

Perceba que ao rodar o novo container o script será executado e sua API estará ativada e pronta para uso :)

Essa não é uma boa prática para ter scripts dentro do container, pois o tempo de `uptime` é maior do que ter uma imagem do container já instalado nas dependências `json-server`. Vamo ver isso na próxima sessão.

## Criando imagem Docker

Existe duas formas de criar uma imagem Docker, ou por `commit` ou via `Dockerfile`. Nos testes que fiz `commit` não se mostrou eficiente pois ficou com um baixa rastreabilidade.

O processo de criar imagem no Docker é simples, você escolhe uma imagem base para seguir, faz suas alterações e gera uma nova imagem.

Crie um arquivo chamado `Dockerfile` com o seguinte conteúdo:

```
FROM node:latest
RUN npm install -g json-server
CMD bash -c 'json-server --watch /api/db.json --port 8080'
```

Neste arquivo instruímos essa imagem seguir da imagem `node`, na construção rodamos o `npm install` para instalar o pacote na mesma e depois descrevemos o comando que será executado como default para a imagem. Para gerar uma nova imagem digite o comando abaixo:

```
docker image build -t node:api .
```

Neste comando estamos informando o nome da imagem `-t` que será `node:api`, e qual o diretório que ele irá ler para pegar o Dockfile. Também é possível copiar arquivos deste diretório para dentro do container com o comando `COPY`, mas deixaremos isso para outro momento.

Se executar agora o comando `docker image list` irá ver que existe uma nova imagem, sendo assim vamos criar um novo container a partir dessa nova imagem customizada:

```
docker container run -it  --rm -p 80:8080 -m 512M -v c:\api\:/api node:api
```

Perceba que utilizamos a imagem nova e não passamos um comando a ser executado assim que tiver ativa, pois queremos que seja executado o comando default informado na criação desta imagem. Perceba também que o tempo de subir o novo container é mais rápido :)