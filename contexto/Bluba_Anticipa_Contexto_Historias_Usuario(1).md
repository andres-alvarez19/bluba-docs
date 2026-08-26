# Bluba Anticipa — Especificación de Producto, Historias de Usuario y Requerimientos del Sistema

**Versión:** MVP NeuroHack 2026  
**Plataforma objetivo:** Aplicación móvil (iOS / Android)  
**Propósito del documento:** establecer la especificación funcional de referencia para el MVP de Bluba Anticipa. El documento consolida contexto de producto, actores, historias de usuario, requerimientos funcionales y no funcionales, decisiones de producto/ingeniería, el corte recomendado del MVP y una matriz de implementación HU → RF → pantalla/API → criterio de aceptación, manteniendo trazabilidad entre necesidades de usuario e implementación.

---

## 1. Contexto del proyecto

**Bluba Anticipa** es un sistema inteligente de apoyo preventivo integrado a Bluba que analiza longitudinalmente los registros cotidianos de cada niño neurodivergente para identificar señales asociadas a una posible desregulación durante las próximas horas o el día siguiente.

La propuesta parte de una idea central: **cada niño debe compararse principalmente consigo mismo, no con reglas universales aplicadas a toda la población**. Por ello, Bluba Anticipa construye una línea base individual dinámica y analiza:

- el estado actual del niño;
- las desviaciones respecto de su patrón habitual;
- la acumulación de factores durante varias horas o días;
- la interacción entre diferentes señales de contexto;
- los antecedentes de desregulación;
- las estrategias que anteriormente han resultado útiles para ese niño.

El sistema transforma estos datos en información preventiva comprensible y accionable para tres contextos principales:

1. **Familia / cuidador**: necesita registrar información con baja fricción y entender qué puede hacer hoy.
2. **Profesor / educador**: necesita información breve, contextual y operativa para actuar en el aula sin interrumpir su trabajo.
3. **Especialista / terapeuta / psicólogo**: necesita profundidad analítica, trazabilidad y contexto longitudinal para interpretar tendencias y ajustar estrategias.

Bluba Anticipa **no diagnostica**, **no reemplaza el juicio profesional** y **no garantiza que una desregulación ocurrirá**. La interfaz debe comunicar el resultado como una estimación de apoyo a la toma de decisiones.

---

## 2. Problema de experiencia de usuario

Bluba puede disponer de registros cotidianos provenientes de familia, escuela y profesionales, por ejemplo:

- horas y calidad del sueño;
- estado basal al despertar;
- nivel de apoyo requerido para iniciar el día;
- salud gastrointestinal;
- alimentación;
- cambios de rutina;
- comportamiento observado;
- estado de alerta;
- regulaciones y desregulaciones;
- participación escolar o terapéutica;
- interacciones sociales;
- recreos;
- eventos excepcionales;
- antecedentes históricos de crisis;
- estrategias aplicadas y sus resultados.

El problema no es solamente capturar esta información. El desafío es que las señales pueden estar **distribuidas entre distintos días, personas y contextos**, lo que dificulta reconocer una acumulación temprana.

La experiencia de Bluba Anticipa debe resolver simultáneamente cinco fricciones:

1. **Registrar sin sobrecargar al usuario.**
2. **Convertir registros dispersos en una visión longitudinal.**
3. **Comunicar riesgo sin generar falsa certeza.**
4. **Explicar por qué el sistema está alertando.**
5. **Traducir la alerta en acciones preventivas apropiadas para cada rol.**

---

## 3. Objetivo de la experiencia móvil

La aplicación debe permitir que un usuario comprenda, en pocos segundos:

> **¿Cómo está el niño hoy, qué cambió respecto de su patrón habitual, qué tan preocupante es ese cambio, qué tan confiable es la estimación y qué acción preventiva corresponde a mi rol?**

La interfaz debe priorizar claridad, velocidad y baja carga cognitiva. La misma información no debe mostrarse de igual forma a todos los roles.

---

## 4. Principios de producto que condicionan el diseño

### 4.1 Personalización longitudinal

La interfaz debe privilegiar comparaciones del tipo **“respecto de su estado habitual”** sobre mensajes basados exclusivamente en umbrales generales.

### 4.2 Riesgo y confianza son diferentes

La UI debe tratar como conceptos separados:

- **Riesgo:** qué tan compatible es el patrón actual con situaciones asociadas a desregulación.
- **Confianza:** qué tan completa y sólida es la información disponible para estimar ese riesgo.

Un riesgo alto puede coexistir con una confianza media o baja.

### 4.3 Explicabilidad obligatoria

Nunca debe mostrarse únicamente un porcentaje o un color. Toda alerta relevante debe permitir entender sus factores principales.

### 4.4 Datos faltantes explícitos

La ausencia de información **no equivale a normalidad**. El sistema debe poder comunicar qué información falta y cuándo esa ausencia reduce la confianza de la predicción.

### 4.5 Registro de baja fricción

El usuario debe poder registrar información mediante:

- selección rápida;
- texto libre;
- nota de voz;
- confirmación de datos estructurados interpretados por IA.

La IA conversacional se utiliza para **estructurar el registro**, no para decidir directamente el riesgo.

### 4.6 Confirmación humana

Cuando una observación de texto o voz se transforme en variables estructuradas, el usuario debe poder **confirmar o corregir** la interpretación antes de guardarla.

### 4.7 Recomendaciones seguras

Las recomendaciones preventivas deben provenir prioritariamente de:

- estrategias aprobadas por profesionales;
- estrategias utilizadas previamente;
- estrategias con resultados registrados;
- un catálogo preventivo validado.

El sistema puede adaptar la redacción al usuario, pero no debe inventar intervenciones clínicas.

### 4.8 Información por rol

Cada usuario debe ver únicamente la profundidad y el tipo de información necesario para su contexto.

### 4.9 Diseño móvil y uso en contexto real

Bluba Anticipa debe diseñarse como aplicación móvil. Especialmente en el entorno escolar, ciertas acciones deben poder realizarse en segundos y con mínima interacción.

---

## 5. Actores del MVP

### 5.1 Familia / cuidador

**Contexto de uso:** hogar, comienzo y cierre del día, momentos de rutina o después de recibir información desde el colegio.

**Objetivos principales:**

- registrar el estado cotidiano sin llenar formularios extensos;
- saber si el día presenta señales distintas de lo habitual;
- comprender una alerta en lenguaje sencillo;
- saber qué puede hacer preventivamente;
- informar si ocurrió o no una desregulación;
- registrar si una estrategia ayudó.

**Principales frustraciones a evitar:**

- cuestionarios largos;
- lenguaje clínico innecesario;
- alertas alarmistas;
- porcentajes sin explicación;
- recomendaciones genéricas sin contexto;
- repetir datos ya registrados por otro actor.

---

### 5.2 Profesor / educador

**Contexto de uso:** aula, recreo, ingreso al establecimiento, transiciones, momentos de alta demanda atencional.

**Objetivos principales:**

- identificar rápidamente qué alumnos requieren mayor atención preventiva;
- conocer señales funcionales relevantes para la jornada escolar;
- registrar una observación sin abandonar el manejo del curso;
- reportar una desregulación o escalada con mínima fricción;
- acceder a estrategias aplicables en contexto educativo;
- contribuir al historial sin acceder a información privada innecesaria.

**Principales frustraciones a evitar:**

- interfaces densas;
- formularios durante una situación crítica;
- exceso de información clínica;
- navegación profunda;
- alertas ambiguas;
- no saber si un registro fue enviado o sincronizado.

---

### 5.3 Especialista / terapeuta / psicólogo

**Contexto de uso:** revisión de casos, sesión clínica o terapéutica, seguimiento longitudinal, preparación de recomendaciones.

**Objetivos principales:**

- analizar evolución y desviaciones respecto de la línea base individual;
- entender qué variables contribuyen al riesgo;
- conocer la calidad y completitud de los datos;
- revisar episodios anteriores y estrategias utilizadas;
- registrar observaciones profesionales;
- definir o validar estrategias preventivas;
- evaluar resultados de intervenciones y patrones longitudinales.

**Principales frustraciones a evitar:**

- datos agregados sin trazabilidad;
- no distinguir predicción de evidencia observada;
- porcentajes de riesgo sin contexto;
- inconsistencias entre fuentes ocultas;
- exceso de simplificación;
- imposibilidad de revisar el origen temporal de una alerta.

---

## 6. Entidad central de la experiencia: el niño

En el MVP, el niño es el **sujeto central del sistema**, pero no se considera un usuario directo de la aplicación.

Toda experiencia debe organizar la información alrededor de un perfil individual que contenga, según permisos:

- estado actual;
- línea base;
- evolución reciente;
- alertas;
- datos faltantes;
- factores principales;
- registros recientes;
- desregulaciones anteriores;
- intervenciones y resultados;
- recomendaciones preventivas vigentes.

---

## 7. Escenarios de uso que las interfaces deben soportar

1. **Día estable:** el niño se mantiene cercano a su patrón habitual y la app evita generar alarma innecesaria.
2. **Deterioro gradual:** varias señales se desvían progresivamente durante 2–3 días.
3. **Cambio de rutina:** aparece un evento excepcional relevante para el perfil individual.
4. **Sueño alterado:** el deterioro se acumula durante varias noches.
5. **Acumulación multifactorial:** sueño, irritabilidad, rutina y contexto adverso coinciden.
6. **Riesgo alto con datos incompletos:** existen señales preocupantes, pero falta información crítica.
7. **Datos insuficientes:** el sistema no puede generar una estimación confiable y solicita el mínimo registro adicional necesario.
8. **Desregulación en curso:** el profesor necesita registrar el evento de forma inmediata.
9. **Prevención efectiva:** se aplicó una estrategia y no ocurrió la desregulación.
10. **Intervención sin efecto:** se aplicó una estrategia, pero el episodio igualmente ocurrió.
11. **Información desde múltiples contextos:** familia, escuela y especialista registran observaciones durante el mismo día.
12. **Conectividad limitada:** el usuario necesita registrar información aunque temporalmente no exista conexión.

---

# 8. Historias de usuario

Las historias están priorizadas para facilitar la definición posterior del MVP:

- **P0:** necesaria para demostrar la propuesta central.
- **P1:** importante para una experiencia coherente y usable.
- **P2:** deseable o extensible después del núcleo del MVP.

---

## 8.1 Historias transversales

### HU-TR-01 — Acceso según rol
**Prioridad:** P0  
**Como** usuario de Bluba Anticipa,  
**quiero** ingresar con mi rol y permisos correspondientes,  
**para** acceder solamente a las funciones y datos necesarios para mi contexto.

**Necesidad de diseño:** la experiencia posterior al acceso debe ser distinta para familia, educación y especialista.

### HU-TR-02 — Selección clara del niño
**Prioridad:** P0  
**Como** usuario autorizado para más de un niño,  
**quiero** identificar y cambiar claramente el perfil activo,  
**para** evitar registrar o consultar información en el niño equivocado.

### HU-TR-03 — Estado diario resumido
**Prioridad:** P0  
**Como** usuario autorizado,  
**quiero** ver un resumen del estado preventivo del niño,  
**para** comprender rápidamente si existen señales que requieren atención hoy.

### HU-TR-04 — Diferenciar riesgo y confianza
**Prioridad:** P0  
**Como** usuario,  
**quiero** ver por separado el nivel de riesgo y la confianza de la estimación,  
**para** no interpretar una predicción como una certeza.

### HU-TR-05 — Comprender los factores de la alerta
**Prioridad:** P0  
**Como** usuario,  
**quiero** conocer los principales factores que explican una alerta,  
**para** entender qué cambios están influyendo en la estimación.

### HU-TR-06 — Saber qué información falta
**Prioridad:** P0  
**Como** usuario,  
**quiero** saber cuándo faltan datos relevantes y cuáles son,  
**para** comprender por qué la confianza es menor y completar información útil cuando sea posible.

### HU-TR-07 — No recibir falsa precisión
**Prioridad:** P0  
**Como** usuario,  
**quiero** que el sistema me indique cuando no existe información suficiente para una estimación confiable,  
**para** no tomar decisiones basadas en una predicción artificialmente precisa.

### HU-TR-08 — Línea temporal compartida
**Prioridad:** P1  
**Como** usuario autorizado,  
**quiero** consultar los registros relevantes en orden temporal,  
**para** comprender cómo evolucionó el estado del niño y qué actores aportaron información.

### HU-TR-09 — Origen del registro
**Prioridad:** P1  
**Como** usuario autorizado,  
**quiero** distinguir si un registro provino de familia, escuela o profesional,  
**para** interpretar correctamente su contexto.

### HU-TR-10 — Estado de sincronización
**Prioridad:** P1  
**Como** usuario móvil,  
**quiero** saber si un registro está guardado localmente, sincronizándose o ya fue enviado,  
**para** confiar en que la información no se perdió.

### HU-TR-11 — Registro sin conexión
**Prioridad:** P1  
**Como** usuario móvil,  
**quiero** poder realizar registros críticos aun sin conexión,  
**para** no perder información por problemas de conectividad.

### HU-TR-12 — Notificaciones contextualizadas
**Prioridad:** P1  
**Como** usuario,  
**quiero** recibir notificaciones solamente cuando exista información relevante para mi rol,  
**para** actuar oportunamente sin sufrir fatiga de alertas.

---

# 9. Historias de usuario — Familia / Cuidador

## Épica F1 — Registro cotidiano de baja fricción

### HU-FAM-01 — Check-in diario breve
**Prioridad:** P0  
**Como** familiar o cuidador,  
**quiero** registrar en pocos pasos cómo durmió y cómo despertó mi hijo,  
**para** mantener actualizado su estado diario sin completar formularios extensos.

### HU-FAM-02 — Registro mediante lenguaje natural
**Prioridad:** P0  
**Como** familiar,  
**quiero** escribir una observación en mis propias palabras,  
**para** registrar información cotidiana sin tener que conocer las categorías internas de Bluba.

### HU-FAM-03 — Registro mediante nota de voz
**Prioridad:** P0  
**Como** familiar,  
**quiero** dictar una observación breve,  
**para** registrar información cuando escribir resulte incómodo o lento.

### HU-FAM-04 — Confirmar interpretación de la IA
**Prioridad:** P0  
**Como** familiar,  
**quiero** revisar las variables que el sistema extrajo de mi texto o voz antes de guardarlas,  
**para** asegurar que mi observación fue interpretada correctamente.

### HU-FAM-05 — Corregir una interpretación
**Prioridad:** P0  
**Como** familiar,  
**quiero** editar una variable interpretada incorrectamente,  
**para** mantener la calidad del registro sin repetir toda la observación.

### HU-FAM-06 — Completar solamente información útil
**Prioridad:** P1  
**Como** familiar,  
**quiero** que el sistema me pregunte prioritariamente por la información faltante que más aporta a la estimación,  
**para** reducir la carga de registro diario.

## Épica F2 — Comprensión del estado preventivo

### HU-FAM-07 — Ver el riesgo del día en lenguaje simple
**Prioridad:** P0  
**Como** familiar,  
**quiero** ver una interpretación sencilla del nivel de riesgo para las próximas horas,  
**para** saber si debo prestar atención adicional hoy.

