# Bluba Anticipa — Contexto de Integración y Contratos

**Versión:** 0.1.0 — MVP NeuroHack 2026  
**Objetivo:** establecer una frontera técnica estable entre **Frontend móvil**, **Backend/API** y **Data/ML**, de modo que los tres equipos puedan evolucionar en paralelo sin duplicar reglas de negocio ni acoplarse a la implementación interna de los otros módulos.

---

## 1. Rol de la carpeta `contracts/`

```text
contracts/
├── openapi.yaml                    # Transporte HTTP y DTOs públicos/internos
├── prediction.schema.json          # Contrato canónico Backend ↔ motor predictivo
├── daily-record.schema.json        # Registro cotidiano confirmado
├── event.schema.json               # Evento de desregulación
├── recommendation.schema.json      # Recomendaciones, intervenciones y resultados
└── features.yaml                   # Catálogo/versionado de features y ventanas
```

Los contratos son la **fuente compartida de verdad** de las interfaces entre módulos. Ningún consumidor debe inferir reglas que pertenezcan a otro componente.

---

## 2. Arquitectura de integración

```text
┌────────────────────┐
│  FRONTEND MÓVIL    │
│ Familia/Edu/Prof.  │
└─────────┬──────────┘
          │ OpenAPI / DTO por rol
          ▼
┌──────────────────────────────────────────┐
│                BACKEND                   │
│                                          │
│ Auth / RBAC / Persistencia / Timeline    │
│ Captura y confirmación humana            │
│ Orquestación de predicción               │
│ Resolución segura de recomendaciones     │
│ Adaptación de DTOs por rol               │
└──────────┬───────────────────┬───────────┘
           │                   │
           │ contratos ML      │ contratos de estrategia
           ▼                   ▼
┌──────────────────────┐   ┌────────────────────────┐
│      DATA / ML       │   │ Recommendation Resolver│
│ Baseline             │   │ Catálogo validado      │
│ Features temporales  │   │ Historial individual   │
│ Risk + Confidence    │   │ Reglas por contexto    │
│ Explainability       │   └────────────────────────┘
└──────────────────────┘
```

### Separación de responsabilidades

**Frontend**
- captura y presenta información;
- consume categorías y explicaciones ya resueltas por Backend;
- no recalcula riesgo, confianza ni umbrales;
- no inventa recomendaciones.

**Backend**
- aplica autenticación y autorización por rol;
- persiste registros y trazabilidad;
- normaliza/orquesta la comunicación con Data/ML;
- dispara recálculo de riesgo cuando entra un dato relevante;
- devuelve DTOs adaptados a familia, educación o especialista;
- selecciona recomendaciones únicamente desde fuentes controladas.

**Data/ML**
- calcula baseline individual;
- construye features longitudinales;
- estima riesgo y confianza de forma independiente;
- devuelve factores explicativos estructurados;
- no genera recomendaciones clínicas libres.

---

## 3. Invariantes del dominio que deben respetar todos los módulos

1. **Horizonte principal:** riesgo de desregulación en las próximas **24 horas** desde `prediction_at`.
2. **Sin fuga temporal:** toda observación/feature usada por el motor debe corresponder a información conocida en o antes de `prediction_at`.
3. **Baseline individual:** el niño se compara prioritariamente con su propio historial.
4. **Ventanas del MVP:** deterioro/acumulación reciente de hasta 72 h, historial de eventos de 7 días y baseline de 14 días válidos cuando exista suficiente información.
5. **Riesgo y confianza son variables distintas.** Una confianza baja no modifica silenciosamente la banda de riesgo.
6. **Dato faltante ≠ dato normal.** La ausencia debe preservarse explícitamente y afectar la calidad/confianza cuando corresponda.
7. **Información insuficiente:** el sistema puede devolver `INSUFFICIENT_DATA` y no presentar un nivel de riesgo aparentemente preciso.
8. **Confirmación humana:** texto/voz estructurado por IA crea primero un `ObservationDraft`; no alimenta el motor hasta ser confirmado.
9. **Recomendaciones controladas:** deben provenir del historial individual, indicaciones profesionales o un catálogo preventivo validado.
10. **Filtrado por rol:** el Backend, no el Frontend, elimina información que un rol no debe recibir.
11. **Recálculo asincrónico:** persistir un registro no debe bloquearse esperando la predicción. Un nuevo dato relevante puede publicar una señal equivalente a `RiskRecalculationRequested(child_id, data_cutoff_at)`.
12. **Trazabilidad:** las predicciones deben identificar la versión del motor/reglas y del schema de features usado para producirlas.
13. **Posicionamiento:** los resultados son apoyo preventivo, no diagnóstico ni probabilidad clínica validada.

---

## 4. Decisiones de contrato tomadas para la versión 0.1

### 4.1 OpenAPI 3.1

Se utiliza **OpenAPI 3.1.0** para mantener compatibilidad conceptual con JSON Schema moderno y facilitar posteriormente referencias externas hacia los archivos `*.schema.json`.

