---
title: >-
  Flutter: Conhecendo e testando o Framework da Google para criar apps nativos
  para Android/iOS
date: 2018-03-07 10:04:31
header-img: /img/bannerflutter.jpg
tags:
- Flutter
- Google
- Multi-plataforma
- Introdução
---

> O desenvolvimento de aplicativos cada vez mais precisa ser escalável e dinamico, ou seja, não podemos perder muito tempo para lançar um aplicativo e muito menos atualizações de novos recursos e bugs, pois com a alta concorrencia de apps nas stores fica cada vez mais difícil fidelizar o smart publico.

Antes de entrar no detalhe técnico é importante entender que existem três tipos de desenvolvimento de apps que podem ser seguido por um desenvolvedor sendo: 

- **Desenvolvimento nativo**
Neste cenário é utilizado o SDK e IDE do sistema operacional a ser implementado como Android Studio utilizando linguagem de programação Java ou Kotlin para apps Android, e XCode utilizando linguagem de programação ObjectC ou Swift para apps iOS.
- **Desenvolvimento híbrido**
Neste cenário é utilizado uma plataforma, como Apache Cordova, onde o aplicativo a ser desenvolvido roda em um browser incorporado no aplicativo sem barra de navegação e com icones de inicialização, com isso seu aplicativo pode ser acesso a recursos nativos, por meio de plugins, e pode ser implementado em HTML, CSS e Javascript. Sendo assim o mesmo aplicativo pode ser compilado para Android, iOS e outros S.O que a plataforma base de suporte. Veja [post relacionado aqui](/2018/02/20/Hibrido-vs-Nativo-Uma-visao-do-time-do-Ionic/).
- **Desenvolvimento nativo multiplataforma**
Este é o ponto que vamos trabalhar neste post, pois neste cenário é utilizado um framework que da suporte em utilizar o mesmo código fonte para compilar o aplicativo final para Android e iOS, como React Native e Flutter. Sendo assim você implementa na linguagem e estrutura do framework e o mesmo é compilado nativamente o aplicativo final.

## Flutter