### HU-FAM-08 — Entender qué cambió respecto de lo habitual
**Prioridad:** P0  
**Como** familiar,  
**quiero** saber qué señales están diferentes respecto del patrón habitual de mi hijo,  
**para** comprender el motivo de la alerta en un contexto personal y no genérico.

### HU-FAM-09 — Comprender acumulaciones
**Prioridad:** P0  
**Como** familiar,  
**quiero** que el sistema me muestre cuando un factor se ha repetido durante varios días,  
**para** reconocer deterioros graduales que podrían pasar desapercibidos.

### HU-FAM-10 — Ver confianza y datos faltantes
**Prioridad:** P0  
**Como** familiar,  
**quiero** saber qué tan confiable es la estimación y qué información falta,  
**para** interpretar la alerta con la cautela adecuada.

## Épica F3 — Apoyo preventivo

### HU-FAM-11 — Recibir acciones preventivas concretas
**Prioridad:** P0  
**Como** familiar,  
**quiero** recibir pocas acciones concretas para aplicar hoy,  
**para** saber cómo apoyar al niño sin tener que interpretar datos técnicos.

### HU-FAM-12 — Reconocer recomendaciones personalizadas
**Prioridad:** P0  
**Como** familiar,  
**quiero** saber cuando una recomendación proviene de una estrategia que anteriormente funcionó para mi hijo o fue indicada por un profesional,  
**para** confiar mejor en su pertinencia.

### HU-FAM-13 — Consultar cómo aplicar una estrategia
**Prioridad:** P1  
**Como** familiar,  
**quiero** abrir una recomendación y ver instrucciones breves y comprensibles,  
**para** aplicarla correctamente en casa.

## Épica F4 — Feedback y aprendizaje

### HU-FAM-14 — Confirmar si ocurrió una desregulación
**Prioridad:** P0  
**Como** familiar,  
**quiero** indicar después del periodo estimado si ocurrió o no una desregulación,  
**para** contribuir al aprendizaje longitudinal del sistema.

### HU-FAM-15 — Registrar una estrategia aplicada
**Prioridad:** P0  
**Como** familiar,  
**quiero** registrar qué estrategia utilicé,  
**para** relacionar la intervención con el resultado posterior.

### HU-FAM-16 — Registrar el resultado de una estrategia
**Prioridad:** P0  
**Como** familiar,  
**quiero** indicar si una estrategia fue exitosa, parcialmente útil o no tuvo efecto,  
**para** construir una memoria de intervenciones personalizada.

### HU-FAM-17 — Consultar historial reciente
**Prioridad:** P1  
**Como** familiar,  
**quiero** revisar los últimos días de forma sencilla,  
**para** reconocer cambios y compartir información relevante con el equipo de apoyo.

---

# 10. Historias de usuario — Profesor / Educador

## Épica E1 — Inicio de jornada y priorización

### HU-EDU-01 — Resumen matutino del aula
**Prioridad:** P0  
**Como** profesor,  
**quiero** ver al iniciar la jornada qué estudiantes presentan señales preventivas relevantes,  
**para** anticipar apoyos antes de comenzar las actividades.

### HU-EDU-02 — Priorizar alumnos que requieren atención
**Prioridad:** P0  
**Como** profesor,  
**quiero** identificar visualmente qué estudiantes requieren mayor atención preventiva,  
**para** distribuir mejor mi atención durante la jornada.

### HU-EDU-03 — Ver información funcional y no clínica
**Prioridad:** P0  
**Como** profesor,  
**quiero** recibir únicamente la información necesaria para actuar en contexto educativo,  
**para** apoyar al estudiante sin acceder a antecedentes privados que no necesito conocer.

### HU-EDU-04 — Ver señales relevantes para el aula
**Prioridad:** P0  
**Como** profesor,  
**quiero** conocer los principales cambios funcionales del estudiante,  
**para** adaptar transiciones, exigencia, ambiente y apoyos durante la jornada.

### HU-EDU-05 — Ver recomendaciones aplicables en escuela
**Prioridad:** P0  
**Como** profesor,  
**quiero** ver un número reducido de estrategias preventivas apropiadas para el contexto escolar,  
**para** actuar sin tener que interpretar recomendaciones clínicas extensas.

## Épica E2 — Registro rápido en contexto escolar

### HU-EDU-06 — Reporte express de desregulación
**Prioridad:** P0  
**Como** profesor en el aula,  
**quiero** registrar una desregulación o escalada con una interacción mínima,  
**para** informar al equipo de apoyo sin abandonar la supervisión del curso.

### HU-EDU-07 — Reportar sin formulario obligatorio
**Prioridad:** P0  
**Como** profesor durante una situación crítica,  
**quiero** poder registrar el evento inmediatamente sin completar detalles adicionales,  
**para** priorizar la atención del estudiante y completar información después si corresponde.

### HU-EDU-08 — Confirmación inmediata del reporte
**Prioridad:** P0  
**Como** profesor,  
**quiero** recibir una confirmación visual clara después de reportar un evento,  
**para** saber que la información fue registrada o quedó pendiente de sincronización.

### HU-EDU-09 — Nota rápida por voz
**Prioridad:** P0  
**Como** profesor,  
**quiero** dictar una observación breve durante una pausa o recreo,  
**para** aportar contexto sin completar formularios extensos.

### HU-EDU-10 — Confirmar observación estructurada
**Prioridad:** P0  
**Como** profesor,  
**quiero** revisar rápidamente cómo el sistema interpretó mi nota,  
**para** evitar que una observación escolar sea almacenada de forma incorrecta.

### HU-EDU-11 — Completar detalles posteriormente
**Prioridad:** P1  
**Como** profesor,  
**quiero** poder complementar un reporte express cuando tenga tiempo,  
**para** enriquecer el historial sin aumentar la fricción durante el evento.

## Épica E3 — Seguimiento dentro de la jornada

### HU-EDU-12 — Registrar cambios respecto del inicio del día
**Prioridad:** P1  
**Como** profesor,  
**quiero** registrar rápidamente si el estudiante mejoró, empeoró o se mantuvo estable,  
**para** aportar señales temporales durante la jornada.

### HU-EDU-13 — Registrar desencadenantes observados
**Prioridad:** P1  
**Como** profesor,  
**quiero** señalar posibles factores contextuales observados,  
**para** ayudar a relacionar eventos escolares con el estado del estudiante.

### HU-EDU-14 — Registrar estrategia aplicada y resultado
**Prioridad:** P1  
**Como** profesor,  
**quiero** registrar qué estrategia utilicé y si ayudó,  
**para** contribuir a identificar apoyos útiles en contexto educativo.

### HU-EDU-15 — Consultar el estado de una alerta durante el día
**Prioridad:** P1  
**Como** profesor,  
**quiero** volver a consultar rápidamente el estado preventivo de un estudiante,  
**para** adaptar el manejo de transiciones o actividades posteriores.

---

# 11. Historias de usuario — Especialista / Terapeuta / Psicólogo

## Épica S1 — Priorización y revisión de casos

### HU-ESP-01 — Lista de pacientes con estado reciente
**Prioridad:** P0  
**Como** especialista,  
**quiero** ver mis pacientes con un resumen de su estado preventivo reciente,  
**para** identificar rápidamente qué casos requieren revisión.

### HU-ESP-02 — Priorizar por cambio respecto del baseline
**Prioridad:** P0  
**Como** especialista,  
**quiero** identificar qué pacientes presentan las mayores desviaciones respecto de su patrón habitual,  
**para** priorizar cambios clínicamente relevantes y no solo valores absolutos.

## Épica S2 — Línea base y evolución longitudinal

### HU-ESP-03 — Visualizar línea base individual
**Prioridad:** P0  
**Como** especialista,  
**quiero** visualizar el patrón habitual del paciente,  
**para** interpretar el estado actual dentro de su contexto individual.

### HU-ESP-04 — Comparar estado actual con baseline
**Prioridad:** P0  
**Como** especialista,  
**quiero** comparar gráficamente el periodo reciente con la línea base,  
**para** detectar desviaciones y tendencias relevantes.

### HU-ESP-05 — Revisar acumulaciones temporales
**Prioridad:** P0  
**Como** especialista,  
**quiero** ver qué factores adversos se han acumulado durante los últimos días,  
**para** comprender la evolución que llevó a una alerta.

### HU-ESP-06 — Cambiar ventana temporal
**Prioridad:** P1  
**Como** especialista,  
**quiero** revisar diferentes periodos de tiempo,  
**para** comparar el estado inmediato, los últimos días y el historial más amplio.

### HU-ESP-07 — Explorar episodios anteriores
**Prioridad:** P1  
**Como** especialista,  
**quiero** revisar desregulaciones pasadas junto con las señales que las precedieron,  
**para** identificar patrones recurrentes del paciente.

## Épica S3 — Explicabilidad y calidad de datos

### HU-ESP-08 — Ver factores contribuyentes
**Prioridad:** P0  
**Como** especialista,  
**quiero** conocer las variables y tendencias que más influyeron en una estimación,  
**para** evaluar si la explicación resulta coherente con el historial del paciente.

### HU-ESP-09 — Revisar nivel de confianza
**Prioridad:** P0  
**Como** especialista,  
**quiero** conocer la confianza de la predicción de forma independiente al riesgo,  
**para** valorar la solidez de la evidencia disponible.

### HU-ESP-10 — Ver datos faltantes relevantes
**Prioridad:** P0  
**Como** especialista,  
**quiero** saber qué variables relevantes están ausentes o desactualizadas,  
**para** interpretar correctamente la incertidumbre.

### HU-ESP-11 — Identificar contradicciones entre fuentes
**Prioridad:** P1  
**Como** especialista,  
**quiero** identificar cuándo existen registros potencialmente contradictorios entre familia, escuela y sesiones profesionales,  
**para** revisar el contexto antes de sacar conclusiones.

### HU-ESP-12 — Trazar el origen de una señal
**Prioridad:** P1  
**Como** especialista,  
**quiero** acceder al registro original que sustenta una señal resumida,  
**para** comprobar su contexto y mantener trazabilidad.

## Épica S4 — Intervenciones y recomendaciones

### HU-ESP-13 — Consultar estrategias históricas
**Prioridad:** P0  
**Como** especialista,  
**quiero** ver qué estrategias fueron aplicadas en situaciones similares,  
**para** evaluar cuáles podrían seguir siendo apropiadas.

### HU-ESP-14 — Ver resultado de intervenciones anteriores
**Prioridad:** P0  
**Como** especialista,  
**quiero** conocer el resultado registrado de cada estrategia,  
**para** distinguir apoyos históricamente útiles de los que no mostraron efecto.

### HU-ESP-15 — Definir o validar estrategias preventivas
**Prioridad:** P1  
**Como** especialista,  
**quiero** registrar estrategias preventivas válidas para el paciente,  
**para** que familia y escuela reciban recomendaciones seguras y contextualizadas.

### HU-ESP-16 — Diferenciar recomendaciones por contexto
**Prioridad:** P1  
**Como** especialista,  
**quiero** definir si una estrategia es adecuada para hogar, escuela o ambos,  
**para** evitar recomendaciones inaplicables al entorno del usuario.

## Épica S5 — Registro profesional

### HU-ESP-17 — Registrar observación de sesión
**Prioridad:** P0  
**Como** especialista,  
**quiero** registrar una observación profesional vinculada a una sesión,  
**para** incorporar evidencia clínica o terapéutica al historial longitudinal.

### HU-ESP-18 — Estructurar una nota profesional desde texto
**Prioridad:** P1  
**Como** especialista,  
**quiero** convertir una nota libre en variables estructuradas revisables,  
**para** reducir trabajo administrativo sin perder control sobre el contenido guardado.

### HU-ESP-19 — Registrar cambios del plan de apoyo
**Prioridad:** P1  
**Como** especialista,  
**quiero** registrar cambios relevantes en estrategias o apoyos,  
**para** que futuras recomendaciones se basen en información vigente.

---

# 12. Historias de usuario — Coordinación entre contextos

Estas historias no corresponden exclusivamente a un rol, pero son importantes para que Bluba Anticipa funcione como sistema longitudinal compartido.

### HU-COL-01 — Evitar duplicar registros
**Prioridad:** P1  
**Como** usuario,  
**quiero** saber qué información relevante ya fue registrada hoy por otro actor,  
**para** no completar datos repetidos innecesariamente.

### HU-COL-02 — Solicitud inteligente de información
**Prioridad:** P1  
**Como** usuario,  
**quiero** que el sistema solicite solamente información faltante que aporte valor,  
**para** mantener suficiente calidad de datos con la menor carga posible.

### HU-COL-03 — Compartir una alerta entre contextos autorizados
**Prioridad:** P1  
**Como** integrante autorizado del equipo de apoyo,  
**quiero** que una alerta relevante pueda reflejarse en los otros contextos correspondientes,  
**para** coordinar acciones preventivas entre hogar, escuela y profesionales.

### HU-COL-04 — Mantener privacidad por contexto
**Prioridad:** P0  
**Como** responsable del niño,  
**quiero** que cada actor acceda solamente a la información necesaria para su función,  
**para** proteger datos sensibles sin impedir la coordinación preventiva.

### HU-COL-05 — Mantener historial de cambios
**Prioridad:** P1  
**Como** usuario autorizado,  
**quiero** que las modificaciones relevantes de un registro queden trazables,  
**para** preservar la confiabilidad del historial.

---

# 13. Historias de seguridad de comunicación

Estas historias son especialmente importantes porque la aplicación comunica estimaciones relacionadas con salud y comportamiento de menores.

### HU-SEG-01 — Evitar lenguaje diagnóstico
**Prioridad:** P0  
**Como** usuario,  
**quiero** que las alertas se expresen como estimaciones preventivas y no como diagnósticos,  
**para** interpretar correctamente el propósito del sistema.

### HU-SEG-02 — Diferenciar dato observado de inferencia
**Prioridad:** P0  
**Como** usuario,  
**quiero** distinguir qué información fue registrada por una persona y qué información fue inferida o calculada por el sistema,  
**para** entender el origen de cada conclusión.

### HU-SEG-03 — Explicar una confianza baja
**Prioridad:** P0  
**Como** usuario,  
**quiero** saber por qué la confianza de una estimación es baja,  
**para** decidir si corresponde completar información antes de actuar sobre la alerta.

### HU-SEG-04 — Evitar recomendaciones no validadas
**Prioridad:** P0  
**Como** usuario,  
**quiero** recibir solamente recomendaciones provenientes de fuentes aprobadas o del historial individual,  
**para** reducir el riesgo de que la aplicación sugiera intervenciones improvisadas.

---

# 14. Mapa de necesidades por rol

| Necesidad | Familia | Educación | Especialista |
|---|---:|---:|---:|
| Registro diario rápido | Alta | Media | Media |
| Voz / texto libre | Alta | Alta | Media |
| Reporte de evento inmediato | Media | Muy alta | Baja |
| Vista de riesgo diario | Alta | Alta | Alta |
| Explicación simple | Muy alta | Muy alta | Media |
| Explicación analítica | Baja | Baja | Muy alta |
| Línea base individual | Media | Baja | Muy alta |
| Confianza / calidad de datos | Alta | Media | Muy alta |
| Recomendaciones preventivas | Muy alta | Muy alta | Alta |
| Historial de intervenciones | Media | Media | Muy alta |
| Registro de resultado | Alta | Alta | Alta |
| Acceso a información clínica detallada | Baja | No necesaria | Muy alta |
| Operación en pocos segundos | Alta | Crítica | Media |

