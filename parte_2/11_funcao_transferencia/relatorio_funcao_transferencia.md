---
title: Tópico 11 - Função de Transferência
---

# Tópico 11 - Função de Transferência

---

## Resumo Conceitual
A **Função de Transferência** $H(s)$ é um conceito central no estudo de sistemas contínuos e invariantes no tempo (LTI - *Linear Time-Invariant*), sendo amplamente utilizada nas áreas de Processamento Digital de Sinais, Teoria de Controle e Automação. Ela expressa a relação matemática entre a transformada de Laplace da saída $Y(s)$ e a transformada de Laplace da entrada $X(s)$, assumindo que todas as condições iniciais do sistema sejam nulas.

A principal vantagem da representação por função de transferência é transformar equações diferenciais lineares no domínio do tempo em equações algébricas no domínio-$s$. A partir da forma racional de $H(s)$, é possível extrair diretamente os **polos** (raízes do denominador) e os **zeros** (raízes do numerador), os quais determinam de forma única a estabilidade assintótica, a velocidade de resposta temporal e as características de amortecimento do sistema.

---

## Formulação Matemática
Para um sistema LTI contínuo descrito por uma equação diferencial linear de ordem $N$, a função de transferência é definida por:
$$H(s) = \frac{Y(s)}{X(s)} \Bigg|_{\text{condições iniciais nulas}}$$

Na forma fatorada por polos e zeros, uma função de transferência racional genérica é escrita como:
$$H(s) = K \frac{(s - z_1)(s - z_2) \cdots (s - z_M)}{(s - p_1)(s - p_2) \cdots (s - p_N)}$$

Onde $K$ é o ganho do sistema, $z_k$ representam os zeros e $p_k$ representam os polos.

### 1. Sistema de Primeira Ordem
A representação canônica de um sistema de primeira ordem é dada por:
$$H(s) = \frac{K}{\tau s + 1}$$

- **Constante de Tempo ($\tau$)**: Tempo necessário para a resposta ao degrau atingir cerca de $63,2\%$ do seu valor final em regime permanente ($\tau > 0$ para estabilidade). O tempo de acomodação ($2\%$ de tolerância) é $t_s \approx 4\tau$.
- **Ganho Estático ($K$)**: Razão de amplitude entre a saída em regime permanente e a entrada constante ($y(\infty) = K$ para entrada degrau unitário).

### 2. Sistema de Segunda Ordem
A forma canônica de um sistema de segunda ordem é dada por:
$$H(s) = \frac{\omega_n^2}{s^2 + 2\zeta\omega_n s + \omega_n^2}$$

- **Frequência Natural não Amortecida ($\omega_n$)**: Mede a velocidade de oscilação própria do sistema (em $\text{rad/s}$).
- **Fator de Amortecimento ($\zeta$)**: Determina o regime de amortecimento do sistema:
  - $\zeta > 1$: **Superamortecido** (dois polos reais e distintos no semiplano esquerdo; sem sobressinal).
  - $\zeta = 1$: **Criticamente Amortecido** (polos reais repetidos; resposta mais rápida sem sobressinal).
  - $0 < \zeta < 1$: **Subamortecido** (par de polos complexos conjugados $p_{1,2} = -\zeta\omega_n \pm j\omega_n\sqrt{1-\zeta^2}$; apresenta sobressinal e oscilação transiente).
  - $\zeta = 0$: **Não Amortecido** (polos puramente imaginários $\pm j\omega_n$; oscilação senoidal pura mantida).

---

## Investigação e Simulação Computacional

A simulação computacional foi desenvolvida em Python no ambiente Jupyter Notebook para analisar numericamente a resposta ao degrau unitário de sistemas de primeira e segunda ordem sob variações nos parâmetros $\tau$, $K$, $\zeta$ e $\omega_n$.

:::: {include} ./simulacao/simulacao_funcao_transferencia.ipynb
::::

No experimento, simularam-se inicialmente sistemas de 1ª ordem alterando a constante de tempo ($\tau \in \{0.5, 1.0, 2.0\}\text{ s}$) e o ganho estático ($K \in \{0.5, 1.0, 2.0\}$), cujos gráficos temporais foram salvos em `grafico_primeira_ordem.png`. Em seguida, simularam-se sistemas de 2ª ordem variando o fator de amortecimento ($\zeta \in \{0.1, 0.4, 0.7, 1.0, 1.5\}$) e a frequência natural ($\omega_n \in \{1.0, 2.0, 5.0\}\text{ rad/s}$), com resultados armazenados em `grafico_segunda_ordem.png`.

---

### Discussão dos Resultados

#### 1. Sistemas de Primeira Ordem: Efeito da Constante de Tempo e do Ganho Estático
- **Influência de $\tau$**: O aumento da constante de tempo de $\tau = 0,5\text{ s}$ para $\tau = 2,0\text{ s}$ eleva o tempo de acomodação do sistema de $t_s \approx 2,0\text{ s}$ para $t_s \approx 8,0\text{ s}$. Isso confirma que $\tau$ atua como a inércia do sistema, ditando a taxa de atenuação do polo real localizado em $s = -1/\tau$.
- **Influência de $K$**: O ganho estático $K$ altera diretamente a amplitude final da saída em regime permanente ($y(\infty) = K$), sem afetar a constante de tempo ou a velocidade de transição dinâmica.

#### 2. Sistemas de Segunda Ordem: Fator de Amortecimento e Frequência Natural
- **Influência de $\zeta$**:
  - Para $\zeta = 1,5$ (superamortecido) e $\zeta = 1,0$ (criticamente amortecido), a resposta é monotonicamente crescente e sem qualquer sobressinal.
  - Para $\zeta < 1,0$ (subamortecido), o sistema exibe oscilações transientes. Quanto menor o valor de $\zeta$ (ex.: $\zeta = 0,1$), maior é o valor de pico do sobressinal (*overshoot*), ultrapassando $70\%$ do valor final, e maior é o tempo necessário para as oscilações dissiparem-se.
- **Influência de $\omega_n$**: Aumentar $\omega_n$ de $1,0\text{ rad/s}$ para $5,0\text{ rad/s}$ sob um mesmo fator de amortecimento ($\zeta = 0,3$) comprime o eixo temporal, acelerando o tempo de subida e aumentando a frequência das oscilações transientes de forma inversamente proporcional a $\omega_n$.