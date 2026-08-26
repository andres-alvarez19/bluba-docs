# DATA-01 — Análisis del dataset Bluba

## 1. Resumen de tablas

| Archivo                             | Registros | Entidad                    | Clave primaria       | Relación principal |
| ----------------------------------- | --------: | -------------------------- | -------------------- | ------------------ |
| `1_casos_anonimizados.csv`          |         4 | Niño/caso                  | `id_caso`            | Tabla raíz         |
| `2_programas_y_objetivos.csv`       |        10 | Objetivos terapéuticos     | `id_objetivo`        | `id_caso → casos`  |
| `3_sesiones_profesionales.csv`      |        37 | Sesiones terapéuticas      | `id_sesion`          | `id_caso → casos`  |
| `4_seguimiento_diario_tutor.csv`    |       102 | Seguimiento cotidiano      | `id_registro_diario` | `id_caso → casos`  |
| `5_eventos_desregulacion_tutor.csv` |         7 | Episodios de desregulación | `id_evento`          | `id_caso → casos`  |

La estructura lógica queda así:

```text
                        ┌─ programas_y_objetivos
                        │
casos_anonimizados ─────┼─ sesiones_profesionales
       id_caso          │
                        ├─ seguimiento_diario_tutor
                        │
                        └─ eventos_desregulacion_tutor
```

`id_caso` es, por tanto, el **identificador central para construir la línea temporal individual de cada niño**.

---

# 2. Diccionario de datos simplificado

## `1_casos_anonimizados.csv`

| Campo                           | Tipo semántico     | Descripción                   | Uso                                |
| ------------------------------- | ------------------ | ----------------------------- | ---------------------------------- |
| `id_caso`                       | Identificador      | ID anonimizado, ej. `PAC-001` | **PK / clave central**             |
| `rango_edad`                    | Categórica ordinal | Ej. `4-5 años`, `8-9 años`    | Contexto basal                     |
| `diagnostico_principal`         | Categórica         | Diagnóstico principal         | Contexto basal                     |
| `perfil_sensorial_predominante` | Categórica / texto | Perfil sensorial del niño     | **Potencial predictor importante** |

### Observación

Los 4 casos tienen diagnóstico:

`Trastorno del Espectro Autista (TEA)`

Por lo tanto, `diagnostico_principal` tiene **varianza cero en esta muestra** y no aportaría información a un modelo entrenado únicamente con estos datos.

---

## `2_programas_y_objetivos.csv`

| Campo                   | Tipo               | Descripción                               | Uso                  |
| ----------------------- | ------------------ | ----------------------------------------- | -------------------- |
| `id_objetivo`           | Identificador      | Objetivo terapéutico                      | PK                   |
| `id_caso`               | Identificador      | Niño asociado                             | FK                   |
| `profesion_responsable` | Categórica         | TO, Fonoaudiólogo, Psicopedagogo          | Contexto profesional |
| `area_intervencion`     | Categórica/texto   | Área terapéutica                          | Contexto             |
| `descripcion_objetivo`  | Texto libre        | Objetivo clínico/terapéutico              | NLP / contexto       |
| `estado_objetivo`       | Categórica ordinal | `En Progreso`, `Logrado`, `En Mantención` | Estado longitudinal  |

### Distribución

* Terapeuta Ocupacional: 6 objetivos
* Fonoaudiólogo: 3
* Psicopedagogo: 1

Este archivo es principalmente **contextual**, no una fuente temporal diaria.

---

## `3_sesiones_profesionales.csv`

| Campo                       | Tipo               | Descripción                    | Uso predictivo          |
| --------------------------- | ------------------ | ------------------------------ | ----------------------- |
| `id_sesion`                 | Identificador      | Sesión profesional             | PK                      |
| `id_caso`                   | Identificador      | Niño                           | FK                      |
| `fecha_sesion`              | **Fecha**          | Día de la sesión               | **Temporal**            |
| `profesion`                 | Categórica         | Profesional que realizó sesión | Contexto                |
| `estado_asistencia`         | Categórica         | Estado de asistencia           | Bajo valor en muestra   |
| `nivel_alerta_inicial`      | Categórica ordinal | Estado al comenzar sesión      | **Muy relevante**       |
| `actividades_realizadas`    | Texto              | Intervenciones realizadas      | NLP/contexto            |
| `nivel_alerta_final`        | Categórica ordinal | Estado tras sesión             | **Relevante**           |
| `observaciones_profesional` | Texto libre        | Observación profesional        | **Muy relevante / NLP** |

Valores de `nivel_alerta_inicial`:

* `Bajo (Letárgico)`
* `Óptimo (Regulado)`
* `Alto (Sobreexcitado)`

Todos los 37 registros tienen:

`estado_asistencia = Presente`

Por tanto, esa variable tampoco discrimina casos en esta muestra.

