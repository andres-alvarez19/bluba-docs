# Bluba Anticipa — Dataset sintético DATA-04

## Propósito
Dataset sintético creado exclusivamente para demostrar el funcionamiento del prototipo Bluba Anticipa.

**No debe utilizarse para afirmar rendimiento clínico, precisión predictiva real, sensibilidad, especificidad ni validación médica.**

## Alcance
- 12 niños sintéticos.
- Periodo: 2026-05-01 a 2026-06-29 (60 días calendario).
- 699 registros diarios observados; existen omisiones intencionales.
- 180 sesiones profesionales.
- 16 eventos sintéticos de desregulación.
- 660 ventanas de predicción a 24 h.
- 34 resultados de intervenciones.
- 23 escenarios de QA/demostración.

## Escenarios incluidos
- Días normales.
- Deterioro gradual con y sin evento.
- Cambios de rutina con y sin evento.
- Sueño alterado con y sin evento.
- Acumulación de múltiples factores con y sin evento.
- Información incompleta con y sin evento.
- Eventos de aparición relativamente abrupta para evitar separación perfecta.

## Archivos
01_child.csv
02_user.csv
03_user_child_access.csv
04_daily_record.csv
05_professional_session.csv
06_dysregulation_event.csv
07_risk_prediction.csv
08_intervention.csv
09_prediction_intervention.csv
10_intervention_result.csv
11_scenario_manifest.csv
12_ml_prediction_dataset.csv
13_data_dictionary.csv

Todos los CSV usan separador `;` y UTF-8 con BOM.

## Reglas
1. `scenario_id` y `scenario_type` son metadatos de QA. **No usar como features.**
2. `target_dysregulation_24h` usa eventos en `(prediction_at, prediction_at + 24h]`.
3. Features históricas usan solo información anterior o igual a `prediction_at`.
4. Valores faltantes permanecen faltantes y no equivalen a normalidad.
5. `risk_score_demo` es una regla sintética ilustrativa; no una probabilidad clínicamente calibrada.
6. Resultados preventivos sin evento no prueban causalidad.
7. Horas de sueño, cambio de rutina, comportamiento y evento excepcional se ampliaron sintéticamente porque no están disponibles como columnas estructuradas completas en el dataset base.

## Uso recomendado
Demo de dashboard, pruebas de API/backend, validación ER, feature engineering, riesgo vs confianza y pruebas de datos incompletos.

## No recomendado
Entrenar un modelo clínico ni reportar métricas de rendimiento como evidencia de eficacia.
