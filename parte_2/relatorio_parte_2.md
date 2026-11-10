# Relatório Geral — Parte 2: Fourier, Laplace, Transformada-Z e Análise de Sistemas

---

## 1. Introdução

### 1.1 Contextualização e Percurso Conceitual
O estudo de Processamento Digital de Sinais (PDS) e Teoria de Controle exige uma compreensão profunda de como sinais e sistemas físicos podem ser modelados, transformados e interpretados sob múltiplos domínios matemáticos. No âmbito do curso de Engenharia da Computação e Telemática do Instituto Federal da Paraíba (IFPB - Campus Campina Grande, Período 2026.2), a **Parte 2 do Estudo Dirigido** expande os fundamentos temporais estabelecidos na Parte 1 para consolidar um percurso analítico integrador:

$$\text{Domínio do Tempo} \longrightarrow \text{Domínio da Frequência (Fourier/FFT)} \longrightarrow \text{Plano-}s \text{ (Laplace)} \longrightarrow \text{Plano-}z \text{ (Transformada-Z)}$$

Esta abordagem multidomínio permite ao engenheiro analisar a resposta dinâmica de um sistema tanto em regime permanente quanto transiente, avaliando estabilidade, frequência de oscilação, resolução espectral e resposta em frequência.

### 1.2 Objetivos da Parte 2
- Compreender o significado físico da frequência e a relação entre senoides e exponenciais complexas via Relação de Euler.
- Dominar as transformadas espectrais (DTFT, DFT e o algoritmo otimizado FFT) e analisar o impacto da resolução espectral ($\Delta f$), frequência normalizada e janelamento (Retangular, Hann, Hamming).
- Investigar o Teorema da Amostragem de Nyquist-Shannon e caracterizar matematicamente e experimentalmente o fenômeno do **aliasing** (recobrimento espectral).
- Analisar sistemas contínuos no plano complexo $s$ por meio da Transformada de Laplace, associando a posição de polos e zeros com a estabilidade BIBO e a resposta transiente no tempo.
- Analisar sistemas discretos no plano complexo $z$ por meio da Transformada-Z, investigando a Região de Convergência (ROC), funções de transferência discretas e o critério de estabilidade do círculo unitário ($|z| = 1$).
- Estabelecer a ponte de transição entre o domínio contínuo e o discreto através do mapeamento exponencial $z = e^{s T_s}$.
- Sintetizar o aprendizado em um **Mini Projeto Integrador**, aplicando o conhecimento multidomínio em um problema de engenharia (monitoramento preditivo de vibração em motores elétricos).

---

## 2. Fundamentação Teórica

A fundamentação teórica da Parte 2 estrutura-se sobre quatro pilares conceituais interligados:

### 2.1 Representação Espectral e Transformada de Fourier
Sinais oscilatórios discretos no tempo $x[n]$ podem ser decompostos em somas de exponenciais complexas $e^{j\omega n}$. A **Transformada de Fourier em Tempo Discreto (DTFT)** representa o espectro contínuo de frequências de uma sequência no intervalo $-\pi \le \omega \le \pi$. Para sequências de duração finita $N$, a **Transformada Discreta de Fourier (DFT)** amostra o espectro em $N$ pontos discretos em frequência, enquanto a **Fast Fourier Transform (FFT)** reduz a complexidade computacional desse cálculo de $\mathcal{O}(N^2)$ para $\mathcal{O}(N \log_2 N)$.

### 2.2 Amostragem, Nyquist e Fenômenos Espectrais
Ao converter um sinal contínuo $x(t)$ em uma sequência discreta $x[n] = x(n T_s)$, o **Teorema de Nyquist** exige que a frequência de amostragem satisfaça $f_s > 2 f_{\max}$. A violação dessa condição provoca **aliasing**, em que altas frequências reaparecem erroneamente como baixas frequências. Além disso, o truncamento de um sinal no tempo equivale à multiplicação por uma janela, gerando **vazamento espectral (*spectral leakage*)**, o qual pode ser mitigado por janelas suavizadas (Hann, Hamming, Blackman).

### 2.3 Sistemas Contínuos no Domínio de Laplace (Plano-$s$)
A Transformada de Laplace converte equações diferenciais lineares em equações algébricas no plano complexo $s = \sigma + j\omega$. A função de transferência $H(s) = \frac{Y(s)}{X(s)}$ descreve a dinâmica do sistema por meio de seus polos e zeros. A estabilidade assintótica e BIBO em tempo contínuo exige que todos os polos causais estejam localizados no semiplano esquerdo (LHP), ou seja, $\text{Re}\{p_k\} < 0$.

