---
title: Trabalho de interpolação entre duas imagens
date: 2016-09-26 08:34:55
header-img: /img/pixelrelevo.jpg
tags:
- processamento digital de imagem
- fundamentos da imagem digital
- interpolação
- trabalho mestrado
---

> Trabalho prático com interpolação de imagens para disciplina de processamento digital de imagens
> **Aluno: Fábio Rogério da Silva José | Professor: Franklin César Flores**

A interpolação de imagens é considerada uma ferramenta básica para utilizada para realizar ampliação, redução, rotação e correções geométricas em imagens digitais. Este assunto está ligado a amostragem e quantização como e pode ser lido com mais detalhes [neste post](/2016/09/25/Amostragem-e-quantizacao-de-imagens/).

A interpolação é o processo que utiliza dados conhecidos para estimar valores em pontos desconhecidos. Por exemplo vamos supor que temos uma imagem de 10 x 10 pixels e queremos aumentar para 20 x 20 pixels, sendo assim precisamos preencher os novos pixels com algum valor. Para realizar esta tarefa podemos utilizar o método de *interpolação por vizinhos mais próximo* que atribui a cada nova posição a intensidade de seu vizinho mais próximo na imagem original. Este método é simples e as vezes, dependendo do resultado esperado, não é o melhor a ser aplicado, pois ele tem a tendência de produzir artefatos indesejáveis na imagem.

## Implementação

O trabalho consiste em utilizar a técnica de interpolação de imagens para fazer uma transição entre duas imagens, resultando nesta saída:

![](/img/trabalhoamostragem.gif)

