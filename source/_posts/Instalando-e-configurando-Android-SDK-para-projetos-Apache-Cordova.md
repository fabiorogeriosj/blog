---
title: Instalando e configurando Android SDK para projetos Apache Cordova
date: 2018-02-14 11:09:10
header-img: /img/bannerandroid.jpg
tags:
- Android SDK
- Java JDK
- cordova build
- Gradle
---

> Se você utiliza Cordova/MobileUI/Ionic/Phonegap e outros frameworks que utilizam o Apache Cordova para buildar os apps finais (.apk) certamente precisa do ambiente do Android SDK configurado, então este post é para você.

Como a Android muda com frequência seu SDK este post precisa de atualizações. Última atualização em: <strong>14/02/2018</strong>

Para buildar um app criado com base no [Apache Cordova](https://cordova.apache.org/) precisamos o Android SDK instalado e configurado, e essa tarefa não é complicada mas requer alguns passos que precisam ser seguidos de forma confiável.

Uma forma fácil de realizar essa tarefa porém pode gerar outros problemas é instalar o Android Studio, pois com ele vem o Android SDK, mas essa tarefa querer mais poder de processamento de sua máquina, então vamos utilizar apenas o Tools do Android SDK.

Uma dica que dou é, caso você já tenha uma das ferramentas instalada, remova e faça a instalação novamente para utilizar a última versão dos mesmos.

## JAVA JDK

O Android SDK precisa o Java JDK para funcionar, uma vez que o mesmo foi desenvolvido em Java. Para isso acesse o site do Java JDK e faça o download e a instalação que é Proximo > Proximo > e Concluir :)

![site Java JDK](/img/sdkimg1.jpg)

![site Java JDK](/img/sdkimg2.jpg)

![site Java JDK](/img/sdkimg3.jpg)

![site Java JDK](/img/sdkimg4.jpg)

Uma vez concluído precisamos criar uma variável no ambiente em execução para o Android SDK saber onde foi instalado o Java JDK. Sendo assim clique com o botão direito do mouse sobre "Meu computador" e siga as imagens:

![Variáveis de Ambiente](/img/sdkimg5.jpg)

![Variáveis de Ambiente](/img/sdkimg6.jpg)

![Variáveis de Ambiente](/img/sdkimg7.jpg)

![Variáveis de Ambiente](/img/sdkimg8.jpg)

O caminho do Java JDK pode mudar dependendo da versão que você instalou, então confira o caminho correto para não ter erro.

Agora vamos colocar o caminho do bin do Java JDK no PATH para poder ser executado as aplicações do Java em diferentes locais via terminal, por exemplo você irá precisar desse passo quando for fazer a assinatura do APK para enviar para play store.

Clique em editar no path e add o caminho completo até a pasta bin do JAVA JDK:

![Variáveis de Ambiente](/img/sdkimg9.jpg)

![Variáveis de Ambiente](/img/sdkimg10.jpg)

Iremos criar outras variáveis de ambiente nos próximos passos. Mas antes vamos instalar as demais ferramentas.

## Gradle Wapper

O Android SDK também utiliza o Gradle para realizar o build de seus apps, então precisamos dessa tools instalada e configurada. A instalação é bem simples, basta fazer o download da versão mais atual e adicionar a mesma nas variáveis de ambiente:

![Variáveis de Ambiente](/img/sdkimg11.jpg)

Descompacte o arquivo baixado e renomeie a pasta gradle-x.x.x para apenas gradle, recomendo para ficar mas fácil de encontrar os caminhos você deixar essa pasta no c:/gradle

![Variáveis de Ambiente](/img/sdkimg12.jpg)

Dentro desta pasta terá outras como bin, lib, etc.

Agora vamos adicionar nas variáveis de ambiente no PATH o caminho da pasta bin do gradle:

![Variáveis de Ambiente](/img/sdkimg13.jpg)

Até aqui tudo show! Agora vamos para o Android SDK.

## Android SDK

Como descrevi anteriormente o Android SDK vem juntamente com o Android Studio (IDE responsável por criar os apps nativos), mas não queremos trazer todas as dependências que o mesmo entrega quando instalado, para isso iremos instalar apenas o SDK Tools e instalar as dependencias apenas que o Apache Cordova precisa.

Na página de download do Android Studio, role a página até o final e baixa apenas o sdk-tools (aceite os termos caso abra uma modal):

![Variáveis de Ambiente](/img/sdkimg14.jpg)

Descompacte essa pasta em um lugar que fica fácil acessar via linha de comando, sugiro você deixar no caminho c:\android-sdk\tools

![Variáveis de Ambiente](/img/sdkimg15.jpg)

Agora vamos criar as variáveis de ambiente para o Android SDK:

![Variáveis de Ambiente](/img/sdkimg16.jpg)

![Variáveis de Ambiente](/img/sdkimg17.jpg)

Agora precisamos instalar as dependências do Android SDK para buildar os apps via Apache Cordova.
Abra o terminal e em qualquer caminho digite `sdkmanager` para testarmos se as variáveis que criamos de ambientes estão corretas e o `sdkmanager` está acessível de forma global.

![Variáveis de Ambiente](/img/sdkimg18.jpg)

Se algo parecido com a tela acima for apresentado significa que estamos no caminho certo.

Agora vamos instalar as dependências, isso pode demorar um pouco. No terminal digite o comando abaixo:

```
sdkmanager "build-tools;27.0.3" "platform-tools" "platforms;android-27" "extras;google;m2repository" "extras;android;m2repository"
```
Ao executar este comando uma licença será exibida e você precisa aceitá la para continuar:

![Variáveis de Ambiente](/img/sdkimg19.jpg)

Agora é preciso ter paciência e aguardar, pois este processo demora um pouco dependendo de sua conexão com a internet.

No final irá aparecer o texto `done` no terminal!

Agora vamos testar se tudo está ok!

## Concluindo

Para testarmos vamos criar um projeto cordova e testar o build. No terminal digite os comandos abaixo:

```
cordova create appTeste
cd appTeste
cordova platform add android
cordova build android
```

Na primeira vez que executar o build do cordova o mesmo irá baixar algumas dependências, isso é normal e é apenas na primeira vez.

Se tudo deu certo e nada deu errado ;) você irá ver seu projeto buildado com sucesso:

![Build OK](/img/sdkimg20.jpg)

### Possíveis erros

Se você já tiver instalado anteriormente uma versão do Android SDK e não removeu completamente alguns erros podem acontecer como o: `Could not find gradle wrapper within android sdk might need to update your android sdk`

Para corrigir isso é simples, crie a estrutura de pastas exibido no comando abaixo e execute o gradle wapper:

```
cd c:\android-sdk\tools
mkdir templates
cd templates
mkdir gradle
cd gradle
mkdir wrapper
cd wrapper
gradle wrapper
```

Isso irá resolver o problema do wrapper não encontrado. Caso ocorra mais algum erro com você poste nos comentários que vou atualizando este documento.