---

# 15. Flujos principales que deberán diseñarse

A partir de las historias anteriores, el diseño UI/UX deberá contemplar al menos estos flujos.

## Flujo A — Familia: registro matutino

```text
Inicio
  ↓
Estado de hoy
  ↓
Check-in breve / voz / texto
  ↓
Interpretación estructurada
  ↓
Confirmar o editar
  ↓
Registro guardado
  ↓
Actualización de riesgo y confianza
```

## Flujo B — Familia: alerta preventiva

```text
Alerta / inicio
  ↓
Nivel de riesgo
  ↓
Nivel de confianza
  ↓
¿Por qué?
  ↓
¿Qué puedo hacer hoy?
  ↓
Aplicar estrategia
  ↓
Registrar resultado posteriormente
```

## Flujo C — Educación: inicio de jornada

```text
Vista aula
  ↓
Estudiantes priorizados
  ↓
Abrir estudiante
  ↓
Señales relevantes
  ↓
2–3 acciones preventivas escolares
```

## Flujo D — Educación: desregulación en curso

```text
Botón de reporte express
  ↓
Seleccionar / confirmar estudiante
  ↓
Reporte registrado
  ↓
Confirmación inmediata
  ↓
Detalles opcionales después
```

## Flujo E — Especialista: revisión longitudinal

```text
Listado de pacientes
  ↓
Seleccionar paciente
  ↓
Resumen actual
  ↓
Baseline vs. periodo reciente
  ↓
Factores + acumulaciones
  ↓
Confianza + datos faltantes
  ↓
Episodios previos
  ↓
Estrategias y resultados
```

## Flujo F — Cierre de predicción / feedback

```text
Ventana de predicción finalizada
  ↓
¿Ocurrió desregulación?
  ↓
¿Se aplicó estrategia?
  ↓
¿Cuál?
  ↓
Resultado
  ↓
Actualizar historial de intervención
```

---

# 16. Pantallas candidatas derivadas de las historias

Esta lista no constituye todavía una especificación de pantallas definitiva; sirve como puente entre historias de usuario y diseño.

## Comunes

- Acceso / autenticación.
- Selector de niño o paciente.
- Centro de notificaciones.
- Estado de sincronización.
- Perfil y permisos.

## Familia

- Inicio / estado de hoy.
- Check-in diario.
- Asistente de voz / chat.
- Confirmación de variables interpretadas.
- Detalle de alerta: riesgo + confianza + factores.
- Recomendaciones preventivas.
- Historial reciente.
- Feedback posterior.

## Educación

- Vista aula / resumen matutino.
- Detalle funcional de estudiante.
- Reporte express.
- Registro rápido por voz.
- Confirmación de observación.
- Registro posterior del evento.
- Estrategias de aula.

## Especialista

- Lista de pacientes.
- Resumen de paciente.
- Línea base y tendencias.
- Factores y acumuladores.
- Calidad de datos / información faltante.
- Línea temporal de registros.
- Historial de desregulaciones.
- Intervenciones y resultados.
- Registro de sesión.
- Gestión de estrategias preventivas.

---

# 17. Priorización preliminar derivada de las historias para el prototipo de hackatón

Para una demo coherente, no es necesario implementar todas las historias. El flujo mínimo debería demostrar el valor diferencial completo.

## Familia — P0 de demo

1. Registro diario breve.
2. Nota por voz o texto.
3. Confirmación de variables extraídas.
4. Riesgo + confianza.
5. Explicación de factores.
6. Recomendación preventiva.
7. Feedback de resultado.

## Educación — P0 de demo

1. Resumen de alumnos con señales relevantes.
2. Detalle funcional de un estudiante.
3. Recomendación escolar breve.
4. Reporte express de desregulación.
5. Nota rápida por voz.

## Especialista — P0 de demo

1. Selección de paciente.
2. Línea base vs. estado reciente.
3. Tendencias y acumuladores.
4. Factores contribuyentes.
5. Riesgo + confianza + datos faltantes.
6. Historial de estrategias y resultados.

---

# 18. Criterio de coherencia para futuras interfaces

Toda pantalla de Bluba Anticipa debería poder justificarse respondiendo al menos una de estas preguntas:

1. **¿Ayuda a registrar información relevante con menor fricción?**
2. **¿Ayuda a comprender el estado actual respecto del baseline individual?**
3. **¿Ayuda a entender el riesgo y su incertidumbre?**
4. **¿Explica qué factores originaron una alerta?**
5. **¿Permite actuar preventivamente de acuerdo con el rol?**
6. **¿Permite registrar el resultado y mejorar el historial del niño?**

Si una pantalla o componente no contribuye claramente a una de estas funciones, probablemente no sea prioritario para el MVP.

---

# 19. Alcance explícitamente fuera del diseño principal del MVP

Para mantener el foco de NeuroHack 2026, este documento no considera como parte principal del MVP:

- diagnóstico médico;
- intervención automática;
- monitoreo continuo por micrófono o cámara;
- reconocimiento facial;
- GPS;
- EEG;
- sensores ambientales físicos;
- construcción de wearables;
- modelos fisiológicos en tiempo real;
- predicción clínicamente validada a 24 horas.

Estas capacidades pueden formar parte de etapas posteriores, pero no son necesarias para demostrar la propuesta de valor actual.

---

# 20. Trazabilidad y siguientes etapas

Las historias de usuario de este documento se convierten en requerimientos funcionales y no funcionales en las secciones siguientes. La cadena de trazabilidad objetivo es:

```text
Historia de usuario
        ↓
Requerimiento funcional
        ↓
Requerimiento no funcional aplicable
        ↓
Criterio de aceptación
        ↓
Pantalla / API / modelo de datos
        ↓
Caso de prueba
```

A partir de esta especificación deberán desarrollarse posteriormente:

1. **Matriz HU → RF → pantalla/API → criterio de aceptación** para los elementos P0.
2. **Arquitectura de información** por rol.
3. **Mapa de navegación móvil**.
4. **Wireframes de baja fidelidad**.
5. **Sistema de diseño y componentes reutilizables**.
6. **Contratos de API** alineados con el modelo de dominio.
7. **Criterios de aceptación** por requerimiento P0.
8. **Casos de prueba funcionales y de UX**, especialmente para registro rápido, datos incompletos, riesgo/confianza y reporte express.

---

## 21. Fuentes de alcance utilizadas

Este documento consolida y amplía, para fines de diseño de interfaz, los elementos definidos en:

- **Bluba Anticipa.md** — propuesta base del sistema: baseline individual, acumulación temporal, riesgo + confianza, explicabilidad, asistente conversacional, memoria de intervenciones, manejo de datos incompletos, tres vistas por rol y feedback longitudinal.
- **Especificación UI/UX & Sistema de Diseño de Bluba** — historias iniciales por rol, reporte express, registro por voz, resumen preventivo, analítica para especialistas, uso móvil, operación de baja fricción y soporte offline.
- **Bases y presentación oficial de NeuroHack 2026** — objetivo del desafío, variables disponibles, necesidad de gestionar datos incompletos, calidad de la predicción, reducción de carga de registro y generación de explicaciones accionables.

---

# 22. Convenciones para los requerimientos

Los requerimientos derivados de las historias de usuario utilizan la siguiente nomenclatura:

| Prefijo | Significado |
|---|---|
| `RF-TR` | Requerimiento funcional transversal |
| `RF-CAP` | Captura y estructuración de información |
| `RF-PRED` | Riesgo, confianza y explicabilidad |
| `RF-FAM` | Funciones específicas de familia/cuidador |
| `RF-EDU` | Funciones específicas de educación |
| `RF-ESP` | Funciones específicas de especialista |
| `RF-COL` | Coordinación entre contextos |
| `RF-SEG` | Seguridad funcional y comunicación |
| `RNF-USA` | Usabilidad |
| `RNF-PERF` | Rendimiento |
| `RNF-OFF` | Operación offline y sincronización |
| `RNF-SEC` | Seguridad y privacidad |
| `RNF-DAT` | Integridad y calidad de datos |
| `RNF-ACC` | Accesibilidad |
| `RNF-REL` | Fiabilidad y tolerancia a fallos |
| `RNF-AI` | Explicabilidad y seguridad de IA |
| `RNF-MNT` | Mantenibilidad y arquitectura |

Las prioridades mantienen el criterio definido para las historias:

- **P0:** necesario para demostrar el núcleo de valor y funcionamiento seguro del MVP.
- **P1:** importante para una experiencia completa y una futura integración productiva, pero simplificable en la demo.
- **P2:** evolución posterior al núcleo del MVP.

---

# 23. Requerimientos funcionales transversales

| ID | Requerimiento | Prioridad | Trazabilidad |
|---|---|---:|---|
| **RF-TR-01** | El sistema deberá autenticar al usuario antes de permitir acceso a información de niños. | P0 | HU-TR-01 |
| **RF-TR-02** | El sistema deberá asociar cada usuario a uno o más roles autorizados: familia/cuidador, educador o especialista. | P0 | HU-TR-01 |
| **RF-TR-03** | El sistema deberá determinar las funcionalidades e información visibles según el rol y permisos del usuario. | P0 | HU-TR-01, HU-COL-04 |
| **RF-TR-04** | El sistema deberá permitir seleccionar el niño activo cuando el usuario tenga acceso a más de uno. | P0 | HU-TR-02 |
| **RF-TR-05** | El sistema deberá mantener visible o claramente identificable el niño actualmente seleccionado durante los flujos de consulta y registro. | P0 | HU-TR-02 |
| **RF-TR-06** | El sistema deberá presentar un resumen preventivo del estado actual del niño. | P0 | HU-TR-03 |
| **RF-TR-07** | El resumen preventivo deberá adaptar su profundidad y contenido al rol del usuario. | P0 | HU-TR-01, HU-EDU-03 |
| **RF-TR-08** | El sistema deberá permitir consultar una línea temporal de registros relevantes del niño. | P1 | HU-TR-08 |
| **RF-TR-09** | Cada registro deberá identificar su origen: familia, establecimiento educacional o profesional. | P1 | HU-TR-09 |
| **RF-TR-10** | El sistema deberá mostrar el estado de persistencia/sincronización de los registros móviles. | P1 | HU-TR-10 |
| **RF-TR-11** | El sistema deberá permitir generar registros críticos temporalmente sin conexión. | P1 | HU-TR-11 |
| **RF-TR-12** | Los registros realizados sin conexión deberán sincronizarse cuando se recupere conectividad. | P1 | HU-TR-10, HU-TR-11 |
| **RF-TR-13** | El sistema deberá permitir generar notificaciones relevantes según el rol del receptor y la política de alertas definida. | P1 | HU-TR-12 |

---

# 24. Requerimientos funcionales de captura y estructuración

Este bloque materializa el principio de **voz/texto → variables estructuradas → confirmación humana**. La IA conversacional ayuda a estructurar observaciones, pero no decide directamente el riesgo.

| ID | Requerimiento | Prioridad | Trazabilidad |
|---|---|---:|---|
| **RF-CAP-01** | El sistema deberá permitir registrar información mediante controles estructurados de selección rápida. | P0 | HU-FAM-01 |
| **RF-CAP-02** | El sistema deberá permitir registrar observaciones mediante texto libre. | P0 | HU-FAM-02 |
| **RF-CAP-03** | El sistema deberá permitir registrar observaciones mediante notas de voz. | P0 | HU-FAM-03, HU-EDU-09 |
| **RF-CAP-04** | El sistema deberá transformar observaciones de texto o voz en variables estructuradas compatibles con el modelo de datos de Bluba Anticipa. | P0 | HU-FAM-04, HU-EDU-10 |
| **RF-CAP-05** | El sistema deberá presentar al usuario las variables extraídas antes de almacenarlas como confirmadas. | P0 | HU-FAM-04, HU-EDU-10 |
| **RF-CAP-06** | El usuario deberá poder confirmar la interpretación propuesta. | P0 | HU-FAM-04 |
| **RF-CAP-07** | El usuario deberá poder corregir individualmente las variables interpretadas sin repetir el registro completo. | P0 | HU-FAM-05 |
| **RF-CAP-08** | Las variables estructuradas provenientes de IA deberán registrar que su origen fue una interpretación automática confirmada por un usuario. | P0 | HU-SEG-02 |
| **RF-CAP-09** | El sistema deberá poder identificar variables relevantes que se encuentran ausentes o desactualizadas. | P0 | HU-TR-06 |
| **RF-CAP-10** | El sistema podrá solicitar prioritariamente la información faltante que resulte más útil para mejorar la estimación. | P1 | HU-FAM-06, HU-COL-02 |
| **RF-CAP-11** | El sistema deberá evitar solicitar nuevamente información equivalente que ya haya sido registrada durante el periodo correspondiente, cuando los permisos lo permitan. | P1 | HU-COL-01 |

---

# 25. Requerimientos funcionales del motor de riesgo, confianza y explicabilidad

| ID | Requerimiento | Prioridad | Trazabilidad |
|---|---|---:|---|
| **RF-PRED-01** | El sistema deberá construir o mantener una representación de la línea base individual del niño a partir de su información longitudinal válida. | P0 | HU-FAM-08, HU-ESP-03 |
| **RF-PRED-02** | El sistema deberá comparar el estado reciente del niño con su línea base individual. | P0 | HU-FAM-08, HU-ESP-04 |
| **RF-PRED-03** | El sistema deberá identificar desviaciones relevantes respecto del comportamiento habitual del niño. | P0 | HU-FAM-08, HU-ESP-02 |
| **RF-PRED-04** | El sistema deberá analizar la persistencia o acumulación de factores a través de múltiples registros o días. | P0 | HU-FAM-09, HU-ESP-05 |
| **RF-PRED-05** | El sistema deberá combinar múltiples factores disponibles para producir un indicador preventivo de riesgo. | P0 | HU-TR-03 |
| **RF-PRED-06** | El sistema deberá generar un nivel de riesgo para una ventana temporal definida correspondiente, por defecto, a las próximas 24 horas. | P0 | HU-FAM-07 |
| **RF-PRED-07** | El nivel de riesgo deberá almacenarse y presentarse independientemente del nivel de confianza. | P0 | HU-TR-04 |
| **RF-PRED-08** | El sistema deberá calcular un nivel de confianza de la estimación considerando la calidad de la información disponible. | P0 | HU-TR-04, HU-ESP-09 |
| **RF-PRED-09** | La confianza deberá considerar, al menos, completitud, vigencia de los registros, cobertura de fuentes, profundidad de historial y consistencia entre registros. | P0 | HU-TR-06, HU-ESP-10, HU-ESP-11 |
| **RF-PRED-10** | El sistema deberá identificar los principales factores que contribuyeron a una estimación de riesgo. | P0 | HU-TR-05, HU-ESP-08 |
| **RF-PRED-11** | El sistema deberá presentar los factores explicativos utilizando lenguaje adecuado al rol del usuario. | P0 | HU-FAM-07, HU-EDU-04, HU-ESP-08 |
| **RF-PRED-12** | El sistema deberá identificar explícitamente la información relevante que falta para realizar la estimación. | P0 | HU-TR-06 |
| **RF-PRED-13** | Cuando la información disponible sea insuficiente, el sistema deberá evitar presentar una estimación como confiable. | P0 | HU-TR-07 |
| **RF-PRED-14** | Ante información insuficiente, el sistema deberá indicar qué datos adicionales podrían completar la evaluación. | P0 | HU-TR-07 |
| **RF-PRED-15** | La incorporación de un nuevo registro relevante deberá permitir actualizar la evaluación preventiva del niño. | P0 | HU-TR-03, HU-FAM-01 |
| **RF-PRED-16** | El especialista deberá poder consultar distintas ventanas temporales de análisis cuando corresponda. | P1 | HU-ESP-06 |

