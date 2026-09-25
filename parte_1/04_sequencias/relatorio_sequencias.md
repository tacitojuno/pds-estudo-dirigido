---
title: Tópico 4 - Sequências Fundamentais
---

# Tópico 4 - Sequências Fundamentais

---

## Resumo Conceitual

Na análise de sistemas lineares discretos, sinais complexos provenientes de fenômenos físicos podem ser decompostos em representações mais simples. Essas representações baseiam-se em um conjunto de **sequências fundamentais**, que atuam como blocos de construção essenciais na engenharia de sinais. 

Dominar o comportamento do impulso, do degrau, das exponenciais e das senoides é indispensável, pois a resposta de um sistema a esses sinais básicos dita a forma como ele reagirá a qualquer outro sinal arbitrário de entrada.

---

## Formulação Matemática

### 1. Impulso Unitário ($\delta[n]$)
O impulso unitário discreto é a sequência mais elementar, definida por existir apenas na origem:
$$\delta[n] = \begin{cases} 1, & n = 0 \\ 0, & n \ne 0 \end{cases}$$

Sua importância primordial reside na **propriedade da amostragem** (ou propriedade da peneira):
$$x[n] = \sum_{k=-\infty}^{+\infty} x[k]\delta[n-k]$$
Essa expressão significa que qualquer sinal discreto $x[n]$ pode ser representado como uma soma infinita de impulsos deslocados no tempo, onde cada impulso é escalonado pela amplitude da amostra correspondente $x[k]$. Essa é a base matemática para a operação de convolução que define os sistemas LTI.

### 2. Degrau Unitário ($u[n]$)
O degrau unitário modela a ativação de um sistema (o fechar de um interruptor), mantendo o valor unitário a partir da origem:
$$u[n] = \begin{cases} 1, & n \ge 0 \\ 0, & n < 0 \end{cases}$$

Existe uma relação íntima de primeira diferença entre o degrau e o impulso:
$$\delta[n] = u[n] - u[n-1]$$
Isso comprova que o impulso é simplesmente a "derivada discreta" (diferença) do degrau.

### 3. Exponencial Real
Uma sequência exponencial genérica tem a forma:
$$x[n] = a^n$$
O comportamento dinâmico depende estritamente da base $a$. Se multiplicada por um degrau unitário ($a^n u[n]$), a sequência existe apenas para $n \ge 0$. Para a simulação, escolheu-se $|a| < 1$ (especificamente $a=0{,}85$), resultando em uma curva de decaimento assintótico, análoga à descarga de um capacitor em um circuito RC.

### 4. Senoide Discreta e Exponencial Complexa
A senoide discreta é definida pelos parâmetros de amplitude ($A$), frequência angular discreta ($\omega_0$) e fase ($\phi$):
$$x[n] = A \cos(\omega_0 n + \phi)$$

Na matemática avançada de sinais, as senoides estão intimamente ligadas às exponenciais complexas através da **Identidade de Euler**:
$$e^{j\theta} = \cos(\theta) + j \sin(\theta)$$
Substituindo $\theta = \omega n$, obtemos a exponencial complexa discreta:
$$e^{j\omega n} = \cos(\omega n) + j \sin(\omega n)$$
Essa formulação demonstra que qualquer oscilação senoidal real pode ser decomposta em uma soma de exponenciais complexas conjugadas. Essa transição do domínio trigonométrico para o exponencial é o pilar que sustentará o futuro estudo da Transformada de Fourier (Parte 2).

---

## Simulação Computacional

Para validar as equações teóricas, geramos as quatro sequências em um intervalo contíguo de amostras ($n \in [-5, 20]$). A senoide discreta foi modelada com base no projeto elétrico anterior ($f_s = 480\,\text{Hz}$, resultando em $\omega_0 = \pi/4$).

:::: {include} ./simulacao/simulacao_sequencias.ipynb
::::

### Discussão dos Resultados

A disposição gráfica valida integralmente as definições matemáticas estruturadas:
*   O gráfico do **impulso unitário** evidencia o ponto único de energia em $n=0$, garantindo zero absoluto nas demais amostras.
*   O **degrau unitário** demonstra visualmente a sua função de "janela", assumindo a amplitude máxima contínua para $n \ge 0$.
*   A **exponencial** com $a=0{,}85$ apresenta o perfil clássico de dissipação de energia em um sistema físico amortecido, ativada em $n=0$ pela multiplicação implícita com o degrau unitário.
*   A **senoide discreta** reproduz a forma de onda da rede elétrica quantizada, confirmando que a frequência angular $\omega_0 = \pi/4$ encerra exatamente um quarto de ciclo por amostra, necessitando de 8 amostras para completar os $2\pi$ radianos de um período completo.