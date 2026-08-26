## DATA-02 — Variables utilizadas por el MVP

A partir de DATA-01, conviene **ajustar la propuesta inicial antes de implementarla**. La documentación de Bluba contempla sueño, estado al despertar, salud gastrointestinal, cambios de rutina, comportamiento, estado de alerta, eventos excepcionales e historial de crisis como variables relevantes, y fija como objetivo generar un indicador para las próximas horas o el día siguiente. 

La arquitectura de Bluba Anticipa, además, define que estas variables no deben consumirse aisladamente: deben convertirse en desviaciones respecto del baseline individual, acumuladores temporales e interacciones. 

# 1. Decisión final de variables

Propongo **11 variables predictoras + 1 variable objetivo**.

| #  | Variable canónica            | Tipo                | Fuente MVP                      | Estado                    |
| -- | ---------------------------- | ------------------- | ------------------------------- | ------------------------- |
| 1  | `calidad_sueno`              | Categórica          | Seguimiento diario              | ✅ Dataset real            |
| 2  | `horas_sueno`                | Numérica            | Familia / registro              | ⚠️ No está en CSV         |
| 3  | `estado_despertar`           | Categórica          | Seguimiento diario              | ✅ Dataset real            |
| 4  | `nivel_regulacion`           | Categórica          | Seguimiento diario              | ✅ Dataset real            |
| 5  | `nivel_alerta`               | Categórica          | Sesión profesional              | ✅ Dataset real, irregular |
| 6  | `cambio_rutina`              | Boolean/categórica  | Familia / escuela               | ⚠️ Simulada en MVP        |
| 7  | `estado_gastrointestinal`    | Categórica          | Seguimiento diario              | ✅ Dataset real            |
| 8  | `comportamiento_observado`   | Multi-categoría     | Familia / escuela / profesional | ⚠️ No estructurada en CSV |
| 9  | `evento_excepcional`         | Boolean + categoría | Familia / escuela               | ⚠️ Simulada en MVP        |
| 10 | `perfil_sensorial`           | Multi-categoría     | Perfil del caso                 | ✅ Dataset real            |
| 11 | `eventos_desregulacion_7d`   | Integer derivado    | Historial eventos               | ✅ Derivable               |
| 12 | `desregulacion_proximas_24h` | Boolean             | Eventos                         | 🎯 **Target**             |

Esto respeta el alcance del MVP y permite combinar datos reales con variables sintéticas, algo expresamente permitido por la presentación del desafío. 

---

# 2. Cambio importante respecto de la propuesta inicial: `gatillante`

No recomiendo introducir directamente:

```text
gatillante
```

como feature del modelo.

En los datos entregados, `detonante_gatillante` existe dentro de:

```text
5_eventos_desregulacion_tutor.csv
```

y describe el detonante **después de que el evento ya ocurrió**.

Por ejemplo:

```text
evento:
Sobrecarga Sensorial

detonante:
"Música fuerte en supermercado y aglomeración de personas."
```

Si utilizáramos ese campo para predecir ese mismo evento tendríamos **data leakage**:

```text
información del evento futuro
          ↓
modelo
          ↓
predicción del evento futuro
```

Eso invalidaría la evaluación.

### Lo reemplazamos por

```text
exposicion_gatillante_relevante
```

como **feature derivada**, que únicamente puede utilizar información conocida **antes de `prediction_at`**.

Por ejemplo:

```text
perfil_sensorial = hipersensibilidad_auditiva
evento_excepcional = sirena / ruido intenso

→ exposicion_gatillante_relevante = true
```

Los `detonante_gatillante` históricos sí pueden utilizarse para aprender qué contextos han sido relevantes anteriormente para cada niño.

---

# 3. Schema canónico de entrada

Cada predicción representa:

> **estado conocido del niño en un instante T → riesgo entre T y T+24 h**

Por tanto, todo registro del motor debe tener obligatoriamente un instante de corte.

```yaml
PredictionInput:

  # Metadata
  id_caso: string
  prediction_at: datetime
  horizon_hours: 24

  # Variables críticas
  calidad_sueno:
    type: enum
    values:
      - reparador
      - interrumpido
      - dificultad_conciliacion
      - desconocido

  horas_sueno:
    type: float | null
    range: 0..24

  estado_despertar:
    type: enum
    values:
      - tranquilo_alegre
      - irritable_llorando
      - cansado_sueno
      - desconocido

  nivel_regulacion:
    type: enum
    values:
      - excelente
      - estable_con_apoyo
      - desregulacion_frecuente
      - desconocido

  nivel_alerta:
    type: enum
    values:
      - bajo
      - optimo
      - alto
      - desconocido

  cambio_rutina:
    type: boolean | null

  estado_gastrointestinal:
    type: enum
    values:
      - normal
      - estrenimiento
      - diarrea
      - otro
      - desconocido

  comportamiento_observado:
    type: array[string]
    nullable: true

  evento_excepcional:
    type: boolean | null

  perfil_sensorial:
    type: array[string]

  eventos_desregulacion_7d:
    type: integer
    minimum: 0
```

