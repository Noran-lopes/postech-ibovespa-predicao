
# 📈 Tech Challenge – Fase 2
## Previsão de Tendência D+1 do IBOVESPA

**Autor:** Victor Noran Lopes Porcela da Silva  
**Objetivo:** Prever se o IBOVESPA fecha **em alta (↑)** ou **em baixa (↓)** no **dia seguinte (D+1)**, usando **dados históricos diários** do próprio índice.

---

## 🎯 Escopo do Desafio

- Frequência: **diária**
- Histórico: **≥ 2 anos**
- **Teste**: **últimos 30 pregões** (hold-out)
- **Meta**: **acurácia ≥ 75%** no conjunto de teste
- Uso do modelo: insumo para **dashboards internos** e apoio à decisão de **analistas quantitativos**

---

## 🗂️ Dados

- **Índice:** IBOVESPA  
- **Fonte:** Investing (dados históricos públicos)  
- Pré-processamento:
  - Conversão e padronização de campos numéricos
  - Ordenação temporal
  - Remoção de inconsistências
- **Target (D+1):**
  - `1` se **Fechamento(t+1) > Fechamento(t)**
  - `0` caso contrário

> Todos os atributos e transformações são calculados **apenas com informações até t**, evitando **data leakage**.

---

## 🧠 Features (visão geral)

- **Preço/Retorno:** `pct_change`, retornos acumulados curtos  
- **Tendência:** MAs (3/7/14/21/30), `gap` preço–MA  
- **Volatilidade:** desvio-padrão (5/10/20), **ATR(14)**, **largura das Bandas de Bollinger (20,2)**  
- **Momentum/Volume:** **RSI(14)**, **MACD** (linha/sinal/hist), **OBV**  
- **Calendário:** dia da semana

---

## 🧪 Metodologia

- **Split temporal:**  
  - **Treino:** histórico (t0 → t-30)  
  - **Teste:** **últimos 30 pregões** (hold-out)
- **Validação:** `TimeSeriesSplit` (simula uso real: treinar no passado, validar no futuro)
- **Seleção de modelo:** baseada **no treino/validação temporal** (teste usado **uma única vez**)

---

## 🤖 Modelos Avaliados

- Regressão Logística
- **Random Forest** (modelo final)
- SVM Linear
- XGBoost / LightGBM / CatBoost
- Ensemble / Stacking

---

## ✅ Resultados (Teste: últimos 30 pregões)

- **Random Forest:** **80%** de acurácia  
- **Baseline “sempre ↑”:** **60%**  

O modelo final **supera a meta de 75%** e apresenta ganho real sobre uma heurística trivial.

