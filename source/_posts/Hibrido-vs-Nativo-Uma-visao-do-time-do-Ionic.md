---
title: 'Híbrido vs Nativo: Uma visão do time do Ionic'
date: 2018-02-20 10:37:14
header-img: /img/bannerhybridapp.jpg
tags:
- Ionic
- Nativo
- Mobile
- Estudo de caso
---

Em um documento liberado recente pelo time do ionic.io destaca a diferença entre os aplicativos híbridos e nativos. Achei o artigo interessante e vou compartilhar com vocês os pontos mais interessantes desse documento.

> A demanda por experiências móveis está crescendo 5 vezes mais rápido do que as equipes de TI internas podem oferecer. **[Gartner](https://www.gartner.com/newsroom/id/3076817)**

No início a melhor, e única, forma de ter uma performance alta com aplicativos era o desenvolvimento nativo. A um bom tempo atraz, ainda quando o Android 2.2 estava no mercado, tentei utilizar tecnologia híbrida como [Titanium SDK](https://www.appcelerator.com/mobile-app-development-products/) e não obtive sucesso, pois era muito lento a utilização, já no iOS não era tão lento porem era perceptível a diferença.

Graças ao tempo e aos poderes dos devices cada vez maior, hoje podemos dizer que os apps híbridos são tão performático quanto aos nativos, porém não são iguais, sempre o nativo terá uma performance melhor, mas as vezes imperceptível.

## A escolha para ir para o híbrido

> O [Forrester](https://go.forrester.com/) estima que uma abordagem híbrida irá salvar uma organização entre 75-80% em relação ao nativo.

Alguns pontos que mais fazem uma organização escolher implementar seu app com tecnologia híbrida, segundo documento levantado pelo Ionic.io, são:

- **Velocidade**: Criar apps utilizando o mesmo código fonte para várias plataformas pode ser 2-3 vezes mais rápido.
- **Eficiência**: Estima que uma abordagem híbrida irá salvar uma organização entre 75-80% em relação ao nativo.
- **Várias plataformas**: Apps híbridos podem rodar em qualquer lugar que roda aplicações web como no desktop, mobile como um app nativo ou PWA.

Mas um ponto que precisa ser analisado, e sempre falo isso, é que cada projeto app é único e nem sempre o híbrido é a melhor escolha, mesmo colocando em pauta a velocidade de desenvolvimento, eficiência e várias plataformas. Desde 2007 venho criando aplicativos e até hoje alguns apps precisam ser nativos por N motivos que o próprio artigo do Ionic apontou como vantagem em ser Nativo. Mas 80% dos apps que criei até o momento desse artigo foram híbridos.

## O que é App Híbrido?

Aplicativos híbridos são semelhantes a aplicativos nativos. Eles são baixados via store nas suas respectivas plataformas, como Apple Store e Play Store. Os recursos nativos de software e hardware, como câmera, gps, etc, também são acessíveis pelos apps híbridos.

A diferença básica entre apps híbridos e nativos é que aplicativos híbridos são criados utilizando tecnologias abertas da web como: JavaScript, HTML e CSS. Estes aplicativos são executados em um browser no modo full-screen, este browser é chamado de `webview` e é invisível para o usuário.

## Comparando Hibrido vs Nativo

Como mencionado anteriormente é importante levar em consideração o projeto, a equipe e a organização que pretende escolher híbrido ou nativo. Neste ponto o time da Ionic apresentou os benefícios de cada opção. Outro ponto de análise também é que se você tiver muito conhecimento em tecnologia web (HTML/CSS/JavaScript) certamente poderá ter um melhor aproveito do híbrido.

![Hibrido](/img/hibridonativo1.jpg)

### O mesmo código rodando em qualquer lugar

> O Swift da Apple está perdendo desenvolvedores para os frameworks multi-plataformas. **[InfoWorld](https://www.infoworld.com/article/3231664/application-development/apples-swift-is-losing-developers-to-multiplatform-frameworks.html)**

Este ponto é o mais brilhante de se usar frameworks como Ionic, PhoneGap e componentes de interface como MobileUI. Pois você pode ter o mesmo app rodando em Android, iOS e Windows Phone tendo que manter apenas um código.

### Use o talento que você já tem

> De acordo com o Stack Overflow Survery, apenas 6,5% de todos os desenvolvedores que usam a plataforma citaram o Swift e Objective-C como uma linguagem conhecida. Mas por outro 72.6% conhecem sobre desenvolvimento web e JavaScript aparece como a linguagem de programação mais conhecida e popular. **[Ionic Blog](https://blog.ionicframework.com/in-house-teams-talent-rising-to-the-challenge/)**

É comprovado que a comunidade de desenvolvedores web é 30x maior comparado aos desenvolvedores de aplicativos mobile nativos. Então uma boa vantagem do Híbrido é que você pode usar todo conhecimento em web para criar seus apps.

Isso com certeza facilita tanto para capacitação e recrutamento de talentos para trabalhar nos projetos híbridos.

### A melhor experiência do usuário entre plataformas

> Quando estávamos trabalhando nativamente, nossos resultados de satisfação de usuários eram como 3 estrelas em Android e 2,5 no iOS. Agora estamos em 4,5 estrelas no iOS e perto de 4,5 estrelas no Android após mudarmos para app híbrido. **[Brian Aguilar, MarketWatch](https://ionicframework.com/case-studies/MarketWatch.pdf)**

Umas das principais vantagens que este tópico informa é que com CSS é muito mais fácil e versátil criar interfaces mais bonitas e com boa experiência, mas isso também pode ser um problema. Caso não tenha conhecimento em CSS mais aprofundado em performance você pode criar telas bonitas mas lentas e isso significa que terá um UX ruim. A vantagem de utilizar um framework como Ionic pode ajudar nesse ponto, pois seus componentes são pensados para ter uma boa experiência como um todo.

Também existe componentes que podem ser utilizado com Ionic ou apenas com Apache Cordova como [MobileUI](https://mobileui.github.io/).

### Criando para o futuro

As organizações de desenvolvimento são encarregadas de construir aplicações para o futuro. Futuros apps serão executados em uma grande diversidade de plataformas voltadas para a web: wearables, dispositivos IoT, comunicações como GPS em carros, rastreamento de ativos, sistemas ou dispositivos médicos portáteis, etc.

Isso pode ser uma boa vantagem para quem irá precisar implementar os [PWA's](https://blog.ionicframework.com/what-is-a-progressive-web-app/).

Alguns pontos devem ser destacado com relação ao híbrido como:
- O uso da webview pode ter um grau de sobrecarga em comparação com o nativo.
- Os aplicativos híbridos podem acessar quase todas as características nativas de um dispositivo, como a câmera ou o giroscópio, usando plugins nativos.
- Escolher uma abordagem de plataforma híbrida significa que você está colocando confiança no framework para manter as últimas e melhores características nativas e os padrões de design de cada plataforma móvel.

![Hibrido](/img/hibridonativo2.jpg)

Como afirmado anteriormente, o nativo ainda é a abordagem preferida para muitos desenvolvedores móveis. E há algumas boas razões para isso. Enquanto parte disso se baseia no legado de ter poucas alternativas viáveis, o nativo ainda tem suas vantagens.

### Performance

O código nativo ainda é mais rápido que o Javascript e o HTML. Isso importa quando os desenvolvedores estão procurando construir aplicativos com alta performance de processamento e gráfico, como jogos e outras aplicações de animação intensiva. Navegadores móveis estão chegando mais perto de reduzir a lacuna para esses tipos de aplicações usando a especificação WebGL; no entanto, nativo ainda tem a vantagem.

### Bibliotecas mais ricas no nativo

O uso de SDK nativo permite que o desenvolvedor acesse os recursos mais recentes especificamente projetados para essas plataformas, sem a complexidade de lidar com plugins nativos. Isso é fundamental quando você precisa fornecer uma experiência rica para o usuário como reconhecimento facial para iOS ou Touch ID para Android.

### Não há dependências de terceiros

Ao construir exclusivamente com um conjunto de ferramentas nativo, os desenvolvedores não estão vinculados a terceiros para acompanhar o suporte e não existe uma grande dependência de comunidades de código aberto como Cordova para acompanhar os recursos mais recentes.

Veja o material criado pelo time do Ionic.io [aqui](https://cdn2.hubspot.net/hubfs/3776657/Ionic_HybridNative_eBook_Jan_2018_v8.pdf).