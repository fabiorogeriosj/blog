---
title: Trabalho Método Otsu
date: 2016-11-01 10:40:25
header-img: /img/soldefundo.jpg
tags:
- processamento digital de imagem
- Método Otsu
- trabalho mestrado
---

> Trabalho prático com Método Otsu para disciplina de processamento digital de imagens
> **Aluno: Fábio Rogério da Silva José | Professor: Franklin César Flores**


``` python
import numpy as np
import cv2

def aplicaThreshold(t, image):
    LUT = np.arange(256)
    LUT = np.ones(256)*255
    LUT[0:t]=0
    LUT = np.uint8(LUT)
    out = np.zeros(image.shape, dtype=np.uint8)
    out = LUT[image]
    return out

def otsu(i):
    hist, bins = np.histogram(i, 256, [0,256])
    cdf = hist.cumsum()
    cdf_n = cdf * hist.max() / cdf.max()
    q = cdf_n.cumsum()

    bins = np.arange(256)
    fn_min = np.inf
    threshold = -1
    for i in xrange(1,256):
        p1, p2 = np.hsplit(cdf_n, [i])
        q1, q2 = q[i], q[255]-q[i]
        b1,b2 = np.hsplit(bins, [i])
        m1, m2 = np.sum(p1*b1)/q1, np.sum(p2*b2)/q2
        v1,v2 = np.sum(((b1-m1)**2)*p1)/q1,np.sum(((b2-m2)**2)*p2)-q2

        fn = v1*q1 + v2*q2
        if fn < fn_min:
            fn_min=fn
            threshold=i

    return threshold


def correcao(i, p):
    LUT = np.arange(256)
    LUT = np.power(LUT,p)
    amax = LUT.max()
    LUT = 255*LUT/amax

    LUT = np.uint8(LUT)
    out = np.zeros(i.shape, dtype=np.uint8)
    out = LUT[i]
    return out

def merge(c, i):
    t = np.clip(np.floor(i - 254), 0,255)
    t = np.clip(np.floor(t-1),0,255)
    LUT = c * t
    LUT = np.uint8(LUT)
    return LUT

image = cv2.imread('soldefundo.png', 0)
thresholdCalculado = otsu(image)
imgCalculada = aplicaThreshold(thresholdCalculado, image)

imageCont = correcao(image, 0.4)
arrumado = merge(imageCont, imgCalculada)

cv2.imshow('original', image)
cv2.imshow('otsu', imgCalculada)
cv2.imshow('gamma', imageCont)
cv2.imshow('escura', arrumado)

k = cv2.waitKey(0)
if k == 27:
    cv2.destroyAllWindows()

```