### 2.4 Sistemas Discretos no Domínio da Transformada-Z (Plano-$z$)
A Transformada-Z estende a análise de frequência para o plano complexo $z = r e^{j\omega}$, convertendo equações de diferenças finitas em funções de transferência discretas $H(z) = \frac{Y(z)}{X(z)}$. A **Região de Convergência (ROC)** define o conjunto de valores de $z$ para os quais o somatório converge. A estabilidade BIBO de um sistema discreto e causal exige que todos os polos de $H(z)$ estejam estritamente dentro do círculo unitário ($|p_k| < 1$). A relação de mapeamento $z = e^{s T_s}$ une de forma exata a estabilidade no plano-$s$ à estabilidade no plano-$z$.

---

## 3. Formulação Matemática

A tabela a seguir consolida as principais relações matemáticas desenvolvidas ao longo de toda a Parte 2:

| Domínio / Conceito | Equação Matemática Fundamental | Interpretação das Variáveis e Parâmetros |
| :--- | :--- | :--- |
| **Relação de Euler** | $e^{j\theta} = \cos(\theta) + j\sin(\theta)$ <br> $\cos(\omega_0 n) = \frac{1}{2}\left(e^{j\omega_0 n} + e^{-j\omega_0 n}\right)$ | $\theta = \omega_0 n$ é a fase em radianos; decomposição de senoides em exponenciais complexas conjugadas. |
| **DTFT** | $X(e^{j\omega}) = \sum_{n=-\infty}^{+\infty} x[n] e^{-j\omega n}$ | $X(e^{j\omega})$ é o espectro contínuo e periódico de período $2\pi$; $\omega$ é a frequência angular em rad/amostra. |
| **DFT / IDFT** | $X[k] = \sum_{n=0}^{N-1} x[n] e^{-j\frac{2\pi}{N} k n}$ <br> $x[n] = \frac{1}{N} \sum_{k=0}^{N-1} X[k] e^{j\frac{2\pi}{N} k n}$ | $N$ é o número de amostras; $k$ é o índice discreto do bin espectral ($k = 0, \dots, N-1$). |
| **Resolução Espectral** | $\Delta f = \frac{f_s}{N} = \frac{1}{T_{\text{obs}}}$ | $\Delta f$ é a largura de cada bin em Hz; $f_s$ é a amostragem; $T_{\text{obs}}$ é o tempo total de observação. |
| **Frequência Normalizada** | $\omega = 2\pi \frac{f}{f_s}, \quad f_n = \frac{f}{f_s}$ | $\omega \in [-\pi, \pi]$ rad/amostra; $f_n \in [-0.5, 0.5]$ ciclos/amostra. |
| **Teorema de Nyquist** | $f_s > 2 f_{\max}, \quad f_N = \frac{f_s}{2}$ | $f_{\max}$ é a maior frequência do sinal; $f_N$ é a frequência limite de Nyquist. |
| **Aliasing** | $f_{\text{alias}} = \|f_{\text{in}} - k f_s\|$ | $f_{\text{alias}}$ é a frequência aparente observada no espectro quando $f_{\text{in}} > f_N$. |
| **Janelamento** | $x_w[n] = x[n] \cdot w[n]$ | $w[n]$ atenua as bordas do bloco temporal, reduzindo o vazamento de lóbulos secundários. |
| **Transformada de Laplace** | $X(s) = \int_{-\infty}^{+\infty} x(t) e^{-st} dt, \quad s = \sigma + j\omega$ | $s$ é a variável complexa; $\sigma = \text{Re}\{s\}$ dita o amortecimento; $\omega = \text{Im}\{s\}$ dita a oscilação. |
| **Função de Transferência $H(s)$** | $H(s) = \frac{\omega_n^2}{s^2 + 2\zeta\omega_n s + \omega_n^2}$ | $\omega_n$ é a frequência natural; $\zeta$ é o fator de amortecimento ($0 < \zeta < 1 \implies$ subamortecido). |
| **Transformada-Z** | $X(z) = \sum_{n=-\infty}^{+\infty} x[n] z^{-n}, \quad z = r e^{j\omega}$ | $z$ é a variável complexa discreta; $r = \|z\|$ é o raio/módulo; $\omega = \angle z$ é a frequência angular. |
| **Função de Transferência $H(z)$** | $H(z) = \frac{1}{1 - a z^{-1}} = \frac{z}{z - a}$ | $a$ é o polo do sistema no eixo real; a resposta ao impulso é $h[n] = a^n u[n]$. |
| **Mapeamento $s \rightarrow z$** | $z = e^{s T_s} = e^{\sigma T_s} e^{j\omega T_s}$ | $\|z\| = e^{\sigma T_s}$, $\angle z = \omega T_s$; conecta diretamente o plano-$s$ ao plano-$z$. |

