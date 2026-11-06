---
title: Tópico 8 - Aliasing
---

# Tópico 8 - Aliasing

---

## Resumo Conceitual
O **Aliasing** (ou falseamento espectral) ocorre quando o Critério de Amostragem de Nyquist-Shannon é violado, ou seja, quando uma componente senoidal contínua possui frequência $f_0$ superior à metade da taxa de amostragem ($f_N = f_s / 2$). Sob essa condição, a taxa de amostragem torna-se insuficiente para registrar a velocidade real de oscilação do sinal contínuo, fazendo com que as amostras discretas coletadas sigam uma trajetória idêntica à de um sinal de menor frequência.

Espectralmente, o aliasing resulta no dobramento (*folding*) do espectro em torno dos múltiplos da frequência de Nyquist $f_N$. A componente de alta frequência "dobra-se" para dentro da banda passante discreta fundamental $[0, f_N]$, assumindo uma **frequência aparente** $f_{alias} = |f_0 - k f_s|$. O aliasing é uma distorção irreversível no domínio discreto, uma vez que, após a amostragem, o sistema computacional não possui meios de diferenciar se o valor discreto foi gerado por um sinal real de baixa frequência ou por um sinal original de alta frequência rebatido.

---

## Formulação Matemática
Considere a amostragem ideal de uma senoide contínua puramente real de frequência $f_0$:
$$x(t) = \cos(2\pi f_0 t)$$

Amostrada com período $T_s = 1/f_s$, a sequência discreta $x[n]$ é dada por:
$$x[n] = x(n T_s) = \cos\left(2\pi \frac{f_0}{f_s} n\right)$$

Se a frequência $f_0$ exceder a frequência de Nyquist ($f_0 > f_s / 2$), a frequência normalizada $f_n = f_0 / f_s$ sai do intervalo fundamental $[-0,5; 0,5]$. Utilizando a propriedade de periodicidade com período $1$ e a paridade do cosseno $\cos(\theta) = \cos(-\theta)$, pode-se expressar $f_0$ em relação ao múltiplo mais próximo da taxa de amostragem $k \cdot f_s$:
$$\cos\left(2\pi \frac{f_0}{f_s} n\right) = \cos\left(2\pi \left(\frac{f_0 - k f_s}{f_s}\right) n\right) = \cos\left(2\pi \frac{|f_0 - k f_s|}{f_s} n\right)$$

Definindo a **frequência aparente (ou de aliasing)** como:
$$f_{alias} = |f_0 - k \cdot f_s| \quad \text{tal que } 0 \le f_{alias} \le \frac{f_s}{2}, \quad k \in \mathbb{Z}$$

Para o experimento proposto ($f_0 = 70\text{ Hz}$ e $f_s = 100\text{ Hz}$):
- Frequência de Nyquist: $f_N = \frac{f_s}{2} = \frac{100}{2} = 50\text{ Hz}$
- Como $f_0 = 70\text{ Hz} > 50\text{ Hz}$, adota-se $k = 1$:
  $$f_{alias} = |70 - 1 \cdot 100| = |-30| = 30\text{ Hz}$$

Logo, o sinal amostrado $x[n]$ torna-se matematicamente idêntico à amostragem de um sinal de $30\text{ Hz}$:
$$x[n] = \cos\left(2\pi \frac{70}{100} n\right) = \cos\left(2\pi \cdot 0,7 n\right) = \cos\left(2\pi (0,7 - 1) n\right) = \cos(-2\pi \cdot 0,3 n) = \cos(2\pi \cdot 0,3 n)$$

---

## Investigação e Simulação Computacional

A simulação computacional foi desenvolvida em Python no ambiente Jupyter Notebook para analisar o comportamento temporal e espectral de um sinal de $f_0 = 70\text{ Hz}$ amostrado a $f_s = 100\text{ Hz}$.

:::: {include} ./simulacao/simulacao_aliasing.ipynb
::::

No experimento, foram gerados a curva temporal contínua de $70\text{ Hz}$, o sinal reconstruído aparente de $30\text{ Hz}$ e as amostras discretas coletadas a $100\text{ Hz}$ (`grafico_aliasing_tempo.png`). Em seguida, a FFT da sequência foi calculada e plotada no intervalo de $0$ a $60\text{ Hz}$ (`grafico_aliasing_espectro.png`).

---

### Discussão dos Resultados

#### 1. Identificação da Frequência Aparente no Tempo e no Espectro
- **Análise Temporal**: O gráfico no domínio do tempo evidencia que as amostras discretas (hastes azuis) coletadas da onda original de $70\text{ Hz}$ (curva cinza tracejada) coincidem exatamente sobre os picos e vales da senoide aparente de $30\text{ Hz}$ (curva vermelha pontilhada). Como a taxa $f_s = 100\text{ Hz}$ gera apenas $1,428$ amostras por ciclo de $70\text{ Hz}$, os pontos discretos não conseguem acompanhar as oscilações rápidas, criando a ilusão óptica e numérica de uma oscilação mais lenta de $30\text{ Hz}$.
- **Análise Espectral (FFT)**: A transformada de Fourier da sequência discreta registra um único pico de magnitude nítido localizado estritamente em $f_{alias} = 30\text{ Hz}$. Não há qualquer registro de energia na frequência real de $70\text{ Hz}$, pois esta se encontra além do limite de Nyquist de $50\text{ Hz}$.

#### 2. Problemas do Aliasing em Aplicações Reais de Engenharia
O aliasing pode causar falhas críticas em diversos domínios práticos:
- **Sistemas de Áudio Digital**: Frequências ultrassônicas (acima de $20\text{ kHz}$) não filtradas antes da amostragem são rebatidas para a faixa audível ($0\text{ a } 20\text{ kHz}$), produzindo ruídos, apitos e distorções harmônicas severas.
- **Sistemas de Controle e Automação Industrial**: Em malhas de controle digital de motores ou processos químicos, ruídos de alta frequência provenientes de redes elétricas ($60\text{ Hz}$ ou comutação de inversores) subamostrados por sensores digitais podem ser interpretados como oscilações lentas da variável de processo, levando o controlador a correções indevidas e instabilidade.
- **Processamento de Imagens e Vídeo**: O efeito estroboscópico (ex.: rodas de carros girando para trás em filmes) e o padrão Moiré em telas e tecidos são manifestações visuais diretas do aliasing espacial e temporal.
- **Prevenção**: Para evitar o aliasing em sistemas reais, é obrigatória a inclusão de um **filtro anti-aliasing analógico passa-baixas (Anti-Aliasing Filter - AAF)** imediatamente antes do Conversor Analógico-Digital (ADC), atenuando todas as componentes espectrais superiores a $f_s / 2$.