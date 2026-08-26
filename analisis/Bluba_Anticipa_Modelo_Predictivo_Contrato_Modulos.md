# Bluba Anticipa — Especificación del modelo predictivo e integración con Backend/Frontend

**Estado:** definición técnica consolidada para MVP NeuroHack 2026  
**Objetivo del documento:** establecer una única referencia para implementar el motor predictivo de Bluba Anticipa y definir con precisión qué información recibe y qué respuestas entrega a los demás módulos.

---

## 1. Propósito del motor

El motor de Bluba Anticipa debe estimar el **riesgo de que ocurra al menos un evento de desregulación durante las próximas 24 horas**, utilizando únicamente información conocida hasta un instante de corte `prediction_at`.

La unidad de análisis no es una fila aislada del dataset. El problema debe tratarse como un **problema longitudinal por niño**, donde el sistema combina:

1. estado actual;
2. desviación respecto de la línea base individual;
3. acumulación de factores durante los últimos días;
4. combinación simultánea de factores;
5. historial reciente de desregulaciones;
6. calidad y completitud de la información disponible.

El sistema debe producir dos resultados independientes:

- **Riesgo:** qué tan compatible es el patrón actual con una posible desregulación en las próximas 24 h.
- **Confianza:** qué tan sólida es la información disponible para sostener esa estimación.

El motor no debe presentarse como un predictor clínicamente validado. En el MVP se utiliza información real limitada y un dataset sintético para demostrar la arquitectura, los contratos de integración y la experiencia de usuario.

---

# 2. Principio central

La lógica del sistema se resume en:

```text
DATOS OBSERVADOS HASTA T
        │
        ▼
NORMALIZACIÓN
        │
        ▼
LÍNEA BASE INDIVIDUAL
        │
        ├─────────────► Estado actual
        ├─────────────► Desviación vs. baseline
        ├─────────────► Acumulación 72 h
        ├─────────────► Historial 7 días
        └─────────────► Interacciones
                              │
                              ▼
                         MOTOR DE RIESGO
                              │
                              ├────────► Risk score
                              └────────► Factores explicativos

CALIDAD DE LOS DATOS
        │
        ▼
MOTOR DE CONFIANZA
        │
        └─────────────► Confidence score
```

La predicción siempre representa:

```text
estado conocido en T
        ↓
riesgo en (T, T + 24 h]
```

---

# 3. Lo que los datos actuales permiten afirmar

El análisis del dataset base muestra:

- 4 casos anonimizados;
- 102 registros de seguimiento diario;
- 37 sesiones profesionales;
- 7 eventos de desregulación;
- 10 objetivos terapéuticos;
- cobertura diaria irregular.

Aunque los CSV no contienen `NULL` internos, existen **22 días-caso sin seguimiento**, por lo que la cobertura longitudinal es aproximadamente 82,3 %. Además, las fuentes son asincrónicas: una sesión profesional puede existir sin seguimiento familiar del mismo día y viceversa.

El hallazgo más importante es que existen solamente **7 eventos positivos reales**. Esa cantidad es insuficiente para entrenar y validar responsablemente un modelo supervisado que pueda presentarse como clínicamente predictivo.

Por lo tanto:

> **Los datos reales sirven para diseñar el schema, construir la línea temporal, verificar relaciones, crear features y demostrar el flujo. No sirven para afirmar eficacia clínica.**

El dataset sintético amplía el escenario a:

- 12 niños sintéticos;
- 60 días;
- 699 registros diarios;
- 180 sesiones profesionales;
- 16 eventos sintéticos;
- 660 ventanas predictivas;
- 16 targets positivos a 24 h;
- 21 filas con completitud inferior a 0,70;
- escenarios con deterioro gradual, sueño alterado, cambio de rutina, acumulación de factores, información incompleta y eventos con pocas señales previas.

El dataset sintético debe utilizarse para **QA, demo, API, UI, feature engineering y pruebas del pipeline**, no para reportar métricas como evidencia clínica.

---

# 4. Target del modelo

## 4.1 Target primario

```text
desregulacion_proximas_24h
```

Tipo:

```text
boolean
```

Definición exacta:

```text
1 si existe al menos un DysregulationEvent del mismo niño donde:

prediction_at < occurred_at <= prediction_at + 24 horas

0 en caso contrario.
```

Formalmente:

```text
target(T) = 1
si existe evento en (T, T+24h]
```

## 4.2 Regla crítica

`nivel_regulacion_general_dia = "Desregulación Frecuente"` **NO es el target**.

El análisis del dataset muestra que la regulación general del día y la existencia de un evento registrado no son equivalentes. La regulación debe utilizarse como una **feature histórica/contextual**, mientras que el target se obtiene exclusivamente del historial de eventos de desregulación.

---

# 5. Instante de corte y prevención de data leakage