Los campos `desconocido`/`null` son deliberados. La ausencia de información no debe convertirse en normalidad; tanto Bluba como la propuesta del proyecto requieren tratar explícitamente los datos faltantes y disminuir la confianza cuando corresponda.  

---

# 4. Mapeo desde el dataset actual

El backend necesita una capa de normalización entre los CSV de Bluba y este contrato.

| Schema motor                 | Dataset actual                           | Transformación          |
| ---------------------------- | ---------------------------------------- | ----------------------- |
| `id_caso`                    | `id_caso`                                | Directa                 |
| `calidad_sueno`              | `calidad_sueno`                          | Normalización de texto  |
| `horas_sueno`                | —                                        | Sintético / futuro      |
| `estado_despertar`           | `modo_despertar`                         | Renombrar               |
| `nivel_regulacion`           | `nivel_regulacion_general_dia`           | Renombrar               |
| `nivel_alerta`               | `nivel_alerta_inicial/final`             | Último valor previo a T |
| `cambio_rutina`              | —                                        | Sintético / chat        |
| `estado_gastrointestinal`    | `estado_gastrointestinal`                | Directa                 |
| `comportamiento_observado`   | `observaciones_profesional` parcialmente | NLP / sintético         |
| `evento_excepcional`         | —                                        | Sintético / chat        |
| `perfil_sensorial`           | `perfil_sensorial_predominante`          | Convertir a tags        |
| `eventos_desregulacion_7d`   | `fecha_hora` de eventos                  | Calcular ventana        |
| `desregulacion_proximas_24h` | `fecha_hora` de eventos                  | Calcular label          |

---

# 5. Codificación de las variables reales

No recomiendo guardar números como `0, 1, 2` directamente en la capa de datos. El contrato debería mantener valores semánticos y dejar el encoding para el feature pipeline.

### Sueño

Dataset:

```text
Reparador
Interrumpido
Dificultad de Conciliación
```

Normalizado:

```text
reparador
interrumpido
dificultad_conciliacion
```

### Estado al despertar

Dataset:

```text
Tranquilo/Alegre
Irritable/Llorando
Cansado/Con Sueño
```

Normalizado:

```text
tranquilo_alegre
irritable_llorando
cansado_sueno
```

### Regulación

```text
Excelente
Estable con Apoyo
Desregulación Frecuente
```

Normalizado:

```text
excelente
estable_con_apoyo
desregulacion_frecuente
```

### Alerta

Actualmente existen:

```text
Bajo (Letárgico)
Óptimo (Regulado)
Alto (Sobreexcitado)
```

Normalizado:

```text
bajo
optimo
alto
```

Es importante **no tratar alerta como una escala lineal 0-1-2**, porque tanto un nivel excesivamente bajo como excesivamente alto pueden representar una desviación respecto del estado óptimo.

Conviene construir posteriormente:

```text
alerta_fuera_optimo: boolean
direccion_alerta:
  bajo | alto | null
```

---

# 6. Comportamiento observado

Este campo aparece explícitamente entre las variables disponibles en el desafío, pero **no existe como columna estructurada en los CSV entregados**. 

Para el MVP recomiendo:

```yaml
comportamiento_observado:
  type: array[string]
```

Ejemplo:

```json
[
  "irritabilidad",
  "menor_tolerancia_frustracion"
]
```

No fijaría todavía un catálogo clínico definitivo.

El asistente conversacional puede convertir:

> "Hoy estuvo más irritable y se frustró varias veces."

en:

```text
comportamiento_observado:
- irritabilidad
- baja_tolerancia_frustracion
```

y solicitar confirmación antes de guardar. Esta separación entre NLP y motor predictivo ya está definida en la propuesta de arquitectura. 

---

# 7. Perfil sensorial

Actualmente es texto libre:

```text
Buscador Sensorial Vestibular y Propioceptivo

Hipersensible Auditivo y Evitador Táctil

Hiporreactivo Propioceptivo y Visual

Hipersensible Gustativo y Olfativo
```

