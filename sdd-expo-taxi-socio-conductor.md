# SDD · Activación Socio Conductor en Expo Taxi

> **Tipo de documento:** Spec-Driven Design (especificación antes de ejecutar)
> **Fuentes:** Reporte de observación Expo Taxi · Día 1 (viernes 25 de setiembre de 2026) + 2 notas de voz del observador (Día 1)
> **Estado:** Borrador v0.2 · el Día 1 se toma como **línea base**
> **Decisión actual:** **Replicar con ajustes** (puntaje 58/100)

---

## 0. Resumen ejecutivo

El stand de Socio Conductor en Expo Taxi llega a conductores que **no alcanzamos por otros canales** y el stand se ve bien (4.2/5). Aun así, el Día 1 convirtió poco: **15% de los que pasan se registran**, **nadie conocía la marca** y el costo por registro es **S/ 100**.

Las causas principales que se observaron fueron tres:

1. **Registro con fricción:** el QR no reconocía el número y se derivaba al conductor a WhatsApp a mitad del proceso. El conductor sintió que lo "paseaban", se demoraba y varios se fueron.
2. **Promotores con poco dominio del producto** (3.3/5): cierran poco y no transmiten seguridad.
3. **Stand pasivo y sin dinámica:** poca integración con el público. Cuando llegaron dos promotoras comerciales más, el enganche mejoró.

Esta especificación define **qué cambiar para los Días 2 y 3**, **cómo medirlo** y **qué criterios usar** para decidir si se hace un piloto en futuras ferias.

---

## 1. Contexto

### 1.1 El evento

| Dato | Valor |
|---|---|
| Evento | Expo Taxi |
| Duración | 3 días |
| Entrada | Libre |
| Stands | 6 |
| Asistentes Día 1 (organizador) | 5,000 |
| Asistentes Día 1 (estimado propio) | 3,000 |
| % conductores sobre el total | ~80% |
| Inicio del programa oficial | 14:00 |

### 1.2 Segmentos de público observados

| Segmento | Descripción | Peso observado |
|---|---|---|
| **Solo** | Conductor que viene a preguntar e informarse | Mayoría |
| **Grupo** | Conductores que vienen juntos | Medio |
| **Familia** | Conductor que viene con su familia | Muy bajo (1 caso en 3 h) |
| **Transeúnte** | No viene al evento; pasa, siente curiosidad y camina | Bajo, no es público objetivo |

### 1.3 Dinámica del evento (Día 1, mañana)

- Ambiente poco integrador: música de fondo, cada stand por su lado.
- Promotoras del organizador llamaban a los conductores, pero sin conectar con los stands.
- Era el día de inauguración y la primera hora tuvo **muy poca gente y poca visibilidad**. Se espera que el flujo suba a medida que avanza el día.
- El programa oficial empieza a las 14:00. Las observaciones de la mañana **no representan la hora pico del evento**.
- **Infraestructura:** el recinto no tiene techo. Con calor o lluvia, el evento se ve muy afectado (ver §12).

### 1.4 Difusión digital del evento

- En la búsqueda previa **no se encontró publicidad ni señales de recurrencia** en la página web: pocos seguidores.
- La cuenta asociada al juego de fútbol tiene algo más de 1,000 seguidores, pero con **muy poca interacción** (likes).
- **Implicancia:** el evento no trae una comunidad digital propia. El tráfico depende de lo presencial y la recordación de marca tiene que generarla el stand.

---

## 2. Línea base (Día 1)

### 2.1 Embudo del stand

Conteo propio: 10 min por hora, en 4 horas (11:00, 12:00, 13:00 y 14:00). Son 40 min de muestra.

| Paso | Personas | Conversión vs. paso anterior | Conversión vs. total |
|---|---|---|---|
| Pasan frente al stand | 53 | — | 100% |
| Se detienen / miran | 21 | 40% | 40% |
| Conversan con un promotor | 21 | 100% | 40% |
| **Se registran / dejan datos** | **8** | **38%** | **15%** |

- **Hora pico:** 12:00 (~174 personas/hora pasan, estimado).
- **Cuello de botella:** entre **conversar y registrarse** (se pierde el 62%). Todo el que se detiene conversa, así que atraer no es el problema. El problema es cerrar.

### 2.2 Recurrencia y marca

| Indicador | Valor |
|---|---|
| Ya conocían Socio Conductor | 0 (0%) |
| Volvieron al stand en el día | 0 |
| Dicen que volverán a la expo (entrevistas) | 0% |

