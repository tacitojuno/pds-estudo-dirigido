# Sinais Contínuos e Discretos

---

# Resumo Conceitual

Na engenharia, sinais são modelos matemáticos que descrevem a evolução de um fenômeno físico ao longo de uma ou mais variáveis independentes. Uma das grandezas mais comuns é a tensão elétrica, que varia em função do tempo. Dependendo da natureza dessa variável temporal, os sinais dividem-se fundamentalmente em **contínuos** e **discretos**.

Um **sinal contínuo no tempo**, denotado por $x(t)$, possui valores definidos para qualquer instante real $t$. Entre dois instantes arbitrários, existe uma infinidade de valores assumidos pelo sinal. A forma de onda fornecida pela rede elétrica analógica, por exemplo, é um sinal perfeitamente contínuo. 

Por outro lado, sistemas digitais como computadores e microcontroladores não conseguem processar informações infinitas. Para que um sinal contínuo seja processado digitalmente, ele precisa ser convertido em um **sinal discreto**, representado por $x[n]$. Nessa forma, o sinal só existe em instantes específicos, espaçados de forma regular, onde $n$ é um número inteiro adimensional correspondente ao índice da amostra.

O processo de extrair essas amostras periódicas do sinal analógico é chamado de **amostragem**. Para que o sinal discreto gerado seja uma representação fiel do sinal físico original e possa ser reconstruído no futuro sem perda de informação, a taxa (ou frequência) de amostragem deve respeitar limites matemáticos fundamentais, especificamente o critério de Nyquist, evitando que altas frequências se disfarcem de baixas frequências no domínio digital (fenômeno conhecido como *aliasing*).

---

# Formulação Matemática

Um sinal analógico genérico dependente do tempo é formalizado como:

$$x(t), \qquad t\in\mathbb{R}$$

Ao realizarmos a amostragem desse sinal em intervalos regulares, definimos o período de amostragem $T_s$ (em segundos). O tempo físico $t$ passa a ser substituído pelo produto do índice $n$ pelo período $T_s$, resultando na sequência discreta:

$$x[n] = x(nT_s), \qquad n\in\mathbb{Z}$$

A frequência de amostragem $f_s$ (em hertz) é inversamente proporcional a esse período:

$$f_s = \frac{1}{T_s}$$

## Sinais Senoidais e Teorema de Nyquist

Para um sinal senoidal de amplitude $A$, frequência $f_0$ e fase inicial nula, a equação contínua é:

$$x(t) = A\cos(2\pi f_0 t)$$

Ao discretizá-lo, substituímos $t = nT_s = n/f_s$:

$$x[n] = A\cos\left(2\pi \frac{f_0}{f_s} n\right)$$

Esta relação evidencia que o sinal discreto depende estritamente da proporção entre a frequência natural da onda ($f_0$) e a velocidade com que o sistema captura as amostras ($f_s$). 

Para garantir que o sinal amostrado não sofra *aliasing*, o **Teorema da Amostragem de Nyquist-Shannon** dita que a frequência de amostragem deve ser estritamente superior ao dobro da maior frequência presente no sinal:

$$f_s > 2f_{\max}$$

---

# Exemplo Analítico: Rede Elétrica

Para demonstrar o processo, analisaremos o sinal de tensão da rede elétrica brasileira, que opera a $60\,\text{Hz}$ com um valor eficaz de $127\,\text{V}_{\text{rms}}$. O sinal será amostrado a $480\,\text{Hz}$.

**Parâmetros:**
* Tensão de pico (Amplitude): $A = 127\sqrt{2} \approx 179{,}6\,\text{V}$
* Frequência do sinal: $f_0 = 60\,\text{Hz}$
* Frequência de amostragem: $f_s = 480\,\text{Hz}$

**Desenvolvimento:**
Primeiro, determinamos o período de amostragem $T_s$:
$$T_s = \frac{1}{480} \approx 0{,}002083\,\text{s} = 2{,}083\,\text{ms}$$

