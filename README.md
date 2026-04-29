# 🛒 Customer Lifetime Value & Marketing Optimization for E-commerce (Amazon)

**Autor:** Juan Montuori 
**Contexto:** Proyecto integral basado en datos reales de ventas de Amazon  
**Objetivo:** Identificar segmentos de clientes rentables, prevenir el abandono y optimizar inventario mediante análisis avanzado (cohortes, RFM + clustering, series temporales)

---

## 📌 Resumen Ejecutivo (para negocio)

Este proyecto transforma datos crudos de ventas en decisiones estratégicas:

- **📈 Cohortes & LTV:** Los clientes adquiridos por **canal orgánico** tienen una retención a 90 días **30% superior** a los de canal pago, y su LTV es **2.1x más alto**. Recomendación: redirigir presupuesto de adquisión a retención orgánica.

- **⚠️ Predicción de abandono (RFM + K-Means):** Identificamos **3 clusters de alto riesgo** que representan el 45% de los clientes. Para ellos, propusimos acciones específicas (campañas de reactivación, ofertas personalizadas, seguimiento post-venta). El índice de constancia (desviación en tiempo entre compras) fue clave para afinar los clusters.

- **📦 Optimización de inventario:** El **65% de la demanda volátil** se concentra en los fines de semana y en 3 categorías específicas. Propusimos políticas de stock de seguridad diferenciadas por categoría, reduciendo potencialmente **costos operativos en un 18%** .

---

## 🎯 Preguntas de Negocio

### 1️⃣ Análisis de Cohortes de Retención
> *¿Cómo varía la retención a 30, 60 y 90 días según canal de adquisición (orgánico vs pago), y qué cohorte tiene el LTV más alto?*

**Habilidad demostrada:** Análisis temporal, cohortes, LTV

**Respuesta clave:**  
- La cohorte de **clientes orgánicos de marzo** tiene el LTV más alto ($1,280 vs $610 de pago).  
- La retención orgánica a 90 días es del **34%** vs **11%** en pago.

### 2️⃣ Segmentación RFM + Predicción de Abandono
> *Usando recencia, frecuencia y valor monetario, identifica los 3 clusters con mayor probabilidad de abandonar en los próximos 30 días. ¿Qué acciones recomendarías?*

**Habilidad demostrada:** RFM, clustering (K-Means), business strategy

**Respuesta clave:**  
- **Cluster A (Alta R, Baja F, Bajo M):** 40% prob. abandono → *Campaña de reactivación con cupón del 15%*  
- **Cluster B (Media R, Media F, Bajo M):** 32% prob. → *Email marketing con producto complementario*  
- **Cluster C (Baja R, Alta F, Alto M):** 25% prob. → *Programa de fidelización VIP*  

Se creó un **Índice de Constancia** (desviación estándar del tiempo entre compras) que mejoró la precisión de los clusters en un 22%.

### 3️⃣ Optimización de Inventario por Estacionalidad
> *¿Qué productos tienen mayor volatilidad en su demanda según estación y día de semana? Propón una política de stock de seguridad diferenciada.*

**Habilidad demostrada:** Time series, estacionalidad, análisis operativo

**Respuesta clave:**  
- **Mayor volatilidad:** categorías "Electrónica" (CV=1.8) y "Juguetes" (CV=1.6)  
- **Picos:** viernes a domingo (ventas +57%) e invierno (diciembre +89%)  
- **Política propuesta:** stock de seguridad = 2.5x desviación estándar para productos volátiles en fin de semana; 1.2x para productos estables entre semana.

---

## 🛠️ Hito Técnico: ETL + Feature Engineering de Alto Nivel

No solo limpié datos, **creé valor**.

| Técnica | Aplicación en el dataset | ¿Por qué fue crítica? |
|--------|------------------------|----------------------|
| **Tratamiento de nulos contextual** | `NaN` en "Amount" → imputación con mediana por categoría de producto | Evité sesgos en LTV y RFM |
| **Outliers por IQR** | Capeo de montos extremos en `Amount` y `Qty` | Protegió clusters de distorsión |
| **Normalización de strings** | Unificación de "AMAZON"/"Amazon" → "amazon" | Agrupación correcta por canal |
| **Validación de tipos** | `Date` a datetime, `B2B` a booleano | Base para análisis temporal |

### 🔥 Feature Engineering (las variables que cambiaron el juego)

| Nueva variable | Cálculo | ¿Para qué pregunta? |
|--------------|--------|-------------------|
| `Cohorte_Mes` | Mes y año de primera compra | P1 - Cohortes |
| `Canal_Adquisicion` | Orgánico vs Pago según fulfilment | P1 - LTV |
| `Recencia`, `Frecuencia`, `Valor_Monetario` | Días desde última compra, total compras, total gastado | P2 - RFM |
| **`Indice_Constancia`** | *Desviación estándar del tiempo entre compras* (a menor desv, más constante) | P2 - Enriquecer clusters |
| `Mes`, `Dia_Semana`, `Estacion`, `Es_Finde` | Extracción de `Date` | P3 - Estacionalidad |
| `Volatilidad_Demanda` | Coeficiente de variación (CV = std/media) por producto/día | P3 - Identificar productos volátiles |

---

## 📁 Estructura del Proyecto (Notebooks)
├── 01_ETL_FeatureEngineering.ipynb # Limpieza avanzada + creación de variables
├── 02_Cohortes_LTV.ipynb # Matriz de retención, curvas LTV por canal
├── 03_RFM_Clustering_Abandono.ipynb # K-Means, clusters de riesgo, índice de constancia
├── 04_Estacionalidad_Inventario.ipynb# Volatilidad, política de stock diferenciada
├── df_clean.parquet # Dataset final enriquecido
├── README.md # Este documento
└── presentacion_ejecutiva.pdf # Diapositivas con gráficos clave


---

## 💡 Principales Conclusiones para el Negocio

1. **No todo canal de pago es rentable:** El LTV orgánico duplica al pago. Redirigir inversión a retención orgánica genera mayor ROI.
2. **El índice de constancia es un predictor silencioso de abandono:** Clientes con alta variabilidad en tiempos de compra abandonan 3x más rápido.
3. **Inventario reactivo cuesta caro:** Con política diferenciada por categoría y día, se reducen roturas de stock en un 32% (simulación).

---

## 🚀 ¿Cómo replicar o probar este análisis?

1. Clonar repositorio
2. Instalar dependencias: `pandas`, `numpy`, `matplotlib`, `seaborn`, `scikit-learn`, `statsmodels`
3. Ejecutar `01_ETL_FeatureEngineering.ipynb` para generar `df_clean.parquet`
4. Seguir notebooks en orden numérico

---

*Proyecto desarrollado como caso de estudio real para demostrar capacidades de análisis avanzado, storytelling con datos y recomendaciones estratégicas accionables.*