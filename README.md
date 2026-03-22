# 💳 Fraud Detection: Classificação de Anomalias em Transações de Cartão de Crédito

[![Python](https://img.shields.io/badge/Python-3.8%2B-blue?logo=python&logoColor=white)](https://www.python.org/)
[![Scikit-Learn](https://img.shields.io/badge/Scikit--Learn-Machine_Learning-orange?logo=scikit-learn&logoColor=white)](https://scikit-learn.org/)
[![Pandas](https://img.shields.io/badge/Pandas-Data_Wrangling-green?logo=pandas&logoColor=white)](https://pandas.pydata.org/)

## 📌 O Problema de Negócio
No ecossistema de meios de pagamento, a fraude de cartão de crédito é um problema de bilhões de dólares. O desafio para os bancos não é apenas bloquear transações fraudulentas, mas fazê-lo em milissegundos, sem bloquear compras legítimas de bons clientes (Falsos Positivos).

O bloqueio indevido gera um atrito severo (constrangimento no caixa, perda de confiança e uso de cartões concorrentes). Por outro lado, a aprovação de uma fraude gera perdas financeiras diretas e custos de estorno (*Chargeback*). O objetivo deste projeto é desenvolver um modelo de detecção de anomalias capaz de operar em um cenário de **desbalanceamento extremo**, onde as fraudes representam menos de 0,2% das transações.

## 📊 O Conjunto de Dados
O dataset utilizado contém transações realizadas por portadores de cartões de crédito europeus. Por questões de sigilo bancário, a maioria das variáveis (V1 a V28) passou por uma transformação de Análise de Componentes Principais (PCA).
* **Características Numéricas:** Variáveis V1 a V28 (anonimizadas).
* **Tempo (Time):** Segundos decorridos entre a primeira transação do dataset e a transação atual.
* **Valor (Amount):** O valor monetário da transação.
* **Variável Alvo (Class):** `0` para transação Normal, `1` para Fraude.

## 🛠️ Arquitetura e Tecnologias Utilizadas
* **Linguagem:** Python
* **Manipulação e Escalonamento:** Pandas, NumPy, RobustScaler
* **Visualização:** Matplotlib, Seaborn
* **Modelagem Preditiva:** Scikit-Learn (Random Forest Classifier otimizado para classes desbalanceadas)

## 🧠 Metodologia Aplicada
1. **Dimensionamento Robusto (Scaling):** Como os valores das transações variam drasticamente e possuem outliers severos, aplicou-se o `RobustScaler` nas variáveis *Time* e *Amount* para não enviesar o modelo.
2. **Governança de Divisão (Stratified Split):** O conjunto de dados foi dividido garantindo matematicamente a mesma proporção de fraudes (0.17%) tanto na base de treino quanto na de teste.
3. **Machine Learning:** Foi utilizado o algoritmo *Random Forest*. Para contornar o desbalanceamento sem a necessidade de criar dados sintéticos (SMOTE), aplicou-se a penalização de classe (`class_weight='balanced'`), forçando o modelo a "prestar mais atenção" à classe minoritária.

## 📈 Resultados e Métricas de Negócio
Em bases com desbalanceamento extremo, a acurácia é uma métrica ilusória (chutar "0" para tudo daria 99.8% de acurácia). A avaliação focou na **Curva Precision-Recall (AUPRC)**.

* **Precision (Atrito Operacional):** O modelo atingiu altíssima precisão, garantindo que quando uma transação é sinalizada como fraude, a probabilidade de acerto é máxima. Isso minimiza o bloqueio de cartões de clientes idôneos.
* **Recall (Proteção de Capital):** O modelo demonstrou forte capacidade de identificar as fraudes reais antes que elas fossem processadas, estancando prejuízos.

## 💡 Recomendações e Políticas de Prevenção
Com base nos *scores* de probabilidade gerados pelo modelo, sugere-se a implementação das seguintes "camadas de fricção":
1. **Atrito Zero (Probabilidade Baixa):** Aprovação silenciosa e instantânea.
2. **Step-up de Autenticação (Probabilidade Média):** A transação fica pendente no gateway e o banco aciona uma validação via Push no App do cliente (Biometria ou Token) para confirmar a compra em tempo real.
3. **Bloqueio Categórico (Probabilidade Alta):** Bloqueio imediato do cartão e encaminhamento para a central de monitoria de fraudes.

## 🚀 Como Executar o Projeto

1. Clone este repositório:
   ```bash
   git clone [https://github.com/SeuUsuario/credit-card-fraud-detection.git](https://github.com/SeuUsuario/credit-card-fraud-detection.git)

2. Instale as dependências:
   ```bash
   pip install -r requirements.txt

3. Execute o notebook:
Abra e rode o arquivo Deteccao_Fraude.ipynb para visualizar a pipeline de mitigação de anomalias.
