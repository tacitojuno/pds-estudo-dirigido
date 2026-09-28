---
title: Tópico 8 - Sistemas LTI
---

# Tópico 8 - Sistemas LTI (Linear Time-Invariant)

---

## Investigação Teórica

Os Sistemas Lineares e Invariantes no Tempo (LTI) constituem a classe mais importante de sistemas na engenharia, pois as suas propriedades matemáticas permitem modelar e prever com exatidão o comportamento de circuitos, filtros e canais de comunicação. A equipe investigou os seis conceitos fundamentais que sustentam essa teoria.

### 1. O Significado de Linearidade
A linearidade determina que um sistema obedeça ao princípio da superposição, que se divide em duas partes: aditividade e homogeneidade[cite: 7]. Isso significa que a resposta do sistema a uma combinação linear de múltiplos sinais de entrada é igual à mesma combinação linear das respostas individuais do sistema a cada um desses sinais[cite: 7]. Se multiplicarmos a entrada por uma constante, a saída é multiplicada pela mesma constante[cite: 7].

### 2. O Significado de Invariância no Tempo
A invariância no tempo garante que as características e o comportamento do sistema não se alterem à medida que o tempo avança[cite: 7]. Matematicamente, se aplicarmos um atraso ao sinal de entrada, o sinal de saída sofrerá exatamente o mesmo atraso, mas manterá o seu formato e amplitude originais[cite: 7].

### 3. O que é a Resposta ao Impulso
A resposta ao impulso, denotada por $h[n]$, é o sinal de saída que o sistema produz quando o sinal de entrada é um impulso unitário discreto ($\delta[n]$), assumindo que as condições iniciais do sistema são nulas[cite: 7]. 

### 4. Por que a Resposta ao Impulso é Importante
Em sistemas gerais, saber a resposta a um impulso não diz muito sobre a resposta a outros sinais. No entanto, em sistemas LTI, a resposta ao impulso $h[n]$ atua como uma "impressão digital" matemática do sistema. Ela contém absolutamente toda a informação sobre as características do sistema (memória, causalidade, estabilidade), permitindo prever a saída para *qualquer* entrada arbitrária.

### 5. Representação de um Sinal utilizando Impulsos Deslocados
Através da propriedade da amostragem (ou peneira), qualquer sequência discreta $x[n]$ pode ser reescrita como uma soma infinita de impulsos unitários escalonados em amplitude e deslocados no tempo[cite: 7]:
$$x[n] = \sum_{k=-\infty}^{+\infty} x[k]\delta[n-k]$$
Cada amostra individual do sinal $x[n]$ atua como um impulso isolado que ocorre no instante $k$, com amplitude $x[k]$.

### 6. Caracterização de Sistemas LTI através dessa Propriedade
A junção desses conceitos explica o poder dos sistemas LTI:
1. Como o sinal $x[n]$ é uma soma de impulsos, e o sistema é **Linear**, a resposta total do sistema será a soma das respostas a cada um desses impulsos individuais.
2. Como o sistema é **Invariante no Tempo**, a resposta a um impulso atrasado $\delta[n-k]$ é simplesmente a resposta ao impulso original atrasada, ou seja, $h[n-k]$.

Essa lógica conduz diretamente à operação de **convolução**[cite: 7]. A propriedade permite afirmar categoricamente que um sistema LTI fica 100% caracterizado pela sua resposta ao impulso, não sendo necessária nenhuma outra informação ou equação complementar[cite: 7].