---

# 26. Requerimientos funcionales específicos — Familia / cuidador

| ID | Requerimiento | Prioridad | Trazabilidad |
|---|---|---:|---|
| **RF-FAM-01** | El sistema deberá proporcionar un check-in cotidiano breve para registrar información relevante del comienzo del día. | P0 | HU-FAM-01 |
| **RF-FAM-02** | La vista familiar deberá presentar el nivel de riesgo utilizando lenguaje comprensible y no clínico. | P0 | HU-FAM-07 |
| **RF-FAM-03** | La vista familiar deberá indicar qué elementos cambiaron respecto del patrón habitual del niño. | P0 | HU-FAM-08 |
| **RF-FAM-04** | La vista familiar deberá comunicar acumulaciones relevantes ocurridas durante varios días. | P0 | HU-FAM-09 |
| **RF-FAM-05** | La vista familiar deberá mostrar de manera diferenciada riesgo, confianza y datos faltantes. | P0 | HU-FAM-10 |
| **RF-FAM-06** | El sistema deberá proporcionar un conjunto reducido de acciones preventivas aplicables al contexto familiar. | P0 | HU-FAM-11 |
| **RF-FAM-07** | Cada recomendación deberá poder indicar su procedencia, por ejemplo, recomendación profesional o estrategia previamente utilizada. | P0 | HU-FAM-12 |
| **RF-FAM-08** | El usuario deberá poder consultar instrucciones breves asociadas a una estrategia cuando estas se encuentren disponibles. | P1 | HU-FAM-13 |
| **RF-FAM-09** | Tras finalizar una ventana de predicción, el sistema deberá permitir registrar si ocurrió o no una desregulación. | P0 | HU-FAM-14 |
| **RF-FAM-10** | El usuario deberá poder registrar una estrategia aplicada durante la ventana correspondiente. | P0 | HU-FAM-15 |
| **RF-FAM-11** | El usuario deberá poder registrar el resultado de una estrategia como exitosa, parcialmente útil o sin efecto. | P0 | HU-FAM-16 |
| **RF-FAM-12** | El usuario deberá poder revisar un historial simplificado de días recientes. | P1 | HU-FAM-17 |

---

# 27. Requerimientos funcionales específicos — Profesor / educador

| ID | Requerimiento | Prioridad | Trazabilidad |
|---|---|---:|---|
| **RF-EDU-01** | El sistema deberá proporcionar una vista de aula con los estudiantes autorizados para el educador. | P0 | HU-EDU-01 |
| **RF-EDU-02** | La vista de aula deberá permitir identificar estudiantes con señales preventivas relevantes. | P0 | HU-EDU-01, HU-EDU-02 |
| **RF-EDU-03** | La información mostrada al educador deberá limitarse a aquella necesaria para la intervención educativa. | P0 | HU-EDU-03 |
| **RF-EDU-04** | El detalle del estudiante deberá mostrar señales funcionales relevantes para la jornada escolar. | P0 | HU-EDU-04 |
| **RF-EDU-05** | El sistema deberá proporcionar estrategias preventivas compatibles con contexto escolar. | P0 | HU-EDU-05 |
| **RF-EDU-06** | El educador deberá poder iniciar un reporte express de una desregulación o escalada. | P0 | HU-EDU-06 |
| **RF-EDU-07** | El reporte express deberá poder guardarse sin exigir la incorporación inmediata de información complementaria. | P0 | HU-EDU-07 |
| **RF-EDU-08** | Después de un reporte express, el sistema deberá indicar claramente si el registro quedó almacenado localmente, pendiente de sincronización o sincronizado. | P0 | HU-EDU-08 |
| **RF-EDU-09** | El educador deberá poder complementar posteriormente un reporte express. | P1 | HU-EDU-11 |
| **RF-EDU-10** | El educador deberá poder registrar durante la jornada si el estado del estudiante mejoró, empeoró o permaneció estable. | P1 | HU-EDU-12 |
| **RF-EDU-11** | El educador deberá poder registrar factores contextuales o posibles desencadenantes observados. | P1 | HU-EDU-13 |
| **RF-EDU-12** | El educador deberá poder registrar una estrategia aplicada y su resultado. | P1 | HU-EDU-14 |
| **RF-EDU-13** | El educador deberá poder consultar nuevamente durante la jornada el estado preventivo actualizado del estudiante. | P1 | HU-EDU-15 |

---

# 28. Requerimientos funcionales específicos — Especialista

| ID | Requerimiento | Prioridad | Trazabilidad |
|---|---|---:|---|
| **RF-ESP-01** | El sistema deberá proporcionar al especialista una lista de pacientes autorizados con un resumen de su estado reciente. | P0 | HU-ESP-01 |
| **RF-ESP-02** | La lista deberá permitir identificar pacientes con desviaciones relevantes respecto de su línea base. | P0 | HU-ESP-02 |
| **RF-ESP-03** | El especialista deberá poder visualizar la línea base individual del paciente. | P0 | HU-ESP-03 |
| **RF-ESP-04** | El sistema deberá permitir comparar visualmente el estado reciente con la línea base. | P0 | HU-ESP-04 |
| **RF-ESP-05** | El sistema deberá presentar tendencias y acumulaciones temporales relevantes. | P0 | HU-ESP-05 |
| **RF-ESP-06** | El especialista deberá poder consultar episodios previos junto con los registros que los precedieron. | P1 | HU-ESP-07 |
| **RF-ESP-07** | El sistema deberá presentar las variables o tendencias que contribuyen a una estimación. | P0 | HU-ESP-08 |
| **RF-ESP-08** | El especialista deberá visualizar independientemente riesgo y confianza. | P0 | HU-ESP-09 |
| **RF-ESP-09** | El sistema deberá presentar variables relevantes ausentes o desactualizadas. | P0 | HU-ESP-10 |
| **RF-ESP-10** | El sistema deberá poder señalar registros potencialmente contradictorios provenientes de fuentes diferentes. | P1 | HU-ESP-11 |
| **RF-ESP-11** | El especialista deberá poder acceder desde una señal resumida al registro original que la sustenta. | P1 | HU-ESP-12 |
| **RF-ESP-12** | El especialista deberá poder consultar las estrategias aplicadas previamente al paciente. | P0 | HU-ESP-13 |
| **RF-ESP-13** | El sistema deberá presentar los resultados históricos registrados para dichas estrategias. | P0 | HU-ESP-14 |
| **RF-ESP-14** | El especialista deberá poder registrar o validar estrategias preventivas asociadas al paciente. | P1 | HU-ESP-15 |
| **RF-ESP-15** | Las estrategias deberán poder etiquetarse según su contexto de aplicación: hogar, escuela o ambos. | P1 | HU-ESP-16 |
| **RF-ESP-16** | El especialista deberá poder registrar una observación vinculada a una sesión profesional. | P0 | HU-ESP-17 |
| **RF-ESP-17** | El especialista podrá convertir una observación textual en variables estructuradas revisables. | P1 | HU-ESP-18 |
| **RF-ESP-18** | El especialista deberá poder registrar modificaciones relevantes del plan o estrategias de apoyo. | P1 | HU-ESP-19 |

---

# 29. Requerimientos funcionales de coordinación entre contextos

| ID | Requerimiento | Prioridad | Trazabilidad |
|---|---|---:|---|
| **RF-COL-01** | El sistema deberá integrar los registros autorizados de diferentes contextos dentro del historial longitudinal del niño. | P0 | HU-TR-08 |
| **RF-COL-02** | El sistema deberá prevenir o advertir al usuario cuando determinada información relevante ya se encuentre registrada y repetirla no sea necesario. | P1 | HU-COL-01 |
| **RF-COL-03** | Una alerta relevante deberá poder reflejarse en los contextos autorizados correspondientes. | P1 | HU-COL-03 |
| **RF-COL-04** | La información compartida deberá filtrarse según los permisos asociados al contexto y rol. | P0 | HU-COL-04 |
| **RF-COL-05** | Las modificaciones relevantes realizadas a los registros deberán preservar trazabilidad. | P1 | HU-COL-05 |
| **RF-COL-06** | El historial deberá conservar la identidad funcional de la fuente de un registro y su momento de creación. | P1 | HU-TR-09, HU-COL-05 |

---

# 30. Requerimientos funcionales de seguridad de comunicación

| ID | Requerimiento | Prioridad | Trazabilidad |
|---|---|---:|---|
| **RF-SEG-01** | El sistema deberá comunicar las predicciones como estimaciones preventivas de riesgo y no como diagnóstico o certeza clínica. | P0 | HU-SEG-01 |
| **RF-SEG-02** | La interfaz deberá distinguir información observada/registrada por personas de información calculada o inferida por el sistema. | P0 | HU-SEG-02 |
| **RF-SEG-03** | Una confianza baja deberá incluir una explicación de los factores que redujeron dicha confianza. | P0 | HU-SEG-03 |
| **RF-SEG-04** | Las recomendaciones ofrecidas deberán provenir de estrategias previamente aprobadas, registradas o incluidas en un catálogo validado. | P0 | HU-SEG-04 |
| **RF-SEG-05** | El componente generativo no deberá crear libremente nuevas intervenciones clínicas para el usuario. | P0 | HU-SEG-04 |
| **RF-SEG-06** | Cuando se utilice IA para adaptar la redacción de una recomendación, el contenido de la estrategia deberá mantenerse vinculado a su fuente original. | P0 | HU-FAM-12, HU-SEG-04 |

---

# 31. Requerimientos no funcionales

## 31.1 Usabilidad

| ID | Requerimiento | Prioridad |
|---|---|---:|
| **RNF-USA-01** | La aplicación deberá estar diseñada prioritariamente para dispositivos móviles iOS y Android. | P0 |
| **RNF-USA-02** | Las operaciones críticas deberán minimizar navegación y carga cognitiva. | P0 |
| **RNF-USA-03** | El reporte express de educación deberá poder completarse mediante una secuencia mínima de interacción, sin formularios complementarios obligatorios. | P0 |
| **RNF-USA-04** | Riesgo y confianza deberán utilizar representaciones visuales y textuales diferenciables entre sí. | P0 |
| **RNF-USA-05** | La interpretación del estado preventivo no deberá depender exclusivamente del color. | P0 |
| **RNF-USA-06** | Los mensajes dirigidos a familia y educadores deberán evitar terminología técnica o clínica innecesaria. | P0 |

## 31.2 Rendimiento

Los valores numéricos siguientes son **objetivos técnicos del MVP**, no requisitos clínicos.

| ID | Requerimiento | Objetivo MVP |
|---|---|---|
| **RNF-PERF-01** | La aplicación deberá proporcionar respuesta interactiva sin bloqueos perceptibles en operaciones comunes. | Interacciones locales típicas < 300 ms |
| **RNF-PERF-02** | La pantalla inicial de estado deberá cargarse en un tiempo adecuado bajo conectividad normal. | ≤ 3 s en p95 |
| **RNF-PERF-03** | Una operación de registro no deberá depender de que el motor predictivo termine su cálculo para confirmar la persistencia del dato. | Persistencia y predicción desacopladas |
| **RNF-PERF-04** | La actualización del riesgo podrá realizarse asincrónicamente después de incorporar nueva información. | Actualización en segundos, sin bloquear captura |

## 31.3 Operación offline y sincronización

| ID | Requerimiento |
|---|---|
| **RNF-OFF-01** | La pérdida temporal de conectividad no deberá causar pérdida de un registro crítico ya confirmado localmente. |
| **RNF-OFF-02** | Cada registro pendiente deberá mantener un estado de sincronización inequívoco. |
| **RNF-OFF-03** | El sistema deberá reintentar automáticamente la sincronización cuando se recupere conectividad. |
| **RNF-OFF-04** | Una falla de sincronización deberá conservar el registro local y permitir reintento posterior. |
| **RNF-OFF-05** | La interfaz no deberá representar datos desactualizados como información actual sin advertencia cuando se encuentre offline. |

## 31.4 Seguridad y privacidad

| ID | Requerimiento |
|---|---|
| **RNF-SEC-01** | Todo acceso a información del niño deberá requerir autenticación. |
| **RNF-SEC-02** | La autorización deberá aplicarse en backend y no depender exclusivamente de ocultar elementos en la interfaz. |
| **RNF-SEC-03** | Los datos deberán protegerse mediante cifrado durante su transmisión. |
| **RNF-SEC-04** | Los datos sensibles almacenados deberán protegerse mediante mecanismos de cifrado adecuados a la plataforma y al entorno de despliegue. |
| **RNF-SEC-05** | El sistema deberá implementar el principio de mínimo privilegio por usuario y rol. |
| **RNF-SEC-06** | Un educador no deberá poder consultar información clínica o familiar para la que no posea autorización explícita. |
| **RNF-SEC-07** | Las operaciones sensibles deberán mantener trazabilidad suficiente para auditoría. |
| **RNF-SEC-08** | No deberán almacenarse secretos, credenciales o tokens de acceso en texto plano en el cliente móvil. |

## 31.5 Integridad y calidad de datos

| ID | Requerimiento |
|---|---|
| **RNF-DAT-01** | Un dato faltante deberá conservarse explícitamente como desconocido y no convertirse automáticamente en un valor normal. |
| **RNF-DAT-02** | Cada registro deberá preservar su timestamp, fuente y autor/contexto cuando corresponda. |
| **RNF-DAT-03** | La corrección de información no deberá eliminar silenciosamente la trazabilidad requerida por el sistema. |
| **RNF-DAT-04** | Las variables derivadas deberán poder relacionarse con los registros utilizados para producirlas. |
| **RNF-DAT-05** | Las predicciones almacenadas deberán conservar la versión de datos o información necesaria para reconstruir su contexto. |

## 31.6 Accesibilidad

| ID | Requerimiento |
|---|---|
| **RNF-ACC-01** | La interfaz móvil deberá mantener tamaños de texto y controles táctiles apropiados para uso móvil. |
| **RNF-ACC-02** | La información crítica no deberá comunicarse exclusivamente mediante color, iconos o gráficos. |
| **RNF-ACC-03** | Los componentes interactivos deberán proporcionar etiquetas accesibles para tecnologías asistivas. |
| **RNF-ACC-04** | La aplicación deberá tolerar escalado de texto del sistema sin pérdida de funcionalidad esencial. |
| **RNF-ACC-05** | El contraste visual deberá cumplir como objetivo al menos WCAG 2.2 AA en componentes relevantes. |

## 31.7 Fiabilidad y tolerancia a fallos

