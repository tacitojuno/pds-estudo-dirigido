---
title: Tópico 10 - Transformada de Laplace
---

# Tópico 10 - Transformada de Laplace

---

## Resumo Conceitual
A **Transformada de Laplace** é uma ferramenta matemática fundamental para a análise de sinais e sistemas de tempo contínuo, atuando como uma ponte essencial para o estudo de Controle e Automação. Enquanto a Transformada de Fourier analisa sinais decompondo-os em senoides puras e exponenciais complexas de amplitude constante, a Transformada de Laplace estende essa representação para o plano complexo $s$, permitindo tratar simultaneamente oscilações e comportamentos exponenciais de atenuação ou crescimento.

A variável complexa $s = \sigma + j\omega$ combina a taxa de atenuação/crescimento exponencial $\sigma$ com a frequência angular de oscilação $\omega$. Essa generalização converte equações diferenciais lineares que governam sistemas físicos contínuos em equações algébricas no domínio-$s$, simplificando a obtenção das respostas temporais, bem como a avaliação da estabilidade por meio da localização dos polos do sistema no plano-$s$.

---

## Formulação Matemática
A Transformada de Laplace bilateral de um sinal de tempo contínuo $x(t)$ é definida por:
$$X(s) = \mathcal{L}\{x(t)\} = \int_{-\infty}^{+\infty} x(t) e^{-st} \, dt, \quad s = \sigma + j\omega$$

Onde:
- $\sigma = \text{Re}\{s\}$ é a parte real, responsável pela taxa de amortecimento ou crescimento exponencial (medida em $\text{Np/s}$ ou $\text{rad/s}$).
- $\omega = \text{Im}\{s\}$ é a parte imaginária, representando a frequência angular de oscilação (medida em $\text{rad/s}$).
- $e^{-st} = e^{-\sigma t} e^{-j\omega t}$ decompõe o núcleo da transformada em um fator de atenuação real $e^{-\sigma t}$ e uma rotação fasorial complexa $e^{-j\omega t}$.

### Transformadas de Sinais Elementares e Região de Convergência (ROC)
1. **Sinal Exponencial Decrescente**:
   $$x_1(t) = e^{-at} u(t) \stackrel{\mathcal{L}}{\longleftrightarrow} X_1(s) = \frac{1}{s + a}, \quad \text{ROC: } \text{Re}\{s\} > -a$$
2. **Sinal Senoidal Amortecido**:
   $$x_2(t) = e^{-at} \cos(\omega_0 t) u(t) \stackrel{\mathcal{L}}{\longleftrightarrow} X_2(s) = \frac{s + a}{(s + a)^2 + \omega_0^2}, \quad \text{ROC: } \text{Re}\{s\} > -a$$

### Relação com a Estabilidade no Plano-$s$
Para um sistema contínuo causal, a resposta ao impulso $h(t)$ é formada por termos exponenciais da forma $e^{p_k t}$, em que $p_k = \sigma_k + j\omega_k$ representa a posição dos polos no plano-$s$. O critério de estabilidade assintótica exige que todos os polos estejam localizados no semiplano esquerdo do plano-$s$:
$$\text{Re}\{p_k\} = \sigma_k < 0, \quad \forall k$$

---

## Investigação e Simulação Computacional

A simulação computacional foi desenvolvida em Python no ambiente Jupyter Notebook para analisar as respostas temporais associadas às transformadas de Laplace elementares e mapear a influência da posição dos polos no plano-$s$ sobre a dinâmica do sistema.

:::: {include} ./simulacao/simulacao_transformada_laplace.ipynb
::::

No experimento, simularam-se inicialmente dois exemplos analíticos clássicos: a exponencial decrescente $x_1(t) = e^{-2t}u(t)$ e a senoide amortecida $x_2(t) = e^{-t}\cos(4\pi t)u(t)$ (`grafico_exemplos_laplace.png`). Em seguida, avaliaram-se 4 cenários característicos de posicionamento de polos no plano-$s$ ($\sigma < 0$, $\sigma = 0$ e $\sigma > 0$), gerando a comparação visual exportada para `grafico_laplace_plano_s.png`.

---

### Discussão dos Resultados

#### 1. Análise dos Exemplos Analíticos
- **Exponencial Decrescente ($\sigma = -2, \omega = 0$)**: O polo único em $s = -2$ produz uma resposta puramente real $x_1(t) = e^{-2t} u(t)$. Como $\text{Im}\{s\} = 0$, não há oscilação temporal. A velocidade de decaimento é ditada pela constante de tempo $\tau = 1/|\sigma| = 0,5\text{ s}$, ponto em que a amplitude decai para aproximadamente $36,8\%$ do valor inicial.
- **Senoide Amortecida ($\sigma = -1, \omega = 4\pi\text{ rad/s}$)**: O par de polos complexos conjugados em $s = -1 \pm j4\pi$ introduz uma oscilação com frequência $f_0 = \frac{4\pi}{2\pi} = 2\text{ Hz}$ sob um envelope de amortecimento exponencial com constante de tempo $\tau = 1,0\text{ s}$.

#### 2. Efeito da Posição dos Polos no Plano-$s$ sobre a Dinâmica Temporal
- **Semieixo Real Negativo ($\sigma = -2, \omega = 0$)**: Decaimento exponencial sem oscilações, caracterizando uma resposta superamortecida e estritamente estável.
- **Semiplano Esquerdo Complexo ($\sigma = -0,5, \omega = 4\pi$)**: Oscilação amortecida que converge para zero quando $t \rightarrow \infty$, representando um sistema subamortecido estável.
- **Eixo Imaginário ($j\omega$) ($\sigma = 0, \omega = 4\pi$)**: Oscilação de amplitude constante mantida indefinidamente, caracterizando o limite de estabilidade (sistema marginalmente estável).
- **Semiplano Direito Complexo ($\sigma = +0,5, \omega = 4\pi$)**: O envelope da oscilação cresce exponencialmente sem limite ($e^{+0,5t} \rightarrow \infty$), caracterizando um sistema instável. Esse comportamento confirma que a condição $\text{Re}\{s\} < 0$ é indispensável para a estabilidade de sistemas contínuos.