---

## 4. Simulações Computacionais

As investigações do Estudo Dirigido foram desenvolvidas utilizando a linguagem **Python 3.12** com as bibliotecas científicas **NumPy**, **SciPy** (`scipy.signal`) e **Matplotlib**. A arquitetura do repositório foi organizada em 19 diretórios independentes:

```text
parte_2/
├── 01_frequencias_exponenciais/
├── 02_transformada_fourier_discreto/
├── 03_dft/
├── 04_fft/
├── 05_resolucao_espectral/
├── 06_frequencia_normalizada/
├── 07_teorema_amostragem_nyquist/
├── 08_aliasing/
├── 09_janelamento_vazamento_espectral/
├── 10_transformada_laplace/
├── 11_func_transferencia/
├── 12_polos_zeros_estabilidade_plano_s/
├── 13_transformada_z/
├── 14_regiao_de_convergencia/
├── 15_func_transferencia_discreta/
├── 16_estabilidade_plano_z/
├── 17_relacao_transformada_z_dtft/
├── 18_relacao_laplace_transformada_z/
└── mini_projeto/
```

Cada tópico contém seu script/notebook Jupyter de simulação, seus gráficos exportados em alta resolução e seu relatório individual em Markdown.

---

## 5. Análise Espectral

A análise espectral investigou a transição da representação contínua de Fourier (DTFT) para a discretizada (DFT/FFT) e avaliou os principais fenômenos práticos:

### 5.1 Resolução Espectral e Duração Temporal
A resolução espectral $\Delta f = \frac{f_s}{N} = \frac{1}{T_{\text{obs}}}$ demonstrou que a capacidade de distinguir duas componentes senoidais de frequências muito próximas ($\Delta f_{\text{sinal}} = |f_2 - f_1|$) depende exclusivamente da duração total da janela de observação no tempo ($T_{\text{obs}}$). Aumentar $N$ via *zero-padding* melhora a suavidade gráfica do espectro, mas não altera a resolução física real ($\Delta f$).

### 5.2 Violação de Nyquist e Aliasing
Os experimentos comprovaram que quando $f_s < 2 f_{\max}$, a amostragem provoca um espelhamento das frequências superiores a $f_N = f_s/2$ para dentro da banda primária. Esse recobrimento degrada irreversivelmente o sinal, gerando frequências aparente falsas que não existem no sinal físico original.

### 5.3 Janelamento e Compromisso Espectral
O truncamento temporal por janela retangular introduz descontinuidade nas bordas, espalhando energia espectral através dos lóbulos secundários ($-13\text{ dB}$). As janelas **Hann** e **Hamming** aplicam um atenuador suave nas extremidades do bloco, reduzindo o vazamento lateral para abaixo de $-31\text{ dB}$ e $-43\text{ dB}$, à custa de um pequeno alargamento do lóbulo principal.

---

## 6. Análise no Plano-$s$

A modelagem de sistemas contínuos no plano complexo $s = \sigma + j\omega$ permitiu correlacionar a posição de polos e zeros com a dinâmica temporal:

### 6.1 Papel da Parte Real ($\sigma$) e Imaginária ($\omega$)
- **Parte Real ($\sigma = \text{Re}\{p_k\}$)**: Dita o envelope de amortecimento exponencial $e^{\sigma t}$. Se $\sigma < 0$, o transiente atua como uma exponencial decrescente (estável); se $\sigma > 0$, atua como uma exponencial crescente (instável).
- **Parte Imaginária ($\omega = \text{Im}\{p_k\}$)**: Dita a frequência de oscilação natural do sistema $\cos(\omega t)$.

