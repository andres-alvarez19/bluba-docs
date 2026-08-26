
## Sistema inteligente de alerta temprana y apoyo preventivo para niños neurodivergentes

### 1. Resumen ejecutivo

**Bluba Anticipa** es una extensión inteligente de la plataforma Bluba orientada a **identificar señales tempranas de riesgo de desregulación conductual en niños neurodivergentes durante las próximas horas o el día siguiente**, utilizando información cotidiana registrada por familias, establecimientos educacionales y profesionales.

La propuesta no busca diagnosticar una crisis ni reemplazar el juicio de cuidadores o especialistas. Su objetivo es transformar registros cotidianos dispersos en una herramienta preventiva capaz de responder cuatro preguntas:

1. **¿El niño presenta hoy un patrón diferente de su estado habitual?**
    
2. **¿Se están acumulando factores que históricamente han precedido episodios de desregulación?**
    
3. **¿Qué factores explican el riesgo estimado?**
    
4. **¿Qué estrategias han resultado útiles anteriormente para este niño en situaciones similares?**
    

El elemento central de Bluba Anticipa es que **no compara al niño principalmente contra otros niños**, sino contra su propio comportamiento histórico.

El sistema construye una **línea base individual dinámica**, analiza desviaciones y tendencias recientes, combina información proveniente de diferentes contextos y genera:

- un **nivel de riesgo estimado**;
    
- un **nivel independiente de confianza de la predicción**;
    
- las principales variables que explican el riesgo;
    
- recomendaciones preventivas contextualizadas;
    
- una advertencia cuando la información disponible es insuficiente.
    

El registro de información puede realizarse mediante los mecanismos tradicionales de Bluba o mediante un **asistente conversacional por texto o voz**, capaz de transformar lenguaje cotidiano en variables estructuradas, siempre solicitando confirmación al usuario antes de guardar la información.

---

# 2. Problema que buscamos resolver

Bluba dispone de información longitudinal relevante sobre el funcionamiento cotidiano de cada niño:

- sueño;
    
- estado al despertar;
    
- salud gastrointestinal;
    
- cambios de rutina;
    
- nivel de alerta;
    
- regulación o desregulación;
    
- comportamiento observado;
    
- interacciones sociales;
    
- participación escolar y terapéutica;
    
- eventos excepcionales;
    
- antecedentes históricos de crisis;
    
- observaciones de familias y profesionales.
    

Sin embargo, estos datos adquieren mayor valor cuando pueden ser analizados conjuntamente a lo largo del tiempo.

Actualmente, una familia, profesor o profesional puede reconocer retrospectivamente que varios factores se acumularon antes de una crisis:

> “Durmió mal durante varios días, estuvo más irritable, cambiaron su rutina y además hubo mucho ruido en el colegio.”

El problema es que estas señales pueden encontrarse distribuidas entre múltiples registros, días y personas.

**Bluba Anticipa busca identificar esa acumulación antes de que el episodio ocurra.**

La documentación oficial del desafío plantea justamente la necesidad de generar un indicador para las próximas horas o el día siguiente, explicar el resultado de manera comprensible y considerar situaciones de información incompleta.

---

# 3. Hipótesis central

Nuestra hipótesis es que una desregulación no necesariamente aparece como consecuencia de una única variable extrema, sino que puede estar precedida por una **combinación temporal de cambios respecto del patrón habitual del niño**.

Por ejemplo:

```text
Déficit de sueño
        +
Cambio de rutina
        +
Mayor irritabilidad
        +
Contexto sensorial desfavorable
        +
Menor capacidad de regulación
        ↓
Aumento progresivo del riesgo
```

Por lo tanto, Bluba Anticipa no trabaja únicamente con el valor actual de cada variable.

Analiza cuatro dimensiones:

### 1. Estado actual

¿Qué ocurre hoy?

### 2. Desviación respecto del patrón individual

¿Qué tan diferente está hoy respecto de cómo suele estar este niño?

### 3. Acumulación temporal

¿El mismo problema viene ocurriendo durante varios días?

### 4. Combinación de factores

¿Existen varios factores adversos ocurriendo simultáneamente?

---

# 4. Fundamento científico de la aproximación

La propuesta debe distinguir entre dos cosas:

**a. Variables y principios que cuentan con respaldo científico.**

**b. La capacidad específica de predecir una desregulación 24 horas antes utilizando los registros de Bluba, que todavía debe ser validada experimentalmente.**

No afirmamos que exista actualmente evidencia científica suficiente para garantizar una predicción clínica a 24 horas basada exclusivamente en estos registros.

Lo que sí existe es evidencia que respalda varios de los componentes sobre los que construimos nuestra hipótesis.

## 4.1 Sueño

Los problemas de sueño presentan asociaciones consistentes con dificultades conductuales en niños y adolescentes autistas.

