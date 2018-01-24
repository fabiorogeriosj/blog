---
title: Trabalho aplicação de Chroma Subsampling
date: 2016-10-26 10:12:01
header-img: /img/flores_campo.jpg
tags:
- processamento digital de imagem
- Chroma Subsampling
- trabalho mestrado
---

> Trabalho prático com aplicação de Chroma Subsampling para disciplina de processamento digital de imagens
> **Aluno: Fábio Rogério da Silva José | Professor: Franklin César Flores**


![](/img/resultado_trab2.png)

Este processo consiste em melhorar os componentes de cores do que luminância, pois a visão humana tem uma capacidade menor para diferenciar cores do que luminosidades. Este esquema de codificação faz com que imagens e vídeos tenham um tamanho menor em bits.

Para implementação em Python foi utilizado a lib `numpy` e `OpenCV`:

``` python
import numpy as np
import cv2
```

Primeiro criamos duas funções uma para fazer a quantização e outra para alterar as cores:

``` python
def quantiz(arr, n):
    nmin = np.min(arr)
    nmax = np.max(arr)
    b = np.float32(arr)/(nmax-nmin)
    c = np.round(b*(n-1))
    d = np.round(c/(n-1)*(nmax-nmin))
    d = d + nmin
    return d

def alteraCor(arr,n):
    b = quantiz(arr, n)
    b = np.uint8(b)
    return b
```

Assim carregamos uma imagem e separamos o H, S e V. Em seguida alteramos a cor da banda H e S:

``` python
img = cv2.imread('flores_campo.jpg')
cv2.imshow("in", img)
hsv_img = cv2.cvtColor(img, cv2.COLOR_BGR2HSV)
h, s, v = cv2.split(hsv_img)
n = 18
hNovo = alteraCor(h, n)
sNovo = alteraCor(s, n)
```

Para concluir aplicamos o novo esquema de cor:

``` python
imgFinal = np.dstack((np.dstack((hNovo, sNovo)), v))

out = cv2.cvtColor(imgFinal, cv2.COLOR_HSV2BGR)

cv2.imshow("out", out)
k = cv2.waitKey(0)
if k == 27:
    cv2.destroyAllWindows()
```
