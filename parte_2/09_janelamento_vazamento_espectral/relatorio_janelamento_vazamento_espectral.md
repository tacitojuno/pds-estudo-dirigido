---
title: Tópico 9 - Janelamento e Vazamento Espectral (Spectral Leakage)
---

# Tópico 9 - Janelamento e Vazamento Espectral (Spectral Leakage)

---

## Resumo Conceitual
O **Vazamento Espectral** (*Spectral Leakage*) é uma distorção inevitável no processamento digital de sinais que surge devido ao truncamento de um sinal contínuo em um registro temporal de duração finita. Ao capturar uma sequência de $N$ amostras, aplica-se implicitamente uma **janela retangular** no tempo, assumindo-se que o sinal é nulo fora dessa janela (ou periodicamente repetido no cálculo da DFT).

Se a frequência de uma componente senoidal contida no sinal não for um múltiplo exato do espaçamento de frequência da DFT ($\Delta f = f_s / N$), o bloco amostrado conterá um número não-inteiro de ciclos. Isso cria uma descontinuidade abrupta de borda na transição entre blocos consecutivos. No domínio da frequência, a multiplicação no tempo por uma janela equivale à convolução do espectro do sinal com o espectro da janela (uma função do tipo *sinc*). A energia da frequência pura vaza (*leakage*) para os lóbulos secundários da janela, espalhando-se por múltiplos compartimentos discretos (*bins*) adjacentes.

Para mitigar o vazamento espectral, aplicam-se **funções de janelamento** (como **Hann** e **Hamming**). Essas janelas atenua suavizadamente as amostras nas bordas do bloco temporal até zero (ou próximo de zero), eliminando as descontinuidades de borda. Isso reduz drasticamente a amplitude dos lóbulos secundários no domínio da frequência, ao custo de um discreto alargamento do lóbulo principal (pequena redução na resolução espectral).

---

## Formulação Matemática
Seja $x[n]$ uma sequência discreta de tempo e $w[n]$ uma função de janela de duração $N$. O sinal janelado $x_w[n]$ é obtido pelo produto elemento a elemento:
$$x_w[n] = x[n] \cdot w[n], \quad 0 \le n \le N-1$$

Pelo Teorema da Modulação (ou Convolução no domínio da frequência), a DTFT do sinal janelado $X_w(e^{j\omega})$ é a convolução periódica das transformadas individuais:
$$X_w(e^{j\omega}) = \frac{1}{2\pi} \int_{-\pi}^{\pi} X(e^{j\theta}) W(e^{j(\omega - \theta)}) \, d\theta$$

### Funções de Janela Estudadas:

1. **Janela Retangular**:
   $$w_{\text{rect}}[n] = 1, \quad 0 \le n \le N-1$$
   - **Primeiro lóbulo secundário**: $\approx -13\text{ dB}$ em relação ao pico principal.
   - **Largura do lóbulo principal**: $\Delta \omega = \frac{4\pi}{N}$ (máxima resolução espectral, porém alto vazamento).

2. **Janela de Hann (Hanning)**:
   $$w_{\text{hann}}[n] = 0,5 - 0,5 \cos\left(\frac{2\pi n}{N-1}\right) = \sin^2\left(\frac{\pi n}{N-1}\right), \quad 0 \le n \le N-1$$
   - **Primeiro lóbulo secundário**: $\approx -31,5\text{ dB}$.
   - **Taxa de decaimento dos lóbulos**: $-18\text{ dB/oitava}$ (excelente para isolar frequências distantes).
   - **Largura do lóbulo principal**: $\Delta \omega = \frac{8\pi}{N}$.

3. **Janela de Hamming**:
   $$w_{\text{hamming}}[n] = 0,54 - 0,46 \cos\left(\frac{2\pi n}{N-1}\right), \quad 0 \le n \le N-1$$
   - **Primeiro lóbulo secundário**: $\approx -42,7\text{ dB}$ (excelente cancelamento do primeiro lóbulo).
   - **Taxa de decaimento dos lóbulos**: $-6\text{ dB/oitava}$.
   - **Largura do lóbulo principal**: $\Delta \omega = \frac{8\pi}{N}$.

---

## Investigação e Simulação Computacional

A simulação computacional foi desenvolvida em Python no ambiente Jupyter Notebook para analisar um sinal senoidal de $f_0 = 105\text{ Hz}$ com amostragem $f_s = 1000\text{ Hz}$ e $N = 64$ amostras (cujo bin ideal de frequência seria $k = \frac{105 \times 64}{1000} = 6,72$, um número não-inteiro).

:::: {include} ./simulacao/simulacao_janelamento_vazamento_espectral.ipynb
::::

No experimento, o sinal foi multiplicado pelas janelas Retangular, Hann e Hamming (`grafico_janelas_tempo.png`). Em seguida, calculou-se a FFT de alta densidade (com zero-padding a 1024 pontos) para avaliar a magnitude espectral em escala linear e em escala de decibéis $\text{dB}$ (`grafico_vazamento_espectral.png`).

---

### Discussão dos Resultados

#### 1. Origem do Vazamento e Efeito da Janela Retangular
Quando o bin de frequência não é inteiro ($k = 6,72$), a janela retangular faz com que as descontinuidades de amostragem espalhem a energia da frequência pura de $105\text{ Hz}$ ao longo de todo o espectro. No gráfico em escala logarítmica ($\text{dB}$), observa-se que os lóbulos secundários da janela retangular permanecem em níveis elevados ($\approx -13\text{ dB}$ no primeiro lóbulo), o que pode facilmente mascarar componentes fracas vizinhas no sinal.

#### 2. Comparação entre Janelas: Resolução vs. Atenuação de Lóbulos
- **Janela de Hann**: Reduz as amostras nas extremidades para zero, eliminando o degrau de borda. No espectro em $\text{dB}$, o primeiro lóbulo secundário cai para cerca de $-32\text{ dB}$ e os lóbulos subsequentes decaem rapidamente com o aumento da frequência. Isso limpa o ruído de vazamento nas faixas distantes, ao custo de alargar a base do pico em $105\text{ Hz}$.
- **Janela de Hamming**: Apresenta uma atenuação ainda mais pronunciada no primeiro lóbulo secundário ($\approx -43\text{ dB}$), fornecendo uma excelente supressão de ruído espectral próximo ao pico.
- **Compromisso de Projeto (*Trade-off*)**: Nenhuma janela é perfeita para todos os casos. Se a prioridade for separar duas frequências muito próximas de amplitudes semelhantes, a **janela retangular** oferece a maior resolução (lóbulo principal mais estreito). Se a prioridade for detectar componentes de baixa amplitude próximas a uma componente muito forte, as **janelas de Hann ou Hamming** são indispensáveis para impedir que o vazamento da frequência forte encubra o sinal fraco.