No conviene alimentar directamente estas frases al predictor.

Deberían convertirse en tags:

```yaml
perfil_sensorial:
  - hipersensibilidad_auditiva
  - evitacion_tactil
```

o:

```yaml
perfil_sensorial:
  - buscador_vestibular
  - buscador_propioceptivo
```

Esto permitirá construir después una feature particularmente importante:

```text
exposicion_gatillante_relevante
```

Ejemplo:

```text
perfil:
hipersensibilidad_auditiva

evento:
ruido_intenso

→ match_sensorial = 1
```

---

# 8. Variable histórica

En vez de entregar al modelo los siete eventos completos, generamos:

```text
eventos_desregulacion_7d
```

Ejemplo:

```text
0
1
2
...
```

Solo se consideran eventos anteriores al momento de predicción:

```text
prediction_at - 7 días
         ≤ evento <
prediction_at
```

Esta feature captura el **estado de vulnerabilidad reciente** y está alineada con la propuesta de utilizar historial y acumulaciones, no registros aislados. 

También mantendremos internamente:

```text
dias_desde_ultima_desregulacion
```

aunque no requiere registro manual.

---

# 9. Variable objetivo

La definición debe ser exacta desde este momento para evitar inconsistencias posteriores.

## Target primario

```text
desregulacion_proximas_24h
```

Tipo:

```text
boolean
```

Definición:

```text
1
si existe al menos un evento para id_caso donde:

prediction_at < fecha_hora <= prediction_at + 24 horas

0
en caso contrario.
```

Formalmente:

[
y_t =
\begin{cases}
1 & \text{si existe evento en }(t,t+24h] \
0 & \text{en otro caso}
\end{cases}
]

### Importante

`nivel_regulacion_general_dia = Desregulación Frecuente`

**no es el target.**

DATA-01 mostró que ambas cosas no son equivalentes.

El target debe provenir de:

```text
5_eventos_desregulacion_tutor.csv
```

---

# 10. Target secundario opcional

Podemos conservar además:

```yaml
intensidad_maxima_24h:
  type: enum | null
  values:
    - leve
    - moderada
    - severa
```

Pero no lo usaría como objetivo principal del MVP.

Con solo siete eventos dividirlos además por tres niveles produciría una muestra extremadamente pequeña.

---

# 11. Feature engineering del motor

Las 11 variables anteriores son las **variables semánticas críticas**.

El predictor no debería trabajar solamente con sus valores actuales.

El feature builder genera automáticamente variables temporales:

```yaml
DerivedFeatures:

  sueno_alterado_dias_3d: integer

  desviacion_sueno_baseline_14d: float

  despertar_adverso_dias_3d: integer

  regulacion_baja_dias_3d: integer

  tendencia_regulacion_3d: float

  eventos_desregulacion_7d: integer

  dias_desde_ultima_desregulacion: float

  factores_adversos_simultaneos: integer

  exposicion_gatillante_relevante: boolean

  alerta_fuera_optimo: boolean
```

Esto implementa directamente la lógica de Bluba Anticipa: **estado actual + desviación individual + acumulación + combinación de factores**. 

---

# 12. Ventanas del MVP

Para dejar la implementación cerrada, usaría inicialmente:

| Ventana         | Uso                 |
| --------------- | ------------------- |
| Último dato     | Estado actual       |
| 72 h / 3 días   | Acumuladores        |
| 7 días          | Crisis recientes    |
| 14 días válidos | Baseline individual |
| Próximas 24 h   | Target              |

```text
             PASADO                            FUTURO

────── baseline 14d ──────┬──── 72h ─────┬──────── 24h ────────
                          │              │
                       contexto    prediction_at
                          │              │
                          └──── features ┤
                                         └──── target
```

Estos tamaños son **hiperparámetros del prototipo**, no valores clínicamente validados. La propuesta del proyecto contempla ventanas de 24–72 h para corto plazo y aproximadamente 7–14 días para baseline, indicando explícitamente que deben validarse posteriormente. 

---

# 13. Regla fundamental contra leakage

El pipeline debe aplicar:

```python
feature.timestamp <= prediction_at
```

Siempre.

Por ejemplo, para una predicción realizada a:

```text
2026-07-12 08:00
```

no podemos utilizar:

```text
evento 2026-07-12 11:15
detonante del evento
estrategia aplicada
resultado de estrategia
```

porque pertenecen al futuro respecto de la predicción.

Esto parece trivial, pero es uno de los errores más frecuentes en modelos longitudinales.

