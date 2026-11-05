---
title: Tópico 6 - Frequência Normalizada
---

# Tópico 6 - Frequência Normalizada

---

## Resumo Conceitual
Em sistemas de tempo contínuo, a frequência é expressa em Hertz ($\text{Hz}$), representando o número de ciclos oscilatórios por segundo. Contudo, ao realizar a amostragem de um sinal com uma frequência de amostragem $f_s$, o eixo temporal contínuo $t$ é discretizado na sequência de índices inteiros de amostras $n$. Dessa forma, no domínio discreto, a noção de segundo se perde e a frequência passa a ser mensurada de forma relativa ao intervalo de amostragem.

A **Frequência Normalizada** descreve a taxa de oscilação do sinal por amostra discreta. Ela pode ser expressa em duas formas fundamentais:
1. **Frequência Normalizada Linear ($f_n$)**: expressa em ciclos por amostra, dada pela razão $f_n = \frac{f}{f_s}$.
2. **Frequência Angular Discreta ($\omega$)**: expressa em radianos por amostra, dada por $\omega = 2\pi f_n = 2\pi \frac{f}{f_s}$.

Devido à natureza periódica das exponenciais complexas discretas $e^{j\omega n}$, frequências discretas que diferem por múltiplos inteiros de $2\pi$ radianos são computacionalmente indistinguíveis. Portanto, o intervalo fundamental único para análise espectral discreta situa-se entre $-\pi \le \omega \le \pi$ rad/amostra (correspondendo a $-0,5 \le f_n \le 0,5$ ciclos/amostra), onde $\omega = \pi$ representa a frequência de Nyquist ($f_s / 2$).

---

## Formulação Matemática
A conversão do tempo contínuo $t$ para o índice discreto $n$ sob um período de amostragem $T_s = 1/f_s$ é estabelecida por:
$$t = n \cdot T_s = \frac{n}{f_s}$$

Substituindo a relação temporal na equação de uma senoide contínua $x(t) = A \cos(2\pi f t + \phi)$:
$$x[n] = x(n T_s) = A \cos\left(2\pi f \frac{n}{f_s} + \phi\right) = A \cos\left(2\pi f_n n + \phi\right) = A \cos(\omega n + \phi)$$

Dessa relação, derivam-se as equações de conversão entre os três domínios de frequência:

- **Frequência Normalizada em Ciclos por Amostra ($f_n$)**:
  $$f_n = \frac{f}{f_s} \quad [\text{ciclos/amostra}]$$

- **Frequência Angular Discreta em Radianos por Amostra ($\omega$)**:
  $$\omega = 2\pi f_n = 2\pi \frac{f}{f_s} \quad [\text{rad/amostra}]$$

- **Número de Amostras por Ciclo Completo ($N_{\text{ciclo}}$)**:
  $$N_{\text{ciclo}} = \frac{1}{f_n} = \frac{f_s}{f} = \frac{2\pi}{\omega} \quad [\text{amostras/ciclo}]$$

- **Intervalo Fundamental de Nyquist**:
  $$f \in \left[-\frac{f_s}{2}, \frac{f_s}{2}\right] \iff f_n \in [-0,5; 0,5] \iff \omega \in [-\pi, \pi]$$

---

## Investigação e Simulação Computacional

A simulação computacional foi desenvolvida em Python no ambiente Jupyter Notebook para analisar a conversão numérica de unidades e o impacto visual da variação de $f_s$ sobre a frequência discreta $\omega$.

:::: {include} ./simulacao/simulacao_frequencia_normalizada.ipynb
::::

No experimento, foi montada primeiramente uma tabela computacional convertendo frequências físicas $f \in \{50, 100, 250, 500\}\text{ Hz}$ para $f_n$ e $\omega$ sob duas taxas de amostragem distintas ($f_s = 1000\text{ Hz}$ e $f_s = 8000\text{ Hz}$).

Na sequência, fixou-se um sinal senoidal de $f = 100\text{ Hz}$ e analisou-se sua representação discreta $x[n]$ ao variar a taxa de amostragem em três cenários: $f_{s1} = 400\text{ Hz}$, $f_{s2} = 1000\text{ Hz}$ e $f_{s3} = 4000\text{ Hz}$. O gráfico gerado foi exportado para `grafico_frequencia_normalizada.png`.

---

### Discussão dos Resultados

#### 1. Conversão de Unidades e Densidade de Amostragem
A análise numérica e gráfica demonstra que um mesmo sinal analógico de $100\text{ Hz}$ assume comportamentos discretos totalmente distintos dependendo da frequência de amostragem escolhida:
- **Cenário $f_s = 400\text{ Hz}$**: A frequência normalizada é $f_n = \frac{100}{400} = 0,25$ ciclos/amostra ($\omega = 0,5\pi\text{ rad/amostra}$). Cada ciclo da senoide é representado por exatamente $N_{\text{ciclo}} = \frac{1}{0,25} = 4$ amostras.
- **Cenário $f_s = 1000\text{ Hz}$**: A frequência normalizada reduz-se para $f_n = \frac{100}{1000} = 0,10$ ciclos/amostra ($\omega = 0,2\pi\text{ rad/amostra}$). A densidade aumenta para $N_{\text{ciclo}} = 10$ amostras por ciclo.
- **Cenário $f_s = 4000\text{ Hz}$**: A frequência normalizada passa a ser $f_n = \frac{100}{4000} = 0,025$ ciclos/amostra ($\omega = 0,05\pi\text{ rad/amostra}$), resultando em $N_{\text{ciclo}} = 40$ amostras por ciclo.

#### 2. Invariância Discreta e Limites Espectrais
- **Significado da Frequência Discreta**: Em processamento digital de sinais, a frequência angular $\omega$ representa a rotação de fase por amostra no plano complexo. Dois sinais analógicos de frequências físicas completamente diferentes (ex: $100\text{ Hz}$ e $800\text{ Hz}$) produzirão exatamente a mesma sequência discreta $x[n]$ se forem amostrados com taxas $f_s$ proporcionalmente equivalentes ($1000\text{ Hz}$ e $8000\text{ Hz}$, respectivamente, ambas com $\omega = 0,2\pi\text{ rad/amostra}$).
- **Limite de Nyquist ($\omega = \pi$)**: A maior frequência angular discreta não-ambígua possível é $\omega = \pi\text{ rad/amostra}$ ($f_n = 0,5$), o que exige no mínimo 2 amostras por ciclo ($N_{\text{ciclo}} = 2$). Frequências físicas acima de $f_s / 2$ ultrapassam esse limite e sofrem dobramento espectral (*aliasing*).