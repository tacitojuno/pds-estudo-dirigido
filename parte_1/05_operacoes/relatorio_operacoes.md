---
title: Tópico 5 - Operações com Sinais
---

# Tópico 5 - Operações com Sinais

---

## Resumo Conceitual

O processamento digital de sinais baseia-se na capacidade de manipular matematicamente sequências discretas. Essas manipulações podem ocorrer no domínio da variável independente (o índice de tempo $n$) ou no domínio da variável dependente (a amplitude do sinal).

Compreender o efeito dessas transformações é fundamental, pois elas formam a base matemática para a construção de sistemas mais complexos, como filtros digitais, onde versões atrasadas e escalonadas do sinal de entrada são combinadas para produzir uma saída filtrada.

---

## Formulação Matemática

Considerando uma sequência discreta genérica $x[n]$, as operações fundamentais investigadas são:

### 1. Operações no Tempo (Variável Independente)
As modificações no eixo temporal afetam o momento em que as amostras ocorrem, sem alterar os seus valores de energia ou amplitude.

*   **Atraso (Deslocamento para a Direita):** Representado por $x[n - n_0]$, com $n_0 > 0$. A amostra que originalmente ocorria em $n=0$ passa a ocorrer no instante $n = n_0$. Corresponde a atrasar a propagação da informação.
*   **Avanço (Deslocamento para a Esquerda):** Representado por $x[n + n_0]$. O sinal ocorre antecipadamente no tempo.
*   **Inversão Temporal (Rebatimento):** Representada por $x[-n]$. Corresponde a um espelhamento do sinal em torno do eixo das ordenadas ($n=0$). Geometricamente, o fim do sinal passa a ser o início e vice-versa.

### 2. Operações na Amplitude (Variável Dependente)
As modificações na amplitude afetam a intensidade do sinal em cada instante $n$, sem alterar a sua distribuição temporal.

*   **Escalonamento de Amplitude:** O sinal é multiplicado por uma constante escalar $A$, resultando em $y[n] = A x[n]$. Se $|A| > 1$, ocorre amplificação; se $0 < |A| < 1$, ocorre atenuação.
*   **Inversão de Amplitude:** Ocorre quando o fator de escalonamento é negativo (tipicamente $A = -1$), resultando em $y[n] = -x[n]$, o que espelha o sinal em torno do eixo das abscissas.

---

## Investigação e Simulação Computacional

Para ilustrar o impacto das operações, definiu-se um sinal base assimétrico $x[n]$ correspondente a um pulso transitório de tensão, com valores não nulos entre as amostras $n=0$ e $n=4$. A assimetria do pulso é essencial para evidenciar graficamente os efeitos de inversão temporal e direção de deslocamento.

:::: {include} ./simulacao/simulacao_operacoes.ipynb
::::

### Discussão dos Resultados

A análise visual do painel de simulação permite confirmar diretamente a teoria matemática:

1.  **Atraso $x[n-2]$:** Todo o sinal foi transladado 2 amostras para a direita. O pico máximo de $4\,\text{V}$, originalmente na amostra $n=1$, encontra-se agora em $n=3$.
2.  **Avanço $x[n+2]$:** O pulso foi empurrado 2 amostras para a esquerda. O evento transitório começou "mais cedo", com o pico ocorrendo no instante passado $n=-1$.
3.  **Inversão Temporal $x[-n]$:** O sinal sofreu um espelhamento simétrico em relação ao eixo $n=0$. A cauda de atenuação, que antes se prolongava para a direita (até $n=4$), prolonga-se agora para a esquerda (até $n=-4$).
4.  **Escalonamento de Amplitude $2x[n]$:** O formato temporal manteve-se inalterado (suporte de $n=0$ a $n=4$), mas todas as tensões foram duplicadas. O pico máximo atingiu $8\,\text{V}$.
5.  **Inversão de Amplitude $-x[n]$:** O pulso foi rebatido para o quadrante negativo da tensão, com o pico máximo invertido para $-4\,\text{V}$, preservando intacta a sequência cronológica dos eventos.