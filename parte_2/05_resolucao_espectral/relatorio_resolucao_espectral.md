---
title: Tópico 5 - Resolução Espectral
---

# Tópico 5 - Resolução Espectral

---

## Resumo Conceitual
A **Resolução Espectral** ($\Delta f$) define a capacidade de um sistema de processamento de sinais de discriminar e separar duas componentes senoidais com frequências muito próximas no domínio da frequência. Em simulações e sistemas computacionais baseados na Transformada Discreta de Fourier (DFT/FFT), a resolução espectral não é determinada apenas pela matemática do algoritmo, mas fundamentalmente pela duração temporal total do registro de dados observados.

O espaçamento entre dois bins vizinhos de frequência no espectro discreto é dado por $\Delta f = \frac{f_s}{N}$, onde $f_s$ é a frequência de amostragem e $N$ é o número total de amostras. Esse quociente equivale ao inverso do tempo de observação $T_{obs} = N \cdot T_s = \frac{N}{f_s}$. Portanto, quanto maior for o tempo de observação contínua do sinal, menor será o espaçamento $\Delta f$ e maior será a capacidade de diferenciar picos espectrais adjacentes. Quando a separação entre duas frequências presentes no sinal é menor do que a resolução $\Delta f$, as componentes se fundem em um único pico alargado, impossibilitando a identificação individual dos sinais.

---

## Formulação Matemática
A frequência de amostragem $f_s$ e o período de amostragem $T_s$ relacionam-se por:
$$f_s = \frac{1}{T_s} \quad [\text{Hz}]$$

Para uma sequência discreta de $N$ amostras, o **tempo total de observação** $T_{obs}$ é definido por:
$$T_{obs} = N \cdot T_s = \frac{N}{f_s} \quad [\text{segundos}]$$

A **resolução espectral** ($\Delta f$), que representa a largura de cada compartimento (*bin*) de frequência da DFT, é dada por:
$$\Delta f = \frac{f_s}{N} = \frac{1}{T_{obs}} \quad [\text{Hz}]$$

Em termos de frequência angular discreta $\Delta \omega$:
$$\Delta \omega = \frac{2\pi}{N} \quad [\text{rad/amostra}]$$

Para que duas componentes senoidais puras de frequências $f_1$ e $f_2$ (com $\Delta f_{sinal} = |f_1 - f_2|$) possam ser distinguidas como dois picos distintos no espectro de magnitude da DFT, é condição necessária que:
$$\Delta f_{sinal} > \Delta f \implies |f_1 - f_2| > \frac{f_s}{N}$$

Isso estabelece o número mínimo de amostras $N$ necessário para resolver as duas frequências:
$$N > \frac{f_s}{|f_1 - f_2|}$$

No experimento realizado:
- $f_s = 1000\text{ Hz}$, $f_1 = 100\text{ Hz}$, $f_2 = 105\text{ Hz} \implies \Delta f_{sinal} = 5\text{ Hz}$
- **Para $N = 64$**: $\Delta f = \frac{1000}{64} = 15,625\text{ Hz} > 5\text{ Hz}$ (Incapaz de separar as frequências)
- **Para $N = 256$**: $\Delta f = \frac{1000}{256} = 3,906\text{ Hz} < 5\text{ Hz}$ (Início da separação dos picos)
- **Para $N = 1024$**: $\Delta f = \frac{1000}{1024} \approx 0,976\text{ Hz} \ll 5\text{ Hz}$ (Separação nítida dos dois picos)

---

## Investigação e Simulação Computacional

A simulação computacional foi desenvolvida em Python no ambiente Jupyter Notebook para analisar um sinal composto por duas senoides com frequências muito próximas ($f_1 = 100\text{ Hz}$ e $f_2 = 105\text{ Hz}$) sob uma frequência de amostragem fixa $f_s = 1000\text{ Hz}$.

:::: {include} ./simulacao/simulacao_resolucao_espectral.ipynb
::::

No experimento, variou-se o tamanho da janela de amostragem $N \in \{64, 256, 1024\}$ para observar a evolução da resolução espectral. O sinal temporal composto foi plotado em `grafico_sinal_tempo_resolucao.png` e os espectros de magnitude normalizados para cada valor de $N$ foram comparados e salvos em `grafico_resolucao_espectral.png`.

---

### Discussão dos Resultados

#### 1. Análise da Discriminação de Picos por Tamanho de Janela ($N$)
A comparação gráfica dos três cenários de amostragem valida diretamente o critério de resolução espectral:
- **Cenário $N = 64$ ($\Delta f = 15,625\text{ Hz}$)**: Como a resolução espectral ($\Delta f = 15,625\text{ Hz}$) é três vezes maior do que a diferença real entre as frequências ($\Delta f_{sinal} = 5\text{ Hz}$), a DFT não possui pontos suficientes para registrar a variação entre $100\text{ Hz}$ e $105\text{ Hz}$. O gráfico resultante mostra uma única grande protuberância espectral centrada em torno de $100\text{ Hz}$, mascarando completamente a existência da segunda senoide.
- **Cenário $N = 256$ ($\Delta f = 3,906\text{ Hz}$)**: Ao aumentar o registro para $256$ amostras ($T_{obs} = 256\text{ ms}$), a resolução reduz-se para $3,906\text{ Hz}$, valor inferior aos $5\text{ Hz}$ de separação. No gráfico, observa-se o surgimento de dois picos distintos com um pequeno vale intermediário entre eles, confirmando o início da resolução espectral efetiva.
- **Cenário $N = 1024$ ($\Delta f = 0,976\text{ Hz}$)**: Com $1024$ amostras ($T_{obs} = 1,024\text{ s}$), a resolução aproxima-se de $1\text{ Hz}$. Os dois picos aparecem totalmente isolados e nítidos exatamente em $100\text{ Hz}$ e $105\text{ Hz}$, com amplitudes normalizadas próximas da unidade ($1.0$).

#### 2. Resolução Espectral Real vs. Interpolação por Zero-Padding
Um aspecto teórico crucial evidenciado pela simulação é que o aumento da resolução espectral física exige o incremento do tempo real de observação $T_{obs} = N / f_s$. Adicionar zeros ao final da sequência temporal (*zero-padding*) apenas aumenta a densidade de pontos calculados pela FFT (interpolando o gráfico suavemente), mas não melhora a resolução física nem permite separar duas frequências que foram truncadas em um tempo de observação insuficiente.