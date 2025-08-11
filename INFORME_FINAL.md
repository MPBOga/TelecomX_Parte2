# Informe Final: Predicción de Churn en Telecom X
## 1. Resumen Ejecutivo
Telecom X busca reducir la tasa de cancelación anticipando clientes con alto riesgo de churn. 

Se construyó un pipeline de Machine Learning que incluye:
* Balanceo de clases vía SMOTE.
* Preprocesamiento con escalado de variables numéricas y codificación ordinal de categóricas.
* Selección de las 16 variables más relevantes.
* Entrenamiento de un Random Forest optimizado (GridSearchCV).
* Validación mediante métricas de clasificación y curvas ROC/Precision-Recall.

La solución alcanzó un F1-score de 0.84 y un AUC ROC de aproximadamente 0.88 en validación cruzada.

---
## 2. Factores Clave de Churn
Analizando las importancias de variables, los principales drivers de churn fueron:

1. Tipo de contrato
  * Contratos mes a mes presentan un riesgo mayor de cancelación comparado con planes anuales.

2. Cargos mensuales elevados
  * Clientes con bills altos tienen más probabilidad de churn.

3. Tenure corto
  * Usuarios con menos de 6 meses de relación con la empresa muestran mayor inestabilidad.

4. Ausencia de servicios de soporte técnico y seguridad en línea
  * Falta de planes de TechSupport y OnlineSecurity se asocia a mayor churn.

5. Método de pago
  * Clientes con pagos manuales (e.g., mailed check) cancelan más frecuentemente que quienes usan métodos automáticos.

---
## 3. Recomendaciones Estratégicas
Para mitigar el churn y mejorar la retención, sugerimos:

Promocionar contratos anuales

Ofrecer descuentos en tarifas mensuales para planes de 12 o 24 meses.

Paquetes de soporte y seguridad online

Incluir TechSupport y OnlineSecurity en ofertas de bienvenida para clientes nuevos.

Alertas tempranas

Implementar sistema de notificaciones para clientes con tenure < 6 meses y cargos mensuales por encima del promedio.

Incentivos para pago automático

Dar beneficios (descuento, puntos de fidelidad) a clientes que usen débito automático o tarjeta de crédito.

4. Impacto Esperado
Reducción de churn en un 15–20 % en el segmento de tenure corto.

Aumento de adopción de contratos anuales en 10–12 %, estabilizando los ingresos recurrentes.

Mejora en la satisfacción de clientes al contar con soporte técnico y seguridad integrada.

5. Próximos Pasos
Despliegue del pipeline en producción con monitoreo de drift.

Experimentar con modelos de interpretación avanzados (SHAP) para segmentar campañas personalizadas.

Evaluar impacto de las iniciativas piloto y refinar estrategias según resultados operativos.