| ID | Requerimiento |
|---|---|
| **RNF-REL-01** | Un error del servicio de IA no deberá impedir el registro manual de información. |
| **RNF-REL-02** | Un error del motor predictivo no deberá provocar pérdida de registros ingresados. |
| **RNF-REL-03** | Cuando no pueda calcularse una estimación, el sistema deberá representar explícitamente ese estado en vez de reutilizar silenciosamente un resultado obsoleto. |
| **RNF-REL-04** | Los registros enviados más de una vez por reintentos de red deberán procesarse de forma idempotente cuando corresponda. |
| **RNF-REL-05** | Las operaciones de escritura deberán evitar estados parcialmente persistidos que generen información inconsistente. |

## 31.8 Explicabilidad y seguridad de IA

| ID | Requerimiento |
|---|---|
| **RNF-AI-01** | Toda estimación de riesgo visible deberá disponer de factores explicativos asociados. |
| **RNF-AI-02** | El sistema deberá identificar el nivel de confianza independientemente del riesgo. |
| **RNF-AI-03** | Una observación estructurada automáticamente no deberá almacenarse como confirmada hasta la intervención del usuario cuando el flujo exija confirmación humana. |
| **RNF-AI-04** | Las salidas generativas no deberán poder modificar directamente el resultado calculado por el motor predictivo. |
| **RNF-AI-05** | Las recomendaciones preventivas deberán limitarse a fuentes previamente autorizadas por el sistema. |
| **RNF-AI-06** | El sistema deberá poder diferenciar, tanto en UI como en datos, una observación humana, una extracción automática y una inferencia predictiva. |

## 31.9 Mantenibilidad y arquitectura

| ID | Requerimiento |
|---|---|
| **RNF-MNT-01** | El módulo de estructuración mediante IA deberá mantenerse desacoplado del motor de riesgo. |
| **RNF-MNT-02** | El motor de recomendaciones deberá mantenerse desacoplado de la generación de lenguaje natural. |
| **RNF-MNT-03** | Las reglas, modelos o parámetros del motor predictivo deberán poder evolucionar sin modificar las interfaces móviles. |
| **RNF-MNT-04** | Los contratos API deberán estar versionados. |
| **RNF-MNT-05** | Las funcionalidades compartidas por los tres roles deberán reutilizar servicios y componentes comunes cuando sea posible. |
| **RNF-MNT-06** | Los eventos relevantes del sistema deberán permitir observabilidad mediante logs estructurados, evitando exposición de información sensible innecesaria. |

---

# 32. Decisiones de especificación para cerrar vacíos del MVP

Las siguientes definiciones se adoptan como **criterios de producto e ingeniería para el prototipo**. No representan umbrales clínicamente validados ni métricas de eficacia médica. Su objetivo es eliminar ambigüedades entre UI, Backend y Data/ML durante la implementación de NeuroHack 2026.

## 32.1 Representación del riesgo

### Decisión

El MVP utilizará un **índice interno de riesgo de 0 a 100**, tratado como un score relativo del motor y **no como una probabilidad clínica calibrada**.

Las bandas iniciales serán:

| Índice | Nivel mostrado | Interpretación de producto |
|---:|---|---|
| 0–39 | **Bajo** | El patrón reciente se mantiene relativamente cercano al comportamiento esperado según la información disponible. |
| 40–69 | **Moderado** | Existen desviaciones o acumulaciones que justifican atención preventiva adicional. |
| 70–100 | **Elevado** | Se observa una combinación de señales suficientemente relevante como para priorizar medidas preventivas y seguimiento. |

### Regla de UI

- **Familia y educación:** mostrar principalmente la categoría y explicación; evitar presentar el score como una probabilidad porcentual.
- **Especialista:** puede visualizarse el índice numérico acompañado explícitamente por la etiqueta **“índice de riesgo — no equivale a probabilidad clínica”**.
- El color puede complementar la señal, pero nunca ser el único medio de comunicación.

### Motivo

El dataset del desafío no permite validar una probabilidad clínica de desregulación a 24 horas. Mostrar `78 %` como si significara “78 % de probabilidad” generaría una precisión que el MVP no puede justificar.

---

## 32.2 Representación de la confianza

### Decisión

La confianza se calculará como un índice independiente de 0 a 100 y se mostrará en tres categorías:

| Confianza | Categoría |
|---:|---|
| 0–59 | **Baja** |
| 60–79 | **Media** |
| 80–100 | **Alta** |

Adicionalmente, se utilizará el estado especial **“Información insuficiente”** cuando no se cumpla el mínimo de datos definido en la sección 32.4.

### Regla

Un riesgo elevado puede mostrarse con confianza baja si las señales disponibles son relevantes pero la evidencia está incompleta. Riesgo y confianza nunca deberán fusionarse en un único indicador.

---

## 32.3 Fórmula heurística de confianza del MVP

Para la demo se utilizará una fórmula explícita y auditable, con puntuación total máxima de 100:

```text
confianza =
    completitud_variables_criticas * 0.40
  + vigencia_registros             * 0.20
  + cobertura_fuentes              * 0.15
  + profundidad_historial          * 0.15
  + consistencia_registros         * 0.10
```

Cada componente se normaliza entre 0 y 100.

### Componentes

1. **Completitud de variables críticas — 40 %**
   - Evalúa cuántas variables mínimas relevantes están disponibles para la ventana actual.

2. **Vigencia de registros — 20 %**
   - Registro de hoy o dentro de la ventana esperada: puntuación alta.
   - Registros progresivamente más antiguos: puntuación decreciente.

3. **Cobertura de fuentes — 15 %**
   - Se incrementa cuando existe información de más de un contexto autorizado, sin exigir siempre las tres fuentes.

4. **Profundidad de historial — 15 %**
   - Refleja cuánta información individual válida existe para construir baseline y tendencias.

5. **Consistencia entre registros — 10 %**
   - Disminuye cuando existen observaciones potencialmente contradictorias que no han sido contextualizadas.

### Restricción

Esta fórmula es un mecanismo de demostración y transparencia del MVP. En un piloto real deberá ser validada y recalibrada con datos longitudinales.

---

## 32.4 Datos mínimos y estado “Información insuficiente”

Para evitar falsa precisión, el MVP definirá un conjunto mínimo de señales críticas para producir una estimación.

### Variables críticas mínimas del estado reciente

1. **Sueño**: horas y/o calidad del sueño.
2. **Estado basal al despertar** o estado inicial equivalente.
3. **Regulación / comportamiento observado** reciente.

### Variables contextuales de alto valor

4. Cambio de rutina.
5. Estado de alerta.
6. Eventos relevantes o excepcionales.
7. Salud gastrointestinal cuando exista registro.
8. Participación/interacciones relevantes según contexto.

### Regla de suficiencia

El sistema mostrará **“Información insuficiente”** en vez de un nivel de riesgo cuando se cumpla cualquiera de estas condiciones:

- faltan **dos o más de las tres variables críticas mínimas** del estado reciente; o
- la confianza calculada es inferior a **40/100**; o
- no existe historial individual suficiente para aplicar el baseline y tampoco existe una tendencia reciente mínima interpretable.

En este estado, la UI deberá solicitar únicamente los datos prioritarios faltantes y no asumir que su ausencia corresponde a normalidad.

---

## 32.5 Horizonte de predicción

### Decisión

La predicción principal del MVP utilizará una ventana por defecto de:

> **Próximas 24 horas desde el momento de cálculo.**

### Reglas

- La predicción deberá almacenar `prediction_start_at` y `prediction_end_at`.
- Un nuevo registro relevante podrá generar una nueva predicción antes del vencimiento de la anterior.
- En contexto escolar, la interfaz puede redactar la información como **“durante la jornada / próximas horas”**, pero deberá preservar la ventana técnica de la predicción.
- El especialista podrá comparar la ventana actual con periodos de 24 h, 72 h y 7 días para análisis, aunque no todos ellos constituyan horizontes de predicción independientes.

---

## 32.6 Definición operativa de baseline individual

### Decisión

El baseline del MVP será **individual, dinámico y robusto a cambios recientes**, evitando usar promedios poblacionales como sustituto del historial personal.

### Reglas iniciales

1. **Baseline provisional:** disponible con al menos **7 días válidos** de información individual.
2. **Baseline suficiente para mayor confianza:** al menos **14 días válidos**.
3. **Ventana de referencia:** últimos **14 días válidos previos**, excluyendo preferentemente las últimas 72 horas utilizadas para detectar deterioro reciente.
4. Para variables numéricas se priorizarán estadísticas robustas como mediana y dispersión robusta.
5. Para variables categóricas se utilizará frecuencia/moda y distribución histórica individual.
6. Los días con información insuficiente no deberán interpretarse como días normales.
7. Si no existe historial suficiente, el sistema deberá reducir confianza y mostrar el baseline como **“en construcción”**, en lugar de reemplazarlo silenciosamente por una norma poblacional.

### Evolución futura

Las ventanas y transformaciones deberán convertirse en parámetros configurables y validarse con un dataset longitudinal real.

---

## 32.7 Manejo de registros contradictorios

### Decisión

El sistema **no resolverá automáticamente una contradicción sobrescribiendo una fuente con otra**.

Ejemplo:

```text
Familia: despertar tranquilo
Escuela: ingreso con irritabilidad alta
```

Ambas observaciones pueden ser verdaderas en momentos o contextos diferentes.

### Reglas

1. Conservar ambos registros con fuente, timestamp y contexto.
2. Detectar potencial contradicción solo cuando las variables sean comparables temporal y semánticamente.
3. Reducir el componente de consistencia de la confianza cuando corresponda.
4. Mostrar al especialista la contradicción y sus fuentes.
5. A familia y educación mostrar únicamente la advertencia compatible con sus permisos, sin revelar información privada de otro contexto.
6. Nunca corregir automáticamente el registro de un usuario basándose en otro actor.

---

## 32.8 Matriz base de acceso por rol

La autorización definitiva deberá implementarse en Backend. Para el MVP se adopta la siguiente matriz conceptual:

| Información / acción | Familia | Educación | Especialista |
|---|---:|---:|---:|
| Estado preventivo del niño autorizado | Sí | Sí | Sí |
| Riesgo + confianza | Sí | Sí, simplificado | Sí, detallado |
| Factores principales | Sí, lenguaje simple | Sí, funcionales | Sí, completos según permiso |
| Check-in de hogar | Registrar / ver propio | No | Ver si está autorizado y es relevante |
| Observaciones escolares | Resumen relevante | Registrar / ver contexto escolar | Ver si está autorizado |
| Notas clínicas detalladas | No por defecto | No | Sí |
| Reporte de desregulación | Sí | Sí, express | Sí |
| Estrategias para hogar | Sí | No salvo “ambos” | Gestionar/validar |
| Estrategias para escuela | No salvo “ambos” | Sí | Gestionar/validar |
| Historial longitudinal completo | Resumen | Limitado al contexto | Sí |
| Gestión de estrategias | No | No | Sí |
| Origen de señales | Contexto permitido | Contexto permitido | Trazabilidad ampliada |

### Principio

La UI puede ocultar información, pero la restricción real debe existir en la autorización del backend.

---

## 32.9 Política inicial de notificaciones

Para evitar fatiga de alertas, el MVP utilizará notificaciones solamente para cambios accionables.

### Eventos notificables

1. Cambio hacia **riesgo elevado** con confianza suficiente para comunicarlo.
2. Aumento relevante de riesgo respecto de la predicción anterior.
3. Aparición del estado **Información insuficiente** cuando el usuario receptor pueda aportar uno de los datos críticos faltantes.
4. Solicitud de feedback al finalizar una ventana de predicción relevante.

### Reglas

- No notificar repetidamente mientras el estado no cambie de forma material.
- Agrupar actualizaciones cercanas cuando sea posible.
- Aplicar un cooldown inicial de **6 horas por niño y tipo de alerta**, salvo un reporte explícito de desregulación.
- Adaptar contenido al rol y no incluir datos sensibles innecesarios en la notificación del sistema operativo.

Las notificaciones push completas son P1; para la demo puede simularse el centro de alertas interno.

---

## 32.10 Política de notas de voz

### Decisión

El audio será un mecanismo de captura temporal y no un dato longitudinal por defecto.

### Flujo recomendado

```text
Audio voluntario
      ↓
Transcripción
      ↓
Extracción estructurada
      ↓
Confirmación / corrección humana
      ↓
Guardar registro estructurado + texto fuente necesario
      ↓
Eliminar audio temporal
```

### Reglas

1. No se realizará escucha continua.
2. El audio se eliminará después de completar correctamente la transcripción/estructuración y confirmación, salvo que una futura política explícita justifique conservarlo.
3. El MVP podrá conservar el texto original/transcripción como evidencia de origen si es necesario para trazabilidad, sujeto a permisos.
4. La falla de transcripción deberá permitir regresar al registro manual sin pérdida de la intención de registro.

---

## 32.11 Catálogo inicial de estrategias preventivas

### Decisión

El motor no generará intervenciones clínicas libres. Las recomendaciones deberán seleccionarse desde un conjunto controlado.

Cada estrategia deberá contener como mínimo:

```text
strategy_id
name
description_short
context: HOME | SCHOOL | BOTH
source_type: PROFESSIONAL | HISTORICAL | VALIDATED_CATALOG
approved_by (nullable)
active
```

### Prioridad de selección

1. Estrategias vigentes aprobadas por un profesional para el niño.
2. Estrategias históricas del niño con resultados positivos registrados.
3. Estrategias de catálogo preventivo validadas para el contexto correspondiente.

El componente generativo podrá adaptar la redacción, pero no crear una estrategia nueva que no exista en una de estas fuentes.

---

## 32.12 Reglas para compartir estrategias entre hogar y escuela

1. Toda estrategia deberá tener un contexto explícito: `HOME`, `SCHOOL` o `BOTH`.
2. Una estrategia `HOME` no se mostrará a educación.
3. Una estrategia `SCHOOL` no se mostrará a familia salvo permiso o indicación profesional explícita.
4. Una estrategia `BOTH` podrá mostrarse en ambos contextos con redacción adaptada.
5. Si una estrategia contiene información clínica adicional, la versión mostrada a educación deberá limitarse a instrucciones funcionales necesarias para su aplicación.

---

## 32.13 Versionamiento y trazabilidad de predicciones

Cada cálculo de riesgo deberá generar una entidad `RiskPrediction` inmutable o versionada, no sobrescribir silenciosamente el resultado anterior.

### Campos mínimos recomendados

```text
prediction_id
child_id
generated_at
prediction_start_at
prediction_end_at
risk_score
risk_level
confidence_score
confidence_level
status
main_factors[]
missing_critical_data[]
data_cutoff_at
model_or_rules_version
feature_schema_version
```

### Reglas

- Un nuevo cálculo crea una nueva predicción.
- La UI utiliza la predicción vigente más reciente para la ventana correspondiente.
- El especialista puede consultar predicciones anteriores si el producto lo habilita.
- El feedback posterior debe asociarse a la predicción que se está evaluando.

---

## 32.14 Estado de una predicción

Para evitar ambigüedades operativas, una predicción deberá distinguir al menos los siguientes estados:

| Estado | Significado |
|---|---|
| `ACTIVE` | Predicción vigente dentro de su ventana. |
| `EXPIRED` | La ventana de predicción terminó. |
| `SUPERSEDED` | Existe una predicción más reciente que la reemplaza operativamente. |
| `INSUFFICIENT_DATA` | No se pudo emitir un nivel de riesgo con el mínimo de confianza requerido. |

