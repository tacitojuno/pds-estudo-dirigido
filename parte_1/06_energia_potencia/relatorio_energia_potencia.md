---
title: Tópico 6 - Energia e Potência
---

# Tópico 6 - Energia e Potência

---

## Resumo Conceitual

No estudo de sinais e sistemas, a classificação de um sinal quanto ao seu conteúdo energético é fundamental para compreender o seu comportamento a longo prazo. Essa classificação divide os sinais em duas categorias mutuamente exclusivas: sinais de energia e sinais de potência.

*   **Sinais de Energia:** São sinais de duração finita ou que decaem rapidamente para zero (transitórios). Possuem energia total mensurável e finita, o que implica que, diluída ao longo de um tempo infinito, a sua potência média seja estritamente nula.
*   **Sinais de Potência:** São sinais que perduram indefinidamente no tempo, como ondas periódicas ou ruído contínuo. Como não terminam, a sua energia acumulada tende para o infinito. No entanto, a taxa na qual essa energia é entregue (potência média) permanece constante e finita.

---

## Formulação Matemática

Para um sinal discreto genérico $x[n]$, a **Energia Total** ($E$) é definida pelo somatório do quadrado das amplitudes ao longo de todo o domínio temporal:
$$E = \sum_{n=-\infty}^{+\infty} |x[n]|^2$$
Um sinal classifica-se como Sinal de Energia se $0 < E < \infty$.

A **Potência Média** ($P$) é calculada distribuindo a energia por um intervalo de observação ($2N+1$ amostras) e aplicando o limite quando este intervalo tende para o infinito:
$$P = \lim_{N\rightarrow\infty} \frac{1}{2N+1} \sum_{n=-N}^{N} |x[n]|^2$$
Um sinal classifica-se como Sinal de Potência se $0 < P < \infty$.

---

## Investigação e Simulação Computacional

Para esta verificação, selecionamos dois sinais do nosso contexto de estudo:
1.  O pulso transitório assimétrico (suporte finito), como exemplo de Sinal de Energia.
2.  A senoide da rede elétrica (**179,6 V**), como exemplo de Sinal de Potência.

:::: {include} ./simulacao/simulacao_energia_potencia.ipynb
::::

### Discussão dos Resultados

A simulação comprova inequivocamente a teoria matemática:

**1. Sinal de Energia (Pulso Transitório):**
Sendo um sinal limitado às amostras de $n=0$ a $n=4$, o somatório de energia converge. O cálculo analítico da energia total é dado pela soma dos quadrados das amplitudes:
$$E = 2^2 + 4^2 + 3^2 + 1^2 + 0{,}5^2 = 4 + 16 + 9 + 1 + 0{,}25 = 30{,}25$$
A simulação reproduziu exatamente o valor de **30,25 J**, corroborando a natureza finita do sinal. Se tentarmos calcular a sua potência média para $N \rightarrow \infty$, o resultado será invariavelmente **0 W**.

**2. Sinal de Potência (Senoide Periódica):**
A onda da rede elétrica estende-se ciclicamente. Se somarmos a energia de infinitos ciclos, o valor diverge. Contudo, a potência de um sinal senoidal da forma $A\cos(\omega_0 n)$ é determinada analiticamente pela relação $P = A^2 / 2$.
Substituindo a amplitude de pico do nosso modelo da rede elétrica ($A = 179{,}6$):
$$P = \frac{179{,}6^2}{2} = \frac{32256{,}16}{2} = 16128{,}08$$
O código iterou sobre múltiplos ciclos e confirmou precisamente o valor de **16128,08 W**, comprovando que a taxa de transferência de energia se mantém perfeitamente constante ao longo do tempo.