Cada predicción debe tener un `prediction_at`.

Regla obligatoria:

```text
feature.timestamp <= prediction_at
```

Nunca se puede utilizar información posterior a `prediction_at`.

Ejemplo:

```text
08:00  prediction_at
  │
  ├── sueño de la noche anterior        VÁLIDO
  ├── despertar de la mañana            VÁLIDO
  ├── sesión registrada a las 07:30     VÁLIDO
  │
11:00  ocurre desregulación
  ├── evento                             NO VÁLIDO como feature
  ├── detonante identificado             NO VÁLIDO
  ├── estrategia aplicada                NO VÁLIDO
  └── resultado de la estrategia         NO VÁLIDO
```

La desregulación de las 11:00 solamente se utiliza después para determinar si la predicción de las 08:00 acertó su target.

Esta regla debe verificarse con tests automatizados.

---

# 6. Variables canónicas del MVP

Se definen **11 variables predictoras semánticas** y un target.

| # | Variable | Tipo | Rol |
|---|---|---|---|
| 1 | `calidad_sueno` | categórica | feature directa |
| 2 | `horas_sueno` | numérica nullable | feature directa/sintética |
| 3 | `estado_despertar` | categórica | feature directa |
| 4 | `nivel_regulacion` | categórica | feature actual/histórica |
| 5 | `nivel_alerta` | categórica | feature directa |
| 6 | `cambio_rutina` | boolean nullable | feature directa |
| 7 | `estado_gastrointestinal` | categórica | feature directa |
| 8 | `comportamiento_observado` | lista de tags | feature directa/NLP |
| 9 | `evento_excepcional` | boolean nullable | feature contextual |
| 10 | `perfil_sensorial` | lista de tags | personalización |
| 11 | `eventos_desregulacion_7d` | integer | feature histórica derivada |
| — | `desregulacion_proximas_24h` | boolean | target |

---

# 7. Valores normalizados

## 7.1 Calidad del sueño

```text
reparador
interrumpido
dificultad_conciliacion
desconocido
```

## 7.2 Estado al despertar

```text
tranquilo_alegre
irritable_llorando
cansado_sueno
desconocido
```

## 7.3 Regulación

```text
excelente
estable_con_apoyo
desregulacion_frecuente
desconocido
```

## 7.4 Nivel de alerta

```text
bajo
optimo
alto
desconocido
```

**Importante:** `nivel_alerta` no debe codificarse conceptualmente como una escala lineal simple. Tanto `bajo` como `alto` pueden representar un alejamiento del estado óptimo.

Features derivadas recomendadas:

```text
alerta_fuera_optimo: boolean
direccion_alerta: bajo | alto | null
```

## 7.5 Perfil sensorial

El texto libre del perfil debe convertirse en tags.

Ejemplo:

```text
"Hipersensible Auditivo y Evitador Táctil"
```

se normaliza como:

```json
[
  "hipersensibilidad_auditiva",
  "evitacion_tactil"
]
```

Esto permite cruzar el perfil del niño con eventos/contextos actuales sin utilizar el detonante futuro del evento.

---

# 8. Variables que NO deben utilizarse directamente

## 8.1 `detonante_gatillante`

No debe utilizarse como predictor contemporáneo de un evento porque normalmente se identifica **después de que el evento ocurrió**.

Hacerlo generaría data leakage.

Sí puede conservarse en el historial para responder:

```text
¿Qué contextos fueron relevantes anteriormente para este niño?
```

A partir de información conocida antes de `prediction_at`, se puede construir:

```text
exposicion_gatillante_relevante
```

Ejemplo:

```text
perfil_sensorial = hipersensibilidad_auditiva
evento_excepcional = ruido_intenso

→ exposicion_gatillante_relevante = true
```

## 8.2 `diagnostico_principal`

En el dataset real los cuatro casos tienen TEA. La variable tiene varianza cero y no aporta capacidad discriminativa.

## 8.3 `estado_asistencia`

Los registros disponibles presentan asistencia constante, por lo que no aporta señal en la muestra.

## 8.4 `adherencia_medicacion`

No se recomienda inicialmente porque está fuertemente asociada a un caso concreto y puede transformarse accidentalmente en un proxy de identidad.

## 8.5 `scenario_id` y `scenario_type`

Son metadatos de QA del dataset sintético.

**Nunca deben utilizarse como features.**

---

# 9. Feature engineering

El predictor no debe trabajar solamente con los valores del día actual.

Debe generar variables temporales automáticamente.

## 9.1 Features derivadas mínimas

```yaml
DerivedFeatures:
  sueno_alterado_dias_3d: integer
  desviacion_sueno_baseline_14d: float

  despertar_adverso_dias_3d: integer

  regulacion_baja_dias_3d: integer
  tendencia_regulacion_3d: float

  eventos_desregulacion_7d: integer
  dias_desde_ultima_desregulacion: float | null

  factores_adversos_simultaneos: integer

  exposicion_gatillante_relevante: boolean

  alerta_fuera_optimo: boolean
```