---

## `4_seguimiento_diario_tutor.csv`

Esta es probablemente la **tabla principal de features para el modelo de anticipación**.

| Campo                          | Tipo               | Descripción               | Uso predictivo                      |
| ------------------------------ | ------------------ | ------------------------- | ----------------------------------- |
| `id_registro_diario`           | Identificador      | Registro diario           | PK                                  |
| `id_caso`                      | Identificador      | Niño                      | FK                                  |
| `fecha`                        | **Fecha**          | Día observado             | **Eje temporal principal**          |
| `calidad_sueno`                | Categórica ordinal | Calidad del sueño         | **Muy relevante**                   |
| `modo_despertar`               | Categórica ordinal | Estado al despertar       | **Muy relevante**                   |
| `adherencia_medicacion`        | Categórica         | Adherencia                | Potencial                           |
| `estado_gastrointestinal`      | Categórica         | Estado GI                 | **Relevante**                       |
| `nivel_regulacion_general_dia` | Categórica ordinal | Regulación global del día | Relevante, pero debe usarse con lag |

### Valores principales

**Calidad de sueño**

| Valor                      | Registros |
| -------------------------- | --------: |
| Reparador                  |        52 |
| Interrumpido               |        34 |
| Dificultad de Conciliación |        16 |

**Modo de despertar**

| Valor              | Registros |
| ------------------ | --------: |
| Tranquilo/Alegre   |        52 |
| Irritable/Llorando |        28 |
| Cansado/Con Sueño  |        22 |

**Estado gastrointestinal**

| Valor         | Registros |
| ------------- | --------: |
| Normal        |        75 |
| Estreñimiento |        17 |
| Diarrea       |        10 |

**Regulación del día**

| Valor                   | Registros |
| ----------------------- | --------: |
| Estable con Apoyo       |        47 |
| Excelente               |        30 |
| Desregulación Frecuente |        25 |

Estas variables encajan directamente con el enfoque de **baseline individual + acumuladores temporales** que estamos planteando.

---

## `5_eventos_desregulacion_tutor.csv`

Esta es la principal candidata para generar la **variable objetivo/label** del modelo.

| Campo                       | Tipo               | Descripción               | Uso                 |
| --------------------------- | ------------------ | ------------------------- | ------------------- |
| `id_evento`                 | Identificador      | Evento                    | PK                  |
| `id_caso`                   | Identificador      | Niño                      | FK                  |
| `fecha_hora`                | **Timestamp**      | Momento exacto del evento | **Target temporal** |
| `tipo_evento`               | Categórica         | Naturaleza del episodio   | Target/contexto     |
| `intensidad`                | Categórica ordinal | Leve / Moderada / Severa  | **Severidad**       |
| `detonante_gatillante`      | Texto libre        | Gatillante identificado   | Explicabilidad/NLP  |
| `estrategia_calma_aplicada` | Texto libre        | Intervención utilizada    | Recomendaciones     |
| `resultado_estrategia`      | Categórica         | Resultado                 | Recomendaciones     |

Tipos presentes:

| Tipo                    | Nº |
| ----------------------- | -: |
| Sobrecarga Sensorial    |  2 |
| Transición de Actividad |  2 |
| Alimentación            |  2 |
| Desregulación Emocional |  1 |

Intensidad:

| Intensidad     | Nº |
| -------------- | -: |
| Leve (1-3)     |  2 |
| Moderada (4-7) |  3 |
| Severa (8-10)  |  2 |

Aunque contiene números dentro del texto, `intensidad` **no está almacenada como una variable numérica**, sino como categoría ordinal.

---

# 3. Variables categóricas, numéricas y temporales

Un hallazgo importante es que **el dataset no contiene variables numéricas continuas reales**.

La gran mayoría de las variables son categóricas o texto libre.

### Temporales

```text
sesiones_profesionales.fecha_sesion       DATE
seguimiento_diario_tutor.fecha           DATE
eventos_desregulacion_tutor.fecha_hora   DATETIME
```

### Ordinales que podemos codificar

```text
calidad_sueno
modo_despertar
nivel_regulacion_general_dia
nivel_alerta_inicial
nivel_alerta_final
intensidad
estado_objetivo
rango_edad
```

Por ejemplo:

```text
calidad_sueno

Reparador                  → 0
Interrumpido               → 1
Dificultad de Conciliación → 2
```

Esto sería una transformación nuestra; **el archivo original no define una escala numérica**.

---

# 4. Datos faltantes

Aquí aparece una distinción importante.

## Nulls dentro de los CSV

**No existe ningún `NULL`, `NaN` o campo vacío** en los 160 registros.

A nivel tabular, la completitud es 100 %.

Pero longitudinalmente sí faltan datos.

El seguimiento cubre julio de 2026, es decir:

```text
31 días × 4 niños = 124 días potenciales
```

