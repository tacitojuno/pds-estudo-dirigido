---
title: Tópico 3 - Transformada Discreta de Fourier (DFT)
---

# Tópico 3 - Transformada Discreta de Fourier (DFT)

---

## Resumo Conceitual
Enquanto a Transformada de Fourier em Tempo Discreto (DTFT) produz um espectro contínuo em frequência $\omega$, no processamento computacional prático é necessário trabalhar com sequências de duração finita $N$ e frequências discretas [1]. A **Transformada Discreta de Fourier (DFT)** é a ferramenta matemática que possibilita a análise espectral em computadores digitais, mapeando uma sequência temporal discreta finita de $N$ amostras em exatamente $N$ coeficientes espectrais complexos no domínio da frequência [1].

Conceitualmente, a DFT pode ser interpretada como a amostragem uniforme da DTFT ao longo de um período de $2\pi$ radianos, dividindo a circunferência de frequência em $N$ compartimentos discretos chamados de *bins* ou índices de frequência $k$ [1]. Cada coeficiente $X[k]$ representa a contribuição de amplitude e fase da exponencial complexa de frequência angular discreta $\omega_k = \frac{2\pi}{N} k$. O espectro gerado é caracterizado pelo espectro de magnitude $|X[k]|$, que indica a distribuição de energia por componente de frequência, e pelo espectro de fase $\angle X[k]$, que contém as informações de alinhamento temporal [1, 2].

---

## Formulação Matemática
A Transformada Discreta de Fourier (DFT) de uma sequência de tempo discreto de duração finita $x[n]$ com $n \in \{0, 1, \dots, N-1\}$ é definida por [1]:
$$X[k] = \sum_{n=0}^{N-1} x[n] e^{-j \frac{2\pi}{N} k n}, \quad k = 0, 1, \dots, N-1$$

A reconstrução exata da sequência temporal original a partir de seus coeficientes espectrais $X[k]$ é dada pela Transformada Discreta de Fourier Inversa (IDFT) [1]:
$$x[n] = \frac{1}{N} \sum_{k=0}^{N-1} X[k] e^{j \frac{2\pi}{N} k n}, \quad n = 0, 1, \dots, N-1$$

A relação entre o índice de frequência discreto $k$ e a frequência angular contínua $\omega_k$ é expressa por:
$$\omega_k = \frac{2\pi}{N} k \quad \text{(rad/amostra)}$$

Para cada bin $k$, o resultado $X[k] \in \mathbb{C}$ é decomposto em módulo e argumento:
- **Espectro de Magnitude**:
  $$|X[k]| = \sqrt{\text{Re}\{X[k]\}^2 + \text{Im}\{X[k]\}^2}$$
- **Espectro de Fase**:
  $$\angle X[k] = \text{atan2}\left(\text{Im}\{X[k]\}, \text{Re}\{X[k]\}\right)$$

Para sinais reais no tempo ($x[n] \in \mathbb{R}$), a DFT exibe simetria hermitiana (conjugada):
$$X[k] = X^*[N-k]$$
o que implica em $|X[k]| = |X[N-k]|$ (magnitude par) e $\angle X[k] = -\angle X[N-k]$ (fase ímpar).

---

## Investigação e Simulação Computacional

A simulação computacional foi desenvolvida em Python no ambiente Jupyter Notebook para analisar um sinal composto pela soma de duas senoides com frequências e amplitudes distintas [1, 2]:
$$x[n] = A_1 \cos(\omega_1 n) + A_2 \cos(\omega_2 n)$$

Os parâmetros adotados na simulação foram:
- Número de amostras: $N = 64$
- Componente 1: $A_1 = 1.5$, $\omega_1 = 0.2\pi$ rad/amostra
- Componente 2: $A_2 = 0.8$, $\omega_2 = 0.5\pi$ rad/amostra

:::: {include} ./simulacao/simulacao_dft.ipynb
::::

No experimento, gerou-se primeiramente a sequência temporal $x[n]$ no intervalo $0 \le n \le 63$. Em seguida, calculou-se a matriz exponencial da DFT para obter o vetor espectral $X[k]$ de 64 pontos. Os gráficos do sinal temporal (`grafico_sinal_tempo_dft.png`) e dos espectros de magnitude e fase (`grafico_dft_magnitude_fase.png`) foram exportados para inclusão no relatório [2].

---

### Discussão dos Resultados

#### 1. Identificação de Frequências e Mapeamento dos Picos Espectrais
A análise do espectro de magnitude $|X[k]|$ permitiu relacionar diretamente os picos observados com as frequências contidas no sinal temporal gerado [2]:
- **Componente de Frequência $\omega_2 = 0.5\pi$**: O índice $k$ teórico associado é dado por $k_2 = \frac{\omega_2 N}{2\pi} = \frac{0.5\pi \cdot 64}{2\pi} = 16$. Como $k_2 = 16$ é um número inteiro exato (um bin exato da DFT), toda a energia dessa componente se concentrou estritamente no índice $k = 16$ (e seu conjugado simétrico em $k = 64 - 16 = 48$). A amplitude observada no pico é de $|X[2]| = \frac{A_2 \cdot N}{2} = \frac{0.8 \cdot 64}{2} = 25.6$.
- **Componente de Frequência $\omega_1 = 0.2\pi$**: O índice $k$ teórico associado corresponde a $k_1 = \frac{\omega_1 N}{2\pi} = \frac{0.2\pi \cdot 64}{2\pi} = 6.4$. Como $k_1$ não é um número inteiro, a frequência cai entre os compartimentos discretos $k=6$ e $k=7$. Isso provoca o espalhamento da energia entre os bins adjacentes, evidenciando o fenômeno de interpolação espectral inerente à amostragem discreta em frequência.

#### 2. Simetria Hermitiana e Interpretação da Fase
- **Simetria Espectral**: Como $x[n]$ é estritamente real, os gráficos de magnitude e fase confirmaram a simetria ao redor do bin central $k = N/2 = 32$ (frequência de Nyquist $\omega = \pi$). Os primeiros 32 bins ($0 \le k \le 31$) representam o intervalo espectral único de frequências de $0$ a $\pi$ rad/amostra.
- **Espectro de Fase**: Nos pontos de picos espectrais ($k=16$ e $k=48$), a fase observada aproxima-se de zero radianos (pois as senoides geradas tinham fase inicial nula $\phi = 0$). Nos bins sem componente ativa, a fase apresenta variações numéricas pequenas decorrentes do piso de precisão de ponto flutuante.