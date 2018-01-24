---
title: Amostragem e quantização de imagens
date: 2016-09-25 20:27:42
header-img: /img/amostragem_quantizacao.png
tags:
- processamento digital de imagem
- fundamentos da imagem digital
- amostragem
- quantização
---

No post sobre [sensores e aquisição de imagens](/2016/09/25/Sensores-e-aquisicao-de-imagens/) foi apresentando várias formas de aquisição de imagens, porem o objetivo em todos os casos foi o mesmo: gerar imagens digitais a partir de dados captados por sensores. A saída da maioria dos sensores consiste em uma forma de onda de tensão contínua cuja amplitude e comportamento no espaço estão relacionados ao fenômeno físico que está sendo captado pelos sensores. Para criar uma imagem digital precisamos converter os dados contínuos para o formato digital. Isso envolve dois processos: amostragem e quantização.

Na figura abaixo (a) mostra uma imagem contínua *f* que queremos converter em formato digital, para isso temos que fazer a amostragem da função tanto nas coordenadas *x* e *y* quanto na amplitude. A digitalização dos valores de coordenadas é chamado de amostragem. A digitalização dos valores de amplitude é chamado de quantização.

![](/img/produzindo_imagem.png)

Na figura (b) é apresentado um gráfico que representa os valores de amplitude da imagem contínua ao longo do segmento de reta *AB* da figura (a). Para realizar a amostragem foi colhido amostras igualmente espaçadas ao longo da linha *AB*, exibido na figura (c). A posição de cada amostra no espaço é indicada por uma pequena marca vertical na parte inferior da figura. O conjunto dessas localizações discretas nos dá a função de amostragem, porem os valores das amostras ainda cobrem uma faixa contínua de valores de intensidade.

Para formar uma função digital é necessário converter também os valores de intensidade (quantizados). O lado direito da figura (c) mostra uma escala de intensidade divido em oito intervalos discretos, variando do preto ao branco. As marcas verticais indicam o valor específico atribuído a cada um dos oitos níveis de intensidade. Os níveis de intensidade contínuos são quantizados atribuindo um dos oito valores para cada amostra. Essa atribuição é feita dependendo da proximidade vertical de uma amostra a uma marca indicadora. As amostras digitais resultantes da amostragem e da quantização pode ser vista na figura (d).

Este processo é feito linha por linha começando na parte superior resultando em uma imagem digital bidimensional.

Está implícito na figura a cima que, além do número discreto de níveis utilizados, a precisão atingida na quantização depende muito do conteúdo de ruído do sinal da amostragem.

Quando sensores por varredura de linha são utilizados para a aquisição da imagem, o número de sensores da linha define as limitações da amostragem na direção da imagem.

Quando uma matriz de sensores é utilizada para a aquisição de imagem, os limites da amostragem em ambas direções são determinados pelos sensores.

A figura abaixo (a) mostra uma imagem contínua projetada sobre o plano de uma matriz de sensores. Na figura (b) mostra a imagem após a amostragem e a quantização.

![](/img/imagemcontinuamatriz.png)


***

## Referências

Gonzalez, Rafael C. Processamento digital de imagens. 3. ed. São Paulo: Pearson Prentice Hall, 2010.