Kim et al. realizaron una revisión sistemática y metaanálisis de 22 estudios que incluyeron 2.655 participantes y encontraron una asociación entre problemas globales de sueño y problemas conductuales.

Por ello, Bluba Anticipa considera especialmente relevante:

- deterioro de calidad del sueño;
    
- noches consecutivas con sueño alterado;
    
- cambios respecto del patrón habitual;
    
- interacción entre sueño y estado al despertar.
    

**Referencia:**  
Kim, H. et al. (2024). _Correlations between sleep problems, core symptoms, and behavioral problems in children and adolescents with autism spectrum disorder: a systematic review and meta-analysis_. European Child & Adolescent Psychiatry. DOI: 10.1007/s00787-023-02253-1.

---

## 4.2 Procesamiento sensorial

Las diferencias de procesamiento sensorial también pueden relacionarse con irritabilidad y otras conductas.

Griffin et al. estudiaron 75 niños autistas y encontraron asociaciones entre diferentes patrones de procesamiento sensorial y conductas como irritabilidad e hiperactividad/no cooperación.

Por ello, el **perfil sensorial individual** es particularmente importante.

Una situación de ruido elevado no debería interpretarse igual para todos los niños.

Ejemplo:

```text
Niño A
Perfil: hipersensibilidad auditiva
Evento: música fuerte + aglomeración
→ Contexto potencialmente significativo

Niño B
Perfil: buscador vestibular
Mismo evento
→ Relevancia potencial diferente
```

**Referencia:**  
Griffin, Z. A. M. et al. (2022). _Atypical sensory processing features in children with autism, and their relationships with maladaptive behaviors and caregiver strain_. Autism Research, 15(6), 1120–1129. DOI: 10.1002/aur.2700.

---

## 4.3 Salud gastrointestinal

Los síntomas gastrointestinales son frecuentes en población autista y algunos estudios encuentran relaciones con sueño, conducta y otros factores, aunque la evidencia sobre estas relaciones es heterogénea.

Una revisión sistemática de 30 estudios de Leader et al. concluyó que los síntomas gastrointestinales son frecuentes, pero también enfatizó la existencia de resultados contradictorios respecto de varias asociaciones.

Por esto Bluba Anticipa **no interpreta una alteración gastrointestinal como causa de una crisis**.

La utiliza como una señal contextual que puede o no adquirir relevancia dependiendo del historial individual.

**Referencia:**  
Leader, G. et al. (2022). _Gastrointestinal Symptoms in Autism Spectrum Disorder: A Systematic Review_. Nutrients, 14(7), 1471. DOI: 10.3390/nu14071471.

---

# 5. Principio diferenciador: línea base individual

El componente principal del sistema es un **baseline o línea base dinámica por niño**.

En lugar de utilizar reglas universales como:

> “Dormir mal aumenta el riesgo en 20 %.”

el sistema analiza:

> “¿Dormir así es anormal para este niño?”

Esto permite capturar la elevada variabilidad individual de la neurodivergencia.

Ejemplo:

```text
Patrón histórico de Juan
─────────────────────────────
Sueño reparador       80 %
Despertar tranquilo   75 %
Regulación estable    78 %

Últimos 3 días
─────────────────────────────
Sueño alterado        3/3
Despertar irritable   2/3
Regulación baja       2/3

→ Desviación importante
→ Acumulación temporal
→ Aumento del índice de riesgo
```

La relevancia de la personalización también aparece en investigaciones de biosensores.

Goodwin et al. utilizaron datos cardiovasculares, actividad electrodérmica y movimiento en 20 jóvenes autistas. Los modelos dependientes de la persona alcanzaron un AUC promedio de 0,84 para anticipar agresión un minuto antes, frente a 0,71 en el modelo global.

Este resultado corresponde a otro horizonte temporal y otra fuente de datos, por lo que **no demuestra nuestra predicción a 24 horas**. Sin embargo, proporciona evidencia relevante a favor de considerar la heterogeneidad individual en sistemas predictivos.

**Referencia:**  
Goodwin, M. S. et al. (2019). _Predicting aggression to others in youth with autism using a wearable biosensor_. Autism Research, 12(8), 1286–1296. DOI: 10.1002/aur.2151.

---

# 6. Fuentes de información utilizadas

Bluba Anticipa integra información longitudinal proveniente de diferentes actores.

## Familia

Puede registrar:

- sueño;
    
- despertar;
    
- alimentación;
    
- síntomas gastrointestinales;
    
- regulación;
    
- comportamiento;
    
- cambios en rutina;
    
- eventos excepcionales;
    
- posibles gatillantes;
    
- estrategias utilizadas.
    

## Establecimiento educacional

Puede registrar:

- estado de alerta;
    
- comportamiento observado;
    
- participación;
    
- recreos;
    
- interacciones sociales;
    
- cambios en rutina;
    
- posibles sobrecargas sensoriales;
    
- episodios de regulación/desregulación.
    