El dataset sintético de ML ya materializa varias de estas señales:

```text
sleep_altered_days_3d
wake_adverse_days_3d
low_regulation_days_3d
routine_changes_3d
gi_alterations_3d
exceptional_events_3d
dysregulation_events_7d
days_since_last_dysregulation
adverse_factor_count_current
data_completeness
```

---

# 10. Ventanas temporales del MVP

Para el prototipo se establece:

| Ventana | Uso |
|---|---|
| último registro válido | estado actual |
| 72 h / 3 días | acumuladores |
| 7 días | historial reciente de desregulación |
| 14 días válidos | baseline individual |
| próximas 24 h | target |

Representación:

```text
PASADO                                             FUTURO

──────── baseline 14d ────────┬──── 72 h ────┬──────── 24 h ────────
                              │               │
                       historia reciente  prediction_at
                                              │
                                              └──── target
```

Estos valores son **hiperparámetros del prototipo**, no ventanas clínicamente validadas.

Deben quedar configurables.

---

# 11. Línea base individual

La línea base debe calcularse por niño, no globalmente.

La pregunta no es:

```text
¿Este valor es anormal para todos los niños?
```

sino:

```text
¿Este valor representa un cambio relevante respecto del patrón habitual de este niño?
```

Ejemplos de baseline:

```text
porcentaje_sueno_reparador_14d
frecuencia_despertar_irritable_14d
frecuencia_regulacion_estable_14d
distribucion_alerta_14d
```

Luego se calculan desviaciones.

Ejemplo conceptual:

```text
baseline:
sueño alterado = 15 % de días

últimos 3 días:
sueño alterado = 3/3

→ desviación relevante
```

La implementación puede usar inicialmente proporciones y conteos simples. No necesita un modelo temporal complejo para el MVP.

---

# 12. Arquitectura recomendada del motor

```text
┌─────────────────────────────────────────────┐
│ 1. CANONICAL INPUT BUILDER                  │
│ raw DB → esquema normalizado                │
└──────────────────────┬──────────────────────┘
                       ▼
┌─────────────────────────────────────────────┐
│ 2. FEATURE BUILDER                          │
│ baseline + ventanas + acumuladores          │
└──────────────────────┬──────────────────────┘
                       ▼
        ┌──────────────┴──────────────┐
        ▼                             ▼
┌───────────────────────┐   ┌────────────────────────┐
│ 3A. RISK ENGINE       │   │ 3B. CONFIDENCE ENGINE │
│ patrón de riesgo      │   │ calidad de evidencia   │
└────────────┬──────────┘   └────────────┬───────────┘
             │                           │
             └──────────────┬────────────┘
                            ▼
┌─────────────────────────────────────────────┐
│ 4. EXPLANATION BUILDER                      │
│ códigos de factores + descripción           │
└──────────────────────┬──────────────────────┘
                       ▼
┌─────────────────────────────────────────────┐
│ 5. OUTPUT CONTRACT                          │
│ riesgo + confianza + factores + warnings    │
└─────────────────────────────────────────────┘
```

La selección de intervenciones se recomienda mantener como un componente separado del predictor.

---

# 13. Estrategia del modelo para el MVP

## 13.1 Lo que existe hoy

El dataset sintético contiene:

```text
risk_score_demo
confidence_score_demo
```

y las predicciones están versionadas como:

```text
synthetic-rule-demo-v1
```

El repositorio deja explícito que este score es una **regla sintética ilustrativa**, no una probabilidad clínicamente calibrada.

## 13.2 Decisión recomendada para implementar ahora

Para el hackathon conviene utilizar un **motor híbrido explicable**:

```text
features temporales
      ↓
scoring/modelo interpretable
      ↓
risk_score
      ↓
risk_level
```

Dos implementaciones válidas:

### Opción A — scoring explícito

Ventajas:

- máxima trazabilidad;
- fácil de explicar en la demo;
- permite probar todas las rutas del backend/frontend;
- no genera una falsa narrativa de “modelo entrenado” con pocos eventos.

Los pesos deben permanecer en configuración y documentarse como valores de demostración, no aprendidos clínicamente.

### Opción B — regresión logística

Puede utilizarse como baseline técnico sobre datos sintéticos para demostrar el pipeline de ML.

Sin embargo:

> Una métrica obtenida entrenando y evaluando sobre datos sintéticos demuestra que el pipeline funciona, no que el sistema predice crisis reales.

### Recomendación final para NeuroHack

Usar **scoring explicable como motor oficial de demo** y mantener una interfaz que permita reemplazarlo posteriormente por una regresión logística/Gradient Boosting sin cambiar Backend ni Frontend.

