---
title: Tópico 1 - Frequência e Exponenciais Complexas
---

# Tópico 1 - Frequência e Exponenciais Complexas

---

## Resumo Conceitual
A representação de sinais oscilatórios por meio de senoides e exponenciais complexas é a pedra angular do Processamento Digital de Sinais (PDS) [1]. No domínio discreto, um sinal oscilatório básico é representado matematicamente por uma função cossenoide caracterizada por três parâmetros fundamentais: **amplitude** ($A$), **frequência angular discreta** ($\omega_0$) e **fase inicial** ($\phi$) [1, 2].

A **Relação de Euler** fundamenta a conexão direta entre a trigonometria e a análise espectral baseada em exponenciais complexas [1]. Ela demonstra que a exponencial complexa $e^{j\omega_0 n}$ descreve um fasor de módulo unitário girando no plano complexo a uma taxa constante de $\omega_0$ radianos por amostra. A projeção desse fasor sobre o eixo real resulta na função cosseno, enquanto a projeção sobre o eixo imaginário resulta na função seno [1]. A utilização de exponenciais complexas simplifica significativamente a manipulação matemática de sistemas Lineares e Invariantes no Tempo (LTI), visto que exponenciais complexas são autofunções desses sistemas [1, 3].

---

## Formulação Matemática
Uma senoide discreta é definida por [1]:
$$x[n] = A \cos(\omega_0 n + \phi)$$

Onde:
- $A \in \mathbb{R}^+$ representa a **amplitude** (valor máximo de pico do sinal) [1, 2].
- $\omega_0$ é a **frequência angular discreta** em radianos por amostra ($\text{rad/amostra}$), com o intervalo fundamental $-\pi \le \omega_0 \le \pi$ [1, 4].
- $\phi$ é a **fase inicial** em radianos, responsável por introduzir um deslocamento temporal na forma de onda [1, 2].
- $n \in \mathbb{Z}$ representa o índice discreto de amostragem [1].

A **Relação de Euler** expressa a exponencial complexa como [1]:
$$e^{j\theta} = \cos(\theta) + j\sin(\theta)$$

Aplicada ao domínio do tempo discreto ($\theta = \omega_0 n$), obtém-se [1]:
$$e^{j\omega_0 n} = \cos(\omega_0 n) + j\sin(\omega_0 n)$$

A partir dessa igualdade, as funções trigonométricas senoidais e cossenoidais podem ser decompostas analiticamente como a combinação linear de exponenciais complexas conjugadas [2]:
$$\cos(\omega_0 n) = \frac{1}{2}\left(e^{j\omega_0 n} + e^{-j\omega_0 n}\right)$$
$$\sin(\omega_0 n) = \frac{1}{2j}\left(e^{j\omega_0 n} - e^{-j\omega_0 n}\right)$$

Dessa forma, extrai-se diretamente que [1, 2]:
$$\text{Re}\{e^{j\omega_0 n}\} = \cos(\omega_0 n)$$
$$\text{Im}\{e^{j\omega_0 n}\} = \sin(\omega_0 n)$$

---

## Investigação e Simulação Computacional

A simulação computacional foi desenvolvida em Python no ambiente Jupyter Notebook para analisar o impacto dos parâmetros $A$, $\omega_0$ e $\phi$, além de verificar a decomposição da exponencial complexa [2, 5].

:::: {include} ./simulacao/simulacao_frequencias_exponenciais.ipynb
::::

Na primeira etapa do experimento, fixou-se um intervalo discreto de $n \in [6]$ amostras e analisou-se o sinal base $x_1[n] = 1.0 \cdot \cos(0.1\pi n)$ sob três variações isoladas:
1. **Alteração de Amplitude**: $x_2[n] = 2.0 \cdot \cos(0.1\pi n)$ (amplitude duplicada).
2. **Alteração de Frequência**: $x_3[n] = 1.0 \cdot \cos(0.3\pi n)$ (frequência multiplicada por 3).
3. **Alteração de Fase**: $x_4[n] = 1.0 \cdot \cos(0.1\pi n + \pi/4)$ (avanço de fase de $\pi/4$ radianos).

Na segunda etapa, foi gerada a exponencial complexa $e^{j 0.1\pi n}$ e suas partes real e imaginária foram extraídas através das funções `np.real()` e `np.imag()`. Os gráficos gerados foram exportados e salvos localmente (`grafico_senoides_variacao.png` e `grafico_exponencial_complexa.png`).

---

### Discussão dos Resultados

#### 1. Análise Paramétrica das Senoides Discretas
A comparação gráfica entre o sinal base $x_1[n]$ e os sinais modificados confirma as previsões teóricas [2]:
- **Efeito da Amplitude ($A$)**: A alteração de $A=1.0$ para $A=2.0$ redimensionou a escala vertical da oscilação, fazendo com que os picos atingissem $\pm 2.0$, sem alterar a taxa de amostragem por ciclo ou a posição dos cruzamentos por zero [2].
- **Efeito da Frequência Angular ($\omega_0$)**: Ao elevar a frequência de $\omega_0 = 0.1\pi$ para $\omega_0 = 0.3\pi$ rad/amostra, o número de amostras necessárias para completar um ciclo completo diminuiu de $N = 20$ amostras para $N \approx 6,67$ amostras. Isso resultou em uma oscilação significativamente mais rápida no domínio discreto $n$ [2].
- **Efeito da Fase Inicial ($\phi$)**: A adição da fase $\phi = \pi/4$ rad deslocou o sinal no eixo horizontal. O primeiro pico positivo, que no sinal base ocorria em $n=0$, passou a ocorrer antecipadamente, demonstrando o efeito de avanço de fase no tempo discreto [2].

#### 2. Validação da Relação de Euler
A simulação da exponencial complexa $e^{j 0.1\pi n}$ demonstrou empiricamente que a sua **parte real** $\text{Re}\{e^{j 0.1\pi n}\}$ coincide rigorosamente com a curva de um cosseno puro $\cos(0.1\pi n)$ iniciando em $+1.0$ para $n=0$ [1, 2]. Paralelamente, a **parte imaginária** $\text{Im}\{e^{j 0.1\pi n}\}$ descreve uma senoide pura $\sin(0.1\pi n)$ iniciando em $0.0$ para $n=0$ [1, 2].

Ambas as partes possuem a mesma frequência angular $\omega_0 = 0.1\pi$, mas mantêm uma defasagem ortogonal relativa de $90^\circ$ ($\pi/2$ rad). Esse resultado valida computacionalmente a representação polar e Euleriana dos sinais oscilatórios discretos [1, 2].