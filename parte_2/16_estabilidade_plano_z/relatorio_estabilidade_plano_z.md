---
title: Tópico 16 - Estabilidade no Plano-z
---

# Tópico 16 - Estabilidade no Plano-z

---

## Resumo Conceitual
No processamento digital de sinais e na teoria de controle discreto, o estudo da **Estabilidade no Plano-z** busca determinar se a resposta transiente de um sistema discreto Linear e Invariante no Tempo (LTI) permanece limitada ao longo do tempo diante de entradas limitadas. Para um sistema causal, a estabilidade BIBO (*Bounded-Input Bounded-Output*) e a estabilidade assintótica são ditadas de forma única e exclusiva pela localização dos **polos** de sua função de transferência $H(z)$ no plano complexo $z$.

A fronteira fundamental de estabilidade no domínio discreto é o **círculo unitário** ($|z| = 1$), que desempenha o papel análogo ao eixo imaginário $j\omega$ no plano-$s$ de sistemas contínuos. Para que um sistema discreto causal seja BIBO estável, a Região de Convergência (ROC) de sua função de transferência deve conter o círculo unitário, o que implica que **todos os seus polos devem estar estritamente localizados dentro do círculo unitário** ($|p_k| < 1$).

---

## Formulação Matemática
Considere a função de transferência racional de um sistema LTI discreto e causal:
$$H(z) = \frac{Y(z)}{X(z)} = \frac{\sum_{m=0}^{M} b_m z^{-m}}{1 + \sum_{k=1}^{N} a_k z^{-k}} = \frac{B(z)}{A(z)}$$

Fatorando o denominador $A(z)$ em termos de seus $N$ polos $p_k$:
$$H(z) = G \cdot \frac{\prod_{m=1}^{M} (z - z_m)}{\prod_{k=1}^{N} (z - p_k)}$$

Expandindo em frações parciais, a resposta ao impulso $h[n]$ no tempo discreto é dada pela combinação linear das respostas modais dos polos:
$$h[n] = \sum_{k=1}^{N} A_k (p_k)^n u[n]$$

Escrevendo cada polo $p_k$ em sua forma polar $p_k = r_k e^{j\omega_k}$, onde $r_k = |p_k|$ é o raio/módulo e $\omega_k = \angle p_k$ é a frequência angular discreta:
$$(p_k)^n = (r_k e^{j\omega_k})^n = r_k^n e^{j\omega_k n} = r_k^n \left(\cos(\omega_k n) + j \sin(\omega_k n)\right)$$

### Regimes de Estabilidade no Plano-$z$:
1. **Sistema Assintoticamente Estável ($|p_k| = r_k < 1, \forall k$)**:
   - Como $r_k < 1$, o termo $r_k^n \rightarrow 0$ quando $n \rightarrow \infty$.
   - A resposta ao impulso atenoa-se assintoticamente até zero, garantindo a estabilidade BIBO.
2. **Sistema Marginalmente Estável ($|p_k| = r_k = 1$ para polos simples sobre o círculo unitário)**:
   - Como $r_k = 1$, o termo $r_k^n = 1^n = 1$.
   - A resposta ao impulso mantêm oscilações senoidais de amplitude constante $\cos(\omega_k n)$ sem atenuar nem divergir.
3. **Sistema Instável ($|p_k| = r_k > 1$ para ao menos um polo)**:
   - Como $r_k > 1$, o envelope exponencial cresce de forma ilimitada ($r_k^n \rightarrow \infty$ quando $n \rightarrow \infty$).
   - A resposta ao impulso explode numericamente, caracterizando a instabilidade.

---

## Investigação e Simulação Computacional

A simulação computacional foi desenvolvida em Python no ambiente Jupyter Notebook para analisar quatro configurações dinâmicas de polos no plano-$z$ e suas respectivas respostas temporais ao impulso $h[n]$.

:::: {include} ./simulacao/simulacao_estabilidade_plano_z.ipynb
::::

No experimento, simularam-se no domínio do tempo a resposta $h[n]$ para 25 amostras (`grafico_estabilidade_plano_z_tempo.png`) e plotaram-se os diagramas de polos e zeros com destaque para o círculo unitário (`grafico_estabilidade_plano_z_mapa.png`).

---

### Discussão dos Resultados

#### 1. Análise dos Quatro Cenários Simulados
- **Cenário 1: Polo Real Interno ($p = 0,6, |p| = 0,6 < 1$)**: O polo situa-se sobre o eixo real positivo no interior do círculo unitário. A resposta ao impulso exibe decaimento geométrico monotônico suave ($0,6^n$), tornando-se desprezível após a amostra $n = 10$.
- **Cenário 2: Polos Complexos Conjugados Internos ($|p| = 0,8 < 1, \omega = \pi/4$)**: Os polos estão dentro do círculo unitário. A resposta temporal apresenta oscilações senoidais amortecidas envelopeadas por $0,8^n$, convergindo para zero à medida que $n \to \infty$.
- **Cenário 3: Polos Complexos Conjugados sobre o Círculo Unitário ($|p| = 1,0, \omega = \pi/4$)**: Os polos estão posicionados exatamente sobre a fronteira $|z| = 1$. A resposta ao impulso é uma senoide pura mantida de amplitude $1,0$ com período de 8 amostras por ciclo, caracterizando a estabilidade marginal.
- **Cenário 4: Polos Complexos Conjugados Externos ($|p| = 1,1 > 1, \omega = \pi/4$)**: Os polos encontram-se fora do círculo unitário. O envelope das oscilações cresce exponencialmente a uma taxa de $1,1^n$, ultrapassando amplitude $10,0$ em poucas amostras e confirmando a instabilidade.

#### 2. Importância Prática no Projeto de Filtros Digitais
- No projeto de **Filtros IIR (Infinite Impulse Response)**, a garantia de que todos os polos fiquem estritamente dentro do círculo unitário é um requisito de projeto indispensável. Efeitos de quantização de coeficientes em hardware de ponto fixo podem deslocar polos próximos de $|z| = 1$ para fora do círculo unitário, tornando um filtro teoricamente estável em um sistema instável na prática.
- Em **Filtros FIR (Finite Impulse Response)**, como não há realimentação, todos os polos situam-se na origem $z = 0$, o que garante estabilidade incondicional.