Aplicando o modelo da senoide amostrada, a equação do sinal discreto torna-se:
$$x[n] = 179{,}6\cos\left(2\pi \frac{60}{480} n\right) = 179{,}6\cos\left(\frac{\pi}{4} n\right)$$

Podemos calcular analiticamente as primeiras amostras do sistema:
* $x[0] = 179{,}6 \cos(0) = 179{,}60\,\text{V}$
* $x[1] = 179{,}6 \cos(\pi/4) \approx 127{,}00\,\text{V}$
* $x[2] = 179{,}6 \cos(\pi/2) = 0{,}00\,\text{V}$
* $x[3] = 179{,}6 \cos(3\pi/4) \approx -127{,}00\,\text{V}$
* $x[4] = 179{,}6 \cos(\pi) = -179{,}60\,\text{V}$

**Critério de Nyquist:**
A maior frequência do sinal é $60\,\text{Hz}$. A taxa de Nyquist é $120\,\text{Hz}$. Como nossa frequência de amostragem é $480\,\text{Hz}$, temos que $480 > 120$, confirmando que a aquisição é perfeitamente capaz de representar a onda.

---

:::: {include} ./simulacao/simulacao_sinais.ipynb
::::

## Resultados da Simulação

A simulação computacional modelou 3 ciclos completos ($50\,\text{ms}$) da rede elétrica e extraiu as amostras utilizando os instantes definidos no cálculo teórico.

**Tabela 1 — Resumo dos Parâmetros**

| Grandeza | Símbolo | Valor | Unidade |
| :--- | :---: | :---: | :--- |
| Amplitude de Pico | $A$ | $179{,}6$ | V |
| Frequência Analógica | $f_0$ | $60$ | Hz |
| Frequência de Amostragem | $f_s$ | $480$ | Hz |
| Período Analógico | $T_0$ | $16{,}67$ | ms |
| Período Discreto | $T_s$ | $2{,}083$ | ms |

A visualização gráfica das marcações (`stems`) corrobora a formulação de que a proporção $f_s / f_0 = 480/60 = 8$ resulta exatamente em 8 amostras perfeitamente espaçadas para cada ciclo temporal da onda contínua. 

A exatidão da simulação pode ser comprovada cruzando a projeção teórica inicial com a matriz gerada numericamente pelo algoritmo em Python, descrita na tabela abaixo:

**Tabela 2 — Validação Numérica das Amostras**

| Índice ($n$) | Instante ($t$) | Teórico $x[n]$ | Simulado $x[n]$ |
| :---: | :---: | :---: | :---: |
| 0 | $0{,}000\,\text{ms}$ | $179{,}60\,\text{V}$ | $179{,}60\,\text{V}$ |
| 1 | $2{,}083\,\text{ms}$ | $127{,}00\,\text{V}$ | $127{,}00\,\text{V}$ |
| 2 | $4{,}167\,\text{ms}$ | $0{,}00\,\text{V}$ | $0{,}00\,\text{V}$ |
| 3 | $6{,}250\,\text{ms}$ | $-127{,}00\,\text{V}$ | $-127{,}00\,\text{V}$ |
| 4 | $8{,}333\,\text{ms}$ | $-179{,}60\,\text{V}$ | $-179{,}60\,\text{V}$ |

# Discussão

A abordagem adotada neste estudo validou de forma empírica e analítica a mecânica fundamental de discretização de sinais elétricos. Ao escolher uma frequência de captura de $480\,\text{Hz}$ para uma onda de $60\,\text{Hz}$, o critério de Nyquist foi plenamente satisfeito. 

O alinhamento milimétrico entre os cálculos analíticos de mesa e a representação vetorial em ponto flutuante produzida pela máquina (Tabela 2) reforça a validade do modelo cosenoidal implementado. Ressalta-se que, como em qualquer modelo elementar, o projeto assumiu um sinal analógico perfeitamente senoidal e livre de harmônicos ou ruídos termoelétricos, condições que em um circuito físico real exigiriam um filtro *anti-aliasing* (passa-baixas) antes do estágio de amostragem.