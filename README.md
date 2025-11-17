# Projeto_DBSCAN_Fraude_Financeira
Análise e detecção de fraudes financeiras com DBSCAN. Projeto de portfólio em Ciência de Dados (Python, Pandas, Scikit-learn).

# Detecção de Anomalias em Transações Financeiras com DBSCAN

Este projeto aplica o algoritmo de clusterização não supervisionada **DBSCAN** (Density-Based Spatial Clustering of Applications with Noise) para identificar transações financeiras fraudulentas ou anômalas em um conjunto de dados do metaverso.

O projeto foi desenvolvido como um estudo de caso prático para a disciplina de Aprendizado de Máquina II, do curso de Ciência de Dados da Universidade Presbiteriana Mackenzie.

---

## 📂 Dataset

O conjunto de dados utilizado ("Metaverse Financial Transactions Dataset") foi obtido da plataforma Kaggle e pode ser acessado [neste link](https://www.kaggle.com/datasets/faizaniftikharjanjua/metaverse-financial-transactions-dataset).

Ele contém 78.600 transações com 14 características, incluindo tipo de transação, localização, valor (`amount`) e um `risk_score` pré-calculado.

---

## 🛠️ Metodologia

O projeto foi dividido em três etapas principais, seguindo um pipeline clássico de Data Science.

### 1. Coleta e Tratamento dos Dados

Esta etapa foi crucial para preparar os dados para um algoritmo baseado em distância como o DBSCAN.

* **Análise Inicial:** Verificou-se que os dados não possuíam valores nulos, mas continham 8 colunas do tipo `object` (texto) que precisavam de tratamento.
* **Remoção de Colunas:** Colunas com alta cardinalidade e baixo valor preditivo para clusterização (como `timestamp`, `sending_address` e `receiving_address`) foram removidas.
* **Encoding:** As colunas categóricas (`transaction_type`, `location_region`, etc.) foram transformadas em variáveis numéricas usando **One-Hot Encoding** (`pd.get_dummies`).
* **Padronização:** Como o DBSCAN é sensível à escala das variáveis, todos os dados foram padronizados com o **`StandardScaler`** do Scikit-learn, garantindo que nenhuma variável (como `amount`) dominasse o cálculo da distância.

### 2. Aplicação do Algoritmo (DBSCAN)

O DBSCAN foi escolhido por sua capacidade de encontrar clusters de formas arbitrárias e, principalmente, por sua eficácia em **isolar outliers (anomalias)**, classificando-os como "ruído".

Para isso, dois parâmetros-chave foram definidos:

1.  **`min_samples` (44):** Definido pela regra de `2 * número_de_features`, resultando em 44.
2.  **`eps` (1.5):** O parâmetro de distância foi estimado usando o **K-distance Elbow Plot**. O gráfico mostrou que o "ponto de inflexão" (cotovelo), onde a distância dos vizinhos começa a disparar, estava em aproximadamente 1.5.

*(**Dica:** Adicione o print do seu gráfico K-distance aqui no GitHub para ilustrar)*

### 3. Análise dos Resultados

O algoritmo foi executado e identificou **40 transações como anomalias** (classificadas com o `label = -1`), enquanto as 78.560 transações restantes foram agrupadas em 90 clusters "normais".

O foco da análise foi entender o que essas 40 transações tinham em comum, criando a **"Persona da Anomalia"**.

---

## 🎯 Resultados: A "Persona" da Fraude

Ao cruzar as 40 anomalias encontradas pelo DBSCAN com os dados originais, foi possível traçar um perfil claro da transação suspeita:

* **Tipo de Transação:** O tipo **`phishing`** é absurdamente mais frequente nas anomalias (45% dos casos) do que na base geral (apenas 3,2%).
* **Valor (`amount`):** O valor médio das anomalias é **R$ 1042**, mais que o dobro da média geral de **R$ 502**.
* **Padrão de Compra:** O padrão **`random`** é duas vezes mais comum nas anomalias (67,5%) do que na base geral (33,2%).
* **Localização:** A **`Asia`** foi a localização mais comum para as anomalias (30% dos casos).
* **Score de Risco (Gabarito):** O `risk_score` médio das anomalias foi de **71.4**, muito superior à média geral de **44.9**, validando que o modelo encontrou transações de fato arriscadas.
* **Duração da Sessão:** As anomalias tiveram sessões mais curtas, com duração mediana de **35 segundos**, contra 60 segundos da média geral.

### Conclusão
O DBSCAN provou ser eficaz para isolar transações suspeitas sem supervisão prévia. O perfil da fraude identificada é caracterizado por transações do tipo **'phishing'**, originadas da **'Asia'**, com **valores atipicamente altos**, **padrão de compra aleatório** e executadas em **sessões muito curtas**.

---

## 💻 Tecnologias Utilizadas

* **Python 3**
* **Pandas:** Para manipulação e tratamento dos dados.
* **Scikit-learn (sklearn):** Para `StandardScaler`, `NearestNeighbors` e `DBSCAN`.
* **Matplotlib:** Para a visualização do K-distance plot.
* **KaggleHub:** Para download do dataset.
* **Google Colab:** Como ambiente de desenvolvimento.

