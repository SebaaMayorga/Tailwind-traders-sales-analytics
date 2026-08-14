# 📊 Dashboard de Performance Comercial: Tailwind Traders Analysis

## 🎯 Objetivo del Proyecto
Transformar datos heterogéneos y desestructurados de ventas globales de *Tailwind Traders* en una herramienta interactiva y dinámicamente convertida a USD para la toma de decisiones estratégicas. El enfoque principal consistió en ejecutar un proceso completo de ETL (incluyendo integración con Python), modelado de datos relacional y desarrollo de medidas en DAX para responder a preguntas críticas sobre la rentabilidad, volumen transaccional y comportamiento geográfico del negocio.

---

## 📂 Recursos del Proyecto
* **Dataset Utilizado:** https://github.com/SebaaMayorga/Tailwind-traders-sales-analytics/blob/main/Data/Countries.xlsx.
https://github.com/SebaaMayorga/Tailwind-traders-sales-analytics/blob/main/Data/Purchases.xlsx.
https://github.com/SebaaMayorga/Tailwind-traders-sales-analytics/blob/main/Data/Tailwind-Traders-Sales.xlsx.

* **Herramientas:** Power BI Desktop / Power Query / Python (Pandas) / DAX.
* **Técnicas:** Limpieza e ingesta heterogénea, tipado estricto, script en Python para divisas, creación de tabla Calendario, modelado relacional 1:1 y 1:* y métricas avanzadas de Time Intelligence.

---

## ❓ Preguntas de Negocio Resueltas (KPIs)
Siguiendo un enfoque de análisis de datos profesional, el reporte responde a:

* **Salud Financiera Global (KPIs):** ¿Cuál es el nivel de inventario, volumen de ventas y la mediana de ingresos por transacción? 
  * *Solución:* Implementación de tarjetas dinámicas (Stock: 14 mil, Cantidad comprada: 152 unidades, Mediana de Ventas: $222,50 USD).
* **Lealtad y Desempeño Regional:** ¿Qué países concentran la mayor fidelidad de clientes y cuál es su contribución a los ingresos?
  * *Solución:* Gráfico de barras de Puntos de Fidelización (liderado por UK con 315 puntos y USA con 305) combinado con un gráfico de dona para la Distribución de la Mediana de Ventas (encabezado por UAE con el 45,07%).
* **Rentabilidad por Producto:** ¿Cuáles son los artículos que generan un mayor margen e ingresos netos?
  * *Solución:* Ranking de productos por ingresos netos, destacando el *Modular Sofa Set* ($928 USD) y productos de alta rotación como *Floral Wallpaper*.
* **Evolución Temporal y Márgenes:** ¿Cómo se comporta la rentabilidad a lo largo del tiempo y existen caídas puntuales en los márgenes?
  * *Solución:* Gráficos de tendencia de la Mediana de Ventas e Inteligencia Temporal para el Margen Anual (identificando un margen histórico de 80%-90% con una caída puntual en octubre de 2023).

---

## 🖼️ Dashboard Final

### 1. Resumen de Ventas
*https://github.com/SebaaMayorga/Tailwind-traders-sales-analytics/blob/main/Img/Resumen%20de%20ventas-Dashboard.PNG*

### 2. Resumen de Beneficios
*https://github.com/SebaaMayorga/Tailwind-traders-sales-analytics/blob/main/Img/Resumen%20de%20beneficios-Dashboard.PNG*

---

## 🛠️ Mi Proceso de Trabajo (ETL, Modelado y DAX)

### 1. Ingesta, Limpieza y Transformación (Power Query & Python)
* **Cálculos Financieros Iniciales:** Creación de columnas derivadas para Coste por Unidad, Ingresos Brutos, Impuestos, Ingresos Netos y Beneficio.
* **Tipado Estricto:** Asignación de tipos de datos en Power Query (`OrderID` como Entero, `Precio Bruto` como Decimal Fijo, etc.).
* **Integración de Divisas con Python:** Ejecución de un script con la librería `pandas` para ingerir dinámicamente las tasas de cambio históricas (USD, GBP, EUR, AED, AUD).

python:
import pandas as pd
from io import StringIO

data = """Exchange ID;ExchangeRate;Exchange Currency
1;1;USD
2;0.75;GBP
3;0.85;EUR
4;3.67;AED
5;1.3;AUD"""

df = pd.read_csv(StringIO(data), sep=';')

---

# 2. Modelado de Datos Relacional
Construcción de un esquema en estrella conectando las tablas Sales, Purchases, Countries, df (Exchange) y Sales in USD.

Tabla Dimensión Calendario: Creación de la tabla Calendar mediante DAX para análisis de Inteligencia de Tiempo (2020 - 2023).

Estandarización Multimoneda: Generación de la tabla calculada Sales in USD con la función RELATED para convertir las transacciones a una moneda homogénea.

---

# 3. Cálculos DAX Avanzados
Margen de Beneficio Anual = 
DIVIDE(
    SUM('Sales in USD'[Ingresos Netos USD]), 
    SUM('Sales in USD'[Ingresos Brutos USD])
)

Beneficio Trimestral = CALCULATE(SUM('Sales in USD'[Ingresos Netos USD]), DATESQTD('Calendar'[Date]))

Beneficio YTD = TOTALYTD(SUM('Sales in USD'[Ingresos Netos USD]), 'Calendar'[Date])

Mediana de Ventas = MEDIAN('Sales in USD'[Ingresos Brutos USD])

---

## 💡 Insights Clave

* **Equilibrio Geográfico del Margen:** El margen de beneficio anual se encuentra distribuido de manera notablemente equitativa entre todos los países analizados (~20% por región entre UK, UAE, Australia, USA y Francia).

* **Atípicos Financieros:** La mediana de ventas se mantuvo estable en $222,50 USD, pero el análisis temporal identificó un pico cercano a los $1.000 USD a mediados de septiembre y una caída pronunciada en el margen de beneficio en octubre de 2023, alertando sobre variaciones operativas en ese período.

* **Concentración del Margen de Producto:** A pesar de que productos como Floral Wallpaper dominan en volumen de unidades vendidas (6 unidades), el verdadero driver de ingresos netos es el Modular Sofa Set ($928 USD), lo que sugiere enfocar esfuerzos promocionales en esta categoría de alto valor.

---

*Desarrollado por Sebastian Mayorga - Data Analyst*
