# 🚀 Deep Learning para Previsão de Séries Temporais (Mercado de Ações)

Este repositório contém o Trabalho Final de Deep Learning do **MBA em Data Science & AI da FIAP (10DTSR)**.

Como parte de uma simulação da equipe de cientistas de dados da *Quantum Finance*, o objetivo deste projeto foi desenvolver e comparar diferentes arquiteturas de Deep Learning para prever sinais de **'Compra' (1) ou 'Venda' (0)** para ações do mercado brasileiro, com base no histórico de 15 dias de negociação.

O projeto não se concentrou em encontrar um único "melhor modelo", mas sim em uma **análise experimental** iterativa, testando diferentes abordagens de modelagem e engenharia de features em diferentes ativos (VALE3, PETR4, BBAS3, CSNA3).

---

## 🏛️ Metodologia Experimental

A performance de cada modelo foi avaliada em duas frentes:
1.  **Acurácia Estatística:** Utilizando `classification_report` (Acurácia, Precisão, Recall, F1-Score) para medir a precisão da classificação.
2.  **Resultado Financeiro:** Utilizando uma função de `backtest` para simular o retorno financeiro (crescimento do patrimônio) ao seguir os sinais do modelo.

### 1. (VALE3) - Baseline: Modelos Sequenciais Puros
O primeiro experimento estabeleceu uma linha de base. Testamos três arquiteturas de Deep Learning usando *apenas* a sequência de 15 dias de preços (normalizados como retornos) como entrada.

* **Modelos:** CNN 1D, RNN Simples, LSTM.
* **Resultados (Acurácia):** RNN (~88%) e LSTM (~89%) foram muito superiores à CNN 1D (~69%), provando sua capacidade de capturar tendências sequenciais.
* **Resultados (Backtest):** Uma descoberta crítica. **A maior acurácia não levou ao lucro.** Todos os modelos tiveram um resultado financeiro próximo de zero (break-even).

### 2. (PETR4) - Modelos de Entrada Dupla (Sequência + Features)
A hipótese seguinte foi que a sequência de preços por si só era insuficiente. Neste experimento, enriquecemos os modelos com *features* de análise técnica.

* **Modelos:** Arquiteturas de entrada dupla (CNN 1D, RNN, LSTM) que recebiam:
    1.  A sequência de 15 dias de preços.
    2.  Um vetor de *features* (Médias Móveis, Retornos/Momentum, Volatilidade).
* **Resultados (Acurácia):** Acurácia manteve-se alta (~87%).
* **Resultados (Backtest):** **Um salto drástico de performance.** A adição de features de análise técnica tornou os modelos lucrativos, com o `CNN1D + Features` gerando o melhor retorno (quase R$ 6.000 de lucro no período de teste).

### 3. (BBAS3) - Modelos Híbridos (RNN + MLP)
Aqui, testamos se uma arquitetura híbrida (onde um "ramo" RNN processa a sequência e um "ramo" MLP processa as *features* tabelares) aprenderia de forma mais eficaz.

* **Modelos:**
    1.  MLP (apenas features).
    2.  RNN (apenas sequência).
    3.  Híbrido (RNN + MLP) com aprendizado conjunto.
* **Resultados (Acurácia):** O modelo Híbrido teve a maior acurácia estatística (~91%).
* **Resultados (Backtest):** Novamente, a maior acurácia não venceu. O modelo `MLP (apenas features)` foi o mais lucrativo. O Híbrido (e o RNN puro) tiveram prejuízo, sugerindo que, para este ativo, a sequência de preços pode ter adicionado *ruído* ou redundância às features.

### 4. (CSNA3) - Visão Computacional (CNN 2D com Imagens)
A etapa final testou uma abordagem completamente diferente: tratar o problema como Visão Computacional.

* **Modelo:** Uma CNN 2D (rede convolucional de imagens).
* **Entrada:** Imagens (gráficos de barras) representando os últimos 15 dias de preço.
* **Resultados (Acurácia):** Excelente acurácia (~88%) e métricas de precisão/recall muito equilibradas.
* **Resultados (Backtest):** **O melhor resultado financeiro de todo o projeto.** O modelo de CNN 2D, treinado nas imagens, gerou o maior retorno de patrimônio, capturando padrões visuais que os modelos sequenciais/tabelares não identificaram.

---

## 💡 Principais Conclusões

1.  **Acurácia Estatística ≠ Lucratividade:** A principal lição do projeto. Modelos com acurácia mais baixa (mas que "acertavam nos momentos certos") foram consistentemente mais lucrativos no backtest do que modelos com acurácia mais alta.
2.  **Engenharia de Features é Crucial:** A adição de features de análise técnica (visto em PETR4) foi o fator que transformou modelos de *break-even* em modelos lucrativos. A riqueza das *features* provou ser tão importante quanto a arquitetura do modelo.
3.  **Não existe "Melhor Modelo" Único:** A performance variou drasticamente entre os ativos e as abordagens. O modelo de imagens (CNN 2D) foi o campeão na CSNA3, enquanto um modelo de entrada dupla (CNN 1D + Features) foi o melhor para a PETR4.

---

## 🛠️ Stack Tecnológica

* **Linguagem:** Python
* **Bibliotecas de Deep Learning:** TensorFlow, Keras
* **Bibliotecas de Dados:** Pandas, NumPy, Scikit-learn (para pré-processamento, métricas e *class weights*)
* **Visualização:** Matplotlib, Seaborn
* **Ambiente:** Google Colab

## 👥 Autores

* Erika Koyanagui
* Fabio Asnis Campos da Silva
* Lucas Huber Pissaia
* Matheus Raeski
