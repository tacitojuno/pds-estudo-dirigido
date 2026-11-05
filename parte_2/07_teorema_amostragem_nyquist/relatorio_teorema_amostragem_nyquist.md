---
title: Tópico 7 - Teorema da Amostragem e Nyquist
---

# Tópico 7 - Teorema da Amostragem e Nyquist

---

## Resumo Conceitual
O **Teorema da Amostragem de Nyquist-Shannon** é o postulado teórico central que estabelece a ponte entre o processamento de sinais analógicos em tempo contínuo e o processamento de sinais digitais em tempo discreto. Ele define a condição mínima sob a qual um sinal contínuo limitado em frequência pode ser completamente recuperado a partir de suas amostras discretas, sem qualquer perda de informação.

De acordo com o teorema, a taxa de amostragem $f_s$ deve ser estritamente maior que o dobro da maior frequência contida no sinal original ($f_{max}$). Metade da frequência de amostragem ($f_N = f_s / 2$) é denominada **Frequência de Nyquist** (ou frequência de dobra). Ela estabelece a máxima frequência física não-ambígua capaz de ser representada no domínio discreto. Se o sinal contiver componentes de frequência acima de $f_N$, ocorre a violação do critério de amostragem, desencadeando o fenômeno de **falseamento espectral (*aliasing*)**, no qual altas frequências são rebatidas para a banda passante discreta e assumem a identidade de frequências mais baixas, corrompendo o sinal de forma irreversível.

---

## Formulação Matemática
Seja $x(t)$ um sinal de tempo contínuo com transformada de Fourier $X(f)$ tal que $X(f) = 0$ para $|f| \ge f_{max}$ (sinal limitado em banda). 

O **Critério de Nyquist** estabelece que a reconstrução exata de $x(t)$ exige:
$$f_s > 2 f_{max}$$

A **Frequência de Nyquist** ($f_N$) define o limite espectral máximo não-ambíguo:
$$f_N = \frac{f_s}{2} \quad [\text{Hz}]$$

A amostragem ideal por trem de impulsos no domínio do tempo corresponde, no domínio da frequência, à replicação periódica do espectro original $X(f)$ a cada múltiplo inteiro da taxa de amostragem $f_s$:
$$X_s(f) = f_s \sum_{k=-\infty}^{+\infty} X(f - k f_s)$$

Quando $f_s < 2 f_{max}$, as réplicas espectrais adjacentes sobrepõem-se (*aliasing*). Para uma componente senoidal pura de frequência $f_0$ subamostrada com $f_s < 2f_0$, a frequência aparente rebatida ($f_{alias}$) que surge no intervalo de Nyquist $[0, f_N]$ é calculada por:
$$f_{alias} = |f_0 - k \cdot f_s| \le \frac{f_s}{2}, \quad k \in \mathbb{Z}$$

No experimento realizado com $f_0 = 100\text{ Hz}$:
1. **Superamostrado ($f_s = 500\text{ Hz}$)**: $f_N = 250\text{ Hz} > 100\text{ Hz}$ (Sem aliasing).
2. **Amostragem Crítica ($f_s = 210\text{ Hz}$)**: $f_N = 105\text{ Hz} > 100\text{ Hz}$ (Sem aliasing, $f_0$ aproxima-se do limite).
3. **Subamostrado ($f_s = 130\text{ Hz}$)**: $f_N = 65\text{ Hz} < 100\text{ Hz}$ (Aliasing severo). A frequência aparente é:
   $$f_{alias} = |100 - 1 \cdot 130| = |-30| = 30\text{ Hz}$$

---

## Investigação e Simulação Computacional

A simulação computacional foi desenvolvida em Python no ambiente Jupyter Notebook para analisar uma senoide pura de $f_0 = 100\text{ Hz}$ sob três taxas de amostragem distintas ($f_s = 500\text{ Hz}$, $f_s = 210\text{ Hz}$ e $f_s = 130\text{ Hz}$).

:::: {include} ./simulacao/simulacao_teorema_amostragem_nyquist.ipynb
::::

No experimento, a reconstrução temporal foi comparada com o sinal contínuo de referência (`grafico_amostragem_tempo_nyquist.png`) e os espectros de magnitude obtidos por FFT foram plotados e demarcados com o limite de Nyquist em cada caso (`grafico_espectro_nyquist.png`).

---

### Discussão dos Resultados

#### 1. Análise Temporal da Amostragem
A inspeção dos gráficos no domínio do tempo evidencia visualmente o efeito da taxa de amostragem sobre a fidelidade do sinal:
- **Caso $f_s = 500\text{ Hz}$ ($N_{ciclo} = 5$ amostras por ciclo)**: A envoltória discreta descreve com alta fidelidade a senoide contínua de fundo.
- **Caso $f_s = 210\text{ Hz}$ ($N_{ciclo} \approx 2,1$ amostras por ciclo)**: O sinal preserva as oscilações básicas, embora a densidade de pontos por ciclo seja extremamente reduzida.
- **Caso $f_s = 130\text{ Hz}$ ($N_{ciclo} = 1,3$ amostras por ciclo)**: As amostras discretas perdem o alinhamento com a oscilação real de $100\text{ Hz}$. Quando interpoladas, as amostras passam a desenhar uma oscilação significativamente mais lenta.

#### 2. Análise Espectral e Validação do Aliasing
A transformação para o domínio da frequência via FFT confirma com precisão rigorosa as previsões teóricas do Teorema de Nyquist:
- **Superamostragem ($f_s = 500\text{ Hz}$)**: O limite de Nyquist é $250\text{ Hz}$. O espectro registra um único pico nítido localizado exatamente em $100\text{ Hz}$, comprovando a ausência de distorção.
- **Amostragem Crítica ($f_s = 210\text{ Hz}$)**: O limite de Nyquist posiciona-se em $105\text{ Hz}$. O pico surge em $100\text{ Hz}$, ligeiramente antes da linha pontilhada de Nyquist. Embora preservado, o sinal exige um filtro anti-aliasing ideal com transição extremamente abrupta para separação.
- **Subamostragem ($f_s = 130\text{ Hz}$)**: O limite de Nyquist cai para $65\text{ Hz}$. Como a frequência original ($100\text{ Hz}$) supera o limite, o pico em $100\text{ Hz}$ desaparece da banda passante e surge um pico falso em $30\text{ Hz}$ ($f_{alias} = |100 - 130| = 30\text{ Hz}$). Isso demonstra que, uma vez subamostrado, torna-se fisicamente impossível distinguir a componente original de $100\text{ Hz}$ de um sinal real de $30\text{ Hz}$.