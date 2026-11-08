---
title: Tópico 17 - Relação entre Transformada-Z e DTFT
---

# Tópico 17 - Relação entre Transformada-Z e DTFT

---

## Resumo Conceitual
A **Transformada de Fourier em Tempo Discreto (DTFT)** e a **Transformada-Z** são duas das representações em frequência e no plano complexo mais importantes no Processamento Digital de Sinais. A relação entre elas é direta e profunda: a DTFT de uma sequência discreta $x[n]$ corresponde exatamente à avaliação de sua Transformada-Z $X(z)$ ao longo da trajetória do **círculo unitário** ($|z| = 1$) no plano complexo $z = r e^{j\omega}$.

Essa equivalência matemática exige estritamente que o círculo unitário esteja contido na **Região de Convergência (ROC)** da Transformada-Z. Sob essa condição, a posição geométrica das raízes da função de transferência — polos ($p_k$) e zeros ($z_m$) — determina de forma visual e intuitiva a resposta em frequência em magnitude $|H(e^{j\omega})|$ e em fase $\angle H(e^{j\omega})$:
- A aproximação de $z = e^{j\omega}$ a um **polo** encurta o vetor no denominador, provocando um **pico de ressonância ou amplificação**.
- A aproximação de $z = e^{j\omega}$ a um **zero** encurta o vetor no numerador, provocando um **vale de atenuação ou nulo (*notch*)**.

---

## Formulação Matemática

### 1. Mapeamento no Círculo Unitário
Substituindo a variável complexa em sua forma polar $z = r e^{j\omega}$ com raio unitário $r = 1$:
$$z = 1 \cdot e^{j\omega} = e^{j\omega}$$

A Transformada-Z bilateral reduz-se à DTFT:
$$X(e^{j\omega}) = \left. X(z) \right|_{z = e^{j\omega}} = \sum_{n=-\infty}^{+\infty} x[n] e^{-j\omega n}, \quad \text{se } |z| = 1 \in \text{ROC}$$

### 2. Interpretação Geométrica por Vetores no Plano-$z$
Expressando uma função de transferência racional $H(z)$ na forma fatorada:
$$H(z) = G \cdot \frac{\prod_{m=1}^{M} (z - z_m)}{\prod_{k=1}^{N} (z - p_k)}$$

A resposta em frequência $H(e^{j\omega})$ é calculada avaliando-se os vetores com origem nos zeros $z_m$ e polos $p_k$ e extremidades sobre o círculo unitário no ponto $z = e^{j\omega}$:

- **Magnitude Linear**:
  $$|H(e^{j\omega})| = |G| \cdot \frac{\prod_{m=1}^{M} |e^{j\omega} - z_m|}{\prod_{k=1}^{N} |e^{j\omega} - p_k|} = |G| \cdot \frac{\prod \text{Distância aos Zeros}}{\prod \text{Distância aos Polos}}$$

- **Fase**:
  $$\angle H(e^{j\omega}) = \arg(G) + \sum_{m=1}^{M} \angle(e^{j\omega} - z_m) - \sum_{k=1}^{N} \angle(e^{j\omega} - p_k)$$

### 3. Sistema Analisado no Experimento
Considerou-se um sistema discreto com:
- Polos em $p_{1,2} = 0,85 e^{\pm j \pi/4}$ (frequência $\omega = \pi/4 \approx 0,785\text{ rad/amostra}$).
- Zeros em $z_{1,2} = 0,90 e^{\pm j 3\pi/4}$ (frequência $\omega = 3\pi/4 \approx 2,356\text{ rad/amostra}$).

A função de transferência resultante é:
$$H(z) = \frac{z^2 - 2(0,90)\cos(3\pi/4)z + 0,90^2}{z^2 - 2(0,85)\cos(\pi/4)z + 0,85^2}$$

---

## Investigação e Simulação Computacional

A simulação computacional foi desenvolvida em Python no ambiente Jupyter Notebook para avaliar numericamente a resposta em frequência ao longo do círculo unitário e relacioná-la visualmente com a distribuição geométrica de polos e zeros.

:::: {include} ./simulacao/simulacao_relacao_transformada_z_dtft.ipynb
::::

No experimento, geraram-se os gráficos de magnitude em dB e fase da DTFT (`grafico_relacao_z_dtft_frequencia.png`), correlacionando-os com o mapa no plano complexo $z$ (`grafico_relacao_z_dtft_plano_z.png`).

---

### Discussão dos Resultados

#### 1. Efeito dos Polos na Resposta em Frequência
No ponto do círculo unitário correspondente à frequência do polo ($\omega = \pi/4 \approx 0,785\text{ rad/amostra}$), a distância até o polo $p_1 = 0,85 e^{j\pi/4}$ atinge seu valor mínimo ($d_{\min} = 1 - 0,85 = 0,15$). Como essa distância aparece no denominador do módulo $|H(e^{j\omega})|$, a curva de magnitude exibe um **pico acentuado de ressonância** de aproximadamente $+12\text{ dB}$.

#### 2. Efeito dos Zeros na Resposta em Frequência
No ponto correspondente à frequência do zero ($\omega = 3\pi/4 \approx 2,356\text{ rad/amostra}$), a distância até o zero $z_1 = 0,90 e^{j 3\pi/4}$ atinge seu valor mínimo ($d_{\min} = 1 - 0,90 = 0,10$). Como essa distância está no numerador da magnitude, produz-se um **vale pronunciado de atenuação (*notch*)** que reduz a magnitude para abaixo de $-18\text{ dB}$.

#### 3. Conclusão sobre a Avaliação Espectral Geométrica
A avaliação geométrica sobre o círculo unitário confirma que a DTFT é um caso particular da Transformada-Z restringida ao contorno $|z| = 1$. Esse princípio permite que engenheiros projetem filtros digitais com especificações espectrais desejadas simplesmente posicionando polos próximos às frequências de passagem e zeros próximos às frequências de rejeição.