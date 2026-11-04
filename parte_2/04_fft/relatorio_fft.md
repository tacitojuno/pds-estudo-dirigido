---
title: Tópico 4 - FFT (Fast Fourier Transform)
---

# Tópico 4 - FFT (Fast Fourier Transform)

---

## Resumo Conceitual
A **Transformada Rápida de Fourier** (FFT - *Fast Fourier Transform*) não é uma nova transformada matemática, mas sim um conjunto de algoritmos altamente eficientes projetados para calcular a Transformada Discreta de Fourier (DFT) e sua inversa [1, 2]. Enquanto o cálculo direto da DFT avalia a definição formal realizando produtos matriciais elemento a elemento, o algoritmo FFT explora as propriedades de **simetria** e **periodicidade** dos fatores de rotação complexos ($W_N^{kn}$) para eliminar redundâncias computacionais [1, 2].

A abordagem clássica de divisão e conquista (como o algoritmo de *Cooley-Tukey* no formato Radix-2) decompõe iterativamente uma DFT de $N$ pontos em duas DFTs menores de $N/2$ pontos (separando índices pares e ímpares), reduzindo drasticamente a complexidade computacional de $\mathcal{O}(N^2)$ para $\mathcal{O}(N \log_2 N)$ [1, 2]. Essa otimização viabilizou a aplicação da análise espectral em tempo real, tornando a FFT o pilar fundamental do Processamento Digital de Sinais em áreas como engenharia de telecomunicações, sistemas de áudio, processamento de imagens e sistemas de controle.

---

## Formulação Matemática
A expressão matemática para o cálculo direto da DFT de $N$ pontos de uma sequência $x[n]$ é dada por [3]:
$$X[k] = \sum_{n=0}^{N-1} x[n] W_N^{kn}, \quad k = 0, 1, \dots, N-1$$

Onde o fator de rotação complexo (ou *twiddle factor*) é definido por:
$$W_N = e^{-j \frac{2\pi}{N}}$$

Para o cálculo direto da DFT, a avaliação de $N$ coeficientes espectrais $X[k]$ exige $N^2$ multiplicações complexas e $N(N-1)$ adições complexas, resultando em uma complexidade de ordem $\mathcal{O}(N^2)$ [1, 2].

### Decomposição de Cooley-Tukey (Radix-2)
Separando a somatório da DFT entre os índices pares ($n = 2m$) e ímpares ($n = 2m+1$):
$$X[k] = \sum_{m=0}^{N/2-1} x[2m] W_N^{(2m)k} + \sum_{m=0}^{N/2-1} x[2m+1] W_N^{(2m+1)k}$$

Utilizando a propriedade periódica dos fatores de rotação $W_N^{2mk} = W_{N/2}^{mk}$ e $W_N^{(2m+1)k} = W_N^k W_{N/2}^{mk}$, reescreve-se $X[k]$ como a combinação de duas DFTs de ordem $N/2$:
$$X[k] = E[k] + W_N^k O[k]$$

Onde:
- $E[k]$ é a DFT de $N/2$ pontos das amostras de índice par $x[2m]$.
- $O[k]$ é a DFT de $N/2$ pontos das amostras de índice ímpar $x[2m+1]$.

Aproveitando a simetria $W_N^{k + N/2} = -W_N^k$, obtém-se a estrutura computacional conhecida como **operação borboleta** (*butterfly operation*):
$$X\left[k\right] = E[k] + W_N^k O[k], \quad 0 \le k < \frac{N}{2}$$
$$X\left[k + \frac{N}{2}\right] = E[k] - W_N^k O[k], \quad 0 \le k < \frac{N}{2}$$

Essa simplificação reduz o número total de multiplicações complexas para $\frac{N}{2} \log_2 N$ e de adições para $N \log_2 N$, alterando a classe de complexidade para $\mathcal{O}(N \log_2 N)$ [2].

---

## Investigação e Simulação Computacional

A simulação computacional foi desenvolvida em Python no ambiente Jupyter Notebook para validar a equivalência matemática entre a DFT direta e a FFT, além de comparar experimentalmente o tempo de execução para diferentes tamanhos de sinal $N$ [2].

:::: {include} ./simulacao/simulacao_fft.ipynb
::::

No experimento, foi gerado inicialmente um sinal de teste $x[n] = \sin(0.2\pi n) + 0.5\cos(0.4\pi n)$ com $N=64$ amostras. A transformada foi calculada simultaneamente por uma implementação explícita da DFT por produto matricial $\mathcal{O}(N^2)$ e pela função otimizada `numpy.fft.fft()` [2]. As saídas de magnitude foram plotadas e salvas em `grafico_comparacao_dft_fft.png`.

Em seguida, foi executado um teste de desempenho (*benchmark*) mensurando os tempos de processamento para $N \in \{32, 64, 128, 256, 512, 1024, 2048\}$. Os tempos medidos foram plotados em escala logarítmica e exportados para `grafico_tempo_execucao_fft.png`.

---

### Discussão dos Resultados

#### 1. Equivalência Numérica entre DFT Direta e FFT
A comparação direta entre o vetor espectral $X_{\text{dft}}[k]$ (calculado numericamente via produto matricial) e o vetor $X_{\text{fft}}[k]$ (computado pela biblioteca `numpy.fft`) resultou em um erro máximo absoluto de magnitude inferior a $10^{-15}$. Essa diferença imperceptível situa-se no limite da precisão de ponto flutuante de 64 bits (`float64`), confirmando experimentalmente que a FFT é rigorosamente equivalente à DFT formal e não introduz qualquer aproximação ou perda de informação espectral [2].

#### 2. Desempenho e Complexidade Computacional
O gráfico de tempo de execução evidencia o impacto real da redução da complexidade teórica:
- **Crescimento da DFT Direta $\mathcal{O}(N^2)$**: O tempo de processamento da DFT direta cresce de forma quadrática acentuada. Para um sinal de $N = 1024$ pontos, o cálculo direto exige aproximadamente $1024^2 \approx 1,05 \times 10^6$ operações complexas.
- **Eficiência da FFT $\mathcal{O}(N \log_2 N)$**: Para o mesmo tamanho $N = 1024$, o algoritmo FFT executa cerca de $1024 \times 10 = 1,02 \times 10^4$ operações, resultando em uma aceleração computacional de mais de 100 vezes em relação ao método direto.
- **Escalabilidade**: Para sinais com milhares ou milhões de amostras (como em processamento de áudio digital a $44,1\text{ kHz}$ ou em sinais de radar), a execução da DFT direta torna-se impraticável, enquanto a FFT realiza o processamento em poucos milissegundos. Esse ganho valida a importância histórica e prática da FFT na engenharia moderna.