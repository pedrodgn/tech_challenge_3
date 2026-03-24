# ✈️ Tech Challenge — Fase 3 | Machine Learning Engineering

Análise e modelagem preditiva de atrasos em voos domésticos nos EUA (2015), desenvolvida como entrega da Fase 3 do programa POSTECH.

---

## 📂 Estrutura do Repositório

```
├── EDA.ipynb                        # Análise Exploratória de Dados
├── Modelo_não_supervisionado.ipynb  # Clusterização (K-Means + PCA)
├── supervised_model.ipynb           # Modelo de Regressão (XGBoost)
└── README.md
```

---

## 🗃️ Dados

A base é composta por três arquivos públicos:

| Arquivo | Descrição |
|---|---|
| `flights.csv` | ~5,8 milhões de voos domésticos (2015) |
| `airports.csv` | Informações dos aeroportos (cidade, estado, lat/lon) |
| `airlines.csv` | Nome completo das companhias aéreas |

> **Download:** [Base de dados — MLET Fase 3](https://www.kaggle.com/datasets/usdot/flight-delays)

---

## 📊 EDA — Análise Exploratória

Investiga padrões nos dados respondendo a seis perguntas principais:

- **Aeroportos críticos:** hubs de Chicago (ORD) e Nova York (JFK/EWR) lideram pelo volume + atraso combinados
- **Dia/hora:** segundas-feiras e voos noturnos concentram os maiores atrasos
- **Sazonalidade:** junho é o pior mês; feriados concentram os piores dias individuais
- **Companhias:** Spirit e Frontier atrasam mais; Alaska e Hawaiian são as mais pontuais
- **Causa principal:** aeronave atrasada (*late aircraft*) e falha operacional da companhia geram mais minutos perdidos; clima é raro, mas produz os maiores atrasos médios
- **Distância:** correlação quase nula — voos muito curtos atrasam mais por operarem em hubs congestionados

---

## 🤖 Modelo Não Supervisionado

**Técnicas:** K-Means + PCA (redução para 2D)

**Resultados dos clusters de aeroportos (k=2):**

| Cluster | Perfil | Volume médio | Atraso médio |
|---|---|---|---|
| 0 | Aeroportos regionais/secundários | ~1.915 voos | 0,45 min |
| 1 | Grandes hubs | ~32.657 voos | 12,6 min |

Silhouette Score ≈ **0.67** — separação excelente entre grupos.

---

## 🎯 Modelo Supervisionado

**Tarefa:** Regressão — prever `ARRIVAL_DELAY` usando apenas informações disponíveis antes da decolagem (sem *data leakage*).

**Feature engineering:** médias históricas expandidas por companhia/rota/aeroporto, efeito cascata (`PREVIOUS_FLIGHT_DELAY`), parte do dia e estação do ano.

**Comparação de modelos (RMSE em validação):**

| Modelo | RMSE |
|---|---|
| **XGBoost** | **~33,4 min** ✅ |
| Random Forest | ~33,4 min |
| Ridge (Linear) | ~35,0 min |

**Métricas no conjunto de teste:**
- MAE ≈ **16,3 minutos**
- RMSE ≈ **33,4 minutos**

**Features mais importantes:**

| # | Feature | Importância |
|---|---|---|
| 1 | `PREVIOUS_FLIGHT_DELAY` | ~42,5% |
| 2 | `BACK_TO_BACK_FLIGHTS` | ~32,2% |
| 3 | `PART_OF_DAY_NUM` | ~3,7% |

O modelo falha principalmente em atrasos extremos (>1.000 min) causados por eventos imprevisíveis como clima severo ou falhas operacionais pontuais.

---

## 🛠️ Tecnologias

`Python` · `Pandas` · `Scikit-learn` · `XGBoost` · `Matplotlib` · `Seaborn`

---

## 👤 Autor

**Matheus Pavani** — POSTECH Machine Learning Engineering