Existen solamente:

```text
102 registros diarios
```

Por tanto:

**22 días-caso no tienen registro**, equivalente a una cobertura temporal aproximada de **82,3 %**.

| Niño    | Registros diarios | Días faltantes |
| ------- | ----------------: | -------------: |
| PAC-001 |                30 |              1 |
| PAC-002 |                21 |         **10** |
| PAC-003 |                29 |              2 |
| PAC-004 |                22 |          **9** |

Esto es mucho más relevante para el MVP que simplemente comprobar `NULL`.

De hecho, reproduce exactamente uno de los problemas que Bluba pide abordar: información cotidiana incompleta o inconsistente. 

---

# 5. Relación seguimiento diario ↔ desregulaciones

La relación natural es:

```text
seguimiento_diario.id_caso
          +
seguimiento_diario.fecha

        ↕ JOIN

eventos.id_caso
          +
DATE(eventos.fecha_hora)
```

Hay **7 eventos de desregulación**.

De ellos:

* 6 tienen seguimiento diario registrado el mismo día.
* 1 no tiene seguimiento diario ese día.

La excepción es:

```text
EVT-004
PAC-002
2026-07-18 19:20
Sobrecarga Sensorial
Moderada (4-7)
```

Para `PAC-002` **no existe registro diario el 18 de julio**.

Esto es un ejemplo muy útil para demostrar en el prototipo cómo manejar datos faltantes.

---

## Hallazgo importante para el modelo de 24 horas

Los **7 eventos sí tienen disponible un seguimiento diario del día anterior**.

Eso significa que podemos construir conceptualmente:

```text
Información de t
     ↓
predicción
     ↓
evento de desregulación en t+1 / próximas 24 h
```

Esto encaja especialmente bien con la pregunta de Bluba sobre anticipar señales aproximadamente 24 horas antes. 

---

# 6. El seguimiento diario NO debe utilizarse directamente como etiqueta de crisis

Este es uno de los hallazgos más importantes del análisis.

De los 6 eventos que pueden cruzarse con seguimiento del mismo día:

| Evento  | Regulación general del día |
| ------- | -------------------------- |
| EVT-001 | Estable con Apoyo          |
| EVT-002 | Desregulación Frecuente    |
| EVT-003 | Estable con Apoyo          |
| EVT-005 | Excelente                  |
| EVT-006 | Excelente                  |
| EVT-007 | Estable con Apoyo          |

Es decir:

**5 de los 6 días con un evento registrado NO están marcados como `Desregulación Frecuente`.**

Por tanto:

```text
nivel_regulacion_general_dia == "Desregulación Frecuente"
```

**no puede interpretarse como sinónimo de “ocurrió una crisis”.**

Para definir la variable objetivo del modelo deberíamos utilizar principalmente:

```text
5_eventos_desregulacion_tutor.csv
```

y considerar `nivel_regulacion_general_dia` como una variable histórica/contextual.

---

# 7. Relación con sesiones profesionales

La relación básica es:

```text
sesiones_profesionales.id_caso
                ↓
          casos.id_caso
```

Hay 37 sesiones.

De ellas:

```text
31 / 37
```

tienen un registro diario correspondiente al mismo niño y fecha.

Las 6 sesiones sin seguimiento diario del mismo día son:

| Sesión  | Niño    | Fecha | Profesional           |
| ------- | ------- | ----- | --------------------- |
| SES-004 | PAC-002 | 03-07 | Psicopedagogo         |
| SES-016 | PAC-002 | 14-07 | Terapeuta Ocupacional |
| SES-021 | PAC-004 | 17-07 | Fonoaudiólogo         |
| SES-023 | PAC-003 | 20-07 | Terapeuta Ocupacional |
| SES-029 | PAC-004 | 24-07 | Fonoaudiólogo         |
| SES-035 | PAC-003 | 30-07 | Fonoaudiólogo         |

Esto vuelve a confirmar que la plataforma debe tolerar **fuentes asincrónicas y parcialmente completas**.

---

# 8. Sesiones ↔ objetivos terapéuticos

Aquí existe una limitación estructural.

`sesiones_profesionales` **no contiene `id_objetivo`**.

Por tanto no se puede hacer:

```text
sesion → objetivo específico
```

de forma inequívoca.

Solamente puede inferirse mediante:

```text
id_caso + profesion
```

Esto genera ambigüedad porque un niño puede tener más de un objetivo asignado al mismo profesional.

Además encontré dos situaciones relevantes:

```text
PAC-003
tiene sesiones de Fonoaudiólogo
pero no tiene objetivo de Fonoaudiólogo.
```

Mientras:

```text
PAC-002
tiene un objetivo de Fonoaudiólogo
pero no aparecen sesiones de Fonoaudiólogo durante el período.
```