## Profesionales

Pueden aportar:

- observaciones terapéuticas;
    
- nivel de alerta inicial/final;
    
- objetivos de intervención;
    
- cambios relevantes;
    
- perfil sensorial;
    
- estrategias recomendadas;
    
- evolución longitudinal.
    

---

# 7. Asistente conversacional para reducir la carga de registro

Uno de los problemas identificados por Bluba es la carga que supone mantener registros completos y oportunos.

Por eso proponemos que el usuario no siempre deba completar formularios manualmente.

Puede escribir o dictar:

> “Anoche se despertó varias veces. Hoy despertó bastante irritable y nos avisaron que cambiarán la sala de clases.”

El sistema transforma esa información en:

```text
Calidad del sueño:
Interrumpido

Estado al despertar:
Irritable

Cambio de rutina:
Sí

Contexto:
Escolar
```

Antes de guardar:

```text
¿Interpreté correctamente el registro?

✓ Sueño interrumpido
✓ Despertar irritable
✓ Cambio de rutina escolar

[Confirmar]    [Editar]
```

## Principio técnico

El modelo de lenguaje **no determina el riesgo directamente**.

Su responsabilidad principal es:

```text
Lenguaje natural
      ↓
Extracción de información
      ↓
Variables estructuradas
      ↓
Confirmación humana
      ↓
Base de datos Bluba
```

Posteriormente, un motor predictivo separado utiliza esas variables.

Esta separación mejora:

- trazabilidad;
    
- mantenibilidad;
    
- seguridad;
    
- explicabilidad;
    
- posibilidad de reemplazar modelos tecnológicos sin alterar la lógica de negocio.
    

---

# 8. Motor Bluba Anticipa

El sistema completo puede conceptualizarse en cinco etapas.

```text
              DATOS BLUBA
                   │
        ┌──────────┼──────────┐
        │          │          │
     Familia    Escuela   Profesional
        │          │          │
        └──────────┼──────────┘
                   ▼
          Normalización de datos
                   │
                   ▼
          Baseline individual
                   │
                   ▼
        Análisis temporal/contextual
                   │
                   ▼
            Motor de riesgo
                   │
        ┌──────────┴───────────┐
        ▼                      ▼
      RIESGO                CONFIANZA
        │                      │
        └──────────┬───────────┘
                   ▼
               EXPLICACIÓN
                   │
                   ▼
             RECOMENDACIÓN
```

---

# 9. Variables derivadas

El valor diferenciador no está solamente en almacenar las variables originales, sino en construir **features temporales e individuales**.

Por ejemplo:

### Variables directas

```text
calidad_sueno
modo_despertar
estado_gastrointestinal
nivel_regulacion
nivel_alerta
cambio_rutina
gatillante
```

### Variables derivadas

```text
dias_sueno_alterado_ultimos_3_dias

desviacion_sueno_vs_baseline

cambio_regulacion_vs_semana_anterior

cantidad_eventos_ultimos_7_dias

dias_desde_ultima_desregulacion

numero_factores_adversos_simultaneos

exposicion_trigger_sensorial_relevante

tendencia_regulacion_ultimos_3_dias
```

Estas variables permiten pasar de un sistema descriptivo a un sistema temporal.

---

# 10. Ventanas temporales

Proponemos analizar diferentes escalas simultáneamente.

## Estado inmediato

Último registro disponible.

## Corto plazo

Últimas 24–72 horas.

Permite identificar acumuladores.

## Baseline reciente

Por ejemplo, últimos 7–14 días válidos.

Permite identificar desviaciones individuales.

## Histórico

Eventos anteriores del niño.

Permite reconocer patrones recurrentes.

Los tamaños definitivos de las ventanas deberán ser validados posteriormente con datos reales longitudinales.

---

# 11. Modelo predictivo del MVP

Dado que el dataset entregado inicialmente contiene:

- 4 casos anonimizados;
    
- 102 seguimientos diarios;
    
- 37 sesiones profesionales;
    
- 7 eventos de desregulación;
    
- 10 objetivos terapéuticos,
    

**no existe suficiente evidencia para entrenar y validar responsablemente un modelo predictivo clínico real.**

Por esta razón, el MVP no debe presentar una supuesta exactitud clínica obtenida desde estos datos.

La propia organización permite utilizar información simulada o sintética para representar escenarios realistas durante el desafío.

## Estrategia propuesta para el prototipo

Implementar un **motor híbrido explicable**.

### Capa 1 — Normalización individual

Cada variable se compara con el historial del niño.

### Capa 2 — Acumuladores temporales

Se detectan factores persistentes.

### Capa 3 — Interacciones

Se identifica cuándo diferentes señales aparecen simultáneamente.

### Capa 4 — Modelo de riesgo

Para el prototipo se utiliza un modelo interpretable sobre escenarios sintéticos.

