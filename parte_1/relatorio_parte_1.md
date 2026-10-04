---
title: "RELATÓRIO PARTE 1"
---

# Relatório: Processamento Digital de Sinais (Parte 1)

## 1. Introdução
Esse relatório tem como objetivo sintetizar os conhecimentos adquiridos durante a ***Parte 1*** da disciplina de Processamento Digital de Sinais (PDS). Ao longo de dez tópicos teóricos e um mini projeto integrador, foram investigados os princípios fundamentais da digitalização de sinais analógicos, as sequências matemáticas elementares e a caracterização de sistemas discretos, culminando no estudo aprofundado dos Sistemas Lineares e Invariantes no Tempo (LTI) e na aplicação prática da convolução para a filtragem de dados. O intuito principal é fornecer uma base sólida sobre como a informação do mundo físico é transposta e processada computacionalmente.

## 2. Fundamentação Teórica
O estudo foi estruturado seguindo os tópicos:
*   **Sinais e Amostragem (Tópicos 1 e 2):** Fenômenos físicos variam continuamente no tempo. Para processá-los digitalmente, o sinal passa por uma amostragem, onde sua amplitude é extraída em intervalos discretos regulares. O limite imposto pelo Teorema de Nyquist é fundamental para evitar a sobreposição de frequências (aliasing).
*   **Quantização (Tópico 3):** A etapa de quantização mapeia as amplitudes infinitas para um conjunto finito de níveis lógicos, dependente do número de bits ($N$) do conversor analógico-digital (ADC). Esse arredondamento introduz o erro (ou ruído) de quantização.
*   **Sequências e Operações (Tópicos 4 e 5):** Sinais complexos são decompostos em sequências fundamentais, como o impulso unitário ($\delta[n]$), o degrau unitário ($u[n]$) e exponenciais/senoides discretas. Tais sequências podem sofrer operações no eixo do tempo (atrasos, avanços, reversões) e de amplitude (escalonamento).
*   **Energia e Potência (Tópico 6):** Sinais são classificados de acordo com seu decaimento ao longo do tempo. Sinais de energia possuem duração finita (como pulsos e transitórios), enquanto sinais de potência perduram indefinidamente (como ondas senoidais periódicas e ruídos estacionários).
*   **Sistemas Discretos e LTI (Tópicos 7 e 8):** Sistemas são classificados por sua memória, linearidade e invariância temporal. Sistemas Lineares e Invariantes no Tempo (LTI) são de suma importância, pois podem ser descritos inteiramente pela sua resposta ao impulso ($h[n]$).
*   **Convolução Discreta e Filtragem (Tópicos 9 e 10):** A operação matemática da convolução ($x[n] * h[n]$) permite determinar a saída de qualquer sistema LTI. Como aplicação direta, pode-se usar janelas de média móvel para construir filtros digitais que suavizam dados corrompidos.

## 3. Formulação Matemática
As equações que fundamentam a teoria abordada incluem:
*   **Amostragem:** $x[n] = x(nT_s)$, onde a frequência de amostragem $f_s = 1/T_s$. O Teorema de Nyquist dita que $f_s > 2f_{\max}$.
*   **Resolução de Quantização:** O tamanho do degrau $\Delta V$ em um ADC de $N$ bits é dado por $\Delta V = V_{ref} / 2^N$. O erro máximo teórico é limitado a $\pm \Delta V / 2$.
*   **Sinal Discreto (Propriedade da Amostragem):** Todo sinal pode ser descrito por meio de impulsos: $x[n] = \sum x[k]\delta[n-k]$.
*   **Energia e Potência:** A energia total é $E = \sum_{n=-\infty}^{\infty} |x[n]|^2$. A potência média é calculada como o limite da energia média no tempo.
*   **Convolução Discreta:** $y[n] = x[n] * h[n] = \sum_{k=-\infty}^{\infty} x[k]h[n-k]$.
*   **Filtro de Média Móvel (LTI):** Descrito pela equação de diferenças $y[n] = \frac{1}{M} \sum_{k=0}^{M-1} x[n-k]$, onde $M$ é o tamanho da janela de filtragem.

## 4. Simulações
As simulações computacionais foram conduzidas em ambiente **Python**, utilizando notebooks interativos (Jupyter Notebook / IPython). Para tal, empregaram-se amplamente as bibliotecas **NumPy** para geração vetorial e processamento matemático de arrays, e **Matplotlib** para a plotagem gráfica de formas de onda e respostas ao impulso (utilizando recursos como a função `stem` para enfatizar o aspecto discreto das amostras). Nos casos envolvendo o Mini Projeto, foram processados e gravados arquivos de áudio em formato digital `.wav`.

## 5. Resultados e Discussão
As experimentações computacionais ao longo dos tópicos confirmaram as formulações teóricas:
*   A escolha de $f_s$ na rede elétrica (60 Hz) validou o aliasing visual quando o sistema capturou dados a taxas menores que a Taxa de Nyquist.
*   O estudo da quantização provou como uma resolução baixa (ex: 3 e 4 bits) gera ruídos em formato de escada na onda perfeitamente senoidal, distorção que foi suprimida nos cenários simulados com 8 bits ou superior.
*   Avaliou-se como a filtragem via média móvel atua eficientemente na supressão de flutuações rápidas (ruídos de alta frequência). Verificou-se que maiores valores de ordem do filtro ($M$) geram atenuações maiores, mas introduzem severos atrasos temporais (mudança de fase) no sistema.

## 6. Mini Projeto
**Projeto: Simulação de um Sistema de Aquisição e Processamento de Áudio**
*   O fenômeno modelado foi um sinal de áudio sintético, compondo duas ondas ($440$ Hz e $880$ Hz), representando uma nota musical e seu primeiro harmônico.
*   O sinal sofreu amostragem a $8000$ Hz e, posteriormente, quantização.
*   Foi introduzido ruído branco Gaussiano ($\sigma = 0.3$) ao sinal quantizado, recriando as condições de um canal analógico sujeito à degradação.
*   Para o processamento de recuperação, submeteu-se a onda contaminada a um filtro FIR de média móvel com janela $M=7$. A filtragem reduziu variações abruptas, mantendo intacta a fundamental. É possível observar, tanto de forma visual (nos gráficos desenvolvidos) quanto auditiva (pelos arquivos WAV gerados), a restauração eficaz do som corrompido, evidenciando o poder prático dos sistemas LTI na vida real.

## 7. Conclusões
O desenvolvimento das atividades da Parte 1 proporcionou um aprofundamento essencial na área de sinais. Compreendeu-se o funcionamento de conversores A/D e as limitações de resolução, assim como a capacidade analítica e de diagnóstico por meio da Convolução. O entendimento prático do equilíbrio entre atraso introduzido e ruído atenuado no design de filtros serviu como preparo para tópicos mais sofisticados nas etapas seguintes, onde as transformações no Domínio da Frequência dominarão a abordagem dos problemas.

## 8. Referências
*   Roteiro disponibilizado pelo professor ao longo da disciplina.
*   OPPENHEIM, Alan V.; WILLSKY, Alan S.; NAWAB, S. Hamid. **Sinais e Sistemas**. 2. ed. Pearson, 2010.
*   LATHI, B. P. **Sinais e Sistemas Lineares**. 2. ed. Bookman, 2007.