Así:

```text
RiskEngine interface
    ├── SyntheticRuleRiskEngine   ← MVP
    ├── LogisticRiskEngine        ← siguiente iteración
    └── GradientBoostRiskEngine   ← futuro
```

---

# 14. Contrato de entrada del módulo predictivo

El Backend debe entregar un snapshot canónico. El servicio predictivo no debería depender de nombres de columnas CSV.

```json
{
  "child_id": "CH-002",
  "prediction_at": "2026-07-17T20:00:00-04:00",
  "horizon_hours": 24,

  "features": {
    "calidad_sueno": "interrumpido",
    "horas_sueno": null,
    "estado_despertar": "irritable_llorando",
    "nivel_regulacion": "estable_con_apoyo",
    "nivel_alerta": "alto",
    "cambio_rutina": true,
    "estado_gastrointestinal": "normal",
    "comportamiento_observado": [
      "irritabilidad"
    ],
    "evento_excepcional": false,
    "perfil_sensorial": [
      "hipersensibilidad_auditiva",
      "evitacion_tactil"
    ],
    "eventos_desregulacion_7d": 0
  },

  "derived": {
    "sueno_alterado_dias_3d": 3,
    "desviacion_sueno_baseline_14d": 0.72,
    "despertar_adverso_dias_3d": 2,
    "regulacion_baja_dias_3d": 1,
    "tendencia_regulacion_3d": -0.35,
    "dias_desde_ultima_desregulacion": 11,
    "factores_adversos_simultaneos": 4,
    "exposicion_gatillante_relevante": true,
    "alerta_fuera_optimo": true
  },

  "data_quality": {
    "variables_criticas_presentes": 9,
    "variables_criticas_totales": 11,
    "completitud": 0.82,
    "horas_desde_ultimo_registro": 4.0,
    "fuentes_disponibles": [
      "familia",
      "profesional"
    ],
    "dias_historial_disponibles": 17,
    "contiene_datos_sinteticos": false
  }
}
```

---

# 15. Contrato de salida del módulo predictivo

El predictor debe responder en términos de máquina, no solamente con texto.

## 15.1 DTO recomendado

```json
{
  "prediction_id": "RPR-00451",
  "child_id": "CH-002",

  "prediction_at": "2026-07-17T20:00:00-04:00",
  "window_end_at": "2026-07-18T20:00:00-04:00",
  "horizon_hours": 24,

  "model_version": "hybrid-mvp-v1",
  "status": "OK",

  "risk": {
    "score": 0.76,
    "level": "HIGH"
  },

  "confidence": {
    "score": 0.68,
    "level": "MEDIUM"
  },

  "top_factors": [
    {
      "code": "SLEEP_ALTERED_3D",
      "label": "Sueño alterado durante 3 días",
      "direction": "INCREASES_RISK",
      "window": "72h"
    },
    {
      "code": "WAKE_IRRITABLE",
      "label": "Despertar irritable",
      "direction": "INCREASES_RISK",
      "window": "current"
    },
    {
      "code": "ROUTINE_CHANGE",
      "label": "Cambio de rutina reciente",
      "direction": "INCREASES_RISK",
      "window": "current"
    },
    {
      "code": "ALERT_OUTSIDE_OPTIMAL",
      "label": "Nivel de alerta fuera del rango habitual",
      "direction": "INCREASES_RISK",
      "window": "current"
    }
  ],

  "data_quality": {
    "completeness": 0.82,
    "history_days": 17,
    "sources": [
      "FAMILY",
      "PROFESSIONAL"
    ],
    "missing_fields": [
      "horas_sueno"
    ]
  },

  "warnings": [
    {
      "code": "MISSING_SCHOOL_RECORD",
      "severity": "INFO",
      "message": "No existe registro escolar reciente."
    }
  ]
}
```

## 15.2 Regla de diseño

El Frontend no debe calcular niveles de riesgo, confianza ni explicaciones.

Debe recibirlos ya resueltos desde Backend/ML para mantener consistencia entre plataformas.

---

# 16. Estados de respuesta

Además de `risk.level`, el motor debe devolver un `status` operacional.

```text
OK
LOW_CONFIDENCE
INSUFFICIENT_DATA
ERROR
```

## `OK`

Existe información suficiente para mostrar el resultado normalmente.

## `LOW_CONFIDENCE`

Existe una señal de riesgo utilizable, pero la calidad de la información requiere una advertencia visible.

Ejemplo:

```text
Riesgo: HIGH
Confianza: LOW
Status: LOW_CONFIDENCE
```

Esto es válido y es importante. Riesgo y confianza no deben confundirse.

## `INSUFFICIENT_DATA`

No se dispone de evidencia mínima para generar una recomendación confiable.