No necesariamente son errores —pueden ser perfectamente válidos clínicamente—, pero **no tenemos información suficiente para vincular sesiones y objetivos de forma exacta**.

---

# 9. Posibles inconsistencias o limitaciones

| Hallazgo                                                                         | Consecuencia                                                              |
| -------------------------------------------------------------------------------- | ------------------------------------------------------------------------- |
| 22 días-caso sin seguimiento                                                     | Ventanas temporales incompletas                                           |
| 1 evento sin seguimiento del mismo día                                           | Debe soportarse predicción con datos parciales                            |
| 6 sesiones sin seguimiento del mismo día                                         | Fuentes asincrónicas                                                      |
| No existe `id_objetivo` en sesiones                                              | No se puede vincular sesión → objetivo exactamente                        |
| Todos los diagnósticos son TEA                                                   | Variable sin capacidad discriminativa                                     |
| Todas las sesiones tienen asistencia `Presente`                                  | Variable sin capacidad discriminativa                                     |
| No hay variables numéricas continuas                                             | Necesidad de encoding / feature engineering                               |
| Solo 7 eventos                                                                   | **Insuficiente para entrenar y validar seriamente un modelo supervisado** |
| Mucho texto repetido en las sesiones                                             | Posible dataset sintético/templateado                                     |
| Intensidad está almacenada como texto                                            | Requiere normalización                                                    |
| Edad almacenada como rango textual                                               | Requiere transformación si se quiere utilizar numéricamente               |
| Medicación es `Sí` únicamente para PAC-002                                       | Puede convertirse accidentalmente en proxy del niño                       |
| `nivel_regulacion_general_dia` no coincide directamente con presencia de eventos | No usar como label principal                                              |

---

# 10. Diferencia entre las variables prometidas por el desafío y el CSV entregado

Las bases mencionan, entre otras:

* horas y calidad de sueño;
* cambios de rutina;
* cambios en alimentación;
* comportamiento observado;
* interacciones sociales;
* estado de alerta;
* actividades escolares;
* eventos excepcionales. 

Sin embargo, el CSV diario entregado contiene únicamente:

```text
calidad_sueno
modo_despertar
adherencia_medicacion
estado_gastrointestinal
nivel_regulacion_general_dia
```

Por ejemplo, **no tenemos `horas_sueno`**, pese a que las bases mencionan horas y calidad del sueño.

Esto no invalida el dataset: la presentación oficial señala que se pueden utilizar **datos sintéticos o simulados para representar escenarios realistas**. 

Para nuestro MVP, esto significa que debemos distinguir claramente entre:

```text
Variables disponibles en los CSV entregados
```

y

```text
Variables que la arquitectura de Bluba podría incorporar posteriormente
```

---

# 11. Variables prioritarias para el MVP

A partir del dataset real entregado, la primera versión del pipeline debería centrarse en:

| Prioridad | Variable                                  | Fuente             |
| --------- | ----------------------------------------- | ------------------ |
| P0        | Calidad del sueño                         | Seguimiento diario |
| P0        | Modo de despertar                         | Seguimiento diario |
| P0        | Estado gastrointestinal                   | Seguimiento diario |
| P0        | Regulación de días anteriores             | Seguimiento diario |
| P0        | Historial reciente de eventos             | Eventos            |
| P0        | Intensidad de eventos anteriores          | Eventos            |
| P1        | Perfil sensorial del niño                 | Casos              |
| P1        | Nivel de alerta inicial de sesión         | Sesiones           |
| P1        | Nivel de alerta final                     | Sesiones           |
| P1        | Observaciones profesionales               | Sesiones           |
| P1        | Detonantes históricos                     | Eventos            |
| P1        | Estrategias que anteriormente funcionaron | Eventos            |

Esto da una base suficiente para implementar la idea de **baseline individual + acumuladores de riesgo + historial de eventos**.

---

# Conclusión de DATA-01

El modelo de datos del MVP debería tomar a:

```text
id_caso
```

como eje central y construir una **línea temporal por niño**:

```text
perfil basal
    ↓
seguimientos diarios
    ↓
sesiones profesionales
    ↓
historial de eventos
    ↓
features de últimos N días
    ↓
riesgo próximas 24 h
```

El hallazgo técnico más relevante es que **no estamos frente a un problema tradicional de filas independientes**, sino frente a un **dataset longitudinal irregular y multimodal**, con fuentes que no siempre se registran el mismo día.

Eso favorece directamente el enfoque que hemos definido para el proyecto: comparar cada niño contra **su propia línea base histórica**, construir variables acumuladas en ventanas temporales y acompañar cada predicción con una medida explícita de **calidad/completitud de los datos**. Además, con solo **7 eventos positivos**, este conjunto debe tratarse como **dataset demostrativo para el prototipo**, no como evidencia suficiente para afirmar rendimiento clínico real del predictor.
