---
title: Tópico 2 - Amostragem
---

# Tópico 2 - Amostragem

---

## Resumo Conceitual

A amostragem é o processo fundamental de transição entre o mundo físico analógico e o ambiente de processamento digital. Fisicamente, consiste em observar e registrar o valor instantâneo de um sinal contínuo em intervalos regulares de tempo. 

Enquanto um sinal contínuo $x(t)$ possui infinitos valores e flui ininterruptamente, sistemas digitais possuem memória e capacidade de processamento finitas. Ao extrairmos "fotografias" periódicas deste sinal, criamos uma sequência discreta de números que o computador consegue armazenar e manipular. 

O grande desafio da engenharia na etapa de amostragem é garantir que a taxa de captura de dados seja suficientemente rápida para não perder as variações fundamentais do fenômeno físico. Se o intervalo entre as observações for demasiado longo, variações rápidas do sinal original passarão despercebidas, resultando em uma sequência digital distorcida que mascara a verdadeira natureza da grandeza medida.

---

## Formulação Matemática

Considere um sinal contínuo genérico $x(t)$. Ao realizarmos a amostragem deste sinal em intervalos de tempo regulares, definimos um período de amostragem $T_s$ constante.

Os instantes exatos de aquisição dos dados formam um vetor de tempo discreto:
$$t_n = nT_s$$

Ao avaliarmos o sinal contínuo nestes instantes específicos, obtemos a representação da sequência discreta:
$$x[n] = x(nT_s)$$

A velocidade de captura, denominada **frequência de amostragem ($f_s$)**, define a quantidade de amostras recolhidas por segundo e é inversamente proporcional ao período de amostragem:
$$f_s = \frac{1}{T_s}$$

Onde:
* $f_s$ é medido em hertz ($\text{Hz}$) ou amostras por segundo.
* $T_s$ é medido em segundos ($\text{s}$).
* $n \in \mathbb{Z}$ representa o índice inteiro e adimensional da amostra na sequência.

---

## Investigação e Simulação Computacional

Para ilustrar de forma prática o impacto da frequência de amostragem sobre a integridade da informação digitalizada, simulamos a captura de um sinal contínuo senoidal correspondente à rede elétrica de distribuição ($60\,\text{Hz}$), cuja equação de modelo temporal é:

$$x(t) = 179{,}6 \cos(120\pi t)$$

O experimento computacional abaixo submete este sinal a três taxas de amostragem distintas para comparar visualmente e numericamente o processo de discretização.

:::: {include} ./simulacao/simulacao_amostragem.ipynb
::::

### Discussão dos Resultados

A simulação ilustra a relação crítica entre a frequência do fenômeno físico ($f_0 = 60\,\text{Hz}$) e a taxa de captura do sistema digital ($f_s$), permitindo as seguintes observações:

1. **Cenário 1 ($f_s = 480\,\text{Hz}$):** O período de amostragem de $2{,}08\,\text{ms}$ garante a extração de 8 amostras completas por cada ciclo da onda de tensão. Como a frequência de amostragem supera largamente o dobro da frequência do sinal, a representação digital preserva integralmente as características de variação temporal da rede.
2. **Cenário 2 ($f_s = 120\,\text{Hz}$):** Este cenário representa o limite teórico absoluto de extração de informação. Com uma taxa de amostragem exatamente igual ao dobro da frequência do sinal fundamental (2 amostras por ciclo), o sistema extrai dados de $8{,}33$ em $8{,}33\,\text{ms}$. Embora os picos máximo e mínimo sejam marcados, qualquer defasagem na captura comprometeria severamente a reconstrução da amplitude da onda original.
3. **Cenário 3 ($f_s = 70\,\text{Hz}$):** Ocorre a degradação estrutural do sinal digitalizado. Sendo $70\,\text{Hz}$ um valor inferior ao exigido para uma representação fiel, o sistema de aquisição (capturando a cada $14{,}29\,\text{ms}$) falha em acompanhar as oscilações da tensão. O resultado, com apenas $1{,}17$ amostras por ciclo, é o fenômeno conhecido como *aliasing*. A distribuição dos pontos discretos cria a falsa ilusão visual de que o sinal oscila de forma muito mais lenta do que os $60\,\text{Hz}$ reais, inviabilizando qualquer processamento fidedigno posterior.