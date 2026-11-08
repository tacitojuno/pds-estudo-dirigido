---
title: Tópico 15 - Função de Transferência Discreta
---

# Tópico 15 - Função de Transferência Discreta

---

## Resumo Conceitual
A **Função de Transferência Discreta** $H(z)$ representa a relação entre a Transformada-Z da sequência de saída $Y(z)$ e a Transformada-Z da sequência de entrada $X(z)$ de um sistema discreto Linear e Invariante no Tempo (LTI), sob a premissa de condições iniciais nulas. Ela constitui o equivalente discreto da função de transferência contínua $H(s)$, permitindo mapear equações de diferenças finitas — que governam os algoritmos de filtros digitais e controladores discretos — em expressões algébricas racionais operadas no plano complexo $z$.

A partir da função de transferência $H(z)$, determina-se a **resposta ao impulso** do sistema $h[n] = \mathcal{Z}^{-1}\{H(z)\}$, bem como a localização de seus **polos** (raízes do denominador) e **zeros** (raízes do numerador). A posição dos polos em relação ao **círculo unitário** ($|z| = 1$) determina a velocidade de decaimento, a presença de oscilações sinal-alternadas ou a divergência da resposta transiente temporal.

---

## Formulação Matemática
Considere um sistema LTI discreto e causal cuja relação entrada-saída é descrita por uma equação de diferenças autorregressiva de 1ª ordem:
$$y[n] - a y[n-1] = x[n]$$

Aplicando a Transformada-Z em ambos os lados, com condições iniciais nulas:
$$Y(z) - a z^{-1} Y(z) = X(z)$$

Fatorando $Y(z)$:
$$Y(z) \left(1 - a z^{-1}\right) = X(z)$$

Obtém-se a **Função de Transferência Discreta $H(z)$**:
$$H(z) = \frac{Y(z)}{X(z)} = \frac{1}{1 - a z^{-1}} = \frac{z}{z - a}$$

### 1. Localização de Polo e Resposta ao Impulso $h[n]$
O sistema possui um **único polo localizado em $z = a$** e um zero na origem $z = 0$.
A resposta ao impulso analítica $h[n]$ corresponde à Transformada-Z Inversa de $H(z)$:
$$h[n] = \mathcal{Z}^{-1}\left\{\frac{1}{1 - a z^{-1}}\right\} = a^n u[n]$$

### 2. Análise dos Regimes de Operação segundo o Valor do Polo $a$
1. **$0 < a < 1$ (Polo Real Positivo Interno)**:
   - $h[n] = a^n u[n]$ exibe **decaimento geométrico monotônico** ($h[n] \rightarrow 0$ quando $n \rightarrow \infty$).
   - Quanto maior o valor de $a$ (ex.: $a = 0,9$ vs $a = 0,5$), mais lenta é a taxa de atenuação (maior constante de tempo discreta).
2. **$-1 < a < 0$ (Polo Real Negativo Interno)**:
   - $h[n] = (-|a|)^n u[n]$ exibe **decaimento amortecido com oscilação sinal-alternada** a cada amostra ($(-1)^n$).
3. **$|a| > 1$ (Polo Externo ao Círculo Unitário)**:
   - $h[n] = a^n u[n]$ cresce de forma **exponencial divergente** ($h[n] \rightarrow \infty$), caracterizando **instabilidade BIBO**.

---

## Investigação e Simulação Computacional

A simulação computacional foi desenvolvida em Python no ambiente Jupyter Notebook para analisar numericamente a resposta ao impulso $h[n]$ e o mapa de polos e zeros para os valores $a \in \{0.5, 0.9, -0.8, 1.1\}$.

:::: {include} ./simulacao/simulacao_funcao_transferencia_discreta.ipynb
::::

No experimento, simularam-se as respostas $h[n]$ ao longo de $20$ amostras temporais (`grafico_funcao_transferencia_discreta_tempo.png`) e plotaram-se os polos no plano complexo $z$ em relação ao círculo unitário (`grafico_funcao_transferencia_discreta_plano_z.png`).

---

### Discussão dos Resultados

#### 1. Análise Temporal das Respostas ao Impulso
- **Caso $a = 0,5$**: O sinal decai rapidamente, reduzindo-se a menos de $1\%$ da amplitude inicial na amostra $n = 7$.
- **Caso $a = 0,9$**: O polo próximo ao círculo unitário mantém a resposta persistente por mais tempo, necessitando de mais de $40$ amostras para amortecer completamente.
- **Caso $a = -0,8$**: A resposta alterna o sinal a cada amostra ($+1,0; -0,8; +0,64; -0,512; \dots$), atenuando-se em módulo graças a $|-0,8| = 0,8 < 1$.
- **Caso $a = 1,1$**: As amostras crescem ilimitadamente ($1,0; 1,1; 1,21; 1,33; \dots; 6,73$), ultrapassando limites físicos e numéricos.

#### 2. Mapeamento de Polos e Critério de Estabilidade
- Para os casos $a = 0,5$, $a = 0,9$ e $a = -0,8$, os polos situam-se estritamente **dentro do círculo unitário** ($|z| < 1$). A Região de Convergência Causal $|z| > |a|$ engloba o círculo unitário ($|z| = 1$), garantindo a estabilidade assintótica.
- Para $a = 1,1$, o polo situa-se **fora do círculo unitário** ($|z| = 1,1 > 1$). A ROC causal $|z| > 1,1$ exclui o círculo unitário, o que confirma formalmente a instabilidade BIBO do sistema.