Una primera alternativa razonable es regresión logística o un sistema de scoring explícito.

Posteriormente se pueden comparar:

- regresión logística;
    
- Random Forest;
    
- Gradient Boosting;
    
- XGBoost;
    
- modelos temporales.
    

El objetivo no debe ser utilizar el algoritmo más complejo, sino encontrar el mejor equilibrio entre:

**capacidad predictiva + calibración + explicabilidad + robustez.**

---

# 12. Riesgo y confianza son conceptos diferentes

Este es uno de los elementos centrales de Bluba Anticipa.

Una predicción puede indicar:

```text
RIESGO
78 %
ALTO
```

pero simultáneamente:

```text
CONFIANZA
54 %
MEDIA
```

¿Por qué?

Porque aunque los datos disponibles muestran señales preocupantes, puede faltar información relevante.

## Riesgo

Responde:

> ¿Qué tan compatible es el patrón actual con situaciones históricamente asociadas a desregulación?

## Confianza

Responde:

> ¿Qué tan sólida es la información disponible para realizar esta estimación?

La confianza puede considerar:

- completitud;
    
- antigüedad del último registro;
    
- número de fuentes disponibles;
    
- cantidad de historial individual;
    
- presencia de variables críticas;
    
- contradicciones entre registros.
    

Ejemplo:

```text
RIESGO: ALTO
78 %

CONFIANZA: MEDIA
61 %

Información faltante:
• No existe registro escolar de hoy.
• No se registró alimentación.
```

Esto evita transmitir una falsa sensación de certeza.

---

# 13. Explicabilidad

Nunca deberíamos entregar solamente:

> “Riesgo 82 %.”

Bluba Anticipa debe explicar:

```text
RIESGO ELEVADO
Próximas 24 horas

Principales factores detectados:

1. Alteración del sueño durante 3 días consecutivos.
2. Regulación inferior a su patrón habitual.
3. Cambio reciente de rutina escolar.
4. Evento sensorial compatible con su perfil de sensibilidad.
```

La explicación transforma una predicción técnica en una herramienta accionable.

---

# 14. Memoria de intervenciones

Bluba ya dispone de una estructura particularmente valiosa:

```text
Detonante
    ↓
Estrategia aplicada
    ↓
Resultado
```

Esto permite que Bluba Anticipa no solamente aprenda:

> “¿Qué situaciones suelen preceder una crisis?”

sino también:

> “¿Qué ha funcionado anteriormente para este niño?”

Ejemplo:

```text
Situación detectada:
Sobrecarga auditiva

Eventos similares anteriores:
3

Estrategia con mejor resultado:
Salida temporal a ambiente de menor estimulación

Resultados anteriores:
2 regulaciones exitosas
```

De esta manera, la recomendación deja de ser completamente genérica.

---

# 15. Generación segura de recomendaciones

No proponemos permitir que un modelo generativo invente libremente intervenciones terapéuticas.

La arquitectura recomendada es:

```text
Riesgo identificado
       ↓
Factores relevantes
       ↓
Consulta a:
• estrategias aprobadas por profesionales
• estrategias históricas exitosas
• catálogo preventivo validado
       ↓
Selección de opciones
       ↓
LLM adapta la explicación al usuario
```

El modelo generativo puede modificar **cómo se comunica la recomendación**, pero no debe inventar una intervención clínica.

---

# 16. Tres experiencias de usuario

Los tres tipos de usuario interactúan con el mismo motor, pero no necesitan la misma información.

## 16.1 Familia

Debe privilegiar:

- registro rápido;
    
- voz/chat;
    
- semáforo de riesgo;
    
- explicación sencilla;
    
- recomendaciones inmediatas;
    
- historial diario.
    

Ejemplo:

```text
RIESGO HOY
ALTO

¿Por qué?
• Sueño alterado durante 3 días.
• Despertar irritable.
• Cambio importante en su rutina.

Sugerencia:
Anticipar transiciones y disponer de un
espacio de baja estimulación.
```

---

## 16.2 Establecimiento educacional

Debe privilegiar:

- información funcional para el contexto escolar;
    
- cambios respecto del estado habitual;
    
- posibles desencadenantes;
    
- acciones preventivas aplicables en aula;
    
- registro rápido posterior.
    

No necesita acceso irrestricto a información clínica o familiar.

---

## 16.3 Profesional

Debe ofrecer mayor profundidad:

- evolución temporal;
    
- baseline;
    
- variables explicativas;
    
- nivel de confianza;
    
- registros incompletos;
    
- correlaciones;
    
- episodios anteriores;
    
- estrategias utilizadas;
    
- respuesta a intervenciones;
    
- comparación entre periodos.
    

---

# 17. Feedback y aprendizaje continuo

Después de cada ventana pronosticada, Bluba solicita feedback.