---

## 32.15 Regla de actualización del riesgo

El motor deberá reevaluar el estado cuando ocurra al menos uno de estos eventos:

- confirmación de un nuevo `DailyRecord` relevante;
- incorporación de una observación escolar/profesional relevante;
- registro de una desregulación;
- actualización de una variable crítica previamente faltante;
- cambio explícito de rutina o evento excepcional;
- ejecución programada de cierre/inicio de ventana si corresponde.

Para el MVP no se requiere procesamiento en tiempo real continuo.

---

# 33. Corte recomendado de MVP — definición vigente

Este apartado establece el **corte de implementación recomendado para NeuroHack 2026**. Ante discrepancias con la priorización preliminar de la sección 17, este corte debe considerarse la referencia vigente para desarrollo y demo.

## 33.1 Objetivo de la demo

La demo debe probar un ciclo completo y entendible:

```text
REGISTRAR
   ↓
ESTRUCTURAR Y CONFIRMAR
   ↓
COMPARAR CON BASELINE
   ↓
DETECTAR ACUMULACIONES
   ↓
CALCULAR RIESGO + CONFIANZA
   ↓
EXPLICAR
   ↓
RECOMENDAR
   ↓
REGISTRAR RESULTADO
```

No es necesario implementar toda la plataforma para demostrar el valor diferencial.

## 33.2 Funcionalidades P0 que sí deben entrar al MVP

### Núcleo transversal

1. Selección de rol o sesión demostrativa por rol.
2. Selección inequívoca del niño/paciente activo.
3. Estado preventivo con **riesgo + confianza** separados.
4. Explicación de factores principales.
5. Manejo explícito de datos faltantes.
6. Integración de registros en una línea temporal simplificada.

### Familia

1. Check-in diario breve.
2. Registro mediante texto y, si el tiempo lo permite, voz.
3. Extracción de variables estructuradas.
4. Confirmación/corrección humana antes de guardar.
5. Vista del estado actual respecto del baseline.
6. Recomendación preventiva controlada.
7. Registro posterior de desregulación y resultado de estrategia.

### Educación

1. Vista simple de estudiantes priorizados.
2. Detalle funcional de un estudiante.
3. 2–3 estrategias de aula relevantes.
4. Reporte express de desregulación/escalada.
5. Confirmación inmediata del reporte.

### Especialista

1. Lista de pacientes o selector de caso.
2. Línea base vs. periodo reciente.
3. Visualización de acumuladores/tendencias.
4. Factores contribuyentes.
5. Riesgo + confianza + datos faltantes.
6. Historial básico de estrategias y resultados.

### Motor / Data

1. Dataset sintético realista para escenarios demostrables.
2. Baseline individual calculable.
3. Features temporales mínimas.
4. Motor interpretable de riesgo o scoring reproducible.
5. Fórmula de confianza independiente.
6. Salida explicable con factores principales.
7. Persistencia/versionamiento básico de predicciones.

### Backend

1. Entidades mínimas del dominio.
2. API de registros diarios/observaciones.
3. API o servicio de predicción.
4. API de intervenciones y resultados.
5. Autorización por rol simplificada pero coherente con la matriz de permisos.
6. Trazabilidad mínima de fuente y timestamps.

### App móvil

1. Navegación diferenciada por rol.
2. Estado de hoy.
3. Captura rápida.
4. Confirmación de IA.
5. Detalle de alerta.
6. Recomendaciones.
7. Feedback.
8. Reporte express de educación.
9. Vistas analíticas esenciales para especialista.

---

## 33.3 Funcionalidades P1 que pueden simplificarse o simularse

| Funcionalidad | Tratamiento recomendado en hackatón |
|---|---|
| Notificaciones push reales | Simular mediante centro interno de alertas. |
| Offline completo | Implementar cola local simple o demostrar el diseño/estado de sincronización. |
| Active feature acquisition | Aplicar reglas simples de “dato crítico faltante”; no ML activo. |
| Detección avanzada de contradicciones | Mostrar uno o dos casos preparados. |
| Edición completa de estrategias por especialista | Puede resolverse con catálogo precargado. |
| Múltiples ventanas configurables | Mantener 24 h como principal; 72 h/7 días solo para visualización. |
| Auditoría completa | Registrar metadatos esenciales; dashboard de auditoría queda fuera. |
| Recuperación y gestión avanzada de sesiones | Simplificada para demo. |

---

## 33.4 Funcionalidades que quedan fuera del MVP

- autenticación productiva completa con todos los flujos de recuperación y administración;
- entrenamiento o validación de un modelo clínico real;
- afirmaciones de sensibilidad, especificidad o exactitud clínica;
- wearables y biometría;
- escucha continua de audio;
- sensores físicos;
- GPS;
- cámaras o reconocimiento facial;
- diagnóstico médico;
- intervención automática;
- generación libre de recomendaciones clínicas;
- resolución automática de contradicciones entre personas;
- active learning o adquisición adaptativa avanzada de variables;
- integraciones productivas con sistemas externos no necesarias para la demo.

---

## 33.5 Escenarios mínimos que la demo debe mostrar

Para evidenciar que el sistema no funciona únicamente en un caso feliz, se recomienda preparar al menos cinco escenarios:

### Escenario A — Día estable

```text
Baseline estable
+ datos completos
+ sin desviaciones relevantes
→ riesgo bajo / confianza alta
```

### Escenario B — Deterioro gradual

```text
3 días de sueño alterado
+ regulación descendente
+ cambio de rutina
→ riesgo elevado
→ explicación de acumuladores
```

### Escenario C — Riesgo con información incompleta

```text
señales adversas disponibles
+ ausencia de una fuente o variable relevante
→ riesgo elevado o moderado
→ confianza baja
→ datos faltantes explícitos
```

### Escenario D — Información insuficiente

```text
faltan 2/3 variables críticas
→ sin nivel de riesgo
→ solicitar mínimo registro adicional
```

### Escenario E — Intervención y feedback

```text
riesgo elevado
→ estrategia histórica/profesional
→ usuario registra aplicación
→ ventana termina
→ ocurre/no ocurre desregulación
→ resultado de intervención almacenado
```

---

## 33.6 Criterio de “MVP terminado” para la hackatón

El prototipo puede considerarse suficientemente completo cuando un evaluador pueda observar, sin explicación técnica extensa, que:

1. un usuario registra información con baja fricción;
2. el sistema transforma esa información en variables revisables;
3. el niño se compara con su propio historial;
4. el sistema detecta una acumulación temporal;
5. riesgo y confianza se presentan por separado;
6. los datos faltantes afectan explícitamente la confianza;
7. la alerta explica por qué apareció;
8. la recomendación proviene de una fuente controlada;
9. los tres roles reciben información diferente según su necesidad;
10. el resultado posterior puede registrarse para cerrar el ciclo longitudinal.

Ese flujo demuestra la propuesta de valor principal de **Bluba Anticipa** sin afirmar una validación clínica que el MVP todavía no posee.

# 34. Matriz de implementación P0 — HU → RF → pantalla/API → criterio de aceptación

Esta sección transforma las historias y requerimientos anteriores en un **contrato compartido de implementación para Frontend móvil, Backend y Data/ML**. Su objetivo es reducir decisiones implícitas: para cada historia P0 se establece qué requerimientos la implementan, qué pantalla la materializa, qué API o servicio participa y qué condición mínima debe cumplirse para considerar la historia aceptada.

La matriz no reemplaza los casos de prueba detallados ni la especificación OpenAPI. Define el comportamiento funcional mínimo que dichos artefactos deberán respetar.

## 34.1 Convenciones de implementación

### Prioridad de esta matriz

- Se incluyen primero todas las **historias P0**.
- Cuando una capacidad P1 sea indispensable para mostrar el ciclo P0 definido en la sección 33, se marca como **P0-MVP simplificado**.
- Los criterios de aceptación describen comportamiento observable y verificable; no miden eficacia clínica.
- Los endpoints indicados representan el **contrato lógico recomendado**. Pueden agruparse físicamente en menos controladores mientras se conserve el comportamiento descrito.
- Todos los endpoints expuestos a clientes deberán respetar el versionamiento indicado en `RNF-MNT-04`.

### Separación de responsabilidades

```text
Frontend móvil
  ↓ consume contratos versionados
Backend / API
  ├─ autorización y filtrado por rol
  ├─ persistencia y trazabilidad
  ├─ orquestación de captura, predicción y recomendaciones
  └─ exposición de DTOs adecuados al contexto
        ↓
Servicios Data/ML
  ├─ ObservationStructuringService
  ├─ BaselineService
  ├─ FeatureEngineeringService
  ├─ PredictionService
  └─ RecommendationRankingService
```

El **Frontend no calcula el riesgo**, el **LLM no decide el riesgo** y el **motor predictivo no inventa recomendaciones clínicas**.

---

## 34.2 Catálogo mínimo de pantallas del MVP

Los identificadores siguientes se utilizan en la matriz para evitar que diferentes equipos nombren el mismo flujo de forma distinta.

| ID | Pantalla / flujo | Rol principal | Responsabilidad |
|---|---|---|---|
| `AUTH-01` | Acceso / sesión demostrativa | Todos | Resolver sesión, rol y contexto autorizado. |
| `COMMON-01` | Selector de niño/paciente | Todos | Mostrar y cambiar el sujeto activo sin ambigüedad. |
| `COMMON-02` | Estado preventivo / detalle de alerta | Todos, adaptado | Mostrar riesgo, confianza, factores, faltantes y vigencia. |
| `COMMON-03` | Información insuficiente | Todos, adaptado | Explicar por qué no se emite riesgo y qué dato mínimo falta. |
| `COMMON-04` | Línea temporal simplificada | Todos según permiso | Mostrar registros longitudinales con fuente y timestamp permitidos. |
| `CAP-01` | Captura libre texto/voz | Familia / educación | Recibir una observación cotidiana en lenguaje natural. |
| `CAP-02` | Confirmación de variables | Familia / educación | Revisar, corregir y confirmar variables extraídas. |
| `FAM-01` | Inicio familia — Estado de hoy | Familia | Resumen preventivo, cambios respecto del baseline y acción prioritaria. |
| `FAM-02` | Check-in diario | Familia | Registrar variables críticas con baja fricción. |
| `FAM-03` | Recomendaciones hogar | Familia | Mostrar estrategias controladas y su procedencia. |
| `FAM-04` | Feedback de predicción/intervención | Familia | Cerrar la ventana: evento, estrategia aplicada y resultado. |
| `EDU-01` | Vista aula | Educación | Priorizar estudiantes autorizados según estado preventivo. |
| `EDU-02` | Detalle funcional del estudiante | Educación | Mostrar señales y estrategias necesarias para la jornada. |
| `EDU-03` | Reporte express | Educación | Registrar escalada/desregulación con mínima interacción. |
| `ESP-01` | Lista de pacientes | Especialista | Priorizar pacientes por estado y desviación individual. |
| `ESP-02` | Resumen de paciente | Especialista | Estado actual, riesgo, confianza y calidad de datos. |
| `ESP-03` | Baseline y evolución | Especialista | Comparar baseline, periodo reciente, tendencias y acumulaciones. |
| `ESP-04` | Factores y calidad de datos | Especialista | Explicar contribuciones, faltantes y consistencia. |
| `ESP-05` | Intervenciones y resultados | Especialista | Revisar memoria histórica de estrategias y resultados. |
| `ESP-06` | Registro de sesión | Especialista | Incorporar observación profesional al historial. |

### Excepción de alcance

`COMMON-04` deriva de `HU-TR-08`, originalmente P1. Para la hackatón se implementará como **P0-MVP simplificado** porque la sección 33 exige demostrar integración longitudinal de registros. No requiere filtros, edición avanzada ni navegación histórica completa.

---

## 34.3 Contratos API y servicios internos recomendados

### API móvil / Backend

| Operación lógica | Contrato recomendado | Resultado mínimo esperado |
|---|---|---|
| Crear sesión | `POST /v1/auth/session` | `user_id`, `role`, permisos/contexto demostrativo. |
| Leer contexto del usuario | `GET /v1/me/context` | Rol, niños/pacientes autorizados y contextos disponibles. |
| Listar niños autorizados | `GET /v1/children` | Identificador y datos mínimos permitidos. |
| Estado preventivo | `GET /v1/children/{childId}/preventive-status` | Predicción vigente, baseline resumido y DTO adaptado al rol. |
| Predicción vigente | `GET /v1/children/{childId}/risk-predictions/current` | `status`, riesgo, confianza, factores, faltantes, vigencia y versión. |
| Crear check-in | `POST /v1/children/{childId}/daily-records` | `record_id`, timestamps, origen y estado de persistencia. |
| Crear borrador desde texto | `POST /v1/observation-drafts/text` | `draft_id`, texto fuente y variables propuestas. |
| Crear borrador desde voz | `POST /v1/observation-drafts/audio` | `draft_id`, transcripción y variables propuestas. |
| Corregir borrador | `PATCH /v1/observation-drafts/{draftId}` | Borrador actualizado sin persistir aún como registro confirmado. |
| Confirmar borrador | `POST /v1/observation-drafts/{draftId}/confirm` | Registro definitivo con procedencia `AI_EXTRACTED_HUMAN_CONFIRMED`. |
| Obtener recomendaciones | `GET /v1/children/{childId}/recommendations?context=HOME|SCHOOL` | Estrategias permitidas, fuente y evidencia histórica disponible. |
| Reportar desregulación | `POST /v1/children/{childId}/dysregulation-events` | `event_id`, timestamp y estado de persistencia/sincronización. |
| Registrar intervención | `POST /v1/children/{childId}/interventions` | `intervention_id`, estrategia, contexto y predicción asociada. |
| Registrar resultado | `POST /v1/interventions/{interventionId}/result` | Resultado `SUCCESS|PARTIAL|NO_EFFECT`. |
| Feedback de predicción | `POST /v1/risk-predictions/{predictionId}/feedback` | Confirmación de si ocurrió o no el evento en la ventana. |
| Resumen de aula | `GET /v1/classrooms/{classroomId}/students/preventive-status` | Lista filtrada de estudiantes autorizados y señales funcionales. |
| Resumen de pacientes | `GET /v1/professionals/me/patients/preventive-status` | Lista de pacientes con riesgo, confianza y desviación reciente. |
| Baseline individual | `GET /v1/children/{childId}/baseline` | Estado del baseline, días válidos, ventana y estadísticas resumidas. |
| Analítica reciente | `GET /v1/children/{childId}/analytics/recent?window=72h` | Tendencias, acumulaciones y comparaciones con baseline. |
| Línea temporal | `GET /v1/children/{childId}/timeline` | Registros ordenados con procedencia y filtrado por permisos. |
| Historial de intervenciones | `GET /v1/children/{childId}/interventions` | Estrategias aplicadas y resultados asociados. |
| Registrar sesión profesional | `POST /v1/children/{childId}/professional-sessions` | `session_id`, observación profesional y timestamps. |

### Servicios internos Backend ↔ Data/ML

