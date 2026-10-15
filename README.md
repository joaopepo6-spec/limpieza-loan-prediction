# Limpieza y Análisis Exploratorio — Loan Prediction Dataset

## Origen de los datos

Dataset: **Loan Prediction Problem Dataset** (originalmente del hackathon "Loan Prediction" de Analytics Vidhya, replicado en Kaggle). Contiene 614 registros de solicitudes de préstamos bancarios, con datos del solicitante (género, estado civil, dependientes, educación, ingresos) y el resultado de la solicitud (aprobado/rechazado).

## Columnas

| Columna | Descripción |
|---|---|
| Loan_ID | Identificador único de la solicitud |
| Gender | Género del solicitante |
| Married | Estado civil |
| Dependents | Cantidad de personas a cargo |
| Education | Nivel educativo (Graduate / Not Graduate) |
| Self_Employed | Si el solicitante es autónomo |
| ApplicantIncome | Ingreso del solicitante |
| CoapplicantIncome | Ingreso del co-solicitante |
| LoanAmount | Monto del préstamo solicitado (en miles) |
| Loan_Amount_Term | Plazo del préstamo en meses |
| Credit_History | Historial crediticio (1 = cumple requisitos, 0 = no) |
| Property_Area | Zona de la propiedad (Urban / Semiurban / Rural) |
| Loan_Status | Resultado: Y (aprobado) / N (rechazado) |

## Decisiones de limpieza

El dataset original tenía valores nulos en 7 de las 13 columnas. Se trataron según el tipo de variable:

- **Gender, Married, Self_Employed** (categóricas): se rellenaron con la **moda** de cada columna, ya que no existe un "promedio" posible para texto.
- **Dependents**: además de rellenar con la moda, se corrigió el valor "3+" (texto) a "3" y se convirtió la columna de `object` a `int64`, ya que originalmente no podía tratarse como numérica.
- **LoanAmount**: se rellenó con la **mediana**, en lugar del promedio, porque la distribución tiene asimetría (hay préstamos de montos muy altos que distorsionan el promedio).
- **Loan_Amount_Term** y **Credit_History**: al ser variables casi categóricas (plazos típicos y un flag binario), se rellenaron con la **moda** y luego se convirtieron a `int64` (antes eran `float64` solo por la presencia de nulos).
- **Duplicados**: se verificaron con `.duplicated().sum()`, resultando en 0 filas duplicadas — no fue necesario eliminar ni resetear el índice.

## Agregaciones de negocio

1. **Monto promedio de préstamo por zona**: los préstamos rurales son, en promedio, los de mayor monto ($151.4k vs $145.1k en zonas semiurbanas y $141.4k en zonas urbanas).
2. **Aprobación según estado civil**: los solicitantes casados tienen una tasa de aprobación más alta (~72%) que los solteros (~63%).
3. **Ingreso promedio por nivel educativo**: los solicitantes graduados tienen un ingreso promedio ~55% mayor que los no graduados ($5857 vs $3777).

## Cómo correrlo

El notebook `Limpieza_Loan_Prediction.ipynb` se puede abrir directamente en Google Colab. Requiere el archivo `train_u6lujuX_CVtuZ9i.csv` (incluido en este repositorio) en la misma sesión.
