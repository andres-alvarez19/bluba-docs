# QA — DATA-04 Dataset sintético

## Validaciones principales

- Niños sintéticos: 12
- Días calendario: 60
- Días-caso potenciales: 720
- Registros diarios presentes: 699
- Días-caso completamente omitidos: 21
- Sesiones profesionales: 180
- Eventos de desregulación: 16
- Ventanas predictivas: 660
- Targets positivos a 24 h: 16
- Filas con completitud < 0.70: 21
- Predicciones demo de riesgo alto sin evento posterior: 73
- Eventos cuyo score demo previo fue bajo/medio (<0.65): 7

## Cobertura de escenarios

| Escenario | Cantidad |
|---|---:|
| ACCUMULATION_NO_EVENT | 2 |
| ACCUMULATION_WITH_EVENT | 3 |
| GRADUAL_DETERIORATION_NO_EVENT | 2 |
| GRADUAL_DETERIORATION_WITH_EVENT | 2 |
| INCOMPLETE_NO_EVENT | 2 |
| INCOMPLETE_WITH_EVENT | 2 |
| ROUTINE_CHANGE_NO_EVENT | 3 |
| ROUTINE_CHANGE_WITH_EVENT | 3 |
| SLEEP_ALTERED_NO_EVENT | 2 |
| SLEEP_ALTERED_WITH_EVENT | 2 |

## Comprobaciones conceptuales

1. Existen días normales.
2. Existen deterioros graduales con y sin evento posterior.
3. Existen cambios de rutina con y sin evento posterior.
4. Existen secuencias de sueño alterado con y sin evento posterior.
5. Existen acumulaciones multifactoriales con y sin evento posterior.
6. Existen registros completamente ausentes y campos parcialmente ausentes.
7. La presencia de factores de riesgo no determina automáticamente el target.
8. También existen eventos con señales previas limitadas.
9. `scenario_id` y `scenario_type` son exclusivamente metadatos de QA.
10. El target se calcula en `(prediction_at, prediction_at + 24 h]`.
11. Los eventos posteriores a `prediction_at` no se utilizan para las features de esa predicción.

## Importante

El score `risk_score_demo` fue construido con una regla sintética explícita para poder demostrar UI, API, explicabilidad y confianza. No corresponde a un modelo clínicamente validado ni debe utilizarse para reportar métricas de eficacia del sistema.