### 2.3 Stand y promotores

| Elemento | Puntaje |
|---|---|
| Stand | 4.2 / 5 |
| Promotores | 3.3 / 5 |
| Abordajes en pasillo contados | 0 |

- **Fortalezas:** visibilidad, ubicación, mensaje entendible en 5 segundos, material, proactividad, energía sostenida, imagen.
- **Debilidades:** conocimiento del producto y cierre.
- **Promotor al inicio:** se mostró tímido y pedía apoyo. Le faltaba respaldo para abordar solo.
- **Promotora freelance:** sabía lo básico, pero su conocimiento era superficial. Le faltaba profundidad para resolver dudas.
- **Refuerzo en la tarde:** con las dos ayudantes que llegaron en la tarde se espera que suban los registros.
- **Seguridad:** faltó transmitir seguridad al conductor, sobre todo cuando el registro fallaba.

### 2.4 Voz del conductor (n = 2, muestra muy pequeña)

| Dato | Valor |
|---|---|
| Perfil | Taxi formal (2) |
| Vehículo | Alquilado (1), propio (1) |
| Conocían la marca | 0% |
| Se registraron | 50% |
| Problemas mencionados | Financiamiento; "ninguno" |
| Cita | *"Grifo en la esquina"* (taxi formal) |

### 2.5 Competencia y otros stands

| Marca | Afluencia | Qué ofrece | Gancho |
|---|---|---|---|
| Mobil | Poca | Aceites | Juego de fútbol |
| Calia *(confirmar nombre)* | Sin dato | Sin dato | Muchos banners en la **entrada** del evento |

Observaciones generales:
- Los stands más llamativos tenían **autos en exhibición** y más material publicitario.
- Varios stands **compraron más espacio**, lo que les da más visibilidad y más cosas que mostrar. El nuestro tenía un solo elemento.
- Los stands **vacíos** tenían personal **sentado esperando** y no salían a buscar al conductor.
- Traer autos es parte de la marca de la competencia. **No encaja con Socio Conductor**, así que no lo copiaremos.
- **Juegos de fútbol:** son el gancho más usado (tiro al arco para ganar premio) y le atraen al público adulto. Valida la dinámica de RF-02.
- **Banners:** hay muchos y sirven para distinguir cada marca. Calia ocupa la **entrada**, que es el punto de mayor visibilidad: todo el que entra ve su marca primero.
- **Modelos de autos en exhibición:** además de publicidad, le sirven al conductor para conocer los modelos.

### 2.6 Costo

| Concepto | Valor |
|---|---|
| Costo total del stand | S/ 20,050 |
| Registros (dato del stand) | 200 |
| **Costo por registro (CPR)** | **≈ S/ 100** |

---

## 3. Problema a resolver

> **Conductores que no conocen Socio Conductor sí se acercan y conversan, pero la mayoría no llega a registrarse** porque el registro tiene fricción, el promotor no cierra con seguridad y el stand no da una razón inmediata para dejar los datos.

---

## 4. Objetivos

| # | Objetivo | Métrica | Línea base (D1) | Meta D2–D3 |
|---|---|---|---|---|
| O1 | Aumentar el cierre | Registro / Conversan | 38% | **≥ 60%** |
| O2 | Aumentar la conversión total | Registro / Pasan | 15% | **≥ 25%** |
| O3 | Atraer más tráfico al stand | Se detienen / Pasan | 40% | **≥ 50%** |
| O4 | Bajar el costo por registro | CPR | S/ 100 | **≤ S/ 70** |
| O5 | Generar recordación | Abordajes en pasillo / regresos | 0 / 0 | **> 0** (medir) |
| O6 | Validar calidad del registro | % activados a 7 y 30 días | Sin dato | Obtener el dato |

> Las metas son hipótesis iniciales y se recalibran al cierre del Día 2.

---

## 5. Alcance

### 5.1 Dentro del alcance
- Cambiar el flujo de registro en el stand.
- Poner una dinámica con premio **condicionada al registro**.
- Preparar a los promotores (guion, preguntas frecuentes y cierre).
- Hacer abordaje activo en pasillo.
- Medir con el mismo método del Día 1 para poder comparar.

### 5.2 Fuera del alcance
- Exhibir autos (no encaja con la marca).
- Comprar más espacio en esta edición.
- Cambios en la app de Socio Conductor.

---

## 6. Requisitos funcionales

