---
title: Tópico 14 - Região de Convergência
---

# Tópico 14 - Região de Convergência

---

## Resumo Conceitual
No estudo de sinais e sistemas em tempo discreto, a **Transformada-Z** não é caracterizada de forma única apenas por sua expressão algébrica racional $X(z)$. Duas sequências temporais completamente distintas — por exemplo, um sinal causal que se estende para o futuro ($n \ge 0$) e um sinal anti-causal que se estende para o passado ($n < 0$) — podem compartilhar exatamente a mesma fórmula no domínio-$z$.

A **Região de Convergência (ROC - *Region of Convergence*)** é definida como o conjunto de pontos no plano complexo $z$ para os quais o somatório da Transformada-Z converge absolutamente. A especificação da ROC é estritamente necessária para determinar de forma unívoca o sinal temporal original através da Transformada-Z Inversa, além de fornecer os critérios fundamentais para avaliar as propriedades de **causalidade** e **estabilidade BIBO** do sistema.

---

## Formulação Matemática
A Transformada-Z bilateral de uma sequência discreta $x[n]$ é dada por:
$$X(z) = \sum_{n=-\infty}^{+\infty} x[n] z^{-n}, \quad z \in \text{ROC}$$

### 1. Comparação entre Sinal Causal e Anti-causal

Considere a expressão algébrica idêntica $X(z) = \frac{1}{1 - a z^{-1}} = \frac{z}{z - a}$:

- **Sequência Causal**:
  $$x_1[n] = a^n u[n] \stackrel{\mathcal{Z}}{\longleftrightarrow} X_1(z) = \frac{z}{z - a}, \quad \text{ROC}_1: |z| > |a|$$
  O somatório converge apenas se $|a z^{-1}| < 1 \implies |z| > |a|$. A ROC é a região exterior ao círculo de raio $|a|$.

- **Sequência Anti-causal**:
  $$x_2[n] = -a^n u[-n-1] \stackrel{\mathcal{Z}}{\longleftrightarrow} X_2(z) = \frac{z}{z - a}, \quad \text{ROC}_2: |z| < |a|$$
  O somatório converge apenas se $|a^{-1} z| < 1 \implies |z| < |a|$. A ROC é a região interior ao círculo de raio $|a|$.

### 2. Propriedades Fundamentais da ROC
1. **Forma Geométrica**: A ROC é composta por anéis ou discos centrados na origem do plano-$z$.
2. **Ausência de Polos**: A ROC **não contém nenhum polo** do sistema, pois nos polos a transformada diverge ($|X(z)| \rightarrow \infty$).
3. **Critério de Causalidade**: Um sistema LTI discreto é causal se, e somente se, sua ROC se estende para fora do polo de maior módulo.
4. **Critério de Estabilidade BIBO**: Um sistema LTI discreto é estável BIBO se, e somente se, a ROC **inclui o círculo unitário** ($|z| = 1$).

No experimento realizado com $a = 0,8$:
- **Caso Causal ($x_1[n] = 0,8^n u[n]$)**: $\text{ROC}_1: |z| > 0,8$. Como $|z| = 1 > 0,8$, o círculo unitário está contido na ROC $\implies$ **Sistema Causal e Estável**.
- **Caso Anti-causal ($x_2[n] = -0,8^n u[-n-1]$)**: $\text{ROC}_2: |z| < 0,8$. Como $|z| = 1 \notin \text{ROC}_2$, o círculo unitário não pertence à ROC $\implies$ **Sistema Anti-causal e Instável**.

---

## Investigação e Simulação Computacional

A simulação computacional foi desenvolvida em Python no ambiente Jupyter Notebook para analisar o comportamento temporal de sequências causais e anti-causais e mapear graficamente suas respectivas ROCs no plano-$z$.

:::: {include} ./simulacao/simulacao_regiao_convergencia.ipynb
::::

No experimento, simularam-se no domínio do tempo o sinal causal $x_1[n] = 0,8^n u[n]$ e o sinal anti-causal $x_2[n] = -0,8^n u[-n-1]$ (`grafico_roc_tempo.png`). Em seguida, geraram-se os mapas de polos e zeros destacando as hachuras das áreas de convergência no plano complexo $z$ (`grafico_roc_plano_z.png`).

---

### Discussão dos Resultados

#### 1. Unicidade da Transformada-Z no Domínio do Tempo
A simulação temporal comprova que $x_1[n]$ e $x_2[n]$ são sinais completamente distintos:
- $x_1[n]$ é nulo para índices negativos ($n < 0$) e decai exponencialmente para zero à medida que $n \to +\infty$.
- $x_2[n]$ é nulo para índices não-negativos ($n \ge 0$) e cresce em magnitude à medida que $n \to -\infty$.

Como ambos possuem a mesma expressão algébrica $X(z) = \frac{z}{z - 0,8}$, fica demonstrado que a expressão algébrica isoladamente é insuficiente para identificar o sinal temporal, tornando a definição explícita da ROC indispensável.

#### 2. Análise da ROC e Relação com Estabilidade
- **Análise da ROC Causal ($|z| > 0,8$)**: A região sombreada cobre todo o plano-$z$ externo ao círculo de raio $r = 0,8$. Como o círculo unitário ($|z| = 1$) está totalmente imerso na região azul sombreada, a Transformada de Fourier em Tempo Discreto (DTFT) existe e a sequência temporal é BIBO estável.
- **Análise da ROC Anti-causal ($|z| < 0,8$)**: A região sombreada restringe-se ao interior do disco de raio $r = 0,8$. Como o círculo unitário fica de fora dessa região, a sequência diverge e não admite avaliação de estabilidade estável em regime permanente.