En este caso el Frontend debe priorizar una solicitud de información y evitar mensajes falsamente tranquilizadores.

Ejemplo:

```json
{
  "status": "INSUFFICIENT_DATA",
  "risk": null,
  "confidence": {
    "score": 0.18,
    "level": "LOW"
  },
  "required_fields": [
    "calidad_sueno",
    "estado_despertar",
    "nivel_regulacion"
  ]
}
```

### Nota de implementación

El repositorio no define un umbral clínico de “información insuficiente”. Debe configurarse para el MVP y quedar desacoplado del código.

Una regla inicial razonable puede basarse en:

- completitud;
- antigüedad del último registro;
- presencia de variables críticas;
- días de historial;
- número de fuentes disponibles.

---

# 17. Niveles de riesgo y confianza

Los CSV sintéticos utilizan `LOW`, `MEDIUM` y `HIGH`.

En el dataset demo se observa que valores inferiores a `0.65` son tratados como bajo/medio en QA, por lo que un umbral `HIGH >= 0.65` es consistente con la demo.

Para evitar dispersar reglas en varios módulos, los umbrales deben almacenarse en una única configuración del motor.

Configuración de demostración recomendada:

```yaml
risk_levels:
  LOW:
    min: 0.00
    max_exclusive: 0.35

  MEDIUM:
    min: 0.35
    max_exclusive: 0.65

  HIGH:
    min: 0.65
    max: 1.00
```

Para confianza, los ejemplos sintéticos muestran aproximadamente:

```yaml
confidence_levels:
  LOW:
    min: 0.00
    max_exclusive: 0.40

  MEDIUM:
    min: 0.40
    max_exclusive: 0.75

  HIGH:
    min: 0.75
    max: 1.00
```

Estos son **umbrales operacionales de demo**, no límites clínicos.

Backend y Frontend deben consumir el `level` ya calculado y no replicar los thresholds.

---

# 18. Motor de confianza

El motor de confianza debe permanecer separado del riesgo.

Debe considerar como mínimo:

```text
completitud
antigüedad del último registro
variables críticas presentes
número de fuentes disponibles
días de historial individual
contradicciones entre fuentes, si se implementan
```

Ejemplos:

```text
Riesgo = 0.82 HIGH
Confianza = 0.88 HIGH
→ alerta fuerte, datos suficientes
```

```text
Riesgo = 0.82 HIGH
Confianza = 0.31 LOW
→ patrón preocupante, pero advertir falta de información
```

```text
Riesgo = null
Confianza = 0.15 LOW
Status = INSUFFICIENT_DATA
→ no generar recomendación de riesgo
```

El archivo sintético `confidence_score_demo` se basa en completitud e historial; debe tratarse como demostrativo.

---

# 19. Explicabilidad

El motor debe devolver factores estructurados.

No debe devolver únicamente:

```text
risk_score = 0.76
```

Debe poder responder:

```text
¿Por qué?
```

La explicación debe derivarse de las mismas features utilizadas por el predictor.

Ejemplo:

```text
1. SLEEP_ALTERED_3D
2. WAKE_IRRITABLE
3. ROUTINE_CHANGE
4. ALERT_OUTSIDE_OPTIMAL
```

Backend puede transformar los códigos en texto según el rol del usuario.

Ejemplo familia:

```text
"El sueño ha estado alterado durante varios días y hoy hubo un cambio de rutina."
```

Ejemplo profesional:

```text
"3/3 días con sueño alterado, despertar adverso y alerta fuera de óptimo respecto del baseline reciente."
```

No se recomienda que un LLM invente la explicación causal del score. El texto debe estar anclado a factores emitidos por el modelo.

---

# 20. Recomendaciones preventivas

La recomendación debe ser un módulo distinto del cálculo de riesgo.

Flujo:

```text
RiskPrediction
      │
      ├── top_factors
      │
      ▼
Recommendation Resolver
      │
      ├── catálogo validado
      ├── estrategias profesionales
      └── historial individual
              │
              ▼
       intervention_id(s)
```

El dataset sintético ya modela:

```text
RiskPrediction
    ↓
PredictionIntervention
    ↓
Intervention
```

y contiene estrategias como:

- anticipación visual de transición;
- espacio de baja estimulación;
- atenuación de estímulos auditivos;
- pausa sensorial propioceptiva;
- ajuste temporal de demanda;
- rutina de regulación guiada;
- adaptación de presentación de alimentos;
- comunicación anticipada del cambio.

El predictor debería entregar **factores/códigos**, y Backend debería resolver las intervenciones permitidas para ese niño y contexto.

Esto evita que el motor predictivo o un LLM generen libremente una intervención clínica.

---

# 21. Contrato Backend ↔ Motor Predictivo

## Responsabilidad del Backend

Backend debe:

