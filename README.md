>[!NOTE]
># IDE-001-fintrust Prueba Técnica - Ingeniero de Datos - ceiba

  Contexto del problema

FinTrust es una fintech de crédito de consumo que requiere optimizar la generación de indicadores diarios relacionados con:

- Originación de créditos
- Recaudo de pagos
- Mora y recuperación de cartera

Actualmente, el proceso es manual, depende de múltiples fuentes de información, lo que genera:
- retrasos de 1 a 2 días en la generación de reportes
- inconsistencias en los datos
- alta carga operativa para el equipo financiero

---

##  Objetivo de la solución

Diseñar e implementar una solución de datos en Google Cloud Platform (GCP) que:

- Automatice la ingesta de datos hacia BigQuery
- Estandarice y limpie información transaccional
- Construya un modelo analítico confiable (Data Mart)
- Permita análisis de negocio en herramientas BI (Power BI, Tableau, Looker)
- Mejore la calidad y disponibilidad de los datos diarios

---

##  Preguntas de negocio que debe resolver la solución

La plataforma permite responder preguntas como:

- ¿Cuánto desembolso se originó por día y ciudad?
- ¿Cuál es el saldo vigente y vencido por segmento?
- ¿Qué porcentaje del recaudo del día cubrió cuotas en mora?
- ¿Qué cohortes muestran mayor deterioro temprano?


---
##  Arquitectura de la solución

Se implementó una arquitectura en capas tipo Medallion sobre BigQuery, utilizando únicamente las capas Silver y Gold para este proyecto:


Fuentes transaccionales (clientes, créditos, cuotas, pagos)
BigQuery STAGING (limpieza y estandarización)
BigQuery ANALYTICS / DATA MART (modelo de negocio)
Herramientas BI (Power BI)


###  Capa STAGING - DevDatoscaso

Se aplican procesos de:

- normalización de tipos de datos
- validación de campos obligatorios
- limpieza de valores inconsistentes


###  Capa ANALYTICS (Data Mart) - PrdDatoscaso

Se construyen tablas orientadas a negocio:

- originación diaria
- recaudo diario
- mora y cartera vencida
- vista 360 del crédito


##  Data Mart PrdDatoscaso

El modelo analítico se estructuró de tipo estrella y está compuesto por las siguientes tablas:

- fact_creditos
- fact_datos_diarias
- clientes
- creditos
- cuotas_pro_credito
- pagos

Dando respuesta a las pregunta de negocio en la capa analítica

### 1. Originación diaria
- créditos otorgados por día
- monto total desembolsado
- ticket promedio

### 2. Recaudo diario
- pagos recibidos por día
- total recaudado
- promedio de pago

### 3. Mora diaria
- créditos en mora
- saldo vencido
- días de mora promedio

### 4. Vista 360 del crédito
- saldo actual
- total pagado
- categoría de mora 


##  Flujo de datos

1. Ingesta de datos desde archivos PDF hacia BigQuery 
2. Carga de datos  y estandarización en capa STAGING - DevDatoscaso
3. Construcción de Data Mart en capa ANALYTICS - PrdDatoscaso
4. Generación de KPIs para análisis de negocio

---

##  Automatización (Python)

Se desarrollaron scripts en Python para:

- Ejecutar validaciones básicas de calidad

---

##  Resultados esperados

La solución permite:

- Reducir el tiempo de consolidación de indicadores diarios
- Mejorar la confiabilidad de los datos financieros
- Habilitar análisis de negocio en tiempo casi real
- Centralizar la información en una única fuente confiable

---

##  Mejoras futuras

- Implementación de CDC (Change Data Capture)
- Orquestación con Cloud Composer (Airflow en GCP)
- Implementación de reglas avanzadas de Data Quality
- Uso de modelos de Machine Learning para predicción de mora
- Integración de LLMs para análisis automático de KPIs y anomalías

---

##  Entregables

- Scripts SQL organizados por capas (STAGING, ANALYTICS)
- Data Mart analítico en BigQuery
- Script de automatización calidad basica en Python
- Documentación técnica del proyecto
