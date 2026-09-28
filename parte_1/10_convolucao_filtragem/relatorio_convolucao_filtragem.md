---
title: Tópico 10 - Convolução como Filtragem
---

# Tópico 10 - Convolução como Filtragem

---

## Resumo Conceitual

A convolução discreta não é apenas uma ferramenta de análise abstrata; é o mecanismo matemático subjacente à filtragem digital. Ao definirmos criteriosamente a resposta ao impulso $h[n]$ de um sistema, podemos atenuar ou amplificar componentes específicos de um sinal de entrada $x[n]$.

Um dos filtros mais práticos e amplamente utilizados é o **Filtro de Média Móvel**. Esse filtro atua suavizando as flutuações rápidas (ruído de alta frequência) ao calcular a média das últimas $M$ amostras. Contudo, essa operação introduz um compromisso (*trade-off*) inevitável na engenharia: quanto maior for a capacidade de suavização, maior será a distorção introduzida no sinal original, manifestando-se frequentemente através de atraso temporal e atenuação de amplitude.

---

## Formulação Matemática

A resposta ao impulso de um filtro de média móvel causal de ordem $M$ é um pulso retangular:
$$h[n] = \frac{1}{M} \sum_{k=0}^{M-1} \delta[n-k]$$

Aplicando a propriedade da soma de convolução $y[n] = x[n] * h[n]$, obtemos a equação de diferenças que descreve o filtro:
$$y[n] = \frac{1}{M} \sum_{k=0}^{M-1} x[n-k]$$

Cada amostra de saída $y[n]$ é simplesmente a média aritmética das amostras de entrada no instante atual $n$ e nas $M-1$ amostras anteriores. Trata-se de um sistema do tipo FIR (*Finite Impulse Response*).

---

## Investigação e Simulação Computacional

Para avaliar o impacto prático da filtragem, simulamos a medição da tensão da rede elétrica ($60\,\text{Hz}$ amostrada a $1200\,\text{Hz}$) severamente contaminada por ruído branco gaussiano, simulando interferência eletromagnética no canal de aquisição. O sinal ruidoso foi processado por três filtros LTI de média móvel com diferentes tamanhos de janela temporal: $M=3$, $M=5$ e $M=10$.

:::: {include} ./simulacao/simulacao_convolucao_filtragem.ipynb
::::

### Discussão dos Resultados

A análise gráfica evidencia os efeitos dinâmicos da convolução:

1. **Respostas ao Impulso:** O gráfico central demonstra a distribuição dos pesos. Para $M=10$, o peso individual de cada amostra decai para $0{,}1$, distribuindo a energia e a influência da entrada sobre uma janela temporal mais ampla.
2. **Suavização do Ruído:** Ao observarmos o comportamento geral dos sinais filtrados, fica evidente que o filtro com $M=3$ ainda acompanha as flutuações do ruído de forma agressiva. Em contrapartida, o filtro com $M=10$ rejeita eficazmente a maior parte das variações abruptas, resultando em uma curva consideravelmente mais suave e próxima da senoide fundamental.
3. **O Compromisso (Atraso e Atenuação):** O gráfico de detalhe revela o custo da filtragem. O filtro com $M=10$ introduz um atraso de fase substancial (o pico ocorre temporalmente deslocado à direita) e uma distorção de amplitude (a tensão de pico não atinge os $179{,}6\,\text{V}$ nominais). 
    
Conclui-se que o dimensionamento de $M$ em um projeto de filtragem exige um equilíbrio estrito: a ordem deve ser suficientemente elevada para rejeitar a interferência do ambiente, mas contida o bastante para preservar a dinâmica e a energia da informação de interesse.