1. recuperar la línea temporal del niño;
2. aplicar control de acceso;
3. normalizar los datos al schema canónico o delegar explícitamente al feature service;
4. construir/solicitar features usando únicamente información `<= prediction_at`;
5. invocar el motor;
6. persistir `RiskPrediction`;
7. resolver recomendaciones desde `Intervention`;
8. persistir `PredictionIntervention`;
9. exponer un DTO final al Frontend;
10. recibir posteriormente feedback mediante `InterventionResult` y `DysregulationEvent`.

## Responsabilidad del motor Data/ML

Data/ML debe:

1. validar el input;
2. calcular o verificar features derivadas;
3. calcular `risk_score`;
4. calcular `risk_level`;
5. calcular `confidence_score`;
6. calcular `confidence_level`;
7. generar `top_factors`;
8. devolver warnings de calidad;
9. versionar el motor;
10. garantizar ausencia de leakage.

## Responsabilidad del Frontend

Frontend debe:

1. mostrar nivel de riesgo;
2. mostrar confianza separadamente;
3. mostrar factores explicativos;
4. mostrar advertencias por datos incompletos;
5. mostrar intervenciones resueltas por Backend;
6. permitir completar información faltante;
7. capturar feedback posterior;
8. no recalcular lógica predictiva.

---

# 22. DTO recomendado Backend → Frontend

Backend debería exponer un objeto compuesto:

```json
{
  "prediction": {
    "id": "RPR-00451",
    "generated_at": "2026-07-17T20:00:00-04:00",
    "valid_until": "2026-07-18T20:00:00-04:00",
    "status": "OK"
  },

  "risk": {
    "score": 0.76,
    "level": "HIGH"
  },

  "confidence": {
    "score": 0.68,
    "level": "MEDIUM"
  },

  "explanation": {
    "summary": "Se detecta una acumulación de factores respecto del patrón reciente.",
    "factors": [
      {
        "code": "SLEEP_ALTERED_3D",
        "label": "Sueño alterado durante 3 días"
      },
      {
        "code": "ROUTINE_CHANGE",
        "label": "Cambio de rutina reciente"
      }
    ]
  },

  "data_quality": {
    "completeness": 0.82,
    "level": "MEDIUM",
    "missing": [
      "horas_sueno"
    ]
  },

  "recommendations": [
    {
      "intervention_id": "INT-001",
      "name": "Anticipación visual de transición",
      "priority": 1,
      "reason": "Cambio de rutina reciente."
    }
  ],

  "disclaimer": "Estimación preventiva de apoyo; no corresponde a un diagnóstico."
}
```

Este DTO puede ser usado por las tres vistas cambiando la profundidad del texto, no la lógica del modelo.

---

# 23. Comportamiento que debe ver el Frontend

## Caso A — riesgo alto + confianza alta

```text
Riesgo: ALTO
Confianza: ALTA

Factores:
- sueño alterado durante 3 días
- regulación inferior al patrón reciente
- cambio de rutina

Acción:
mostrar recomendaciones preventivas.
```

## Caso B — riesgo alto + confianza baja

```text
Riesgo: ALTO
Confianza: BAJA

Mensaje:
"Se observan señales relevantes, pero faltan datos para sostener una estimación confiable."

Acción:
mostrar advertencia + campos prioritarios faltantes.
```

No ocultar el riesgo solamente porque la confianza sea baja.

## Caso C — riesgo bajo + confianza baja

No mostrar un mensaje equivalente a:

```text
"Todo está bien"
```

porque la falta de información puede producir una falsa tranquilidad.

Mostrar:

```text
"La información disponible es insuficiente para confirmar un riesgo bajo."
```

## Caso D — información insuficiente

```text
Status: INSUFFICIENT_DATA
```

No mostrar porcentaje de riesgo.

Priorizar:

```text
"Complete sueño, estado al despertar y regulación para obtener una estimación."
```

---

# 24. Persistencia

La predicción debe almacenarse como una entidad independiente:

```text
RiskPrediction
────────────────────────────────
id
child_id
prediction_at
window_end_at
horizon_hours
risk_score
risk_level
confidence_score
confidence_level
data_completeness
model_version
explanation
created_at
```

El resultado real se determina después cruzando:

```text
DysregulationEvent.occurred_at
```

contra:

```text
(prediction_at, window_end_at]
```

No es necesario duplicar permanentemente `target_dysregulation_24h` dentro de la tabla operacional.

Para datasets de entrenamiento sí puede materializarse como columna.

---

# 25. Feedback y ciclo de aprendizaje

Después de cada ventana predictiva Backend debe poder registrar:

```text
¿Ocurrió una desregulación?
¿Se aplicó una intervención?
¿Qué intervención?
¿Qué resultado tuvo?
```

Modelo:

```text
RiskPrediction
      ↓
Intervention recomendada
      ↓
InterventionResult
      ↓
DysregulationEvent opcional
```

