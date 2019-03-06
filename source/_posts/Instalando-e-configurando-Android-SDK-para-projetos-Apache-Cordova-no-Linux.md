---
title: Instalando e configurando Android SDK para projetos Apache Cordova no Linux
date: 2019-03-06 11:09:10
header-img: /img/bannerandroid.jpg
tags:
- Android SDK
- Java JDK
- cordova build
- Gradle
---

### Para usuários Windows veja [este post](/2018/02/14/Instalando-e-configurando-Android-SDK-para-projetos-Apache-Cordova/)

> Se você utiliza Cordova/MobileUI/Ionic/Phonegap e outros frameworks que utilizam o Apache Cordova para buildar os apps finais (.apk) certamente precisa do ambiente do Android SDK configurado, então este post é para você.

Como a Android muda com frequência seu SDK este post precisa de atualizações. Última atualização em: <strong>06/03/2019</strong>

## Java Development Kit
Faça o download do Java JDK [aqui](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html). Escolha o (*.tar.gz).

Quando o download terminar, vá para o local desejado e extraia os arquivos:

```terminal
$ cd /usr/local
$ sudo mkdir java && cd java
$ sudo tar xzvf ~/Downloads/jdk-*****_***.tar.gz
```

Precisamos adicionar essa pasta extraída ao caminho do sistema. Isso é feito atualizando o arquivo ~ /.bashrc com um editor (vi, vim, nano, etc.).

```terminal
$ cd ~
$ vim .bashrc
```

Adicione essas duas linhas à parte inferior do arquivo (certifique-se de substituir *** pelo número correto que corresponde ao caminho do diretório).

```terminal
export JAVA_HOME="/usr/local/java/jdk1.8.0_***"
PATH=$PATH:$JAVA_HOME/bin
```

Recarregue bash e confirme as atualizações trabalhadas:

```terminal
$ source .bashrc
$ javac -version      # should print 'javac 1.8.0_***'
```

Java JDK instalado!

## Androd SDK

Se estiver no Ubuntu 64 `sudo apt-get install libc6:i386 libncurses5:i386 libstdc++6:i386 lib32z1 libbz2-1.0:i386`

Se estiver no Fedora 64 `sudo dnf install zlib.i686 ncurses-libs.i686 bzip2-libs.i686`

Vá em Android Studio > Downloads > sdk-tools-linux-XXXXXXX.zip

Espere a página carregar , clique em "I have read and agree with the above terms and conditions" (aceitar) e faça o download.

Depois de baixado criar a seguinte estrutura de pastas (você pode fazer a sua.)

```terminal
$ mkdir -p /opt/android/android-sdk-linux
```

- android: aqui ficaram todas as cosisa relacionadas ao android (mas no momentos só tem uma pasta: android-sdk-linux)
- android-sdk-linux: aqui irá ficar todos os recurso ***

*** Você não irá mecher nessa pasta diretamente, todo o conteúdo dela será criado/excluido/alterado pelas ferramentas do próximo passo.

Dentro da pasta android-sdk-linux descompacte o conteudo do zip baixado (sdk-tools-linux-XXXXXXX.zip), o resultado final será:

```terminal
$ cd /opt/android/android-sdk-linux/tools
$ ls
android  bin  emulator  emulator-check  lib  mksdcard  monitor  NOTICE.txt  package.xml  proguard  source.properties  support
```

Agora vamos adicionar as variaváveis de ambiente, se você reparar algumas pastas ainda não existem, mas calma elas irão ser criadas.

editar o ~/.bashrc e adicionar no final do aquivo:

```
export ANDROID_HOME=/opt/android/android-sdk-linux
export ANDROID_SDK_ROOT=/opt/android/android-sdk-linux
export PATH=$PATH:$ANDROID_HOME/tools
export PATH=$PATH:$ANDROID_HOME/platform-tools
export PATH=$PATH:$ANDROID_HOME/tools/bin
```

Depois recarregue o bashrc e teste o android

```terminal
$ source .bashrc
$ android
```

Entre na pasta onde está o sdkmanager e instale os pacotes de dependência:

```
cd /opt/android/android-sdk-linux/tools/bin
./sdkmanager "build-tools;27.0.3" "platform-tools" "platforms;android-27" "extras;google;m2repository" "extras;android;m2repository"
```

responda [y] para o termo de licença

## Gradle

Para instalar o gradle utilize o gerenciador de pacotes do seu sistema (yum, apt-get, dnf):

```
sudo dnf install gradle
```

## Testando

Por fim, crie um app e gere o APK:

```
cordova create appTesteBuild
cd appTesteBuild
cordova platform add android
cordova build android
```

## Referências
- [https://codeburst.io/configuring-cordova-for-android-development-on-linux-6ee4a28cd432](https://codeburst.io/configuring-cordova-for-android-development-on-linux-6ee4a28cd432)
- [https://brunorozendo.com/post/instalar-adroid-sdk-gnu-linux.html](https://brunorozendo.com/post/instalar-adroid-sdk-gnu-linux.html)