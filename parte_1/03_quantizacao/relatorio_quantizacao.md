---
title: Tópico 3 - Quantização e Resolução
---

# Tópico 3 - Quantização e Resolução

---

## Resumo Conceitual

Para que um sinal físico seja processado digitalmente, a discretização temporal (amostragem) não é suficiente; é imperativo discretizar também a amplitude do sinal. O processo responsável por mapear o número infinito de valores de amplitude de um sinal contínuo para um conjunto finito de níveis lógicos discretos é denominado **quantização**.

Os processadores e microcontroladores operam com base em uma arquitetura binária, o que significa que o número de níveis disponíveis para representar a amplitude depende da quantidade de *bits* ($N$) do Conversor Analógico-Digital (ADC). Essa limitação obriga o sistema a arredondar o valor real da amostra para o nível lógico mais próximo. A diferença entre o valor contínuo original e o valor discretizado gera o chamado **erro de quantização**, que atua como um ruído inerente ao processo de digitalização.

---

## Formulação Matemática

Um sistema de quantização com resolução de $N$ bits é capaz de representar uma quantidade finita de níveis lógicos distintos ($L$), dada por:
$$L = 2^N$$

A resolução do conversor, também conhecida como o "tamanho do degrau" ou *quantum* ($\Delta V$), é determinada pela razão entre a excursão total de tensão admitida pelo sistema ($V_{ref}$) e a quantidade de níveis:
$$\Delta V = \frac{V_{ref}}{L} = \frac{V_{ref}}{2^N}$$

Durante a conversão, a amostra de entrada $x[n]$ é arredondada para o nível quantizado $x_q[n]$. Na região linear de operação do ADC, o erro de quantização $e[n]$ assume um comportamento de variável aleatória uniformemente distribuída, cujo valor absoluto máximo teórico é metade do degrau de resolução:
$$e[n] = x_q[n] - x[n]$$
$$\vert{}e[n]\vert{} \le \frac{\Delta V}{2}$$

---

## Investigação e Simulação Computacional

Para ilustrar o impacto da resolução digital, aplicou-se a teoria de quantização ao sinal elétrico simulado nos tópicos anteriores ($60\,\text{Hz}$, $179{,}6\,\text{V}$ de pico). 

Considerando que a onda senoidal possui ciclos positivos e negativos, a excursão de tensão total de referência ($V_{ref}$) do nosso conversor abrange a amplitude pico a pico: $V_{ref} = 2 \times 179{,}6 = 359{,}2\,\text{V}$. Avaliamos o sinal utilizando conversores teóricos de 3, 4 e 8 bits.

:::: {include} ./simulacao/simulacao_quantizacao.ipynb
::::

### Discussão dos Resultados

A simulação demonstra claramente a relação inversa entre a quantidade de *bits* e a degradação da informação:

1. **Baixa Resolução (3 bits):** Com apenas $2^3 = 8$ níveis disponíveis para mapear $359{,}2\,\text{V}$, a resolução do sistema é extremamente grosseira ($\Delta V = 44{,}90\,\text{V}$). O sinal quantizado assemelha-se a uma função em escada severamente distorcida, com um erro de quantização massivo.
2. **Resolução Intermediária (4 bits):** Ao adicionar apenas 1 bit ao sistema, duplicamos a quantidade de níveis disponíveis para 16. O tamanho do degrau cai pela metade ($22{,}45\,\text{V}$) e a forma de onda quantizada já começa a delinear os contornos da senoide original.
3. **Alta Resolução (8 bits):** Com 256 níveis disponíveis, a curva quantizada sobrepõe-se quase perfeitamente ao sinal original. O tamanho do degrau cai para meros $1{,}40\,\text{V}$. O erro de quantização neste cenário torna-se residual e visualmente imperceptível na escala global do gráfico temporal.

**Análise do Erro de Saturação:**
Uma observação crucial extraída dos gráficos de erro reside no comportamento nos picos máximos da senoide. Embora a teoria dite que o limite do erro de quantização (na região de operação linear) é delimitado por $\pm \Delta/2$ (linhas pontilhadas vermelhas), verificamos que os pontos extremos ultrapassam esse valor, atingindo um erro igual a $\Delta$. Esse fenômeno deve-se ao fato de o modelo simular a **saturação** física do conversor (implementada via `np.clip`). Quando a tensão atinge o valor de pico exato, não existem níveis de quantização superiores para acomodar o arredondamento simétrico, forçando o ADC a fixar o valor máximo permitido, o que duplica o erro pontual nesse instante específico.