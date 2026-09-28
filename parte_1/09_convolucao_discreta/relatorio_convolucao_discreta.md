---
title: Tópico 8 - Convolução Discreta
---

# Tópico 8 - Convolução Discreta

---

## Resumo Conceitual

A convolução discreta é a operação matemática central no estudo de Sistemas Lineares e Invariantes no Tempo (LTI). Ela estabelece que a resposta de um sistema a qualquer sinal de entrada arbitrário pode ser inteiramente determinada se conhecermos a sua resposta ao impulso unitário ($h[n]$). 

Fisicamente, a convolução representa a superposição de múltiplas respostas ao impulso, cada uma escalonada pela amplitude da amostra de entrada correspondente e atrasada no tempo.

---

## Formulação Matemática

Para um sinal de entrada $x[n]$ e uma resposta ao impulso $h[n]$, o sinal de saída $y[n]$ é dado pelo somatório de convolução:
$$y[n] = x[n] * h[n] = \sum_{k=-\infty}^{+\infty} x[k] h[n-k]$$

Na prática computacional com sinais de duração finita, a convolução obedece a propriedades dimensionais e energéticas estritas:
1.  **Comprimento do Sinal Resultante:** Se $x[n]$ possui $L$ amostras e $h[n]$ possui $M$ amostras, o sinal resultante $y[n]$ terá obrigatoriamente um comprimento de $L + M - 1$ amostras.
2.  **Conservação de Área:** A soma de todas as amplitudes do sinal de saída é igual ao produto das somas das amplitudes dos sinais originais:
$$\sum y[n] = \left( \sum x[n] \right) \times \left( \sum h[n] \right)$$

---

## Investigação e Simulação Computacional

Para validar a teoria, simulamos a convolução entre um pulso transitório de 5 amostras ($L=5$) e a resposta de um sistema modelado por um decaimento exponencial de 15 amostras ($M=15$, com fator $a=0{,}75$).

:::: {include} ./simulacao/simulacao_convolucao.ipynb
::::

### Discussão dos Resultados

A análise gráfica e os resultados numéricos confirmam rigorosamente os teoremas da convolução:

1.  **Análise Dimensional:** O sinal de entrada $x[n]$ possui 5 amostras e a resposta ao impulso $h[n]$ possui 15 amostras. O vetor de saída gerado pelo algoritmo computacional apresentou exatamente $19$ amostras, o que valida a relação $L + M - 1 = 5 + 15 - 1 = 19$.
2.  **Comportamento Dinâmico:** O gráfico de $y[n]$ ilustra o efeito de "espalhamento" e acúmulo de energia característico de sistemas LTI com memória. O pico máximo da saída ocorre atrasado em relação ao pico da entrada, refletindo o tempo necessário para o sistema acumular a energia do pulso antes de iniciar o seu decaimento natural.
3.  **Conservação Numérica:** A validação algébrica comprovou que a soma das amplitudes de $x[n]$ ($10{,}50$) multiplicada pela soma das amplitudes de $h[n]$ ($3{,}95$) resulta em $41{,}44$, valor que corresponde com exatidão à soma medida diretamente nas amostras de $y[n]$.