Para implementar os códigos dos trabalhos irei utilizar a linguagem de programação [Python](https://www.python.org/), uma linguagem que permite trabalhar rapidamente e integrar os sistemas de forma mais eficaz.

Utilizei três módulos para este trabalho sendo:
- **numpy**, responsável por fazer as iterações e fornecer funções performáticas para trabalhar com array.
- **cv2**, conhecida como OpenCV, é um dos módulos mais utilizado para trabalhar com tratamento de imagens.
- **time**, este módulo fornece várias funções relacionadas ao tempo, neste contexto utilizei apenas para que a atualização de tela entre uma amostragem e outra sofresse um *sleep*.

``` python
import numpy as np
import cv2
import time
```

Primeiro criei uma função para dispensar um numero *n* de pixel tanto no eixo *x* quanto no eixo *y*, esta função por sua vez utiliza uma funcionalidade do Python para tratar *array*, onde é dispensado a quantidade recebida em parâmetro nos eixos.

Minha função recebe um *array* e um inteiro que é interpretado como a quantidade de vizinhos a ser adicionado, seu retorno é o *array* modificado:

``` python
def remover_vizinhos(a,n):
    return a[::n,::n]
```
Vamos entender melhor como funciona essa manipulação de *array* do Python.

Por exemplo, dado uma matriz de 5x5 de números inteiros reais, temos esta saída:

```
[[ 0  1  2  3  4]
 [ 5  6  7  8  9]
 [10 11 12 13 14]
 [15 16 17 18 19]
 [20 21 22 23 24]]
 ```

 Se queremos eliminar os dois vizinhos próximos tanto da linha quanto da coluna podemos fazer assim:

``` python
matriz[::2,::2]
```

Nossa saída será:

```
[[ 0  2  4]
 [10 12 14]
 [20 22 24]]
```

Continuando o trabalho criei outra função responsável por adicionar vizinhos tanto no eixo *x* quanto no eixo *y*, para fazer com que este processo fique mais performático, utilizei a função de iteração do módulo `numpy`:

``` python
def adicionar_vizinhos(a,n):
    return np.repeat((np.repeat(a,n,axis=0)),n,axis=1)
```

Vamos entender melhor a função `repeat()` do `numpy`. Por exemplo, dado uma matriz de 5x5 de números inteiros reais, temos esta saída:

```
[[ 0  1  2  3  4]
 [ 5  6  7  8  9]
 [10 11 12 13 14]
 [15 16 17 18 19]
 [20 21 22 23 24]]
 ```

 A função `repeat` recebe três parâmetros sendo: o *array* que será iterado, o número de repetições para o elemento e o eixo que será percorrido (opcional).

 O exemplo a baixo percorre duas vezes adicionando um vizinho no eixo *x*:

 ``` python
 np.repeat(matriz, 2, axis=1)
 ```

 Nossa saída será:

 ```
 [[ 0,  0,  1,  1,  2,  2,  3,  3,  4,  4],
  [ 5,  5,  6,  6,  7,  7,  8,  8,  9,  9],
  [10, 10, 11, 11, 12, 12, 13, 13, 14, 14],
  [15, 15, 16, 16, 17, 17, 18, 18, 19, 19],
  [20, 20, 21, 21, 22, 22, 23, 23, 24, 24]]
 ```

Para adicionar no eixo *y* basta alterar o `axis=0`.

E por último crio outra função responsável por dar uma pausa entre uma amostragem a outra, pois se eu apenas rodar minhas funções não vou conseguir visualizar o efeito esperado. A função `pause` apenas aguarda 100 milissegundos  para continuar a execução da aplicação.

``` python
def pause():
    time.sleep(0.1)
```

Com as funções prontas eu posso tanto adicionar vizinhos quanto remover, porem seguindo esta ordem eu não terei o efeito esperado, pois se eu remover vizinhos minha imagem ficará menor, e se eu adicionar ela ira aumentar.

Sendo assim o algoritmo aplicado será remover *n* vizinhos e em seguida adicionar a mesma quantidade em um laço de repetição.

Utilizando o módulo *OpenCV* primeiro exibo a imagem `dog2.jpg` em escala de cinza e aguardo o usuário pressionar alguma tecla.

``` python
img = cv2.imread('dog2.jpg',0)
cv2.imshow('image',img)
cv2.waitKey(0)
```

Agora inicializo duas variáveis onde `n` será o número de vizinhos que vamos remover e adicionar, e `maxn` é o número máximo de vizinhos.

``` python
n = 1;
maxn = 30;
```

Na primeira imagem faço um laço de repetição enquanto o numero de vizinhos é menor que o máximo, sendo assim exibo a imagem aplicando nossa função primeiro de remover vizinhos e com o seu retorno aplico a função de adicionar vizinhos. Para cada iteração do laço aumento a quantidade de vizinhos que vamos aplicar nas funções e chamo a função `pause()` para aguardar 100 milissegundos .

> Observação: Quando utilizamos o `time.sleep()` junto com atualização de tela, o Python não exibe a imagem enquanto o tempo não finaliza, para resolver esta barreira de forma fácil apenas adicionei o `cv2.waitKey(5)` onde será adicionado um delay de 5 milissegundos  anulando o bloqueio do `sleep`.

``` python
while n < maxn:
    cv2.imshow('image', adicionar_vizinhos(remover_vizinhos(img,n), n))
    cv2.waitKey(5)
    n = n + 1
    pause()
```

Agora vou trocar de imagem atualizando a variável `img` para a nova imagem e atribuindo a quantidade de vizinhos com o máximo, ou seja, nossa segunda imagem já vai começar removendo e adicionado o número máximo de vizinhos.

``` python
img = cv2.imread('dog3.jpg',0)
n = maxn;
```

Para esta outra imagem o processo é ao contrario, ou seja, vamos diminuindo o número de vizinhos que vamos passar para nossas funções fazendo com que a cada iteração do laço a imagem fique mais nítida.

``` python
while n > 0:
    cv2.imshow('image', adicionar_vizinhos(remover_vizinhos(img,n), n))
    cv2.waitKey(5)
    n = n - 1
    pause()
```

Para concluir exibimos a imagem sem remover/adicionar nenhum vizinho e aguardamos o usuário pressionar alguma tecla para finalizar o programa.

``` python
cv2.imshow('image',img)
cv2.waitKey(0)
```

O trabalho completo pode ser [testado aqui](https://github.com/fabiorogeriosj/pdi-uem/tree/master/trabalho1).
