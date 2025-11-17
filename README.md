# Desafio de Dados – Análise de Preços de Casas

## 🎯 Contexto do Projeto

Este projeto foi desenvolvido como parte de um **Desafio de Dados** para o Banco de Talentos, com o objetivo de transformar um conjunto de dados de preços de casas em *insights* claros e acionáveis para um time de negócios. O foco principal é demonstrar a capacidade de realizar a exploração, limpeza e comunicação de resultados de dados por meio de um dashboard de Business Intelligence (BI) simples.

## 📝 Objetivos

O projeto visa cumprir os seguintes requisitos:

1.  **Análise Exploratória de Dados (EDA):** Entender a estrutura do conjunto de dados, identificar as principais variáveis, e detectar problemas comuns como valores nulos e *outliers*.
2.  **Tratamento de Dados:** Aplicar um tratamento básico e justificado para tornar os dados utilizáveis para a análise.
3.  **Dashboard BI:** Criar um dashboard simples com indicadores e gráficos que contem a história do que influencia o preço das casas (*SalePrice*).
4.  **Comunicação:** Fornecer um resumo conciso das decisões tomadas e dos principais achados.

## 📦 Entregáveis

Os seguintes artefatos são o resultado deste desafio:

| Entregável | Descrição |
| :--- | :--- |
| **Notebooks de Código** | Arquivos Jupyter/Colab contendo o código Python utilizado para a limpeza, tratamento e análise exploratória dos dados, organizados de forma clara. |
| **Dashboard BI** | Um painel visual (neste caso, visualizações geradas em Python) que apresenta os principais *insights* e indicadores sobre o preço das casas. |
| **Justificativa** | Documentação das decisões tomadas, tanto em relação às técnicas de tratamento de dados (imputação de nulos, remoção de *outliers*) quanto às escolhas de visualização e indicadores para o dashboard. |

## 🛠️ Metodologia e Tratamento de Dados

A metodologia seguiu as etapas clássicas de um projeto de análise de dados, com foco especial no pré-processamento para garantir a qualidade dos *insights*.

### 1. Análise Exploratória (EDA)

A EDA inicial revelou um conjunto de dados com **81 variáveis** e **1460 observações** (antes da remoção de *outliers*). As variáveis foram classificadas em 38 numéricas e 43 categóricas.

**Principais Problemas Identificados:**

*   **Valores Nulos:** Muitas colunas (e.g., `PoolQC`, `MiscFeature`, `Alley`, `Fence`) apresentavam alta contagem de nulos, que, de acordo com a documentação do dataset, significam a ausência da característica (e.g., "Sem Piscina").
*   **Inconsistências:** Casos específicos como `MasVnrType` e `MasVnrArea` (Tipo e Área de Revestimento de Alvenaria) apresentavam valores inconsistentes (e.g., tipo preenchido, mas área zero).
*   ***Outliers***: Variáveis como `GrLivArea` (Área Habitável Acima do Solo) e `LotArea` (Área do Lote) continham valores extremos que poderiam distorcer a análise de correlação com o preço.

### 2. Estratégias de Tratamento

O tratamento de dados foi realizado com as seguintes decisões, visando a usabilidade do conjunto de dados:

| Variável/Problema | Estratégia de Tratamento | Justificativa |
| :--- | :--- | :--- |
| **Nulos Categóricos** | Preenchimento com o valor **"NA"** (Not Applicable) para indicar a ausência da característica (e.g., `PoolQC`, `Alley`). | Mantém a informação de ausência, tratando-a como uma categoria válida, conforme a descrição do dataset. |
| **`Electrical`** | Preenchimento do único valor nulo com a **moda** (valor mais frequente). | Apenas um valor faltante, a imputação pela moda minimiza o impacto na distribuição. |
| **`LotFrontage`** | Preenchimento com a **mediana** de `LotFrontage` agrupada por `Neighborhood` (Bairro). | A mediana é menos sensível a *outliers* e a média por bairro é uma estimativa mais contextualizada. |
| **`MasVnrType` / `MasVnrArea`** | Imputação de nulos em `MasVnrType` com 'None' e em `MasVnrArea` com 0. Correção de inconsistências (tipo != 'None' e área = 0) com a **mediana** da área para o respectivo tipo. | Garante a coerência entre o tipo de revestimento e sua área, corrigindo erros de registro. |
| ***Outliers*** | Remoção de observações com `GrLivArea` > 4000 e `SalePrice` < 300000, e `LotArea` > 100000. | Remove pontos de dados extremos que são raros e distorcem a relação entre área e preço, focando em residências típicas. |

## 📊 Principais Insights para o Dashboard

O dashboard final deve focar em variáveis que demonstrem forte correlação com o preço de venda (`SalePrice`). As análises preliminares indicam que as seguintes variáveis são cruciais:

| Variável | Tipo | Relação com `SalePrice` |
| :--- | :--- | :--- |
| **`OverallQual`** | Numérica (Ordinal) | Forte correlação positiva. Qualidade geral da casa é o principal preditor de preço. |
| **`GrLivArea`** | Numérica | Forte correlação positiva. Quanto maior a área habitável, maior o preço. |
| **`GarageCars` / `GarageArea`** | Numérica | Correlação positiva. O tamanho e a capacidade da garagem influenciam o preço. |
| **`TotalBsmtSF`** | Numérica | Correlação positiva. A área total do porão tem impacto direto no preço. |
| **`FullBath`** | Numérica | Correlação positiva. Casas com mais banheiros completos tendem a ter preços mais altos. |
| **`Neighborhood`** | Categórica | Forte influência. O bairro é um fator chave na determinação do preço. |
| **`PoolQC`** | Categórica | Forte influência. Caso a piscina tenha piscina e ela seja de ótima qualidade, afeta diretamente o preço da casa. |
| **`ExterQual `** | Categórica | Forte influência. Qualidade da área externa afeta essa variação de preço. |
| **`RoofMatl`** | Categórica | é visto que WdShngl tem maior potencial, seguido pelo mais comum(maioria das casas) CompShg, já os outros não tem um potencial grande para afetar positivamente o preço da casa. |

O dashboard incluem visualizações como mapas de calor de correlação, gráficos de dispersão (e.g., `SalePrice` vs. `GrLivArea`), e boxplots/gráficos de barras para variáveis categóricas de alta influência (e.g., `SalePrice` por `OverallQual` e `Neighborhood`).
Contudo, eles não aparecem no GitHub por conta de utilizar a biblioteca *plotly.express*, fazendo gráficos interativos. Só tem como ver esses gráficos pelo link do colab.

## ⚙️ Estrutura do Repositório

```
.
├── README.md               # Este arquivo.
├── ProjectCatiJR.ipynb     # Notebook original do Google Colab com a análise e tratamento.
├── data/                   # Diretório para o conjunto de dados ( train.csv).
```