| Servicio | Responsabilidad | Contrato lógico mínimo |
|---|---|---|
| `ObservationStructuringService` | Extraer variables desde texto/transcripción. | `structure(input, context) -> ObservationDraft` |
| `BaselineService` | Construir baseline individual según sección 32.6. | `getBaseline(childId, cutoffAt) -> BaselineSnapshot` |
| `FeatureEngineeringService` | Generar desviaciones, acumuladores y calidad de datos. | `buildFeatures(childId, cutoffAt) -> FeatureSnapshot` |
| `PredictionService` | Calcular riesgo + confianza sin generar texto clínico libre. | `predict(features, baseline) -> RiskPrediction` |
| `RecommendationRankingService` | Seleccionar estrategias desde fuentes controladas. | `rank(childId, predictionId, context) -> InterventionCandidate[]` |

### Regla de actualización

La confirmación de un `DailyRecord`, una observación relevante o un `DysregulationEvent` deberá publicar o disparar una solicitud equivalente a:

```text
RiskRecalculationRequested(child_id, data_cutoff_at)
```

El cálculo puede ser asincrónico. El guardado del registro no deberá bloquearse esperando al motor predictivo.

---

## 34.4 Matriz P0 — Historias transversales

| HU | RF relacionados | Pantalla(s) | API / servicio | Criterio de aceptación P0 |
|---|---|---|---|---|
| **HU-TR-01 — Acceso según rol** | RF-TR-01, RF-TR-02, RF-TR-03, RF-TR-07, RF-COL-04 | `AUTH-01`, todas las pantallas protegidas | `POST /v1/auth/session`, `GET /v1/me/context` | **AC-TR-01.1:** dada una sesión de familia, educación o especialista, el backend retorna el rol y solo los recursos autorizados.<br>**AC-TR-01.2:** una petición a información no autorizada responde `403` o elimina el campo según política; ocultar el componente solo en UI no se considera suficiente. |
| **HU-TR-02 — Selección clara del niño** | RF-TR-04, RF-TR-05 | `COMMON-01` | `GET /v1/children` | **AC-TR-02.1:** si el usuario tiene más de un niño autorizado puede cambiar el perfil activo y el nombre/identificador visible se actualiza antes de registrar o consultar.<br>**AC-TR-02.2:** todo `POST` de registro se envía con el `childId` del perfil actualmente confirmado. |
| **HU-TR-03 — Estado diario resumido** | RF-TR-06, RF-TR-07, RF-PRED-15 | `FAM-01`, `EDU-02`, `ESP-02` | `GET /v1/children/{childId}/preventive-status`; `PredictionService` | **AC-TR-03.1:** el estado muestra la predicción vigente más reciente o `INSUFFICIENT_DATA`, nunca reutiliza silenciosamente una predicción expirada.<br>**AC-TR-03.2:** después de confirmar un registro relevante, el backend dispara una reevaluación y la UI puede obtener la nueva versión. |
| **HU-TR-04 — Diferenciar riesgo y confianza** | RF-PRED-07, RF-PRED-08, RF-FAM-05, RF-ESP-08 | `COMMON-02`, `FAM-01`, `ESP-02` | `GET /v1/children/{childId}/risk-predictions/current`; `PredictionService` | **AC-TR-04.1:** la respuesta contiene campos independientes `risk_*` y `confidence_*`.<br>**AC-TR-04.2:** una confianza baja no modifica automáticamente la banda de riesgo; ambos valores se muestran por separado conforme a la sección 32. |
| **HU-TR-05 — Comprender los factores de la alerta** | RF-PRED-10, RF-PRED-11 | `COMMON-02`, `ESP-04` | `GET /v1/children/{childId}/risk-predictions/current`; `FeatureEngineeringService` | **AC-TR-05.1:** toda predicción `ACTIVE` con riesgo mostrado incluye al menos un factor explicativo y hasta los factores principales disponibles.<br>**AC-TR-05.2:** cada factor puede indicar tipo —desviación, acumulación o contexto— y la redacción visible se adapta al rol sin cambiar su significado. |
| **HU-TR-06 — Saber qué información falta** | RF-CAP-09, RF-PRED-09, RF-PRED-12 | `COMMON-02`, `COMMON-03`, `ESP-04` | `GET /v1/children/{childId}/risk-predictions/current`; `FeatureEngineeringService` | **AC-TR-06.1:** variables ausentes se devuelven en `missing_critical_data` o equivalente y nunca se codifican como estado normal.<br>**AC-TR-06.2:** si los faltantes reducen confianza, la UI explica cuáles afectan la estimación. |
| **HU-TR-07 — No recibir falsa precisión** | RF-PRED-13, RF-PRED-14 | `COMMON-03` | `GET /v1/children/{childId}/risk-predictions/current`; `PredictionService` | **AC-TR-07.1:** al cumplirse una condición de sección 32.4, `status=INSUFFICIENT_DATA` y no se entrega una categoría visible de riesgo.<br>**AC-TR-07.2:** la respuesta incluye el mínimo de datos prioritarios que permitiría intentar una nueva estimación. |

---

## 34.5 Matriz P0 — Familia / cuidador

| HU | RF relacionados | Pantalla(s) | API / servicio | Criterio de aceptación P0 |
|---|---|---|---|---|
| **HU-FAM-01 — Check-in diario breve** | RF-CAP-01, RF-FAM-01, RF-PRED-15 | `FAM-02` | `POST /v1/children/{childId}/daily-records`; `PredictionService` por evento | **AC-FAM-01.1:** el check-in permite registrar como mínimo sueño, estado al despertar y regulación/estado reciente sin formulario clínico extenso.<br>**AC-FAM-01.2:** al confirmar se crea un `DailyRecord`, se confirma persistencia y se solicita reevaluación de riesgo de forma asincrónica. |
| **HU-FAM-02 — Registro mediante lenguaje natural** | RF-CAP-02, RF-CAP-04, RF-CAP-05 | `CAP-01`, `CAP-02` | `POST /v1/observation-drafts/text`; `ObservationStructuringService` | **AC-FAM-02.1:** un texto libre produce un borrador con variables propuestas y conserva la observación fuente necesaria para revisión.<br>**AC-FAM-02.2:** crear el borrador no genera aún un `DailyRecord` confirmado. |
| **HU-FAM-03 — Registro mediante nota de voz** | RF-CAP-03, RF-CAP-04, RF-CAP-05 | `CAP-01`, `CAP-02` | `POST /v1/observation-drafts/audio`; `ObservationStructuringService` | **AC-FAM-03.1:** una nota de voz devuelve transcripción y variables propuestas antes de guardar.<br>**AC-FAM-03.2:** si falla la transcripción, el usuario puede volver a captura manual; el audio temporal sigue la política de sección 32.10. |
| **HU-FAM-04 — Confirmar interpretación de la IA** | RF-CAP-05, RF-CAP-06 | `CAP-02` | `POST /v1/observation-drafts/{draftId}/confirm` | **AC-FAM-04.1:** el usuario ve los campos extraídos antes de confirmar.<br>**AC-FAM-04.2:** solo al confirmar se crea el registro longitudinal con procedencia `AI_EXTRACTED_HUMAN_CONFIRMED`. |
| **HU-FAM-05 — Corregir una interpretación** | RF-CAP-07 | `CAP-02` | `PATCH /v1/observation-drafts/{draftId}` | **AC-FAM-05.1:** el usuario puede modificar un campo extraído sin reingresar la observación completa.<br>**AC-FAM-05.2:** la confirmación posterior persiste el valor corregido, no el valor inicial propuesto por IA. |
| **HU-FAM-07 — Ver el riesgo del día en lenguaje simple** | RF-FAM-02, RF-PRED-06, RF-SEG-01 | `FAM-01`, `COMMON-02` | `GET /v1/children/{childId}/preventive-status` | **AC-FAM-07.1:** familia visualiza principalmente `Bajo`, `Moderado` o `Elevado`, no una supuesta probabilidad clínica.<br>**AC-FAM-07.2:** se informa la ventana temporal vigente y se evita lenguaje diagnóstico. |
| **HU-FAM-08 — Entender qué cambió respecto de lo habitual** | RF-PRED-01, RF-PRED-02, RF-PRED-03, RF-FAM-03 | `FAM-01`, `COMMON-02` | `GET /v1/children/{childId}/preventive-status`; `BaselineService` | **AC-FAM-08.1:** cuando existe baseline, el estado identifica al menos una desviación relevante como “respecto de su patrón habitual”.<br>**AC-FAM-08.2:** si el baseline está en construcción, la UI no presenta la comparación individual como definitiva. |
| **HU-FAM-09 — Comprender acumulaciones** | RF-PRED-04, RF-FAM-04 | `FAM-01`, `COMMON-02` | `GET /v1/children/{childId}/preventive-status`; `FeatureEngineeringService` | **AC-FAM-09.1:** una señal persistente durante varios días puede aparecer como factor de tipo acumulación.<br>**AC-FAM-09.2:** la explicación incluye la ventana observada, por ejemplo “3 días consecutivos”, cuando ese dato esté disponible. |
| **HU-FAM-10 — Ver confianza y datos faltantes** | RF-FAM-05, RF-PRED-08, RF-PRED-12 | `FAM-01`, `COMMON-02`, `COMMON-03` | `GET /v1/children/{childId}/risk-predictions/current` | **AC-FAM-10.1:** confianza se muestra como `Baja`, `Media` o `Alta`, separada del riesgo.<br>**AC-FAM-10.2:** cuando la confianza no es alta se puede explicar qué faltantes o condiciones la reducen. |
| **HU-FAM-11 — Recibir acciones preventivas concretas** | RF-FAM-06, RF-SEG-04, RF-SEG-05 | `FAM-03` | `GET /v1/children/{childId}/recommendations?context=HOME`; `RecommendationRankingService` | **AC-FAM-11.1:** la vista presenta un conjunto reducido de estrategias aplicables a hogar.<br>**AC-FAM-11.2:** ninguna estrategia devuelta puede existir únicamente como texto generado por un LLM sin `strategy_id` de una fuente controlada. |
| **HU-FAM-12 — Reconocer recomendaciones personalizadas** | RF-FAM-07, RF-SEG-06 | `FAM-03` | `GET /v1/children/{childId}/recommendations?context=HOME` | **AC-FAM-12.1:** cada estrategia indica `source_type=PROFESSIONAL|HISTORICAL|VALIDATED_CATALOG`.<br>**AC-FAM-12.2:** si se afirma que una estrategia funcionó antes, existe al menos un `InterventionResult` asociado que respalda esa etiqueta. |
| **HU-FAM-14 — Confirmar si ocurrió una desregulación** | RF-FAM-09 | `FAM-04` | `POST /v1/risk-predictions/{predictionId}/feedback` | **AC-FAM-14.1:** una vez terminada o evaluable la ventana, el usuario puede registrar `dysregulation_occurred=true|false` vinculado a la predicción.<br>**AC-FAM-14.2:** el feedback queda asociado a la versión exacta de `RiskPrediction`. |
| **HU-FAM-15 — Registrar una estrategia aplicada** | RF-FAM-10 | `FAM-04` | `POST /v1/children/{childId}/interventions` | **AC-FAM-15.1:** el usuario puede seleccionar una estrategia permitida y registrar que fue aplicada.<br>**AC-FAM-15.2:** la intervención conserva `child_id`, `strategy_id`, contexto, timestamp y `prediction_id` cuando corresponda. |
| **HU-FAM-16 — Registrar el resultado de una estrategia** | RF-FAM-11 | `FAM-04` | `POST /v1/interventions/{interventionId}/result` | **AC-FAM-16.1:** el resultado admite exactamente las categorías mínimas `SUCCESS`, `PARTIAL` o `NO_EFFECT`.<br>**AC-FAM-16.2:** el resultado queda trazablemente relacionado con la intervención seleccionada. |

---

## 34.6 Matriz P0 — Profesor / educador

| HU | RF relacionados | Pantalla(s) | API / servicio | Criterio de aceptación P0 |
|---|---|---|---|---|
| **HU-EDU-01 — Resumen matutino del aula** | RF-EDU-01, RF-EDU-02 | `EDU-01` | `GET /v1/classrooms/{classroomId}/students/preventive-status` | **AC-EDU-01.1:** al abrir la vista el educador recibe solo estudiantes autorizados y su estado preventivo resumido.<br>**AC-EDU-01.2:** la información permite detectar qué casos requieren revisión sin abrir cada perfil. |
| **HU-EDU-02 — Priorizar alumnos que requieren atención** | RF-EDU-02 | `EDU-01` | `GET /v1/classrooms/{classroomId}/students/preventive-status` | **AC-EDU-02.1:** la lista distingue visual y textualmente los estudiantes con riesgo elevado o cambio relevante.<br>**AC-EDU-02.2:** la priorización no depende exclusivamente del color. |
| **HU-EDU-03 — Ver información funcional y no clínica** | RF-EDU-03, RF-TR-03, RF-COL-04 | `EDU-01`, `EDU-02` | Endpoints educativos con autorización y DTO por rol | **AC-EDU-03.1:** un educador no recibe notas clínicas detalladas, diagnósticos ni datos familiares no necesarios.<br>**AC-EDU-03.2:** el filtrado se realiza en Backend; inspeccionar la respuesta de red no revela los campos restringidos. |
| **HU-EDU-04 — Ver señales relevantes para el aula** | RF-EDU-04, RF-PRED-11 | `EDU-02` | `GET /v1/children/{childId}/preventive-status` con vista educativa | **AC-EDU-04.1:** el detalle presenta señales funcionales accionables para la jornada, por ejemplo cambio de rutina, regulación o alerta.<br>**AC-EDU-04.2:** las señales se expresan en contexto educativo sin exponer detalles privados innecesarios. |
| **HU-EDU-05 — Ver recomendaciones aplicables en escuela** | RF-EDU-05, RF-SEG-04 | `EDU-02` | `GET /v1/children/{childId}/recommendations?context=SCHOOL`; `RecommendationRankingService` | **AC-EDU-05.1:** solo se devuelven estrategias `SCHOOL` o `BOTH` autorizadas.<br>**AC-EDU-05.2:** la lista contiene pocas acciones concretas y ninguna intervención generada libremente. |
| **HU-EDU-06 — Reporte express de desregulación** | RF-EDU-06 | `EDU-03` | `POST /v1/children/{childId}/dysregulation-events` | **AC-EDU-06.1:** desde el estudiante activo el educador puede registrar una escalada/desregulación mediante el mínimo de campos obligatorios.<br>**AC-EDU-06.2:** la respuesta entrega un identificador local o remoto que permite confirmar que el intento de registro fue retenido. |
| **HU-EDU-07 — Reportar sin formulario obligatorio** | RF-EDU-07 | `EDU-03` | `POST /v1/children/{childId}/dysregulation-events` | **AC-EDU-07.1:** detalles narrativos, desencadenantes y estrategia no son obligatorios para crear el evento inicial.<br>**AC-EDU-07.2:** la API acepta el payload mínimo definido para el reporte express. |
| **HU-EDU-08 — Confirmación inmediata del reporte** | RF-EDU-08 | `EDU-03` | `POST /v1/children/{childId}/dysregulation-events` + capa de persistencia/sincronización móvil | **AC-EDU-08.1:** después del envío, la UI muestra de forma inequívoca `guardado/sincronizado` o `pendiente de sincronización` según el estado real.<br>**AC-EDU-08.2:** un error de red no se representa como éxito sincronizado. |
| **HU-EDU-09 — Nota rápida por voz** | RF-CAP-03, RF-CAP-04, RF-CAP-05 | `CAP-01`, `CAP-02` | `POST /v1/observation-drafts/audio`; `ObservationStructuringService` | **AC-EDU-09.1:** una nota de voz escolar genera un borrador estructurado con `context=SCHOOL`.<br>**AC-EDU-09.2:** el borrador no afecta el historial ni el riesgo hasta ser confirmado. |
| **HU-EDU-10 — Confirmar observación estructurada** | RF-CAP-05, RF-CAP-06, RF-CAP-07 | `CAP-02` | `PATCH /v1/observation-drafts/{draftId}`, `POST /v1/observation-drafts/{draftId}/confirm` | **AC-EDU-10.1:** el educador revisa/corrige la extracción antes de guardarla.<br>**AC-EDU-10.2:** el registro final conserva fuente escolar y procedencia de estructuración automática confirmada. |