### RF-01 · Registro en un solo canal
- **Qué:** el conductor se registra por **un único canal fijo** desde el inicio (app **o** WhatsApp, no los dos).
- **Por qué:** en el Día 1, el cambio de app a WhatsApp desanimó a los conductores y varios se fueron.
- **Criterios de aceptación:**
  - [ ] Se elige un solo canal antes de abrir el stand.
  - [ ] El registro toma **≤ 2 minutos**.
  - [ ] Hay un QR visible y un respaldo (tablet del promotor) por si el conductor no tiene datos o batería.
  - [ ] **El QR se prueba antes de abrir** con al menos 3 celulares distintos. En el Día 1 no reconocía el número y obligaba a derivar a otro canal.
  - [ ] Si el QR falla, el promotor registra al conductor en la tablet en ese mismo momento, sin mandarlo a otro canal.
  - [ ] Ningún promotor deriva a otro canal a mitad del registro.

### RF-02 · Dinámica con premio condicionada al registro
- **Qué:** ruleta o juego de fútbol. **Primero se registra y después juega.** Todos reciben un premio y el que gana se lleva uno mayor.
- **Por qué:** la dinámica atrae y el registro previo asegura que cada participante deje sus datos.
- **Criterios de aceptación:**
  - [ ] Solo juega quien muestra su registro completado.
  - [ ] Hay premio de consuelo para todos y premio mayor para el que gana.
  - [ ] Se registra en una planilla cuántos jugaron y cuántos ganaron.
  - [ ] El stock de premios alcanza para la hora pico (≥ 30 por hora).

### RF-03 · Guion del promotor y cierre
- **Qué:** un guion corto con propuesta de valor, respuestas a las 5 preguntas más frecuentes y una frase de cierre.
- **Por qué:** los promotores tienen 3.3/5 y fallan en conocimiento y cierre. El conductor necesita información de fondo, no superficial.
- **Criterios de aceptación:**
  - [ ] Cada promotor explica el producto en ≤ 30 segundos.
  - [ ] Cada promotor responde las preguntas sobre **financiamiento** y **auto alquilado vs. propio**.
  - [ ] Siempre cierra con una invitación explícita a registrarse.
  - [ ] Hay una hoja de preguntas frecuentes impresa en el stand.
  - [ ] **Los freelance reciben el mismo briefing** que el personal fijo y lo aprueban con un mini quiz de 5 preguntas.
  - [ ] Nunca hay un promotor solo en el stand durante la primera hora. Siempre tiene apoyo de otro promotor o del líder de stand.

### RF-04 · Abordaje activo en pasillo
- **Qué:** al menos un promotor sale al pasillo a invitar a los conductores. Nadie se queda sentado esperando.
- **Por qué:** los stands pasivos se quedaron vacíos. En el Día 1 se contaron 0 abordajes y el enganche mejoró cuando llegaron las promotoras comerciales.
- **Criterios de aceptación:**
  - [ ] Hay rotación: 1 promotor en pasillo y el resto en el stand.
  - [ ] Se cuentan los abordajes en pasillo por hora.

### RF-05 · Segmentación rápida del conductor
- **Qué:** el promotor pregunta si el auto es **alquilado o propio** y si es **taxi formal o de aplicativo**, y adapta el mensaje.
- **Criterios de aceptación:**
  - [ ] El formulario de registro incluye el tipo de vehículo y el perfil.
  - [ ] Hay un mensaje distinto para cada segmento (mínimo 2 variantes).

### RF-06 · Ambiente del stand
- **Qué:** música y animación propias en la hora pico, y material más visible.
- **Criterios de aceptación:**
  - [ ] Hay una dinámica activa entre 12:00 y 16:00.
  - [ ] Hay al menos un elemento visual nuevo (banner o pantalla con la propuesta de valor).

---

## 7. Requisitos no funcionales

| ID | Requisito |
|---|---|
| RNF-01 | **Comparabilidad:** mismo método de conteo que en el Día 1 (10 min por hora, mismas horas y más horas de la tarde). |
| RNF-02 | **Trazabilidad:** cada registro queda marcado con la fuente "Expo Taxi – Día N" para medir la activación después. |
| RNF-03 | **Simplicidad:** ningún paso del registro requiere descargar algo en el momento si la conexión es mala. |
| RNF-04 | **Consistencia de marca:** nada que no encaje con Socio Conductor (por ejemplo, exhibir autos). |
| RNF-05 | **Privacidad:** se piden solo los datos mínimos (nombre, teléfono, tipo de vehículo) y el consentimiento es explícito. |

