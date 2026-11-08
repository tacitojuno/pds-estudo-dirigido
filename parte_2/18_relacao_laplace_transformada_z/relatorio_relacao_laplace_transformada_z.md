---
title: Tópico 18 - Relação entre Laplace e Transformada-Z
---

# Tópico 18 - Relação entre Laplace e Transformada-Z

---

## Resumo Conceitual
A **Transformada de Laplace** e a **Transformada-Z** são as duas ferramentas fundamentais para a análise e modelagem de sistemas LTI nos domínios do tempo contínuo e do tempo discreto, respectivamente. A transição entre o plano complexo $s = \sigma + j\omega$ e o plano complexo $z = r e^{j\Omega}$ é estabelecida pela relação de amostragem ideal exponencial $z = e^{s T_s}$, em que $T_s$ representa o período de amostragem do sistema.

Essa transformação conforme mapeia de forma direta as regiões de estabilidade e a dinâmica de amortecimento entre os dois domínios:
- O **Semiplano Esquerdo** do plano-$s$ ($\text{Re}\{s\} = \sigma < 0$) é mapeado estritamente para o **interior do Círculo Unitário** no plano-$z$ ($|z| = r < 1$).
- O **Eixo Imaginário** do plano-$s$ ($\text{Re}\{s\} = \sigma = 0$) é mapeado exatamente sobre a **fronteira do Círculo Unitário** no plano-$z$ ($|z| = r = 1$).
- O **Semiplano Direito** do plano-$s$ ($\text{Re}\{s\} = \sigma > 0$) é mapeado para o **exterior do Círculo Unitário** no plano-$z$ ($|z| = r > 1$).

---

## Formulação Matemática

### 1. Relação de Mapeamento Exponencial
Substituindo a variável complexa $s = \sigma + j\omega$ na transformação $z = e^{s T_s}$:
$$z = e^{(\sigma + j\omega) T_s} = e^{\sigma T_s} \cdot e^{j\omega T_s}$$

Expressando $z$ em sua forma polar $z = r e^{j\Omega}$:
- **Módulo / Raio ($r$)**: 
  $$r = |z| = e^{\sigma T_s}$$
  - Se $\sigma < 0 \implies r = e^{-|\sigma| T_s} < 1$ (Estável).
  - Se $\sigma = 0 \implies r = e^{0} = 1$ (Marginalmente Estável).
  - Se $\sigma > 0 \implies r = e^{+\sigma T_s} > 1$ (Instável).

- **Fase / Frequência Discreta ($\Omega$)**: 
  $$\Omega = \angle z = \omega T_s \pmod{2\pi} \quad \text{(rad/amostra)}$$

### 2. Mapeamento Periódico de Faixas (Aliasing no Plano-$s$)
Devido à periodicidade da exponencial complexa $e^{j(\omega T_s + 2\pi k)} = e^{j\omega T_s}$, cada faixa horizontal de largura $\omega_s = \frac{2\pi}{T_s}$ no plano-$s$ (conhecida como **faixa primária** $-\frac{\pi}{T_s} \le \omega < \frac{\pi}{T_s}$) é mapeada sobre todo o plano-$z$. Frequências contínuas acima da frequência de Nyquist ($\omega > \frac{\pi}{T_s}$) sobrepõem-se no plano-$z$, caracterizando o fenômeno de **aliasing**.

### 3. Exemplo de Mapeamento Analítico
Para um polo contínuo em $s = -2 + j5\text{ rad/s}$ sob período de amostragem $T_s = 0,1\text{ s}$:
$$r = e^{-2(0,1)} = e^{-0,2} \approx 0,8187$$
$$\Omega = 5 \times 0,1 = 0,5\text{ rad/amostra}$$
$$z = 0,8187 e^{j 0,5} \approx 0,7185 + j 0,3925$$
Como $|z| \approx 0,8187 < 1$, a estabilidade assintótica é rigorosamente preservada na conversão discreta.

---

## Investigação e Simulação Computacional

A simulação computacional foi desenvolvida em Python no ambiente Jupyter Notebook para comparar graficamente a migração de polos do plano complexo $s$ para o plano complexo $z$ sob dois períodos de amostragem diferentes ($T_s = 0,1\text{ s}$ e $T_s = 0,05\text{ s}$).

:::: {include} ./simulacao/simulacao_relacao_laplace_transformada_z.ipynb
::::

No experimento, geraram-se os mapas comparativos de polos e regiões de estabilidade entre o plano-$s$ e o plano-$z$ (`grafico_relacao_laplace_transformada_z.png`).

---

### Discussão dos Resultados

#### 1. Preservação das Regiões de Estabilidade
- Polos localizados no semiplano esquerdo contínuo ($\text{Re}\{s\} < 0$) resultam em polos discretos com módulo estritamente menor que 1 ($|z| < 1$).
- Polos localizados exatamente sobre o eixo $j\omega$ ($\text{Re}\{s\} = 0$) resultam em polos discretos posicionados exatamente sobre a borda do círculo unitário ($|z| = 1$).
- Polos no semiplano direito ($\text{Re}\{s\} > 0$) migram para fora do círculo unitário ($|z| > 1$), confirmando a equivalência biunívoca dos critérios de estabilidade entre os domínios.

#### 2. Efeito da Variação do Período de Amostragem ($T_s$)
- **Redução de $T_s$ ($T_s = 0,05\text{ s}$ vs $T_s = 0,1\text{ s}$)**: À medida que a amostragem se torna mais rápida ($T_s \to 0$), o módulo $r = e^{\sigma T_s} \to 1$ e o ângulo $\Omega = \omega T_s \to 0$. Isso faz com que todos os polos no plano-$z$ se agrupem em torno do ponto $z = 1 + j0$ no eixo real positivo.
- Essa concentração em torno de $z = 1$ exige maior precisão numérica em sistemas digitais (evitando erros de arredondamento em coeficientes de filtros IIR).