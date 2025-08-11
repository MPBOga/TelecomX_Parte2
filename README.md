# Telecom X Parte 2 – Churn Prediction Pipeline

[![Python](https://img.shields.io/badge/python-3.8%2B-blue)](https://www.python.org/)  
[![scikit-learn](https://img.shields.io/badge/scikit--learn-1.0%2B-orange)](https://scikit-learn.org/)  
[![imbalanced-learn](https://img.shields.io/badge/imbalanced--learn-0.9%2B-green)](https://imbalanced-learn.org/)  

## Tabla de Contenidos

- [Descripción del Proyecto](#descripción-del-proyecto)  
- [Estructura del Repositorio](#estructura-del-repositorio)  
- [Datos](#datos)  
- [Preprocesamiento](#preprocesamiento)  
- [Selección de Variables](#selección-de-variables)  
- [Modelado](#modelado)  
  - [Modelos Entrenados](#modelos-entrenados)  
  - [Búsqueda de Hiperparámetros](#búsqueda-de-hiperparámetros)  
- [Interpretación del Modelo](#interpretación-del-modelo)  
- [Serialización del Pipeline](#serialización-del-pipeline)  
- [Reproducción del Experimento](#reproducción-del-experimento)  
- [Requisitos](#requisitos)  
- [Licencia](#licencia)  

---

## Descripción del Proyecto

Telecom X busca anticipar qué clientes tienen alta probabilidad de cancelar sus servicios (churn).  
Este repositorio contiene:

1. Preprocesamiento de datos (SMOTE, escalado, encoding).  
2. Selección de las 16 variables más importantes.  
3. Entrenamiento y validación de un RandomForest optimizado.  
4. Interpretación de resultados y exportación de un pipeline listo para producción.  

---

## Estructura del Repositorio

├── data/

│ └── df_estandarizado.csv

│ └── README.md

├── notebooks/

│ └── 01_EDA.ipynb

│ └── 02_Preprocessing.ipynb

│ └── 03_Modeling.ipynb

├── src/

│ ├── preprocessing.py

│ ├── modeling.py

│ └── utils.py

├── models/

│ └── telecomx_churn_pipeline_<timestamp>.joblib

├── requirements.txt

└── README.md


---

## Datos

- `customerID`: ID único del cliente  
- `Churn`: 0 = no canceló, 1 = canceló  
- **Variables Numéricas Seleccionadas**  
  - `customer.tenure`  
  - `account.Charges.Monthly`  
  - `account.Charges.Total`  
  - `Cuentas_Diarias` (Charges.Monthly / tenure)  
- **Variables Categóricas Seleccionadas**  
  - `account.Contract`  
  - `internet.TechSupport`  
  - `internet.OnlineSecurity`  
  - `account.PaymentMethod`  
  - `internet.OnlineBackup`  
  - `internet.InternetService`  
  - `customer.Partner`  
  - `customer.Dependents`  
  - `account.PaperlessBilling`  
  - `internet.DeviceProtection`  
  - `internet.StreamingMovies`  
  - `customer.SeniorCitizen`  

---

## Preprocesamiento

1. **Balanceo**  
   - SMOTE para generar ejemplos sintéticos de churn (clase minoritaria).  
2. **Escalado**  
   - `StandardScaler` a las variables numéricas.  
3. **Encoding**  
   - `OrdinalEncoder` aplicado a 12 variables categóricas.  
4. **Pipeline**  
   ```python
   from sklearn.compose import ColumnTransformer
   from sklearn.preprocessing import StandardScaler, OrdinalEncoder

   preprocessor = ColumnTransformer([
       ("num", StandardScaler(), selected_numeric),
       ("cat", OrdinalEncoder(),   selected_categorical)
   ], remainder="drop")

## Selección de Variables

Se calcularon las importancias finales con rf_best.feature_importances_.

Top 5 variables:


Feature	Importance
account.Contract	      0.1491
customer.tenure	        0.1383
account.Charges.Total	  0.1372
account.Charges.Monthly	0.1275
Cuentas_Diarias	        0.1267

Se mantuvieron las 16 variables con mayor aporte acumulado.

## Modelado

### Modelos entrenados
* LogisticRegression
* RandomForestClassifier
* XGBClassifier (opcional)

### Búsqueda de Hiperparámetros

Se usó GridSearchCV sobre 24 combinaciones y 5-fold CV:

Mejores parámetros:
{
  "n_estimators": 200,
  "max_depth": 20,
  "min_samples_leaf": 1,
  "class_weight": None
}
Mejor F1 (CV): 0.8377

## Interpretación del Modelo
* Gráfico de feature_importances_ para las top 20 variables.
* Umbral óptimo (Youden’s J) de la curva ROC (AUC ≈ 0.88).
* Métricas en test: precision, recall y F1 para cada clase.

## Serialización del Pipeline
import joblib
from datetime import datetime

timestamp = datetime.now().strftime("%Y%m%d_%H%M%S")
joblib.dump(pipeline, f"models/telecomx_churn_pipeline.joblib")