```text
Predicción realizada:
Riesgo alto durante próximas 24 h

¿Ocurrió una desregulación?
[ Sí ] [ No ]

¿Se utilizó alguna estrategia?
[ Seleccionar ]

Resultado:
[ Exitosa ]
[ Parcial ]
[ Sin efecto ]
```

Esta información crea un ciclo:

```text
OBSERVAR
   ↓
PREDECIR
   ↓
INTERVENIR
   ↓
REGISTRAR RESULTADO
   ↓
APRENDER
   └───────────────→
```

Este ciclo es esencial para que el sistema pueda mejorar longitudinalmente.

---

# 18. Manejo de datos incompletos

Bluba solicita explícitamente contemplar información faltante.

Nuestro principio es:

> **Ausencia de información no significa normalidad.**

Por ejemplo:

```text
salud_gastrointestinal = desconocido
```

no debe convertirse automáticamente en:

```text
salud_gastrointestinal = normal
```

Cuando sea necesario realizar imputación para un algoritmo, el sistema deberá conservar explícitamente un indicador de que el dato era faltante.

Además, cada ausencia relevante disminuye la confianza.

Cuando la información es insuficiente:

```text
No existe información suficiente
para generar una estimación confiable.

Complete al menos:
• Calidad del sueño
• Estado al despertar
• Nivel de regulación
```

---

# 19. Incentivo de registro inteligente

El sistema no debería pedir todas las variables todos los días.

Puede determinar cuál registro produciría mayor valor.

Ejemplo:

```text
Hoy ya contamos con:
✓ Sueño
✓ Despertar
✓ Registro profesional

Falta información relevante sobre:
○ Regulación después del colegio
```

De esta forma se reduce la carga del usuario.

En una etapa posterior puede utilizarse una estrategia de **active feature acquisition**, preguntando solamente por aquellas variables que puedan reducir significativamente la incertidumbre.

---

# 20. Arquitectura tecnológica propuesta

```text
┌───────────────────────────────────────────────┐
│                  CLIENTES                     │
│                                               │
│  Familia     Educación      Profesionales     │
└──────┬───────────┬──────────────┬─────────────┘
       │           │              │
       └───────────┼──────────────┘
                   ▼
┌───────────────────────────────────────────────┐
│       CAPA DE CAPTURA DE INFORMACIÓN          │
│                                               │
│ Formularios │ Chat │ Voz → texto              │
└────────────────────┬──────────────────────────┘
                     ▼
┌───────────────────────────────────────────────┐
│             NLP / ESTRUCTURACIÓN              │
│                                               │
│ lenguaje natural → variables Bluba           │
│ + confirmación humana                        │
└────────────────────┬──────────────────────────┘
                     ▼
┌───────────────────────────────────────────────┐
│              DATOS LONGITUDINALES             │
│                                               │
│ Diario │ Sesiones │ Crisis │ Perfil │ Contexto│
└────────────────────┬──────────────────────────┘
                     ▼
┌───────────────────────────────────────────────┐
│            FEATURE ENGINEERING                │
│                                               │
│ Baseline individual                           │
│ Ventanas temporales                           │
│ Acumuladores                                  │
│ Perfil sensorial                              │
│ Historial                                     │
│ Calidad de datos                              │
└────────────────────┬──────────────────────────┘
                     ▼
┌───────────────────────────────────────────────┐
│            MOTOR BLUBA ANTICIPA               │
│                                               │
│       Riesgo + Confianza + Explicación        │
└────────────────────┬──────────────────────────┘
                     ▼
┌───────────────────────────────────────────────┐
│       MOTOR DE APOYO A LA INTERVENCIÓN        │
│                                               │
│ Historial de estrategias                      │
│ Catálogo validado                             │
│ Contextualización por usuario                 │
└────────────────────┬──────────────────────────┘
                     ▼
              Alertas preventivas
```

---

# 21. Qué incluye el MVP de NeuroHack

El alcance debe mantenerse deliberadamente acotado.

## Funcionalidades prioritarias

### 1. Perfil longitudinal individual

Visualización del estado histórico y baseline.

### 2. Registro mediante voz/chat

Conversión de observaciones cotidianas en datos estructurados.

### 3. Motor de riesgo 24 h

Demostrado mediante información real disponible y escenarios sintéticos.

### 4. Acumuladores temporales

Por ejemplo:

```text
3 días sueño deteriorado
+
2 días regulación baja
+
cambio de rutina
```

### 5. Nivel de confianza

Calculado independientemente del riesgo.

### 6. Explicación

Mostrar los principales factores que contribuyen a la alerta.

### 7. Recomendación personalizada

Basada prioritariamente en estrategias registradas o previamente definidas.

### 8. Tres vistas

Familia, educación y profesional.

### 9. Feedback posterior

Registrar si ocurrió o no el evento y si la estrategia funcionó.

### 10. Manejo explícito de información faltante

No generar una recomendación aparentemente precisa cuando la información es insuficiente.

---

# 22. Qué NO forma parte del MVP

