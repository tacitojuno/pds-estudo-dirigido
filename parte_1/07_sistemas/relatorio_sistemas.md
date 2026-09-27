---
title: Tópico 7 - Sistemas Discretos e Propriedades
---

# Tópico 7 - Sistemas Discretos e Propriedades

---

## Resumo Conceitual

Um sistema discreto é uma abstração matemática ou física que processa um sinal de entrada $x[n]$ para produzir um sinal de saída $y[n]$. No contexto do Processamento Digital de Sinais, a utilidade de um sistema depende diretamente das propriedades matemáticas às quais ele obedece. 

A classificação de sistemas baseia-se em critérios rigorosos como a presença de memória, a linearidade e a invariância no tempo. Sistemas que reúnem as propriedades de Linearidade e Invariância no Tempo (LTI - *Linear Time-Invariant*) formam a espinha dorsal de todo o processamento de sinais, permitindo o uso da convolução e da Transformada de Fourier.

---

## Formulação Matemática das Propriedades

### 1. Memória
Um sistema é considerado **sem memória** (ou instantâneo) se o valor da saída $y[n]$ em um dado instante depender exclusivamente do valor da entrada $x[n]$ nesse exato momento.
*   *Sem Memória:* $y[n] = K \cdot x[n]$
*   *Com Memória:* $y[n] = x[n] + x[n-1]$ (depende do passado)

### 2. Linearidade
Um sistema é **linear** se obedecer ao princípio da superposição, que engloba duas propriedades simultâneas:
*   **Aditividade:** A resposta a uma soma de sinais é a soma das respostas individuais.
*   **Homogeneidade (Escalonamento):** A resposta a um sinal escalonado por uma constante $a$ é a resposta original multiplicada por $a$.
Matematicamente: Se $x_1[n] \rightarrow y_1[n]$ e $x_2[n] \rightarrow y_2[n]$, então $a x_1[n] + b x_2[n] \rightarrow a y_1[n] + b y_2[n]$.

### 3. Invariância no Tempo
Um sistema é **invariante no tempo** se um atraso aplicado ao sinal de entrada gerar um atraso idêntico no sinal de saída, sem alterar o formato da onda.
*   Se $x[n] \rightarrow y[n]$, então $x[n-k] \rightarrow y[n-k]$ para qualquer $k$ inteiro. Se a própria equação do sistema tiver um termo $n$ solto multiplicando a entrada, o sistema será variante.

---

## Investigação e Simulação Computacional

Para investigar a teoria na prática, aplicamos o pulso transitório assimétrico a três sistemas matemáticos distintos.

:::: {include} ./simulacao/simulacao_sistemas.ipynb
::::

### Discussão dos Resultados

A visualização gráfica permite comprovar de imediato as propriedades teóricas de cada sistema:

1. **Sistema 1: Média Móvel (LTI e Com Memória)**
   O cálculo de uma média móvel de 3 pontos exige que o sistema recorde as duas últimas amostras ($x[n-1]$ e $x[n-2]$). O gráfico demonstra esse efeito de memória de forma clara: o pulso, que originalmente possuía 5 amostras não nulas, sofreu um "espalhamento" temporal, resultando em um sinal de saída mais longo e suave. Como a operação utiliza apenas adições e multiplicações por constantes, o sistema é estritamente Linear e Invariante no Tempo (LTI).

2. **Sistema 2: Quadrático (Não Linear e Sem Memória)**
   A equação $y[n] = x[n]^2$ não necessita de amostras anteriores ou futuras, o que o classifica como um sistema sem memória. Contudo, a não linearidade é gráfica e numericamente severa: a amostra de entrada com amplitude $4\,\text{V}$ gerou um pico de $16\,\text{V}$. Se a entrada fosse duplicada, a saída seria quadruplicada, violando diretamente o princípio da homogeneidade.

3. **Sistema 3: Variante no Tempo ($n \cdot x[n]$)**
   Embora seja um sistema sem memória (só analisa o instante atual), a sua resposta depende integralmente do momento cronológico em que ocorre. Na amostra $n=0$, o sinal de entrada tinha uma amplitude de $2\,\text{V}$, mas a saída foi $0 \times 2 = 0$. Na amostra $n=2$, a amplitude de $3\,\text{V}$ foi multiplicada para $6\,\text{V}$. Um simples atraso no sinal de entrada alteraria drasticamente os valores multiplicadores e a forma da saída, violando a propriedade da invariância temporal.