`InterventionResult.outcome`:

```text
EFFECTIVE
PARTIALLY_EFFECTIVE
NO_EFFECT
UNKNOWN
```

Este ciclo permite construir posteriormente:

1. dataset de predicción real;
2. memoria de intervenciones;
3. personalización;
4. evaluación longitudinal del sistema.

---

# 26. Dataset de entrenamiento

La tabla `12_ml_prediction_dataset.csv` es el formato más cercano a un dataset listo para experimentos.

Cada fila representa una ventana de predicción.

Columnas principales:

```text
prediction_id
child_external_case_id
prediction_date
prediction_at

sleep_quality
sleep_hours
wake_state
regulation_level
alert_level
routine_change
gastrointestinal_status
observed_behavior
exceptional_event
sensory_profile

sleep_altered_days_3d
wake_adverse_days_3d
low_regulation_days_3d
routine_changes_3d
gi_alterations_3d
exceptional_events_3d
dysregulation_events_7d
days_since_last_dysregulation
adverse_factor_count_current

data_completeness

risk_score_demo
confidence_score_demo

target_dysregulation_24h
```

Excluir del entrenamiento:

```text
prediction_id
scenario_id
scenario_type
risk_score_demo       # si se entrena un modelo real
confidence_score_demo # si se entrena confianza real
```

`child_external_case_id` debe utilizarse para particionado/agrupación, no como predictor.

---

# 27. Entrenamiento futuro con datos reales

Cuando exista suficiente información longitudinal real:

## Pipeline

```text
raw events
   ↓
canonical schema
   ↓
prediction snapshots
   ↓
feature engineering
   ↓
train/validation/test temporal
   ↓
modelo interpretable baseline
   ↓
calibración
   ↓
comparación de modelos
   ↓
model registry/version
```

## Baseline obligatorio

Comenzar con:

```text
Logistic Regression
```

Luego comparar con:

```text
Random Forest
Gradient Boosting
XGBoost
modelos temporales
```

La complejidad solamente se justifica si mejora de forma reproducible:

- discriminación;
- calibración;
- robustez;
- explicabilidad.

## Particionado

No utilizar un `random split` ingenuo de filas longitudinales.

Se debe evitar que registros muy cercanos del mismo niño aparezcan simultáneamente en entrenamiento y validación de forma que el modelo “vea el futuro”.

Usar evaluación temporal y, cuando el tamaño del dataset lo permita, evaluación agrupada por niño.

---

# 28. Tests obligatorios del módulo

## 28.1 Anti-leakage

```text
Ninguna feature tiene timestamp > prediction_at.
```

## 28.2 Target

```text
target = 1
solo si existe evento en (prediction_at, prediction_at + 24 h].
```

## 28.3 Missing data

```text
NULL/desconocido != normal
```

## 28.4 Riesgo vs confianza

Un input puede generar:

```text
HIGH risk + LOW confidence
```

sin error.

## 28.5 Metadata sintética

```text
scenario_id
scenario_type
```

no llegan al predictor.

## 28.6 Versionado

Toda predicción incluye:

```text
model_version
```

## 28.7 Reproducibilidad

Para scoring/modelos deterministas, el mismo input y la misma versión deben producir el mismo resultado.

---

# 29. Estructura de código recomendada

```text
predictive-model/
│
├── domain/
│   ├── prediction_input.py
│   ├── prediction_output.py
│   ├── enums.py
│   └── factor_codes.py
│
├── normalization/
│   ├── daily_record_mapper.py
│   ├── session_mapper.py
│   └── sensory_profile_mapper.py
│
├── features/
│   ├── baseline.py
│   ├── rolling_windows.py
│   ├── accumulators.py
│   ├── history.py
│   └── data_quality.py
│
├── risk/
│   ├── interface.py
│   ├── synthetic_rule_engine.py
│   └── logistic_engine.py
│
├── confidence/
│   └── confidence_engine.py
│
├── explanation/
│   └── factor_explainer.py
│
├── service/
│   └── prediction_service.py
│
├── tests/
│   ├── test_no_leakage.py
│   ├── test_target_window.py
│   ├── test_missing_data.py
│   ├── test_feature_windows.py
│   └── test_contract.py
│
└── config/
    ├── risk_thresholds.yaml
    ├── confidence_thresholds.yaml
    └── model.yaml
```

La interfaz `RiskEngine` debe permitir sustituir el algoritmo sin cambiar el contrato exterior.

---

# 30. Flujo end-to-end del MVP