---

## 34.7 Matriz P0 — Especialista / terapeuta / psicólogo

| HU | RF relacionados | Pantalla(s) | API / servicio | Criterio de aceptación P0 |
|---|---|---|---|---|
| **HU-ESP-01 — Lista de pacientes con estado reciente** | RF-ESP-01 | `ESP-01` | `GET /v1/professionals/me/patients/preventive-status` | **AC-ESP-01.1:** la lista contiene únicamente pacientes autorizados e indica estado preventivo reciente y vigencia.<br>**AC-ESP-01.2:** un paciente sin predicción vigente se muestra como tal, no con un estado antiguo presentado como actual. |
| **HU-ESP-02 — Priorizar por cambio respecto del baseline** | RF-ESP-02, RF-PRED-03 | `ESP-01` | `GET /v1/professionals/me/patients/preventive-status`; `BaselineService` | **AC-ESP-02.1:** la respuesta incluye una señal de desviación individual o estado del baseline que permite ordenar/identificar casos relevantes.<br>**AC-ESP-02.2:** no se sustituye silenciosamente un baseline insuficiente por una norma poblacional. |
| **HU-ESP-03 — Visualizar línea base individual** | RF-ESP-03, RF-PRED-01 | `ESP-03` | `GET /v1/children/{childId}/baseline`; `BaselineService` | **AC-ESP-03.1:** se muestran estado `BUILDING|PROVISIONAL|SUFFICIENT`, cantidad de días válidos y ventana de referencia.<br>**AC-ESP-03.2:** las estadísticas corresponden exclusivamente al historial individual del paciente. |
| **HU-ESP-04 — Comparar estado actual con baseline** | RF-ESP-04, RF-PRED-02, RF-PRED-03 | `ESP-03` | `GET /v1/children/{childId}/baseline`, `GET /v1/children/{childId}/analytics/recent?window=72h`; `FeatureEngineeringService` | **AC-ESP-04.1:** para variables disponibles se visualizan valor/patrón reciente y referencia individual en una misma comparación.<br>**AC-ESP-04.2:** los datos faltantes no se grafican ni etiquetan como normales. |
| **HU-ESP-05 — Revisar acumulaciones temporales** | RF-ESP-05, RF-PRED-04 | `ESP-03` | `GET /v1/children/{childId}/analytics/recent?window=72h`; `FeatureEngineeringService` | **AC-ESP-05.1:** el especialista puede identificar factores persistentes o tendencias durante la ventana reciente.<br>**AC-ESP-05.2:** cada acumulación reporta variable/factor, ventana y magnitud o conteo interpretable. |
| **HU-ESP-08 — Ver factores contribuyentes** | RF-ESP-07, RF-PRED-10 | `ESP-04` | `GET /v1/children/{childId}/risk-predictions/current` | **AC-ESP-08.1:** la predicción muestra factores ordenados por contribución/relevancia según el motor explicable usado.<br>**AC-ESP-08.2:** el especialista puede distinguir al menos desviación, acumulación y contexto cuando corresponda. |
| **HU-ESP-09 — Revisar nivel de confianza** | RF-ESP-08, RF-PRED-08, RF-PRED-09 | `ESP-02`, `ESP-04` | `GET /v1/children/{childId}/risk-predictions/current`; `PredictionService` | **AC-ESP-09.1:** se muestran `confidence_score` y `confidence_level` independientemente de `risk_score/risk_level`.<br>**AC-ESP-09.2:** el cálculo respeta la heurística versionada de sección 32.3 para el prototipo. |
| **HU-ESP-10 — Ver datos faltantes relevantes** | RF-ESP-09, RF-PRED-12 | `ESP-04` | `GET /v1/children/{childId}/risk-predictions/current` | **AC-ESP-10.1:** el especialista puede ver qué variables críticas o fuentes reducen la confianza.<br>**AC-ESP-10.2:** la UI diferencia `faltante`, `desactualizado` e `información insuficiente` cuando el backend disponga de esa clasificación. |
| **HU-ESP-13 — Consultar estrategias históricas** | RF-ESP-12 | `ESP-05` | `GET /v1/children/{childId}/interventions` | **AC-ESP-13.1:** se listan intervenciones históricas autorizadas con estrategia, fecha y contexto.<br>**AC-ESP-13.2:** la información puede vincularse a la predicción/evento relacionado cuando exista esa relación. |
| **HU-ESP-14 — Ver resultado de intervenciones anteriores** | RF-ESP-13 | `ESP-05` | `GET /v1/children/{childId}/interventions` | **AC-ESP-14.1:** cada intervención con feedback muestra su resultado `SUCCESS|PARTIAL|NO_EFFECT`.<br>**AC-ESP-14.2:** una estrategia no puede etiquetarse como históricamente exitosa si no existe resultado que lo respalde. |
| **HU-ESP-17 — Registrar observación de sesión** | RF-ESP-16 | `ESP-06` | `POST /v1/children/{childId}/professional-sessions` | **AC-ESP-17.1:** el especialista puede registrar una observación asociada a una sesión con timestamp y autor/contexto profesional.<br>**AC-ESP-17.2:** la sesión aparece posteriormente en el historial longitudinal según permisos y puede disparar reevaluación si contiene variables relevantes. |

---

## 34.8 Matriz P0 — Coordinación y privacidad

| HU | RF relacionados | Pantalla(s) | API / servicio | Criterio de aceptación P0 |
|---|---|---|---|---|
| **HU-COL-04 — Mantener privacidad por contexto** | RF-TR-03, RF-COL-04, RNF-SEC-02, RNF-SEC-05, RNF-SEC-06 | Todas las vistas por rol | Middleware/política de autorización en todos los endpoints | **AC-COL-04.1:** las respuestas se filtran en Backend según rol, niño y contexto autorizado.<br>**AC-COL-04.2:** educación no puede obtener notas clínicas detalladas mediante acceso directo al endpoint aunque conozca un `childId` válido.<br>**AC-COL-04.3:** los campos restringidos no viajan al cliente para ser ocultados posteriormente. |

### Capacidad P0-MVP simplificada — línea temporal compartida

Aunque `HU-TR-08` es P1 en la definición original, el corte vigente del MVP requiere una demostración mínima de longitudinalidad.

| HU | RF relacionados | Pantalla(s) | API / servicio | Criterio de aceptación P0-MVP |
|---|---|---|---|---|
| **HU-TR-08 — Línea temporal compartida (implementación simplificada)** | RF-TR-08, RF-COL-01, RF-COL-04 | `COMMON-04` | `GET /v1/children/{childId}/timeline` | **AC-TR-08-MVP.1:** la pantalla muestra registros relevantes ordenados cronológicamente.<br>**AC-TR-08-MVP.2:** cada elemento conserva timestamp y fuente/contexto permitidos.<br>**AC-TR-08-MVP.3:** la API filtra elementos/campos no autorizados antes de responder. |

---

## 34.9 Matriz P0 — Seguridad de comunicación e IA

| HU | RF relacionados | Pantalla(s) | API / servicio | Criterio de aceptación P0 |
|---|---|---|---|---|
| **HU-SEG-01 — Evitar lenguaje diagnóstico** | RF-SEG-01, RF-FAM-02 | `COMMON-02`, `FAM-01`, `EDU-02`, `ESP-02` | DTO de predicción + capa de presentación | **AC-SEG-01.1:** la UI utiliza expresiones como “riesgo estimado”, “señales compatibles” o “requiere atención preventiva”.<br>**AC-SEG-01.2:** no se presenta el score como diagnóstico, certeza de ocurrencia ni probabilidad clínica validada. |
| **HU-SEG-02 — Diferenciar dato observado de inferencia** | RF-SEG-02, RF-CAP-08 | `COMMON-04`, `COMMON-02`, `ESP-04` | Timeline/predicción con metadatos de procedencia | **AC-SEG-02.1:** un registro humano, una extracción de IA confirmada y una inferencia del motor tienen `origin_type` diferenciable.<br>**AC-SEG-02.2:** la UI no presenta una inferencia como si hubiera sido una observación directa de familia, escuela o profesional. |
| **HU-SEG-03 — Explicar una confianza baja** | RF-SEG-03, RF-PRED-09, RF-PRED-12 | `COMMON-02`, `COMMON-03`, `ESP-04` | `GET /v1/children/{childId}/risk-predictions/current`; `PredictionService` | **AC-SEG-03.1:** toda confianza `LOW` incluye al menos una razón visible o `missing_critical_data` que justifique su nivel.<br>**AC-SEG-03.2:** si confianza < 40 se aplica `INSUFFICIENT_DATA` conforme a sección 32.4. |
| **HU-SEG-04 — Evitar recomendaciones no validadas** | RF-SEG-04, RF-SEG-05, RF-SEG-06 | `FAM-03`, `EDU-02`, `ESP-05` | `GET /v1/children/{childId}/recommendations`; `RecommendationRankingService` | **AC-SEG-04.1:** toda recomendación tiene `strategy_id`, `source_type`, `context` y `active=true`.<br>**AC-SEG-04.2:** el servicio rechaza o excluye recomendaciones fuera del catálogo/registro autorizado.<br>**AC-SEG-04.3:** si un LLM adapta la redacción, no modifica la identidad ni el contenido operativo de la estrategia seleccionada. |

---

## 34.10 Dependencias P0 entre equipos

La matriz anterior implica el siguiente orden de dependencia para evitar que un equipo bloquee a los demás.

| Orden | Entregable | Responsable primario | Dependientes |
|---:|---|---|---|
| 1 | DTOs y fixtures de `Child`, `DailyRecord`, `RiskPrediction`, `Intervention` y `InterventionResult` | Backend + Data/ML | Frontend |
| 2 | Contrato `RiskPrediction` con estados, riesgo, confianza, factores y faltantes | Backend + Data/ML | Todas las vistas de estado |
| 3 | `BaselineService` + features sintéticas reproducibles | Data/ML | Backend, especialista, familia |
| 4 | Flujo `ObservationDraft → confirmación → DailyRecord` | Backend + IA/NLP | Familia, educación |
| 5 | Endpoints de lectura con DTO filtrado por rol | Backend | Frontend móvil |
| 6 | Pantallas P0 conectadas primero a fixtures y luego a API | Frontend móvil | Demo integrada |
| 7 | `RecommendationRankingService` con catálogo controlado | Backend + producto/Data | Familia, educación, especialista |
| 8 | Feedback `Prediction → Intervention → InterventionResult` | Backend | Familia, Data/ML futuro |
| 9 | Integración end-to-end de los escenarios A–E de sección 33.5 | Todos | Pitch/demo |

---

## 34.11 Contrato mínimo de `RiskPrediction` consumido por Frontend

Para evitar que Frontend, Backend y Data/ML construyan representaciones incompatibles, el MVP deberá exponer conceptualmente un objeto equivalente a:

```json
{
  "prediction_id": "rp_123",
  "child_id": "child_001",
  "status": "ACTIVE",
  "generated_at": "2026-08-26T18:20:00-04:00",
  "prediction_start_at": "2026-08-26T18:20:00-04:00",
  "prediction_end_at": "2026-08-27T18:20:00-04:00",
  "risk_score": 74,
  "risk_level": "ELEVATED",
  "confidence_score": 67,
  "confidence_level": "MEDIUM",
  "main_factors": [
    {
      "code": "SLEEP_ACCUMULATION",
      "type": "ACCUMULATION",
      "label": "Sueño alterado durante 3 días",
      "window_days": 3
    }
  ],
  "missing_critical_data": [
    {
      "code": "SCHOOL_REGULATION_TODAY",
      "label": "No existe registro escolar de regulación de hoy"
    }
  ],
  "data_cutoff_at": "2026-08-26T18:15:00-04:00",
  "model_or_rules_version": "mvp-rules-1.0",
  "feature_schema_version": "features-1.0"
}
```

### Reglas del contrato

1. `risk_score` es un **índice interno**, no una probabilidad clínica.
2. En `INSUFFICIENT_DATA`, `risk_score` y `risk_level` deberán ser `null` o no exponerse como resultado válido.
3. `main_factors` deberá derivar del snapshot usado para esa misma predicción.
4. `missing_critical_data` nunca deberá representar faltantes como valores normales.
5. La API puede ocultar el score numérico a familia/educación mediante DTO por rol, conservándolo internamente.
6. `model_or_rules_version` y `feature_schema_version` son obligatorios para trazabilidad del prototipo.

---

## 34.12 Contrato mínimo de borrador de observación

```json
{
  "draft_id": "draft_001",
  "child_id": "child_001",
  "context": "HOME",
  "input_type": "TEXT",
  "source_text": "Anoche durmió mal y hoy despertó irritable",
  "proposed_variables": [
    {
      "field": "sleep_quality",
      "value": "ALTERED",
      "evidence": "durmió mal"
    },
    {
      "field": "wake_state",
      "value": "IRRITABLE",
      "evidence": "despertó irritable"
    }
  ],
  "status": "PENDING_CONFIRMATION"
}
```

### Regla de persistencia

```text
ObservationDraft
      ≠
DailyRecord confirmado
```

Un borrador no puede alimentar el motor de riesgo como dato confirmado hasta que el usuario complete `POST /v1/observation-drafts/{draftId}/confirm`.

---

## 34.13 Criterio de integración terminado

La implementación P0 puede considerarse alineada cuando se demuestre el siguiente recorrido utilizando los mismos contratos en las tres capas:

```text
1. Familia confirma un check-in o una observación estructurada
                    ↓
2. Backend persiste DailyRecord y solicita recálculo
                    ↓
3. Data/ML calcula baseline/features + RiskPrediction versionada
                    ↓
4. Backend expone DTO de riesgo + confianza + factores + faltantes
                    ↓
5. Frontend presenta contenido adaptado al rol
                    ↓
6. RecommendationRankingService devuelve estrategia controlada
                    ↓
7. Usuario registra intervención y resultado
                    ↓
8. Feedback queda asociado a la predicción correspondiente
```

Para la demo, este recorrido deberá poder ejecutarse al menos para:

- un día estable;
- un deterioro gradual;
- riesgo con información incompleta;
- información insuficiente;
- intervención con feedback posterior.

Estos son los mismos escenarios definidos en la sección 33.5 y constituyen la base recomendada para los tests end-to-end del MVP.

