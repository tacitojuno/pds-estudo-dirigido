---
title: Tópico 12 - Polos, Zeros e Estabilidade no Plano-s
---

# Tópico 12 - Polos, Zeros e Estabilidade no Plano-s

---

## Resumo Conceitual
O **Diagrama de Polos e Zeros no Plano-s** é a principal ferramenta gráfica e analítica para determinar as propriedades dinâmicas e o comportamento estático/transiente de sistemas contínuos e invariantes no tempo (LTI). A posição relativa das raízes do numerador (zeros) e do denominador (polos) da função de transferência $H(s)$ no plano complexo $s = \sigma + j\omega$ define a estabilidade assintótica, a taxa de amortecimento e o tipo de resposta no domínio do tempo.

Do ponto de vista da **Estabilidade BIBO (Bounded-Input Bounded-Output)**, um sistema LTI contínuo é estritamente estável se, e somente se, todos os seus polos estiverem localizados no semiplano esquerdo do plano-$s$ ($\text{Re}\{p_k\} < 0$). Polos localizados exatamente sobre o eixo imaginário $j\omega$ ($\text{Re}\{p_k\} = 0$) resultam em estabilidade marginal (oscilações sustentadas sem atenuação), enquanto qualquer polo posicionado no semiplano direito ($\text{Re}\{p_k\} > 0$) provoca crescimento exponencial ilimitado, tornando o sistema instável.

---

## Formulação Matemática
Considere a função de transferência racional de um sistema LTI contínuo:
$$H(s) = \frac{N(s)}{D(s)} = K \frac{\prod_{i=1}^{M} (s - z_i)}{\prod_{k=1}^{N} (s - p_k)}$$

Onde:
- **Polos ($p_k$)**: São os valores de $s$ que anulam o denominador ($D(p_k) = 0$), fazendo com que $|H(s)| \rightarrow \infty$.
- **Zeros ($z_i$)**: São os valores de $s$ que anulam o numerador ($N(z_i) = 0$), fazendo com que $H(z_i) = 0$.

### 1. Decomposição em Frações Parciais e Resposta ao Impulso
A resposta ao impulso do sistema no domínio do tempo é dada pela Transformada Inversa de Laplace de $H(s)$:
$$h(t) = \mathcal{L}^{-1}\{H(s)\} = \sum_{k=1}^{N} A_k e^{p_k t} u(t)$$

Onde $A_k$ são os resíduos associados a cada polo $p_k = \sigma_k + j\omega_k$:
$$e^{p_k t} = e^{\sigma_k t} e^{j\omega_k t} = e^{\sigma_k t} \left(\cos(\omega_k t) + j\sin(\omega_k t)\right)$$

- **Se $\sigma_k < 0$**: O termo $e^{\sigma_k t} \rightarrow 0$ quando $t \rightarrow \infty$ (Resposta Estável).
- **Se $\sigma_k = 0$**: O termo $e^{0 \cdot t} = 1$ mantém oscilações de amplitude constante (Marginalmente Estável).
- **Se $\sigma_k > 0$**: O termo $e^{\sigma_k t} \rightarrow \infty$ quando $t \rightarrow \infty$ (Resposta Instável).

### 2. Influência dos Zeros no Domínio do Tempo
Embora os polos determinem as frequências naturais e as taxas de decaimento/crescimento exponencial, os zeros $z_i$ afetam diretamente a magnitude dos resíduos $A_k$:
- **Zero no Semiplano Esquerdo ($z < 0$, Fase Mínima)**: Acelera o tempo de subida da resposta temporal, mas pode aumentar o sobressinal (*overshoot*).
- **Zero no Semiplano Direito ($z > 0$, Fase Não-Mínima)**: Provoca o fenômeno de **resposta inversa inicial (*undershoot*)**, no qual a saída inicialmente move-se na direção oposta ao valor de regime permanente.

---

## Investigação e Simulação Computacional

A simulação computacional foi desenvolvida em Python no ambiente Jupyter Notebook para analisar o diagrama de polos e zeros no plano-$s$ e comparar o comportamento temporal ao degrau unitário para sistemas estáveis, marginalmente estáveis e instáveis, além de verificar o efeito da inclusão de zeros de fase mínima e não-mínima.

:::: {include} ./simulacao/simulacao_polos_zeros_estabilidade_plano_s.ipynb
::::

No experimento, foram gerados os mapas de polos e zeros para os três regimes de estabilidade (`grafico_mapa_polos_zeros.png`). Na sequência, simularam-se as respostas temporais ao degrau e o impacto de zeros no semiplano esquerdo e direito (`grafico_estabilidade_plano_s.png`).

---

### Discussão dos Resultados

#### 1. Mapeamento de Estabilidade no Plano-$s$
- **Sistema Estável ($p_{1,2} = -1 \pm j2$)**: Como os polos situam-se estritamente no semiplano esquerdo ($\sigma = -1 < 0$), a resposta ao degrau converge suavemente para o valor final de regime permanente ($y(\infty) = 1,0$) após um transiente oscilatório amortecido.
- **Sistema Marginalmente Estável ($p_{1,2} = \pm j2$)**: Como os polos estão exatamente sobre o eixo $j\omega$ ($\sigma = 0$), não há amortecimento exponencial. A resposta ao degrau resulta em uma oscilação senoidal contínua de amplitude constante ao redor do valor médio.
- **Sistema Instável ($p_{1,2} = +0,5 \pm j2$)**: Como a parte real dos polos é positiva ($\sigma = +0,5 > 0$), o envelope da oscilação cresce de forma exponencial ilimitada, confirmando a instabilidade do sistema.

#### 2. Efeito dos Zeros na Dinâmica Temporal
- **Zero em $s = -2$ (Fase Mínima)**: A introdução de um zero no semiplano esquerdo introduz um efeito de derivação que antecipa a resposta ao degrau, reduzindo o tempo de subida de $t_r \approx 1,2\text{ s}$ para $t_r \approx 0,5\text{ s}$, ao custo de aumentar o sobressinal.
- **Zero em $s = +2$ (Fase Não-Mínima)**: A adição de um zero no semiplano direito faz com que a saída inicie em sentido negativo ($y(t) < 0$ para $t \to 0^+$), caracterizando o efeito de *undershoot*. Esse comportamento é típico de sistemas com dinâmicas de atraso de fase severo (como em caldeiras e sistemas aerodinâmicos).