```text
1. Familia/escuela/profesional registra información
                     ↓
2. Backend persiste DailyRecord / ProfessionalSession
                     ↓
3. Backend solicita nueva predicción
                     ↓
4. Se fija prediction_at
                     ↓
5. Se recupera solamente historial <= prediction_at
                     ↓
6. Normalización a schema canónico
                     ↓
7. Feature Builder
      - baseline 14d
      - acumuladores 72h
      - eventos 7d
      - calidad de datos
                     ↓
8. Risk Engine
                     ↓
9. Confidence Engine
                     ↓
10. Explanation Builder
                     ↓
11. Se persiste RiskPrediction
                     ↓
12. Backend resuelve Interventions
                     ↓
13. Frontend recibe:
      - riesgo
      - confianza
      - explicación
      - datos faltantes
      - recomendaciones
                     ↓
14. Usuario registra feedback
                     ↓
15. InterventionResult / DysregulationEvent
```

---

# 31. Decisiones cerradas para que los módulos puedan avanzar

## Data/ML

Debe implementar:

```text
PredictionInput
→ features temporales
→ risk
→ confidence
→ top_factors
→ PredictionOutput
```

Horizonte:

```text
24 h
```

Ventanas:

```text
72 h
7 días
14 días
```

Target:

```text
evento real en (T,T+24h]
```

---

## Backend

Debe modelar y exponer como mínimo:

```text
Child
User
UserChildAccess
DailyRecord
ProfessionalSession
DysregulationEvent
RiskPrediction
Intervention
PredictionIntervention
InterventionResult
```

Backend no debe incorporar la fórmula del score en controladores ni DTOs.

---

## Frontend

Debe consumir:

```text
status
risk.score
risk.level
confidence.score
confidence.level
top_factors
missing data
recommendations
valid_until
```

No debe recalcular thresholds.

---

# 32. Qué debe considerarse provisional

No está validado clínicamente:

- pesos del score;
- thresholds;
- baseline de 14 días;
- ventana de 72 horas;
- ventana de historial de 7 días;
- rendimiento del modelo;
- importancia causal de cada variable;
- eficacia clínica de las recomendaciones.

Estos elementos son parámetros del prototipo y deben quedar configurables/versionados.

---

# 33. Respuesta mínima que debe garantizar el motor

Para que Backend y Frontend puedan integrarse desde ahora, el contrato mínimo estable debería ser:

```json
{
  "prediction_id": "string",
  "prediction_at": "datetime",
  "window_end_at": "datetime",
  "horizon_hours": 24,
  "model_version": "string",
  "status": "OK | LOW_CONFIDENCE | INSUFFICIENT_DATA | ERROR",

  "risk": {
    "score": "float|null",
    "level": "LOW|MEDIUM|HIGH|null"
  },

  "confidence": {
    "score": "float",
    "level": "LOW|MEDIUM|HIGH"
  },

  "top_factors": [
    {
      "code": "string",
      "label": "string"
    }
  ],

  "data_quality": {
    "completeness": "float",
    "missing_fields": ["string"]
  },

  "warnings": []
}
```

Todo lo demás puede evolucionar sin romper este núcleo.

---

# 34. Conclusión técnica

El modelo predictivo de Bluba Anticipa no debe construirse como un clasificador sobre filas independientes.

Debe implementarse como un **pipeline longitudinal individual**:

```text
RAW DATA
   ↓
CANONICAL SCHEMA
   ↓
BASELINE INDIVIDUAL
   ↓
TEMPORAL FEATURES
   ↓
RISK ENGINE
   +
CONFIDENCE ENGINE
   ↓
EXPLICACIÓN ESTRUCTURADA
   ↓
BACKEND
   ↓
RECOMENDACIONES SEGURAS
   ↓
FRONTEND
```

La decisión arquitectónica más importante es mantener estable el contrato:

```text
input canónico
        ↓
risk + confidence + factors
```

De esa manera el MVP puede funcionar hoy con un scoring explicable sobre escenarios sintéticos, mientras que en el futuro el motor puede ser reemplazado por un modelo aprendido y calibrado con datos longitudinales reales **sin modificar las interfaces del Backend ni del Frontend**.

---

# 35. Fuentes del repositorio consolidadas

Este documento sintetiza principalmente:

```text
analisis/diccionary.md
analisis/variables.md
analisis/modelo-datos.md

dataset-sintetico/README.md
dataset-sintetico/QA_VALIDATION.md
dataset-sintetico/07_risk_prediction.csv
dataset-sintetico/08_intervention.csv
dataset-sintetico/09_prediction_intervention.csv
dataset-sintetico/11_scenario_manifest.csv
dataset-sintetico/12_ml_prediction_dataset.csv
dataset-sintetico/13_data_dictionary.csv
dataset-sintetico/summary.json
```

Las decisiones de integración añadidas en este documento —como el DTO compuesto Backend → Frontend, la interfaz `RiskEngine`, los estados operacionales y la organización propuesta del código— son recomendaciones de implementación para convertir las definiciones existentes en un contrato ejecutable entre módulos.