### 6.2 Critério de Estabilidade no Plano-$s$
Um sistema contínuo LTI causal é BIBO e assintoticamente estável se, e somente se, todos os polos de sua função de transferência $H(s)$ possuírem parte real estritamente negativa ($\text{Re}\{p_k\} < 0$). O **eixo imaginário ($\text{Re}\{s\} = 0$)** atua como a fronteira exata de estabilidade.

---

## 7. Análise no Plano-$z$

A análise de sistemas discretos no plano complexo $z = r e^{j\omega}$ consolidou a interpretação da Transformada-Z e da estabilidade no tempo discreto:

### 7.1 Região de Convergência (ROC) e Causalidade
A Transformada-Z de uma sequência $x[n]$ exige a definição explícita de sua ROC. Para uma sequência causal $a^n u[n]$, a ROC é $|z| > |a|$; para uma sequência anti-causal $-a^n u[-n-1]$, a ROC é $|z| < |a|$. Ambas possuem a mesma forma algébrica $\frac{z}{z-a}$, provando que a ROC é indispensável para garantir a unicidade no domínio do tempo.

### 7.2 Critério de Estabilidade do Círculo Unitário ($|z| = 1$)
No domínio discreto, a resposta temporal associada a um polo $p_k$ segue a potência $(p_k)^n = |p_k|^n e^{j\angle p_k n}$. O **círculo unitário ($|z| = 1$)** atua como a fronteira fundamental:
- **Polos Internos ($|p_k| < 1$)**: $|p_k|^n \rightarrow 0$ quando $n \rightarrow \infty \implies$ Sistema Causal BIBO Estável.
- **Polos sobre o Círculo ($|p_k| = 1$)**: $|p_k|^n = 1 \implies$ Sistema Marginalmente Estável (oscilação mantida).
- **Polos Externos ($|p_k| > 1$)**: $|p_k|^n \rightarrow \infty$ quando $n \rightarrow \infty \implies$ Sistema Instável.

---

## 8. Relação entre Laplace e Transformada-Z

A transformação exponencial $z = e^{s T_s}$ estabelece a ponte de conversão entre o plano-$s$ e o plano-$z$:

$$z = e^{(\sigma + j\omega) T_s} = e^{\sigma T_s} e^{j\omega T_s} \implies |z| = e^{\sigma T_s}, \quad \angle z = \omega T_s$$

### 8.1 Mapeamento das Regiões de Estabilidade
- **Semiplano Esquerdo ($\text{Re}\{s\} = \sigma < 0$)**: Mapeia para $|z| = e^{\sigma T_s} < 1$ (Interior do Círculo Unitário).
- **Eixo Imaginário ($\text{Re}\{s\} = \sigma = 0$)**: Mapeia para $|z| = e^0 = 1$ (Fronteira do Círculo Unitário).
- **Semiplano Direito ($\text{Re}\{s\} = \sigma > 0$)**: Mapeia para $|z| = e^{\sigma T_s} > 1$ (Exterior do Círculo Unitário).

### 8.2 Efeito do Período de Amostragem ($T_s$)
À medida que a amostragem se torna mais rápida ($T_s \rightarrow 0$), os polos no plano-$z$ migram no eixo real em direção a $z = 1 + j0$. Isso reduz a rotação angular por amostra ($\Omega = \omega T_s$), exigindo maior precisão numérica na implementação de coeficientes de filtros em hardware.

---

## 9. Mini Projeto Integrador

O **Mini Projeto Integrador (Parte 2)** aplicou a análise multidomínio na **Engenharia de Manutenção Preditiva**, monitorando a vibração de um motor elétrico industrial ($f_1 = 30\text{ Hz}$, $1800\text{ RPM}$) com início de falha no mancal de rolamento ($f_2 = 120\text{ Hz}$).

### 9.1 Resumo das Etapas do Projeto Integrador

1. **Etapa 1 (Aplicação)**: Monitoramento de aceleração vibracional em mancal de rolamento de motor elétrico.
2. **Etapa 2 (Sinal)**: $x(t) = 1.0 \cos(2\pi \cdot 30 t) + 0.6 \cos(2\pi \cdot 120 t + \pi/4)$.
3. **Etapa 3 (Amostragem)**: $f_s = 500\text{ Hz} > 2 f_{\max} = 240\text{ Hz}$ ($T_s = 2.0\text{ ms}, N = 500$).
4. **Etapa 4 (FFT)**: Identificação precisa dos picos em $30.0\text{ Hz}$ e $120.0\text{ Hz}$ com $\Delta f = 1.0\text{ Hz}$.
   ![Amostragem](figuras/grafico_mini_projeto_amostragem.png)
   ![Espectro FFT](figuras/grafico_mini_projeto_espectro.png)