---

## 8. Diseño del flujo (to-be)

```
[Pasillo]
   │  Promotor en pasillo aborda (RF-04)
   ▼
[Se detiene en el stand]
   │  Mensaje en 5 s + dinámica visible (RF-06)
   ▼
[Conversa con el promotor]
   │  Segmenta: alquilado/propio, formal/app (RF-05)
   │  Propuesta de valor + FAQ (RF-03)
   ▼
[Registro en UN solo canal]  ← QR o tablet, ≤ 2 min (RF-01)
   │
   ▼
[Juega ruleta / fútbol]  ← solo con registro (RF-02)
   │
   ▼
[Premio + recordatorio de siguiente paso]
   │
   ▼
[Seguimiento 7 y 30 días] → tasa de activación (O6)
```

**Diferencia con el Día 1:** antes el flujo era *conversar → intentar registro en la app → pasar a WhatsApp → abandono*. Ahora el registro es un solo paso y va antes del premio.

---

## 9. Plan de medición

| Métrica | Cómo se mide | Quién | Cuándo |
|---|---|---|---|
| Pasan / se detienen / conversan / registran | Conteo manual de 10 min por hora | Observador | 11:00 a 17:00 |
| Abordajes en pasillo | Conteo manual | Observador | Cada hora |
| Participantes en la dinámica | Planilla de premios | Promotor | Todo el día |
| Registros totales | Sistema (fuente "Expo Taxi") | Socio Conductor | Cierre del día |
| Conocían la marca | Pregunta en la conversación | Promotor | Cada conversación |
| Voz del conductor | Entrevistas cortas (meta ≥ 8 por día) | Observador | Todo el día |
| Activación de registros | Base de datos | Socio Conductor | A los 7 y 30 días |
| CPR | Costo del día / registros del día | Analista | Cierre del evento |

> **Nota de muestra:** el Día 1 tiene solo 2 entrevistas y 40 min de conteo. Para los Días 2 y 3 hay que subir a **≥ 8 entrevistas por día** y agregar **horas de la tarde** (después de las 14:00, cuando empieza el programa oficial).

---

## 10. Criterios de decisión: ¿lo replicamos?

### 10.1 Scorecard del Día 1

| Criterio | Peso | Puntaje D1 | Ponderado |
|---|---|---|---|
| Público objetivo real | 25% | 2/5 | 0.50 |
| Calidad de interacción | 20% | 3/5 | 0.60 |
| Volumen de tráfico | 15% | 3/5 | 0.45 |
| Costo razonable | 15% | 3/5 | 0.45 |
| Encaje con nuestra marca | 10% | 4/5 | 0.40 |
| Capacidad operativa | 10% | 3/5 | 0.30 |
| Medible | 5% | 4/5 | 0.20 |
| **Total** | **100%** | — | **2.90 / 5 ≈ 58/100** |

### 10.2 Reglas de decisión (al cierre del evento)

| Resultado | Decisión |
|---|---|
| ≥ 70/100 **y** CPR ≤ S/ 70 **y** activación a 30 días aceptable | **Replicar** en la próxima feria como piloto formal |
| 50–69/100 **o** CPR entre S/ 70 y S/ 100 | **Replicar con ajustes** y otra prueba controlada |
| < 50/100 **o** CPR > S/ 100 con baja activación | **No replicar**; buscar otros canales |

---

## 11. Plan de tareas (Días 2 y 3)

| # | Tarea | Requisito | Responsable | Plazo |
|---|---|---|---|---|
| T1 | Definir el canal único de registro y generar el QR | RF-01 | Coordinación | Antes de abrir el D2 |
| T2 | Preparar la ruleta o juego y el stock de premios | RF-02 | Marketing | Antes de abrir el D2 |
| T3 | Escribir el guion y las FAQ; briefing de 15 min a promotores | RF-03 | Coordinación | Antes de abrir el D2 |
| T4 | Asignar la rotación de promotor en pasillo | RF-04 | Líder de stand | D2, 10:00 |
| T5 | Agregar el tipo de vehículo y perfil al formulario | RF-05 | Producto / Ops | Antes de abrir el D2 |
| T6 | Armar la playlist o animación y conseguir un banner extra | RF-06 | Marketing | D2 |
| T7 | Conteo del embudo con el mismo método y horas de la tarde | RNF-01 | Observador | D2 y D3 |
| T8 | Hacer ≥ 8 entrevistas por día | §9 | Observador | D2 y D3 |
| T9 | Pedir la tasa de activación a 7 y 30 días | O6 | Analista | +7 y +30 días |
| T10 | Consolidar los datos de D1, D2 y D3 y recalcular el scorecard | §10 | Analista | Cierre del evento |
| T11 | Probar el QR de registro en 3 celulares y corregir el reconocimiento del número | RF-01 | Producto / Ops | Antes de abrir el D2 |
| T12 | Visitar el stand de Calia como benchmark (banners, ubicación, gancho) | §2.5 | Observador | D2 |