### 4.2 Convención de nombres

Los payloads de máquina utilizan **English `snake_case`** (`sleep_quality`, `wake_state`, `risk_score`, etc.). La interfaz visible puede continuar íntegramente en español.

Esto evita acoplar la API a la redacción de UI y mantiene nombres estables para código y modelos.

### 4.3 Escala canónica de scores

Los scores técnicos de riesgo/confianza se normalizan a **0..1**. Cuando el producto quiera mostrar un porcentaje, la UI puede representarlo como `score × 100`, pero **no debe derivar por su cuenta el nivel LOW/MEDIUM/HIGH**.

El campo `risk.score` sigue siendo un **índice del motor**, no una probabilidad clínica validada.

### 4.4 Estados de predicción

```text
OK
LOW_CONFIDENCE
INSUFFICIENT_DATA
ERROR
```

En `INSUFFICIENT_DATA`, `risk.score` y `risk.level` pueden ser `null`; la respuesta debe seguir informando calidad de datos, faltantes y confianza disponible.

### 4.5 Resultados de intervención

El contrato público del MVP usa exactamente:

```text
SUCCESS | PARTIAL | NO_EFFECT
```

porque coincide con los criterios de aceptación de producto. Si el dominio interno de Data/ML conserva `EFFECTIVE | PARTIALLY_EFFECTIVE | NO_EFFECT`, el Backend debe realizar un mapeo explícito y testeado.

### 4.6 Endpoint interno de predicción

`openapi.yaml` documenta un endpoint interno opcional:

```text
POST /internal/v1/predictions:evaluate
```

Su propósito es fijar el contrato Backend ↔ Data/ML. La implementación puede ser HTTP, llamada in-process, cola/event bus u otro transporte; el **payload canónico debe permanecer equivalente**.

### 4.7 Idempotencia

Las operaciones de escritura aceptan `Idempotency-Key` para tolerar reintentos desde la app móvil y escenarios offline/sincronización sin duplicar registros.

---

## 5. Qué debe permanecer fuera del OpenAPI

El archivo HTTP **no es** la ubicación para codificar:

- pesos del scoring;
- umbrales que convierten scores a bandas;
- fórmulas de confianza;
- definición matemática detallada de features;
- parámetros de baseline;
- lógica de recomendación clínica.

Estas decisiones pertenecen a `features.yaml`, configuración/versionado del motor y contratos específicos de Data/ML. OpenAPI debe transportar **el resultado ya resuelto**, no duplicar la lógica.

---

## 6. Relación con los futuros archivos de contrato

La versión inicial de `openapi.yaml` es **autocontenida** para permitir que Frontend y Backend comiencen inmediatamente.

Cuando se creen los demás archivos, la evolución recomendada es:

```text
prediction.schema.json
        ▲
        └── $ref desde componentes de predicción de openapi.yaml

daily-record.schema.json
        ▲
        └── $ref desde POST /daily-records y timeline

event.schema.json
        ▲
        └── $ref desde /dysregulation-events

recommendation.schema.json
        ▲
        └── $ref desde recommendations/interventions/results

features.yaml
        ▲
        └── versionado por feature_schema_version
```

Esto elimina duplicación sin bloquear el desarrollo inicial.

---

## 7. Decisiones todavía abiertas

Estas decisiones deben cerrarse antes de considerar `contracts/` estable para producción:

1. **Visibilidad del score numérico por rol:** definir si familia/educación reciben `risk.score` o solo la banda/texto; especialista sí requiere mayor profundidad.
2. **Enum canónico gastrointestinal:** cerrar categorías exactas basadas en el dataset/modelo normalizado.
3. **Nomenclatura interna definitiva de `InterventionResult`:** mantener mapeo o unificar dominio y API.
4. **Autenticación productiva:** OAuth/OIDC/proveedor final; el MVP solo define una sesión demostrativa y bearer token.
5. **Versionado externo de JSON Schemas:** definir `$id`, política semver y compatibilidad backward/forward.
6. **Eventos de integración:** decidir si `RiskRecalculationRequested` se implementará como cola/event bus o llamada interna en el MVP.

---

## 8. Criterio de integración P0

Las tres capas se consideran alineadas cuando el mismo conjunto de contratos permite ejecutar:

```text
1. Usuario registra/check-in o confirma una observación IA
2. Backend persiste DailyRecord
3. Backend solicita recálculo
4. Data/ML calcula baseline + features + RiskPrediction versionada
5. Backend entrega riesgo + confianza + factores + faltantes
6. Frontend presenta un DTO adecuado al rol
7. Backend resuelve una recomendación desde una fuente controlada
8. Usuario registra intervención y resultado
9. Feedback queda asociado a la predicción evaluada
```

La demo debe cubrir como mínimo: día estable, deterioro gradual, riesgo con información incompleta, información insuficiente e intervención con feedback posterior.
