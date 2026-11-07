---
title: Tópico 13 - Transformada-Z
---

# Tópico 13 - Transformada-Z

---

## Resumo Conceitual
A **Transformada-Z** é a ferramenta matemática fundamental para a análise, síntese e representação de sinais e sistemas discretos no tempo e invariantes no tempo (LTI discretos). Ela desempenha no domínio discreto o mesmo papel que a Transformada de Laplace exerce no domínio contínuo, convertendo equações de diferenças finitas em equações algébricas simples operadas no plano complexo $z$.

A variável complexa $z = r e^{j\omega}$ associa a taxa de crescimento ou decaimento geométrico por amostra ($r = |z|$) com a frequência angular discreta ($\omega = \angle z$, medida em radianos por amostra). O **círculo unitário** ($|z| = 1$) constitui a fronteira crítica no plano-$z$: a avaliação da Transformada-Z ao longo do círculo unitário equivale à Transformada de Fourier em Tempo Discreto (DTFT). A posição dos polos e zeros em relação ao círculo unitário determina de forma direta a estabilidade assintótica e a dinâmica transiente do sistema discreto.

---

## Formulação Matemática
A Transformada-Z bilateral de uma sequência discreta $x[n]$ é definida por:
$$X(z) = \mathcal{Z}\{x[n]\} = \sum_{n=-\infty}^{+\infty} x[n] z^{-n}, \quad z = r e^{j\omega}$$

Onde a Região de Convergência (ROC - *Region of Convergence*) define o conjunto de valores de $z$ para os quais o somatório converge absolutamente:
$$\text{ROC} = \left\{ z \in \mathbb{C} \;\Big|\; \sum_{n=-\infty}^{+\infty} |x[n]| |z|^{-n} < \infty \right\}$$

### Transformadas de Sequências Elementares Causais
1. **Impulso Unitário**:
   $$x_1[n] = \delta[n] \stackrel{\mathcal{Z}}{\longleftrightarrow} X_1(z) = 1, \quad \text{ROC: Todo o plano } z$$
2. **Degrau Unitário Causal**:
   $$x_2[n] = u[n] \stackrel{\mathcal{Z}}{\longleftrightarrow} X_2(z) = \frac{1}{1 - z^{-1}} = \frac{z}{z - 1}, \quad \text{ROC: } |z| > 1$$
3. **Sequência Exponencial Real Causal**:
   $$x_3[n] = a^n u[n] \stackrel{\mathcal{Z}}{\longleftrightarrow} X_3(z) = \frac{1}{1 - a z^{-1}} = \frac{z}{z - a}, \quad \text{ROC: } |z| > |a|$$

### Critério de Estabilidade BIBO Causal no Plano-$z$
Para um sistema LTI discreto e causal com função de transferência $H(z)$, a estabilidade BIBO (*Bounded-Input Bounded-Output*) exige que a Região de Convergência inclua o círculo unitário ($|z| = 1$). Consequentemente, todos os polos $p_k$ do sistema devem estar localizados estritamente dentro do círculo unitário:
$$|p_k| < 1, \quad \forall k$$

---

## Investigação e Simulação Computacional

A simulação computacional foi desenvolvida em Python no ambiente Jupyter Notebook para analisar sequências exponenciais discretas $x[n] = a^n u[n]$ sob diferentes valores da base $a$ e mapear o comportamento de polos e zeros em relação ao círculo unitário no plano-$z$.

:::: {include} ./simulacao/simulacao_transformada_z.ipynb
::::

No experimento, simularam-se sequências causais com $a \in \{0.5, 0.9, -0.8, 1.1\}$, exportando os gráficos temporais para `grafico_transformada_z_sequencias.png`. Em seguida, foram gerados os diagramas de polos e zeros no plano-$z$ para quatro configurações de estabilidade e oscilação, salvos em `grafico_transformada_z_plano_z.png`.

---

### Discussão dos Resultados

#### 1. Análise das Sequências Exponenciais Discretas ($a^n u[n]$)
- **Base Real Positiva Menor que 1 ($a = 0,5$ e $a = 0,9$)**: O polo localiza-se em $z = a$ no eixo real positivo, dentro do círculo unitário. O sinal exibe decaimento geométrico puramente monotônico ($x[n] \rightarrow 0$). Quanto mais próximo de $1$ estiver o polo ($a = 0,9$), mais lenta é a taxa de atenuação.
- **Base Real Negativa ($a = -0,8$)**: O polo posiciona-se no eixo real negativo ($z = -0,8$). A sequência exibe uma oscilação sinal-alternada de período 2 amostras ($(-1)^n$), amortecida progressivamente em amplitude por $|a|^n = 0,8^n$.
- **Base Maior que 1 ($a = 1,1$)**: O polo fica fora do círculo unitário ($z = 1,1 > 1$). A resposta cresce de forma divergente ilimitada ($1,1^n \rightarrow \infty$), caracterizando instabilidade.

#### 2. Mapeamento de Polos no Plano Complexo $z$ e Círculo Unitário
- **Polos Internos ($|p_k| < 1$)**: A resposta transiente do sistema decai assintoticamente para zero quando $n \to \infty$, garantindo a estabilidade assintótica.
- **Polos sobre o Círculo Unitário ($|p_k| = 1$)**: Polos complexos conjugados em $z = e^{\pm j\pi/4}$ geram uma oscilação senoidal contínua discreta na frequência $\omega = \pi/4\text{ rad/amostra}$ sem variação de amplitude (estabilidade marginal).
- **Polos Externos ($|p_k| > 1$)**: Polos situados fora do círculo unitário geram respostas discretas ilimitadas, confirmando que a fronteira $|z| = 1$ no plano-$z$ desempenha o mesmo papel do eixo imaginário $j\omega$ no plano-$s$.