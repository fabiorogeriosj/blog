---
title: Sensores e aquisição de imagens
date: 2016-09-25 18:42:05
header-img: /img/camera_foco.jpg
tags:
- processamento digital de imagem
- fundamentos da imagem digital
- sensores
- aquisição de imagens
---

Como discutido no post sobre [a luz e o espectro eletromagnético](/2016/09/25/A-luz-e-o-espectro-eletromagnetico/), entendemos que a luz é uma onda eletromagnética onde a energia da iluminação é refletida pelos objetos ou transmitida através deles, sendo assim os sensores tem a funcionalidade básica de transformar a energia de iluminação em imagens digitais. A ideia é simples: a energia que entra é transformada em tensão pela combinação da energia elétrica de entrada e do material do sensor, sensível a um tipo específico de energia que está sendo detectado. A forma de onda da tensão de saída é a resposta do(s) sensor(es), e uma quantidade digital é obtida de cada sensor por meio da digitalização de sua resposta.

![](/img/sensoraquisicao.png)

A figura acima (a) mostram os componentes de um único sensor. Talvez o sensor mais conhecido desse tipo seja o fotodiodo, construído com materiais semicondutores cuja forma de onda da tensão de saída é proporcional à intensidade da luz. Por exemplo, um filtro passa-banda para a luz verde, colocado na entrada de um sensor de luz, favorece a luz na banda verde do espectro de cores. Em consequência, a saída do sensor será mais intensa para a luz verde que para outros componentes do espectro visível.

Para gerar uma imagem bidimensional (2-D) utilizando um único sensor, deve haver deslocamentos relativos, tanto na direção x quanto na y entre o sensor e a área de aquisição da imagem.

Uma disposição geométrica utilizada com muito mais frequência que os sensores únicos consiste em um arranjo linear de sensores na forma de uma faixa de sensores, como mostra a acima (b). O arranjo linear dos sensores fornece elementos para a aquisição de imagens em uma direção. Movimentos na direção perpendicular à linha de sensores geram imagens na outra direção.

Analisando a imagem (c) vemos a ilustração de sensores individuais dispostos em forma de uma matriz bidimensional, Esse é o arranjo predominante encontrado nas câmeras digitais.

Um sensor típico para essas câmeras é uma matriz CCD, que pode ser fabricada com uma grande variedade de propriedades sensoras e dispostas em arranjos matriciais de 4.000 × 4.000 elementos ou mais. Os sensores CCD são amplamente utilizados em câmeras digitais e outros instrumentos que utilizam sensores de luz. A resposta de cada sensor é proporcional à integral da energia luminosa projetada sobre a superfície do sensor, uma propriedade que é utilizada em aplicações astronômicas e outras que requerem imagens de baixo nível de ruído. A redução de ruídos é realizada fazendo com que o sensor integre o sinal luminoso de entrada em um intervalo de minutos ou mesmo horas.

A principal forma na qual os sensores matriciais são utilizados é mostrada na figura abaixo:

![](/img/processoaquisicaodigital.png)

Essa figura mostra a energia de uma fonte de iluminação sendo refletida de um elemento de uma cena. A primeira função realizada pelo sistema de aquisição de imagens (c) é coletar a energia de entrada e projetá-la em um plano imagem.

Se a iluminação for luz, a entrada frontal do sistema de aquisição de imagens é uma lente ótica que projeta a cena vista sobre o plano focal da lente (d). O arranjo de sensores, que coincide com o plano focal, produz saídas proporcionais à integral da luz recebida em cada sensor. Circuitos digitais e analógicos realizam uma varredura nessas saídas e as convertem em um sinal analógico, que é então digitalizado por um outro componente do sistema de aquisição de imagens. A saída é uma imagem digital.

### Um modelo simples de formação de imagem

Podemos expressar uma imagem sendo funções bidimensionais na forma *f (x, y)*. O valor ou a extensão de *f* nas coordenadas espaciais *(x, y)* é uma quantidade escalar positiva cujo significado físico é determinado pela origem da imagem. Quando uma imagem é gerada a partir de um processo físico, seus valores de intensidade são proporcionais à energia irradiada por uma fonte real.

A função *f (x, y)* pode ser caracterizada por dois componentes: **(1)** a quantidade de iluminação da fonte que incide na cena que está sendo vista; e **(2)** a quantidade de iluminação refletida pelos objetos na cena. Esses elementos são chamados de componentes de iluminação e refletância e são expressos por *i (x, y)* e *r (x, y)*, respectivamente. As duas funções se combinam como um produto para formar *f (x, y):*

*f (x, y)* =  *i (x, y) r (x, y)*


***

## Referências

Gonzalez, Rafael C. Processamento digital de imagens. 3. ed. São Paulo: Pearson Prentice Hall, 2010.