---

## 12. Riesgos y mitigaciones

| Riesgo | Impacto | Mitigación |
|---|---|---|
| Registros "por el premio" que no se activan | Registros de baja calidad | Medir la activación a 7 y 30 días; condicionar el premio mayor a un paso extra (por ejemplo, validar el teléfono) |
| Se acaban los premios en la hora pico | Frustración y mala imagen | Stock para ≥ 30 por hora y reposición a mediodía |
| Falla de conexión en el stand | Registro bloqueado | Tablet con formulario offline y carga posterior |
| Promotores sin dominio del producto | Bajo cierre | Briefing, FAQ impresa y supervisión la primera hora |
| Muestra pequeña (D1) | Conclusiones débiles | Más entrevistas y horas de conteo |
| Asistencia real menor a la declarada (3,000 vs. 5,000) | Volumen sobreestimado | Usar el estimado propio para proyecciones |
| Calor o lluvia (el recinto no tiene techo) | Baja asistencia y menos permanencia en el stand | Toldo o sombra propia en el stand; agua para los conductores |
| El QR no reconoce el número | Abandono del registro | Probar antes de abrir (T11) y tener la tablet de respaldo |
| Promotor tímido o solo en la primera hora | Pocos abordajes | Siempre en pareja; el líder de stand arranca los primeros abordajes |

---

## 13. Hallazgos cualitativos clave (insights)

1. **El refuerzo comercial funciona:** al inicio el promotor estaba tímido y pedía apoyo. Cuando llegaron las dos promotoras comerciales, el conductor se enganchó más.
2. **La fricción del registro mata la conversión:** el QR no reconocía el número, se derivaba a WhatsApp y el conductor se demoraba. Se sintió como "pasearlo" y varios se fueron.
3. **Canal diferencial:** la expo trae conductores que **no alcanzamos por otros canales**, incluidos los que tienen **auto alquilado**. Por eso vale la pena seguir experimentando.
4. **Dinámica + registro:** la ruleta o el juego de fútbol deben ir **después** del registro, nunca antes.
5. **Seguridad y profundidad:** el conductor pide información **concreta y confiable**, no superficial. La promotora freelance sabía lo básico, pero no a profundidad. Esto afecta directamente la decisión del conductor.
6. **No copiar lo que no es marca:** traer autos le funciona a la competencia, pero no encaja con Socio Conductor.
7. **La entrada es el mejor espacio:** Calia se posiciona con banners en la entrada del evento. Es un punto a evaluar para la próxima edición.
8. **El evento no tiene comunidad digital:** poca publicidad y baja interacción en redes. La recordación depende del stand.

---

## 14. Preguntas abiertas

- [ ] ¿Qué canal de registro tiene mejor activación posterior: app o WhatsApp?
- [ ] ¿De dónde salen los 200 registros del stand si el conteo propio vio 8 en 40 min? Hay que validar el método y la fuente.
- [ ] ¿Qué porcentaje de los registros del evento se activa a 7 y 30 días?
- [ ] ¿Cuál es el CPR máximo aceptable frente a otros canales (volanteo, digital)?
- [ ] ¿Vale la pena comprar más espacio en la próxima edición?
- [ ] ¿Cómo cambia el tráfico después de las 14:00 (programa oficial)?
- [ ] ¿"Calia" es el nombre correcto de la marca con banners en la entrada? ¿Qué ofrece y cuánta afluencia tiene?
- [ ] ¿Cuánto cuesta un espacio o banner en la entrada del evento?
- [ ] ¿Los pocos seguidores y la baja interacción son de la página del evento o de otra marca? Confirmar la fuente.

---

## 15. Próximos pasos

1. Comparar estos números con los Días 2 y 3 (el Día 1 es la línea base).
2. Pedir a Socio Conductor cuántos registros del evento se activan a 7 y 30 días.
3. Definir si se hace un piloto y con qué meta de costo por registro (propuesta: **≤ S/ 70**).
