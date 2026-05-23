#  Decisiones técnicas 

##  1. Plataforma utilizada (GCP + BigQuery)

Se utilizó Google Cloud Platform como infraestructura principal debido a:

- Escalabilidad automática de BigQuery
- Facilidad de integración con fuentes estructuradas
- Bajo mantenimiento de infraestructura
- Alta capacidad para procesamiento analítico

BigQuery fue seleccionado como Data Warehouse central por su enfoque serverless y optimización para consultas analíticas.

---

##  2. Enfoque ELT (Extract, Load, Transform)

Se implementó un enfoque ELT en lugar de ETL debido a que:

- BigQuery permite transformación eficiente directamente sobre el data warehouse
- Se reduce complejidad en la capa de ingesta
- Se mejora la trazabilidad de los datos
- Permite mayor flexibilidad en la creación de modelos analíticos

La transformación se realizó principalmente en la capa analítica (STAGING - DevDatoscaso y DATA MART - PrdDatoscaso).

---

##  3. Arquitectura en capas (Medallion)

Se implementó una arquitectura en dos capas:

- STAGING - DevDatoscaso: limpieza, normalización y estandarización
- ANALYTICS (DATA MART) - PrdDatoscaso: datos agregados para negocio

Este enfoque permite separar responsabilidades y asegurar la calidad del dato antes del consumo analítico.

---

##  4. Modelado de datos (Data Mart)

Se diseñó un modelo orientado a negocio basado en tablas tipo FACT:

- fact_creditos
- fact_datos_diarias
- Vista 360 del crédito

Este diseño permite responder preguntas de negocio de forma eficiente sin joins complejos en tiempo de consulta.

---

##  5. Calidad de datos

Se implementaron reglas básicas de calidad:

- Validación de campos obligatorios (nulos en llaves primarias)
- Eliminación de duplicados
- Validación de valores positivos en montos financieros
  
Los datos inconsistentes no se eliminan del sistema sin control, sino que se identifican para análisis posterior.

---

##  6. Supuestos del modelo

- Los datos provienen de sistemas transaccionales estructurados
- Un crédito puede tener múltiples cuotas y pagos asociados
- Existen posibles inconsistencias en fechas de pago
- BigQuery actúa como única fuente analítica de verdad

---

##  7. Riesgos identificados

- Inconsistencias en el tipo de datos de origen
- Calidad variable en datos transaccionales

---

##  8. Decisiones de diseño

- Se priorizó simplicidad sobre complejidad de arquitectura
- Se evitó el uso de herramientas de orquestación complejas para mantener el alcance del caso
- Se centralizó la lógica de negocio en SQL para facilitar auditoría y mantenimiento
