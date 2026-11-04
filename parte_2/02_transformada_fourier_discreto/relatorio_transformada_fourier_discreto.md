---
title: Tópico 2 - Transformada de Fourier em Tempo Discreto (DTFT)
---

# Tópico 2 - Transformada de Fourier em Tempo Discreto (DTFT)

---

## Resumo Conceitual
A Transformada de Fourier em Tempo Discreto (DTFT - *Discrete-Time Fourier Transform*) permite analisar o conteúdo espectral de sequências de tempo discreto $x[n]$ no domínio da frequência contínua $\omega$ [1, 2]. Enquanto o domínio do tempo descreve como a amplitude de um sinal evolui a cada amostra $n$, a DTFT decompõe esse sinal em uma combinação contínua de exponenciais complexas $e^{j\omega n}$ de diferentes frequências angulares [1].

A representação no domínio da frequência é fundamental para o Processamento Digital de Sinais, pois expressa a resposta em frequência de sistemas LTI e permite mapear a distribuição de energia do sinal entre diferentes faixas espectrais [1, 3]. A DTFT resulta em uma função complexa contínua $X(e^{j\omega})$, cuja análise é decomposta no **espectro de magnitude** $|X(e^{j\omega})|$ (que indica a contribuição de cada frequência) e no **espectro de fase** $\angle X(e^{j\omega})$ (que reflete os alinhamentos e atrasos temporais relativos das componentes) [1, 2]. Além disso, devido à natureza discreta da amostragem no tempo, a DTFT é inerentemente periódica em $\omega$ com período de $2\pi$ radianos [1].

---

## Formulação Matemática
A DTFT direta de uma sequência discreta $x[n]$ é definida matematicamente por [1]:
$$X(e^{j\omega}) = \sum_{n=-\infty}^{+\infty} x[n] e^{-j\omega n}$$

Onde $\omega \in [-\pi, \pi]$ representa a frequência angular discreta contínua em radianos por amostra. A reconstrução da sequência discreta no tempo é obtida através da Transformada Inversa de Fourier em Tempo Discreto (IDTFT) [1]:
$$x[n] = \frac{1}{2\pi} \int_{-\pi}^{\pi} X(e^{j\omega}) e^{j\omega n} \, d\omega$$

Como $X(e^{j\omega})$ é uma função com valores complexos, sua decomposição polar fornece o espectro de magnitude e o espectro de fase [1, 2]:
$$X(e^{j\omega}) = |X(e^{j\omega})| \cdot e^{j \angle X(e^{j\omega})}$$

- **Espectro de Magnitude**:
  $$|X(e^{j\omega})| = \sqrt{\text{Re}\{X(e^{j\omega})\}^2 + \text{Im}\{X(e^{j\omega})\}^2}$$
- **Espectro de Fase**:
  $$\angle X(e^{j\omega}) = \text{atan2}\left(\text{Im}\{X(e^{j\omega})\}, \text{Re}\{X(e^{j\omega})\}\right)$$

Para o pulso retangular discreto $x[n]$ de comprimento $N_1$ considerado na investigação ($x[n] = 1$ para $0 \le n < N_1$ e $0$ nos demais pontos), a DTFT resulta no somatório de uma série geométrica finita:
$$X(e^{j\omega}) = \sum_{n=0}^{N_1-1} 1 \cdot e^{-j\omega n} = \frac{1 - e^{-j\omega N_1}}{1 - e^{-j\omega}}$$

Fatorando os termos $e^{-j\omega N_1 / 2}$ no numerador e $e^{-j\omega / 2}$ no denominador, obtém-se a forma fechada da sinc discreta (função de Dirichlet):
$$X(e^{j\omega}) = e^{-j\omega \frac{N_1 - 1}{2}} \cdot \frac{\sin\left(\frac{\omega N_1}{2}\right)}{\sin\left(\frac{\omega}{2}\right)}$$

---

## Investigação e Simulação Computacional

A simulação foi realizada em Python no Jupyter Notebook para avaliar o pulso retangular discreto $x[n]$ com $N_1 = 8$ amostras no intervalo temporal $n \in [-5, 14]$ e sua correspondente DTFT calculada em uma grade contínua densa de 1000 pontos de frequência $\omega \in [-\pi, \pi]$ [2].

:::: {include} ./simulacao/simulacao_transformada_fourier_discreto.ipynb
::::

O código implementou o cálculo da somatório da DTFT via produto matricial sobre a grade de frequências e gerou os gráficos do sinal no tempo (`grafico_sinal_tempo_dtft.png`) e dos espectros de magnitude e fase (`grafico_dtft_magnitude_fase.png`).

---

### Discussão dos Resultados

#### 1. Relação entre a Duração Temporal e a Largura Espectral
O gráfico no domínio do tempo apresenta um pulso retangular de amplitude unitária limitado no intervalo $0 \le n \le 7$ ($N_1 = 8$). No domínio da frequência, a magnitude $|X(e^{j\omega})|$ apresenta a forma clássica do tipo sinc periódica (lóbulo principal centralizado em $\omega = 0$ acompanhado de lóbulos secundários). 
- O valor máximo de magnitude no centro do espectro ($\omega = 0$) é exatamente igual a $|X(e^{j0})| = \sum_{n=0}^{7} x[n] = N_1 = 8$, correspondendo ao valor de nível DC (média contínua) do sinal.
- Os primeiros cruzamentos por zero da magnitude ocorrem quando $\sin(\omega N_1 / 2) = 0$ (para $\omega \neq 0$), ou seja, em $\omega = \pm \frac{2\pi}{N_1} = \pm \frac{2\pi}{8} = \pm 0.25\pi$. Isso demonstra que a largura do lóbulo principal é inversamente proporcional à duração temporal do pulso $N_1$.

#### 2. Comportamento da Fase e Informações Claras no Domínio da Frequência
- **Espectro de Fase**: O gráfico de fase apresenta uma variação linear dada pela inclinação $-\omega \frac{N_1 - 1}{2} = -3.5\omega$, pontuada por descontinuidades abruptas de $\pi$ radianos. Essas descontinuidades de $\pi$ ocorrem exatamente nos pontos em que a função $\frac{\sin(\omega N_1 / 2)}{\sin(\omega / 2)}$ muda de sinal (atravessa o zero), invertendo a polaridade do sinal complexo.
- **Informações Mais Claras no Domínio da Frequência**: Enquanto no domínio do tempo observamos apenas valores pontuais discretos (presença ou ausência de pulso), no domínio da frequência identificamos diretamente a concentração de energia do sinal (banda passante efetiva), a frequência média (DC), as taxas de decaimento dos lóbulos secundários e os efeitos de fase provocados pelo deslocamento temporal.