---

# 14. Schema de calidad de información

Este bloque **no predice riesgo**. Alimenta el motor de confianza.

```yaml
DataQuality:

  variables_criticas_presentes:
    type: integer

  variables_criticas_totales:
    type: integer

  completitud:
    type: float
    range: 0..1

  horas_desde_ultimo_registro:
    type: float

  fuentes_disponibles:
    type: array
    values:
      - familia
      - escuela
      - profesional

  dias_historial_disponibles:
    type: integer

  contiene_datos_sinteticos:
    type: boolean
```

Así mantenemos separados:

```text
            FEATURES
               │
        ┌──────┴──────┐
        ▼             ▼
  MOTOR RIESGO   MOTOR CONFIANZA
        │             │
        ▼             ▼
      78 %           61 %
      ALTO           MEDIA
```

La separación entre **riesgo y confianza** forma parte explícita de Bluba Anticipa y responde directamente al requisito de Bluba de considerar la calidad de la información disponible.  

---

# 15. Contrato final del motor

Una instancia lista para inferencia tendría esta forma:

```json
{
  "id_caso": "PAC-002",
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
    "regulacion_baja_dias_3d": 1,
    "dias_desde_ultima_desregulacion": 11,
    "factores_adversos_simultaneos": 4,
    "exposicion_gatillante_relevante": true
  },

  "data_quality": {
    "completitud": 0.82,
    "fuentes_disponibles": [
      "familia",
      "profesional"
    ],
    "dias_historial_disponibles": 17
  }
}
```

Y la salida:

```json
{
  "risk_score": 0.76,
  "risk_level": "alto",

  "confidence_score": 0.68,
  "confidence_level": "media",

  "top_factors": [
    "sueno_alterado_3_dias",
    "despertar_irritable",
    "cambio_rutina",
    "alerta_fuera_optimo"
  ]
}
```

Los números de este ejemplo son ilustrativos; no representan probabilidades clínicas obtenidas del dataset.

---

# 16. Variables que excluimos del MVP

### `adherencia_medicacion`

Aunque existe en el dataset:

```text
No Aplica
Sí
```

no la incorporaría inicialmente.

En esta muestra está prácticamente asociada a un caso particular y existe riesgo de que funcione como **proxy de identidad** en vez de como señal predictiva generalizable.

### `diagnostico_principal`

Los cuatro casos tienen TEA.

Varianza:

```text
0
```

No aporta información al predictor.

### `rango_edad`

Lo conservaría como metadata pero no entre las variables críticas iniciales.

Con cuatro niños diferentes, el modelo podría aprender indirectamente qué niño es cada uno.

### `estado_asistencia`

Todos los registros:

```text
Presente
```

No aporta señal.

### `detonante_gatillante`

No como predictor contemporáneo por el riesgo de leakage ya explicado.

Sí se mantiene en el **historial de eventos** para personalización y recomendaciones.

---

# 17. Schema lógico definitivo

```text
                    id_caso
                       │
                       ▼
             ┌──────────────────┐
             │ Perfil sensorial │
             └────────┬─────────┘
                      │
      ┌───────────────┼────────────────┐
      ▼               ▼                ▼
   Sueño          Estado diario     Contexto
   ─────          ─────────────     ────────
 calidad          despertar         cambio rutina
 horas            regulación        comportamiento
                  alerta            evento excepcional
                  GI
      │               │                │
      └───────────────┼────────────────┘
                      ▼
              HISTORIAL INDIVIDUAL
                      │
            ┌─────────┴─────────┐
            ▼                   ▼
        BASELINE            VENTANAS
         14 días            24–72 h
            │                   │
            └─────────┬─────────┘
                      ▼
              FEATURES DERIVADAS
                      │
             ┌────────┴─────────┐
             ▼                  ▼
          RIESGO            CONFIANZA
             │                  │
             └────────┬─────────┘
                      ▼
             DESREGULACIÓN 24 h
```

## DATA-02 queda, por tanto, cerrado con esta definición

**11 variables predictoras semánticas**, **1 target explícito**, baseline individual de **14 días**, acumuladores de **72 horas**, historial reciente de **7 días** y horizonte predictivo fijo de **24 horas** para el MVP.

La decisión más importante es que el motor **no entrenará sobre las filas crudas de los CSV**: tendrá una capa `raw data → canonical schema → temporal features → risk/confidence`. Esa separación permitirá implementar el backend ahora y sustituir más adelante el scoring del prototipo por un modelo aprendido sin modificar el contrato de datos.