Para evitar aumentar innecesariamente la complejidad del proyecto, quedan fuera del prototipo principal:

- construcción de wearables;
    
- cámaras;
    
- reconocimiento facial;
    
- grabación continua de audio;
    
- GPS;
    
- EEG;
    
- sensores respiratorios;
    
- sensores ambientales físicos;
    
- diagnóstico médico;
    
- intervención automática;
    
- entrenamiento de un modelo clínicamente validado.
    

Esto no significa que algunas de estas tecnologías no tengan valor futuro.

Significa que **no son necesarias para resolver correctamente el desafío actual**.

---

# 23. Evolución futura: segunda capa fisiológica

Existe una línea científica prometedora basada en wearables.

Goodwin et al. mostraron en 2019 que señales fisiológicas y de movimiento contenían información predictiva antes de episodios de agresión.

Un estudio posterior de Imbiriba et al. analizó 70 jóvenes autistas hospitalizados y encontró que modelos de machine learning podían predecir conductas agresivas tres minutos antes con AUROC medio cercano a 0,80.

REACT, un piloto de 2024 con tres niños con discapacidades intelectuales y del desarrollo, combinó señales multimodales y obtuvo un F1 promedio de 68,2 % veinte segundos antes de la agitación. La escala reducida obliga a interpretar estos resultados como evidencia preliminar.

En 2026, Shen presentó una arquitectura personalizada que combina Empatica E4, IMU, baseline dinámico y Attention-LSTM ejecutado en ESP32-S3, reportando 92,5 % de verdaderos positivos y una anticipación promedio de 53,5 segundos para el grupo estudiado.

Sin embargo, una revisión sistemática publicada en 2026 identificó únicamente 13 estudios revisados por pares sobre predicción biométrica de problemas conductuales severos en neurodivergencia y señaló limitaciones metodológicas importantes en parte de la literatura.

Por tanto, los wearables deben presentarse como **proyección futura científicamente prometedora**, no como una tecnología clínicamente resuelta.

---

# 24. Dos escalas de anticipación

Esta distinción es estratégica para Bluba Anticipa.

## Capa contextual

### Horizonte: horas–24 horas

Utiliza:

- sueño;
    
- rutina;
    
- regulación;
    
- contexto;
    
- comportamiento;
    
- eventos recientes;
    
- historial.
    

Responde:

> “¿Estamos entrando en un día de mayor vulnerabilidad?”

---

## Capa fisiológica futura

### Horizonte: segundos–minutos

Podría utilizar:

- frecuencia cardíaca;
    
- HRV;
    
- EDA;
    
- temperatura;
    
- movimiento.
    

Responde:

> “¿Está ocurriendo una escalada fisiológica potencialmente relevante en este momento?”

---

La arquitectura futura sería:

```text
           BLUBA ANTICIPA

       Riesgo contextual
           6–24 h
              │
              │
Información ──┤
cotidiana     │
              ▼
        ┌────────────┐
        │ Motor de   │
        │ prevención │
        └─────┬──────┘
              ▲
              │
Wearable ─────┤
              │
       Riesgo fisiológico
         segundos/minutos
```

Ambas capas son complementarias, no competidoras.

---

# 25. Roadmap

## Fase 1 — NeuroHack / MVP

**Datos que ya posee Bluba**

- baseline individual;
    
- análisis temporal;
    
- riesgo;
    
- confianza;
    
- explicabilidad;
    
- asistente conversacional;
    
- recomendaciones;
    
- feedback;
    
- interfaces diferenciadas.
    

---

## Fase 2 — Piloto longitudinal

Recolectar datos reales durante varios meses.

Objetivos:

- validar variables predictivas;
    
- calibrar las probabilidades;
    
- medir sensibilidad y falsos positivos;
    
- estudiar diferencias individuales;
    
- mejorar recomendaciones;
    
- determinar ventanas temporales óptimas.
    

---

## Fase 3 — Contexto digital ampliado

Integrar opcionalmente:

- calendarios;
    
- cambios de rutina programados;
    
- actividad;
    
- sueño proveniente de smartwatches;
    
- contexto escolar estructurado.
    

---

## Fase 4 — Wearables

Incorporar:

- HR;
    
- HRV;
    
- EDA;
    
- temperatura;
    
- IMU.
    

Nuske et al. evaluaron dispositivos cardiovasculares comerciales en 32 niños autistas y encontraron datos suficientemente robustos en una proporción alta de participantes, mostrando la factibilidad de este tipo de medición, aunque también existen consideraciones de comodidad sensorial.

**Referencia:**  
Nuske, H. J. et al. (2022). _Evaluating commercially available wireless cardiovascular monitors for measuring and transmitting real-time physiological responses in children with autism_. Autism Research. DOI: 10.1002/aur.2633.

---

## Fase 5 — Sistema multimodal

Combinar:

```text
Contexto cotidiano
        +
Historial Bluba
        +
Wearable
        +
Sensores ambientales
        ↓
Modelo multimodal personalizado
```

Podría incorporar Edge AI para mantener las señales fisiológicas sensibles procesándose localmente.

---

# 26. Privacidad, ética y seguridad

Bluba Anticipa trabaja con información de menores, por lo que el diseño debe incorporar privacidad desde su arquitectura.

Principios:

- minimización de datos;
    
- consentimiento;
    
- control de acceso por roles;
    
- cifrado;
    
- trazabilidad;
    
- historial de modificaciones;
    
- separación de información clínica y educacional cuando corresponda;
    
- explicación de las alertas;
    
- supervisión humana.
    

## Audio

Las notas de voz deben utilizarse para registro voluntario.

No proponemos escucha continua.

Cuando sea técnicamente posible:

```text
Audio
 ↓
Transcripción
 ↓
Extracción
 ↓
Confirmación
 ↓
Eliminar audio
```

conservando solamente el registro estructurado necesario.

---

# 27. Posicionamiento clínico

El sistema debe describirse siempre como:

> **Herramienta de apoyo preventivo y toma de decisiones.**

No como:

> sistema diagnóstico.

Y tampoco como:

> predictor infalible de crisis.

Una alerta representa evidencia contextual para que una persona pueda intervenir preventivamente.

La decisión final permanece en:

- familia;
    
- educador;
    
- cuidador;
    
- profesional.
    

---

# 28. Cómo evaluar posteriormente el sistema

Cuando exista un dataset longitudinal suficientemente grande, no deberíamos evaluar el sistema solamente con accuracy.

Las métricas relevantes incluyen:

### Discriminación

- AUROC;
    
- PR-AUC;
    
- sensibilidad;
    
- especificidad.
    

### Seguridad

- falsos positivos;
    
- falsos negativos;
    
- alertas por usuario/semana.
    

### Calibración

Si el sistema afirma:

```text
Riesgo 70 %
```

los eventos etiquetados alrededor de ese nivel deberían ocurrir aproximadamente con una frecuencia compatible.

Se pueden utilizar:

- Brier Score;
    
- curvas de calibración.
    

### Utilidad práctica

- tiempo promedio de registro;
    
- porcentaje de registros completados;
    
- anticipación útil;
    
- utilidad percibida de recomendaciones;
    
- reducción de carga para familias;
    
- tasa de alertas ignoradas.
    

---

# 29. Principales elementos diferenciadores

Bluba Anticipa se diferencia por combinar en una única arquitectura:

### 1. Personalización longitudinal

Cada niño se compara principalmente consigo mismo.

### 2. Análisis de acumulación

No analiza registros como eventos aislados.

### 3. Inteligencia colectiva entre contextos

Familia, escuela y profesionales alimentan una misma visión longitudinal.

### 4. Registro de baja fricción

Voz y lenguaje natural reducen la carga administrativa.

### 5. Riesgo + confianza

El sistema diferencia entre la posibilidad estimada de un evento y la calidad de la evidencia disponible.

### 6. Explicabilidad

Cada alerta muestra qué factores contribuyeron al resultado.

### 7. Memoria de intervenciones

Aprende qué estrategias han funcionado previamente para ese niño.

### 8. Arquitectura escalable

Puede incorporar posteriormente wearables y sensores sin reemplazar el núcleo actual.

---

# 30. Qué respalda la ciencia y qué debemos validar

|Elemento|Estado|
|---|---|
|Relación sueño–problemas conductuales|Evidencia científica consistente|
|Relevancia del procesamiento sensorial|Evidencia científica|
|Síntomas gastrointestinales como contexto|Evidencia existente, pero heterogénea|
|Necesidad de personalización|Evidencia prometedora y coherente con heterogeneidad individual|
|Sensores fisiológicos para detección próxima|Evidencia experimental prometedora|
|Predicción fiable de crisis con 24 h usando exclusivamente datos Bluba|**Hipótesis a validar**|
|Ventana óptima de acumulación|**Hipótesis a validar**|
|Pesos de cada variable|**Deben aprenderse/validarse**|
|Eficacia clínica de las recomendaciones|**Debe validarse longitudinalmente**|

Esta diferenciación es importante para mantener rigor científico.

---

# 31. Mensaje central de la propuesta

Bluba actualmente permite registrar información valiosa sobre el día a día de cada niño.

**Bluba Anticipa busca convertir ese historial en capacidad preventiva.**

No intenta encontrar una fórmula universal de comportamiento.

Busca aprender:

> **qué significa un cambio para este niño, en este momento y dentro de su propio contexto.**

Su lógica puede resumirse como:

```text
¿CÓMO ESTÁ?
      +
¿CÓMO SUELE ESTAR?
      +
¿QUÉ HA CAMBIADO?
      +
¿QUÉ SE HA ACUMULADO?
      +
¿QUÉ OCURRIÓ EN SITUACIONES SIMILARES?
      ↓
RIESGO
      +
CONFIANZA
      +
EXPLICACIÓN
      +
ACCIÓN PREVENTIVA
```

---

# 32. Propuesta de valor en una frase

**Bluba Anticipa transforma los registros cotidianos de familias, escuelas y profesionales en un modelo longitudinal personalizado capaz de identificar acumulaciones de riesgo, explicar sus causas y proponer acciones preventivas antes de una posible desregulación.**

---

# 33. Pitch conceptual corto

> **Hoy Bluba registra lo que ocurrió. Con Bluba Anticipa queremos utilizar ese historial para ayudar a anticipar lo que podría ocurrir después.**
> 
> En lugar de comparar a todos los niños con una misma regla, construimos una línea base individual, detectamos cambios y acumulaciones durante los últimos días y combinamos información de familia, escuela y profesionales.
> 
> El resultado no es solamente un semáforo. Entregamos riesgo, nivel de confianza, explicación de los factores que lo provocan y estrategias que anteriormente han funcionado para ese niño.
> 
> Además, mediante voz o chat, convertimos observaciones cotidianas en información estructurada, reduciendo la carga de registro.
> 
> El MVP utiliza los datos que Bluba ya posee. A futuro, la misma arquitectura puede complementarse con wearables para añadir señales fisiológicas de corto plazo.

---

# 34. Referencias científicas principales

1. **Kim, H., et al. (2024).** Correlations between sleep problems, core symptoms, and behavioral problems in children and adolescents with autism spectrum disorder: a systematic review and meta-analysis. _European Child & Adolescent Psychiatry_. DOI: **10.1007/s00787-023-02253-1**.
    
2. **Griffin, Z. A. M., et al. (2022).** Atypical sensory processing features in children with autism, and their relationships with maladaptive behaviors and caregiver strain. _Autism Research, 15_(6), 1120–1129. DOI: **10.1002/aur.2700**.
    
3. **Leader, G., et al. (2022).** Gastrointestinal Symptoms in Autism Spectrum Disorder: A Systematic Review. _Nutrients, 14_(7), 1471. DOI: **10.3390/nu14071471**.
    
4. **Goodwin, M. S., Mazefsky, C. A., Ioannidis, S., Erdogmus, D., & Siegel, M. (2019).** Predicting aggression to others in youth with autism using a wearable biosensor. _Autism Research, 12_(8), 1286–1296. DOI: **10.1002/aur.2151**.
    
5. **Imbiriba, T., et al. (2023).** Wearable Biosensing to Predict Imminent Aggressive Behavior in Psychiatric Inpatient Youths With Autism. _JAMA Network Open, 6_(12), e2348898. DOI: **10.1001/jamanetworkopen.2023.48898**.
    
6. **Khan, N., et al. (2024).** Pilot study of a real-time early agitation capture technology (REACT) for children with intellectual and developmental disabilities. _Digital Health, 10_. DOI: **10.1177/20552076241287884**.
    
7. **Nuske, H. J., et al. (2022).** Evaluating commercially available wireless cardiovascular monitors for measuring and transmitting real-time physiological responses in children with autism. _Autism Research_. DOI: **10.1002/aur.2633**.
    
8. **Romani, P. W., D’Mello, S. K., Moulder, R. M., et al. (2026).** Using Wearable Technology to Predict the Occurrence of Severe Behavior Problems among Neurodiverse Individuals: A Systematic Review. _Perspectives on Behavior Science, 49_, 361–383. DOI: **10.1007/s40614-026-00497-1**.
    
9. **Shen, M. (2026).** Autistic Children's Emotional Behavior Warning System Based on Smart Wearable Devices. _Engineering Reports, 8_(3), e70645. DOI: **10.1002/eng2.70645**.
    

---

# 35. Definición oficial del proyecto

A partir de esta propuesta, la definición que debe mantenerse consistente en documentos, presentaciones y demos es:

**Bluba Anticipa es un sistema inteligente de apoyo preventivo integrado a Bluba que analiza longitudinalmente los registros cotidianos de cada niño neurodivergente, construye una línea base individual y detecta desviaciones, acumulaciones e interacciones entre factores asociados a una posible desregulación durante las próximas horas o el día siguiente. El sistema entrega un nivel de riesgo y un nivel independiente de confianza, explica los factores relevantes y propone estrategias preventivas basadas prioritariamente en el historial individual y recomendaciones profesionales. Para reducir la carga de registro, permite convertir notas escritas o de voz de familias y equipos educativos en variables estructuradas mediante IA con confirmación humana. El MVP utiliza la información que Bluba ya posee, mientras que su arquitectura permite incorporar en etapas posteriores señales fisiológicas provenientes de wearables y datos ambientales para complementar la anticipación contextual con detección de escaladas de corto plazo.**