5. **Etapa 5 (Aliasing)**: Subamostragem a $f_s = 180\text{ Hz} < 240\text{ Hz}$, gerando frequência aparente falsa em $f_{\text{alias}} = |120 - 180| = 60.0\text{ Hz}$.
   ![Aliasing](figuras/grafico_mini_projeto_aliasing.png)
6. **Etapa 6 (Janelamento)**: Teste com $f = 33.3\text{ Hz}$ (desalinhada do bin). As janelas Hann e Hamming reduziram o vazamento espectral lateral de $-13\text{ dB}$ para $-31\text{ dB}$.
   ![Janelamento](figuras/grafico_mini_projeto_janelamento.png)
7. **Etapa 7 (Modelo Contínuo no Plano-$s$)**: $H(s) = \frac{\omega_n^2}{s^2 + 2\zeta\omega_n s + \omega_n^2}$ com $\omega_n = 2\pi \times 50\text{ rad/s}$ e $\zeta = 0.2$. Polos em $p_{1,2} = -62.83 \pm j 307.81\text{ rad/s}$ ($\text{Re}\{p\} < 0 \implies$ Estável).
   ![Plano s](figuras/grafico_mini_projeto_plano_s.png)
8. **Etapa 8 (Modelo Discreto no Plano-$z$)**: Mapeamento para $T_s = 2.0\text{ ms}$. Polos em $p_z = 0.7200 \pm j 0.5093$. Módulo $|p_z| = 0.8819 < 1.0 \implies$ Estável no Plano-$z$.
   ![Plano z](figuras/grafico_mini_projeto_plano_z.png)
9. **Etapa 9 (Relação entre Planos)**: Verificação exata da fórmula $p_z = e^{p_s T_s} = e^{(-62.83 \pm j 307.81) \times 0.002} = 0.8819 e^{\pm j 0.6156}$.
   ![Mapeamento s-z](figuras/grafico_mini_projeto_mapeamento.png)
10. **Etapa 10 (Discussão)**: Resposta completa às 11 perguntas de avaliação do roteiro do mini projeto (conforme documentado na Seção 9 do relatório específico do mini projeto).

---

## 10. Conclusões

A realização da **Parte 2 do Estudo Dirigido** proporcionou uma formação técnica sólida e integrada sobre a análise de sinais e sistemas nos domínios do Tempo, Frequência, Plano-$s$ e Plano-$z$. 

Dentre as principais conclusões desenvolvidas pela equipe, destacam-se:
1. A representação no **domínio da frequência** revela informações de amplitude e fase que são impossíveis de isolar diretamente pela simples inspeção temporal.
2. O **Teorema de Nyquist** e o **Janelamento Espectral** são pré-requisitos práticos indispensáveis na aquisição de dados, prevenindo erros graves de diagnóstico causados por aliasing ou vazamento.
3. A análise de **polos e zeros nos planos $s$ e $z$** fornece uma ferramenta unificada para avaliar estabilidade, velocidade de resposta transiente e dinâmica de oscilação em sistemas contínuos e discretos.
4. A relação $z = e^{s T_s}$ garante a equivalência perfeita na transição entre o mundo analógico e o mundo digital, fundamentando a implementação de filtros digitais e controladores discretos em engenharia.

---

## 11. Referências

1. OPPENHEIM, Alan V.; SCHAFER, Ronald W. *Discrete-Time Signal Processing*. 3rd ed. Upper Saddle River: Pearson, 2010.
2. LATHI, Bhagwandas P. *Sinais e Sistemas Lineares*. 2ª ed. Porto Alegre: Bookman, 2007.
3. OGATA, Katsuhiko. *Engenharia de Controle Moderno*. 5ª ed. São Paulo: Pearson, 2010.
4. PROAKIS, John G.; MANOLAKIS, Dimitris G. *Digital Signal Processing: Principles, Algorithms, and Applications*. 4th ed. Pearson, 2007.
5. LYONS, Richard G. *Understanding Digital Signal Processing*. 3rd ed. Pearson, 2011.
6. DORF, Richard C.; BISHOP, Robert H. *Sistemas de Controle Modernos*. 12ª ed. Rio de Janeiro: LTC, 2013.