[Flutter](https://flutter.io) é um framework criado pelo time da Google para criar aplicativo móvel de alto desempenho e alta fidelidade para iOS e Android, a partir de uma única basecode.

![Flutter](/img/flutter1.jpg)

A linguagem de programação utilizada para implementar os Apps em Flutter é a [Dart](https://www.dartlang.org/) uma linguagem simples de utilizar e bem familiar para quem já conhece Java ou JavaScript. Se você tiver conhecimento com linguagem orientada a objetos certamente você irá pegar bem rápido o esquema do Dart, mas mesmo que não tenha conseguirá criar apps rapidamente com Flutter. 

Um princípio na interação dos elementos de interface com lógicas de negócio são os componentes stateless e stateful, e se você tiver conhecimento sobre o framework ReactJS vejá uma familiaridade no controle de estado das variáveis, neste ponto o Flutter se inspirou no ReactJS. Se você já implementou algo em React Native sugiro você ler [esse post](https://flutter.io/flutter-for-react-native/) onde é comparado a familiaridade entre os dois frameworks.

Flutter também dispoe do famoso `hot reload` que facilita o desenvolvimento do app atualizando em seu device ao salvar seu código e atualizando apenas o elemento que foi alterado, isso ajuda muito para corrigir bugs em fluxos complexos.

Quando começo a trabalhar com um framework a primeira coisa que me preocupo é com a UI, se é flexível e customizavel, pois em casos de limitação isso deixa o trabalho mais "chato". Neste ponto o Flutter se destacou com relação a outros frameworks que usei, pois seus SDK da uma abertura muito alta para customizar as interfaces. Se você tiver conhecimento em CSS surigo ler [este post](https://flutter.io/web-analogs/) sobre uma analogia com CSS.

Podemos dizer que Flutter inclui uma estrutura moderna para controlar o estado dos elementos, um mecanismo de renderização 2D, widgets prontos e ferramentas de desenvolvimento produtivas. 

### Widget

No Flutter tudo é Widget que representa a base da UI. Diferente de outras estrutras que separam seus elementos em View, Controller e Model, o Flutter possui um modelo unificado de objeto conhecido como widget. O Widget pode ser definido como elementos estruturais (como botões ou menu), elementos de estilo (como fonte e cores), 
elemento de layout (como padding e margin) e assim por diante. Os widgets são contruidos em hiearquia, pois cada widget herda as propriedades de seu pai, muito semelhante elementos HTML.

Os widgets geralmente são compostos de muitos pequenos widgets de propósito único que se combinam para criar as interfaces. Por exemplo, o `Container`, um widget de uso comum, é composto por vários widgets responsáveis pelo layout, cor, posicionamento e dimensionamento. A hierarquia do widget é ampla para poder combinar o número máximo de widgets.

![Flutter](/img/flutter2.jpg)

É possível controlar o layout de um widget utilizando outros widgets. Por exemplo, para centralizar um widget, você coloca ele em um widget do `Center`. Existem widgets para preenchimento, alinhamento, linha, colunas e grades. Esses widgets de layout não possuem uma representação visual própria. Em vez disso, seu único propósito é controlar algum aspecto do layout de outro widget.

### Camadas

O Flutter é construido sob camadas e o diagrama abaixo mostra essa estrutura de camadas superiores, que são mais utilizadas que a inferiores.

![Flutter](/img/flutter3.jpg)

O modelo de widget facilita muito a customização de novos elementos, tanto em nível supeior quanto nível inferior, pois olhando alguns elementos de nível supeior do Flutter podemos ver que eles são um conjunto de elementos inferiores.

### Interação do usuário

Se um widget precisa atualizar informações ou fatores de sua exibição quando o usuário realiza algum evento podemos enteder que este widget tem um estado. 

Imagina que temos um widget que dentro dele tem um botão e um número exibido em um label, quando o usuário clicar no botão o número deve ser crementado, neste caso o widget tem um estato e iremos atualizar seu valor pelo `setState()`. Quando o valor é alterado de um `state` o widget é reconstruido para atualizar a interface.

Esses widgets que tem `state` são classes do `StatefulWidget` (em vez de `StatelessWidget`).s

![Flutter](/img/flutter4.jpg)

### Instalação

No momento em que este post foi escrito o Flutter estava na versão beta, então acompanhe a página de [instalação oficial](https://flutter.io/setup-windows/) para analisar se algo mudou. Irie descrever como fazer a instalação no Windows, mas para os outros S.O os passos são semelhantes, mudando a forma de criar as variáveis de ambientes.

Precisamos ter instalado na máquina o Git, pois a instalação nada mais é que um clone do repositório oficial do Flutter. Dendo o Git instalado faça o clone em uma pasta onde você pode mandar atualizado e seguro, no meu caso eu deixei em `d:\flutter`.

> Caso você já tenha uma versão instalada do flutter basta executar `flutter upgrade` para atualizar o mesmo para uma nova versão.

```
git clone -b beta https://github.com/flutter/flutter.git
```

Após o clone concluído precisamos adicionar na varivável `PATH` o caminho `bin` do flutter.

![Flutter](/img/flutter5.jpg)

Agora vamos analisar as dependência do flutter para o SDK funcionar corretamente. No terminal digite:

```
flutter doctor
```

Caso você não tenha as dependências instalada na sua máquina algo assim irá ser exibido no terminal:

```
Doctor summary (to see all details, run flutter doctor -v):
[√] Flutter (Channel beta, v0.1.5, on Microsoft Windows [versÃ£o 10.0.15063], locale pt-BR)
[√] Android toolchain - develop for Android devices (Android SDK 25.0.3)
[X] Android Studio (not installed)
[!] VS Code (version 1.16.1)
[√] Connected devices (1 available)
```

Se você estiver no Windows não irá conseguir gerar os aplicativos para iOS, porque essa tarefa deve ser feito em um Mac, mas você pode criar o app e testar no Android e depois apenas executar testes no iOS utilizando um MacInCloud, ou seja, utilizar um Mac em núvem onde você paga por hora em execução de sua máquina.

Analisando o cenário para Android o Flutter depende que você tenha o Android SDK e o Android Studio, pois ele utiliza algumas libs da IDE. Sendo assim baixe o [Android Studio aqui](https://developer.android.com/studio/index.html?hl=pt-br) e siga passo a passo da instalação.

> Após finalizar a instalação e abrir pela primeira vez o Android Studio ele irá solicitar uma pasta onde o Android SDK irá ser instalado, recomendo você escolher uma pasta de fácil acesso pois iremos precisar inforar a mesma como variável de ambiente.

![Flutter](/img/flutter7.jpg)

![Flutter](/img/flutter8.jpg)

![Flutter](/img/flutter9.jpg)

No meu caso eu utilizo o VS Code e o `flutter doctor` detectou o VS Code e analisou que não tem o plugin do Dart no mesmo, para isso instale a extensão do Dart.

![Flutter](/img/flutter6.jpg)

Agora precisamos criar a variável de ambiente `ANDROID_HOME`:

![Flutter](/img/flutter10.jpg)

Outro detalhe é que o Flutter precisa executar seu app em um device ou emulador, eu conselho você rodar direto no device, pois isso vai trazer uma experiência melhor no desenvolvimento.

Após esses passos abra novamete o terminal e execute o `flutter doctor` e você deverá ver um resultado parecido com esse:

```
Doctor summary (to see all details, run flutter doctor -v):
[√] Flutter (Channel beta, v0.1.5, on Microsoft Windows [versÃ£o 10.0.15063], locale pt-BR)
[√] Android toolchain - develop for Android devices (Android SDK 25.0.3)
[√] Android Studio (version 3.0)
[√] VS Code (version 1.21.0)
[√] Connected devices (1 available)

• No issues found!
```

### Estrutura de um projeto Flutter

Vamos fazer uma anatomia de um projeto Flutter e entender sua estrutura, tenha em mente que os arquivos gerados podem ser alterados cada vez que o build é executado, mas neste momento queremos anaisar, de forma superficial, o arquivos gerados. Sendo assim digite o comando para criar um novo app.

```
flutter create apptask
```

Você pode acompanhar no terminal alguns arquivos sendo criado e por final o flutter executando: `Running "flutter packages get" in apptask...` isso é necessário para que os pacotes, que podemos chamar de recursos, sejam baixados e disponibilizados no app.

Olhado para a raiz da pasta gerada vamos ver o primeiro arquivo importante do Flutter, o `pubspec.yaml`, neste arquivo iremos colocar as dependências do nosso projeto e ao executaroms `flutter packages get` o flutter irá analisar os pacotes que estão informados no arquivo e que não foi baixado e assim executar o download (instalação).

