# Plataforma de priorización de dolores: prompt y código para Lovable

Orden de uso:
1. Pega el **Prompt principal** (sección 1) en Lovable.
2. Cuando Lovable pida conectar Supabase, conéctalo y ejecuta el **SQL** (sección 2) en el SQL Editor.
3. Pega el **Prompt de datos** (sección 3) para cargar los 30 dolores de ejemplo.
4. Adjunta `mockup/logo-calidda.webp` (blanco, para el acceso) y `mockup/logo-calidda-turquesa.png` (para las pantallas con fondo blanco) en el chat de Lovable junto al prompt principal.
5. Compara con el boceto en `mockup/priorizacion-dolores.html`.

---

## 1. Prompt principal (pegar en Lovable)

```
Crea una aplicación web en español llamada "Priorización de dolores", con React + TypeScript + Tailwind y Supabase (base de datos + Realtime). Es para que unas 30 personas elijan en tiempo real los dolores más importantes de la compañía.

DISEÑO
- Fondo turquesa corporativo de Cálidda (#32C5DB) SOLO en la pantalla de acceso. Una vez que la persona ingresa (selección de dolores y ranking en vivo), el fondo de toda la página pasa a BLANCO (#FFFFFF). En esas pantallas las tarjetas, filas y la barra inferior son blancas con borde fino #BFE3EC (sin sombra), la zona de resumen usa fondo #E8F7FA y el logo de Cálidda del pie se muestra en turquesa (#32C5DB) en vez de blanco (te adjunto las dos versiones). En la pantalla de acceso las tarjetas son blancas con sombra suave y esquinas redondeadas; tarjetas blancas con sombra suave y esquinas redondeadas.
- La tarjeta "Participación completada" en las pantallas con fondo blanco lleva un borde grueso de 3 px en azul petróleo (#0B6E99) y una sombra suave azulada, para que destaque sobre el resto.
- Arriba, en lugar del logo, va el título "Sesión de Estrategia" (en blanco y grande sobre la tarjeta de acceso; en pequeño, mayúsculas y azul muy oscuro en la pantalla de la aplicación).
- El logo de Cálidda (te lo adjunto como imagen, es blanco con fondo transparente) va centrado en la parte INFERIOR de ambas pantallas, a unos 150 px de ancho. Guárdalo como asset del proyecto (src/assets/logo-calidda.png).
- Color de acción: azul petróleo (#0B6E99, hover #07556F) para botones, pestañas activas y casillas. Textos sobre el fondo turquesa en azul muy oscuro (#04252F).
- Los 8 dolores con más puntos se resaltan en VERDE (#1FA463 con fondo #E1F5EA). Los demás quedan en blanco con borde gris azulado, sin resaltar.
- Tipografías: "Outfit" para títulos y "Figtree" para texto. 
- Debe verse bien en celular y en computadora.

PANTALLA 1: ACCESO (sin registro)
- Un formulario con "Nombre y apellido" y "Cargo". Ambos obligatorios (el nombre debe tener al menos dos palabras).
- Botón "Ingresar" (también funciona con Enter).
- No hay cuentas ni contraseñas. Al ingresar, crea una fila en la tabla `participantes` y guarda su id en localStorage para que, si recarga la página, siga como la misma persona y vea si ya envió.

PANTALLA 2: APLICACIÓN (dos pestañas)
Arriba, siempre visible:
- Título "Dolores de la compañía" y, a la derecha, el nombre y cargo de quien ingresó.
- Tarjeta "Participación completada": "X de 30 · N%" con barra de progreso. X es la cantidad de participantes que ya enviaron su selección. 30 es el total esperado (constante configurable en la tabla `config`). La barra pasa a verde al llegar a 100%.
- Debajo de la barra, una línea con la última persona que envió: "María Quispe envió su selección".

REGLA CLAVE: el ranking NO debe verse ni poder consultarse antes de enviar, para que nadie vote influenciado por los demás. Mientras la persona no ha enviado, solo ve la pantalla de selección (sin pestañas, sin botón de ranking y sin puntos). La tarjeta de participación completada (X de 30) sí puede verse siempre, porque no revela qué dolores van ganando. Esto debe cumplirse también en la base de datos, no solo ocultando botones.

FILTRO POR ÁREA (en la pantalla de selección)
- Justo DEBAJO de la tarjeta "Participación completada" y encima de la lista de dolores, una fila con el texto "Filtrar por área" (Outfit, negrita) y, a su lado, un menú DESPLEGABLE (select) de ancho máximo 360 px. Solo aparece en la vista de selección; se oculta al pasar a "Ranking en vivo". NO uses chips ni botones en fila: debe verse limpio y compacto, con una sola línea.
- Estilo del desplegable: borde de 2 px azul petróleo (#0B6E99), esquinas redondeadas, fondo blanco, texto 14,5 px semibold, flecha (chevron) azul petróleo a la derecha.
- Opciones: la primera es "Todas las áreas (30)"; después una por cada área distinta que exista en la columna `area` de la tabla `dolores`, ORDENADAS ALFABÉTICAMENTE de la A a la Z (usa localeCompare con "es" e ignora tildes y mayúsculas), por ejemplo: Comercial, Experiencia del Cliente, Finanzas, FNB, Gestión de personas, Legal, Operaciones, Seguridad Integral, Sostenibilidad. Las áreas se generan dinámicamente desde los datos.
- Cada opción muestra entre paréntesis la cantidad de dolores del área y, si la persona ya marcó dolores en esa área, el texto "· 2 marcados" (se actualiza al marcar o desmarcar). Ejemplo: "Operaciones (5)  ·  2 marcados".
- Por defecto está seleccionada "Todas las áreas". Al elegir un área, la lista muestra solo los dolores de esa área. Debajo del desplegable, un texto pequeño: "Mostrando 3 dolores de Finanzas" o "Mostrando los 30 dolores".
- El filtro es solo visual: NO cambia la selección. Los dolores marcados siguen marcados al cambiar de área, el contador "X / 8" y el límite de 8 siguen siendo globales (si ya hay 8, los no marcados de cualquier área se ven bloqueados). Cada dolor conserva su número original (por ejemplo, en Finanzas se ven el 28, 29 y 30).
- Usa un `<select>` nativo con estilo (accesible y cómodo en celular), con etiqueta asociada.

Pantalla de selección (única visible hasta enviar):
- Lista de todos los dolores (30 de ejemplo, luego serán más). Cada fila muestra número, encima del texto el ÁREA a la que pertenece el dolor en letras pequeñas, mayúsculas y color azul petróleo (ej. "OPERACIONES"), el texto del dolor y a la derecha un botón tipo casilla (checklist) cuadrado, celeste oscuro, con check blanco al marcarse.
- Se pueden marcar como máximo 8. Al llegar a 8, todas las filas NO marcadas quedan bloqueadas (opacidad baja, cursor not-allowed, sin poder hacer clic). Las marcadas siguen activas: si la persona desmarca una, se desbloquean las demás y puede elegir otra.
- Barra fija abajo con el contador "5 / 8", un mensaje de ayuda ("Te faltan 3 para poder enviar") y el botón "Enviar selección", habilitado solo cuando hay exactamente 8.
- Al enviar: se guardan los votos, se marca al participante como "completó", se bloquea toda la lista (solo se puede enviar una vez), se muestra el aviso "Tu selección fue enviada" y RECIÉN ENTONCES aparecen las dos pestañas "Mi selección" y "Ranking en vivo", cambiando automáticamente a "Ranking en vivo". Si la persona recarga la página y ya había enviado, entra directo con las pestañas visibles.

Pestaña "Ranking en vivo" (solo existe después de enviar):
- Lista de TODOS los dolores ordenados de mayor a menor cantidad de puntos. Los puntos son cuántas personas lo marcaron (si 30 lo marcaron, tiene 30; si 15, tiene 15).
- Cada fila: posición, el área en letras pequeñas arriba del texto del dolor (igual que en la selección), el texto, una barrita proporcional (puntos / total esperado) y los puntos a la derecha en grande.
- Los 8 primeros llevan el estilo verde. Cuando un dolor sube al top 8, toma el verde; el que sale del top 8 pierde el verde.
- Cuando cambian los puntos, las filas se reordenan con una animación suave (usar transform translateY con transición, filas de altura fija). El empate se desempata por número de dolor.
- Se actualiza en tiempo real con Supabase Realtime sin recargar la página: suscríbete a cambios en `votos` y `participantes` y vuelve a calcular.
- Leyenda: verde = "Top 8 priorizados", gris azulado = "Fuera del top 8".

DATOS (Supabase)
- Usa las tablas del SQL que te voy a pasar: `dolores`, `participantes`, `votos`, `config`.
- NO leas `votos` desde el frontend. El ranking se pide con la función RPC `obtener_ranking(p_participante uuid)`, que solo responde si ese participante ya envió. El avance se pide con `obtener_avance()`.
- Para el tiempo real, suscríbete a cambios en `participantes` (cada vez que alguien envía cambia su fila) y vuelve a llamar a `obtener_ranking` y `obtener_avance`.
- El envío debe ser una sola llamada a la función RPC `enviar_seleccion(p_participante uuid, p_dolores int[])`, que valida que sean exactamente 8 dolores distintos y que el participante no haya enviado antes.

PESTAÑA "Gráfico de resultados" (solo existe después de enviar, junto a "Ranking en vivo")
- Una tercera pestaña con el texto "Gráfico de resultados", al lado de "Ranking en vivo". Muestra en gráficos los 8 dolores más votados y se actualiza en tiempo real con cada voto nuevo y cuando el facilitador guarda un orden final por consenso (cambios en las tablas `participantes` y `orden_final`).
- Arriba, chips de estado: "Participación X de 30", "En vivo" o "Votación completa" y, si hay consenso, "Consenso · se suman N dolores" (amarillo). Leyenda: verde = "Más votados", amarillo = "Se suma por consenso".
- COLORES: VERDE (#1FA463) para los 8 dolores más votados. AMARILLO (#E0A800, fondo #FFF3BF, texto #6B4E00) para los dolores que NO estaban en esos 8 y que el facilitador arrastró hacia arriba en el panel (su posición final es mejor que la que tenían por votos). Los amarillos se SUMAN a los 8 verdes: los verdes nunca desaparecen ni cambian de color.
- Gráfico 1, "Dolores por área": una barra horizontal por área, ordenadas de mayor a menor, con la cantidad de dolores de esa área entre los 8 más votados. La barra es APILADA: segmento verde (dolores más votados) y segmento amarillo (dolores sumados por consenso), con el total a la derecha. Antes del consenso solo hay verde.
- Gráfico 2, "Mapa de calor del Top 8": tabla con una fila por dolor y 7 columnas de dimensiones de impacto (Económico/financiero, Cliente, Operativo, Estratégico, Personas y capacidades, Riesgo/cumplimiento, Otras; las dimensiones salen de la columna `dimensiones`). Primero las 8 filas de los más votados, numeradas 1 a 8; si hay consenso, debajo se agregan las filas amarillas con el signo "+" en lugar de número. Cada celda de una dimensión que toca el dolor se pinta VERDE en las filas verdes y AMARILLA en las filas amarillas; las demás celdas, gris muy claro; si el dolor no tiene dimensiones clasificadas, la fila va en franjas grises.
- Al final del mapa de calor, una fila "Total" con un número grande por columna: cuántos dolores (verdes + amarillos) tocan esa dimensión. Si hay amarillos, debajo del número una línea pequeña "4 + 2" (verdes + amarillos). El número suma las celdas pintadas de la columna.
- NO incluyas otros gráficos (ni el de barras de votos por dolor ni el de impacto por dimensión).
- Las barras cambian de ancho con una transición de 0,6 s cuando llegan votos o cambia el orden. La tabla del mapa de calor tiene scroll horizontal en celular.
- También en la pestaña "Ranking en vivo": si hay orden final, las filas siguen ese orden, los dolores sumados se pintan amarillos con "▲ subió de #N", y los 8 originales siguen en verde. La leyenda incluye "Subió por consenso".

DETALLE DE CADA DOLOR (al pasar el mouse)
- Al pasar el mouse sobre el texto de un dolor (en la lista de selección y en el ranking) aparece una tarjeta flotante blanca con borde azul petróleo de 2 px, esquinas redondeadas y sombra, de máximo 360 px de ancho. Contiene: el área en letras pequeñas, el título del dolor y tres bloques con su etiqueta en mayúsculas pequeñas: "Definición del dolor", "Evidencia" e "Impacto".
- Los textos salen de las columnas `definicion`, `evidencia` e `impacto` de la tabla `dolores` (pueden ser texto largo y varían por dolor).
- Los textos pueden ser largos (varios párrafos): la tarjeta mide hasta 540 px de ancho, su altura máxima es el 72% de la pantalla con scroll interno, y se queda abierta mientras el mouse esté sobre ella. Respeta los saltos de línea del texto original.
- Se posiciona debajo de la fila; si no hay espacio abajo, aparece arriba, sin tapar la fila. Nunca se sale de la pantalla. Aparece con un fundido corto y se cierra al sacar el mouse o al hacer scroll.
- En celular y teclado: junto a cada dolor de la lista hay un pequeño botón circular con una "i". Al tocarlo se abre/cierra la misma tarjeta, SIN marcar ni desmarcar el dolor. Tocar fuera la cierra.
- Marcar el dolor se hace con clic en cualquier parte de la fila, y se puede usar el teclado (Tab para moverse, Espacio o Enter para marcar).

Accesibilidad: botones con role="checkbox" y aria-checked, foco visible, contraste suficiente.
```

---

## 2. SQL para Supabase (SQL Editor)

```sql
-- Dolores a priorizar
create table public.dolores (
  id serial primary key,
  area text not null,
  texto text not null,
  definicion text,
  evidencia text,
  impacto text,
  activo boolean not null default true
);

-- Quien ingresa (sin registro, solo datos básicos)
create table public.participantes (
  id uuid primary key default gen_random_uuid(),
  nombre text not null check (char_length(trim(nombre)) >= 3),
  cargo text not null check (char_length(trim(cargo)) >= 2),
  completo boolean not null default false,
  enviado_en timestamptz,
  creado_en timestamptz not null default now()
);

-- Un voto = un dolor marcado por un participante (máx. 8 por persona)
create table public.votos (
  participante_id uuid not null references public.participantes(id) on delete cascade,
  dolor_id int not null references public.dolores(id) on delete cascade,
  creado_en timestamptz not null default now(),
  primary key (participante_id, dolor_id)
);

-- Total esperado de participantes y máximo de selecciones
create table public.config (
  id int primary key default 1 check (id = 1),
  total_esperado int not null default 30,
  max_selecciones int not null default 8
);
insert into public.config default values;

-- Ranking: SOLO lo entrega a quien ya envió su selección (evita el sesgo)
create or replace function public.obtener_ranking(p_participante uuid)
returns table (id int, area text, texto text, definicion text, evidencia text, impacto text, puntos int, posicion bigint)
language plpgsql
security definer
set search_path = public
as $$
begin
  if not exists (select 1 from public.participantes where participantes.id = p_participante and completo) then
    raise exception 'El ranking se habilita después de enviar tu selección';
  end if;
  return query
    select d.id, d.area, d.texto, d.definicion, d.evidencia, d.impacto, count(v.dolor_id)::int,
           row_number() over (order by count(v.dolor_id) desc, d.id)
    from public.dolores d
    left join public.votos v on v.dolor_id = d.id
    where d.activo
    group by d.id, d.area, d.texto, d.definicion, d.evidencia, d.impacto
    order by 8;
end;
$$;

-- Avance general (no revela votos): cuántos completaron y cuántos se esperan
create or replace function public.obtener_avance()
returns table (completaron int, total_esperado int)
language sql
security definer
set search_path = public
as $$
  select (select count(*) from public.participantes where completo)::int,
         (select c.total_esperado from public.config c where c.id = 1);
$$;

-- Envío atómico y validado de la selección
create or replace function public.enviar_seleccion(p_participante uuid, p_dolores int[])
returns void
language plpgsql
security definer
set search_path = public
as $$
declare
  v_max int;
begin
  select max_selecciones into v_max from public.config where id = 1;

  if (select count(distinct x) from unnest(p_dolores) x) <> v_max
     or array_length(p_dolores, 1) <> v_max then
    raise exception 'Debes elegir exactamente % dolores distintos', v_max;
  end if;

  if not exists (select 1 from public.participantes where id = p_participante and not completo) then
    raise exception 'Participante no existe o ya envió su selección';
  end if;

  insert into public.votos (participante_id, dolor_id)
  select p_participante, unnest(p_dolores);

  update public.participantes set completo = true, enviado_en = now() where id = p_participante;
end;
$$;

-- Seguridad (RLS): los votos solo se leen a través de obtener_ranking
alter table public.dolores enable row level security;
alter table public.participantes enable row level security;
alter table public.votos enable row level security;
alter table public.config enable row level security;

create policy "leer dolores" on public.dolores for select using (true);
create policy "leer participantes" on public.participantes for select using (true);
create policy "crear participante" on public.participantes for insert with check (completo = false);
-- votos: sin política de lectura a propósito (nadie puede leerlos directo)
-- config: sin política de lectura; el total esperado se entrega con obtener_avance()

grant execute on function public.enviar_seleccion(uuid, int[]) to anon, authenticated;
grant execute on function public.obtener_ranking(uuid) to anon, authenticated;
grant execute on function public.obtener_avance() to anon, authenticated;

-- Tiempo real: solo en participantes (no expone votos)
alter publication supabase_realtime add table public.participantes;
```

> Nota: sin registro, cualquiera con el enlace puede ingresar. Para el piloto de 30 personas es suficiente; si luego quieren evitar duplicados, se puede agregar un código de acceso o el DNI.

---

## 3. Prompt de datos: los 30 dolores de ejemplo

```
Inserta en la tabla `dolores` estos 30 dolores de ejemplo, cada uno con su área, definición, evidencia e impacto (Legal, FNB, Experiencia del Cliente, Gestión de personas, Sostenibilidad, Operaciones, Comercial, Seguridad Integral, Finanzas). Después los reemplazaré por los reales, así que la app debe leer siempre la lista y el área desde la tabla y no tener textos fijos en el código. El área puede variar y no todas tienen la misma cantidad de dolores.
```

```sql
insert into public.dolores (area, texto, definicion, evidencia, impacto) values
('Legal','Contratos con aprobaciones lentas y de varios pasos','Los contratos pasan por varias revisiones una tras otra antes de firmarse.','Aprobación promedio de 18 días hábiles frente a una meta de 7.','Retrasa el inicio de obras y servicios y tensa la relación con proveedores.'),
('Legal','Cambios regulatorios que no se comunican a tiempo a las áreas','Las nuevas normas se publican, pero no llegan de forma ordenada a las áreas que deben aplicarlas.','Dos cambios regulatorios del último año se conocieron semanas después de entrar en vigencia.','Riesgo de incumplir y de recibir observaciones o multas.'),
('Legal','Poco seguimiento a procesos y reclamos legales en curso','No hay un tablero único que muestre el estado de los procesos y reclamos legales abiertos.','El seguimiento se lleva en hojas de cálculo distintas, con fechas desactualizadas.','Plazos vencidos y poca visibilidad del riesgo para la gerencia.'),
('FNB','Baja bancarización de clientes que limita el acceso al financiamiento','Muchos clientes no tienen historial bancario y no califican a los productos de financiamiento.','Cerca del 40% de las solicitudes se descarta por falta de historial crediticio.','Se pierden ventas y clientes que podrían conectarse al servicio.'),
('FNB','Demora en la evaluación y aprobación de créditos','La evaluación de un crédito requiere validaciones manuales en varios sistemas.','El tiempo de respuesta supera los 5 días, cuando el cliente espera menos de 2.','Los clientes desisten o buscan otra alternativa.'),
('FNB','Cartera morosa con seguimiento tardío','El contacto con clientes que se atrasan en sus pagos empieza cuando la deuda ya es antigua.','La mayor parte de la mora se gestiona recién después de 60 días de atraso.','Sube la cartera vencida y la provisión de incobrables.'),
('Experiencia del Cliente','Demoras en la atención de reclamos','Los reclamos tardan en resolverse y el cliente debe insistir para tener respuesta.','El tiempo de atención de reclamos supera el plazo interno en uno de cada tres casos.','Cae la satisfacción del cliente y aumentan las quejas en entes reguladores.'),
('Experiencia del Cliente','Respuestas distintas según el canal de atención','Web, teléfono y oficinas entregan información distinta sobre el mismo trámite.','Clientes reportan haber recibido hasta tres respuestas diferentes a una misma consulta.','Genera desconfianza y llamadas repetidas.'),
('Experiencia del Cliente','Clientes sin información clara del estado de su solicitud','Tras pedir una conexión o servicio, el cliente no sabe en qué etapa está su solicitud.','Gran parte de las llamadas al call center son consultas por el estado del trámite.','Satura los canales de atención y deteriora la experiencia.'),
('Experiencia del Cliente','Facturas confusas que generan consultas repetidas','El formato de la factura mezcla conceptos y es difícil de entender.','Las consultas sobre cómo leer la factura se repiten cada mes en los canales de atención.','Costos de atención innecesarios y clientes molestos.'),
('Gestión de personas','Alta rotación de personal clave','Personas con experiencia y conocimiento del negocio se van a otras empresas.','La rotación del último año fue mayor en puestos técnicos y de jefatura.','Se pierde conocimiento, suben los costos de selección y se frenan proyectos.'),
('Gestión de personas','Falta de planes de desarrollo y línea de carrera','No hay rutas claras de crecimiento ni capacitación alineada a cada puesto.','La encuesta de clima muestra baja percepción de oportunidades de desarrollo.','Baja motivación y menor retención del talento.'),
('Gestión de personas','Carga laboral desbalanceada entre equipos','Algunos equipos trabajan en sobrecarga mientras otros tienen capacidad disponible.','Las horas extra se concentran en pocas áreas durante los cierres de mes.','Agotamiento, errores y mayor ausentismo.'),
('Sostenibilidad','Impacto ambiental de las obras sin medición consistente','Las obras se ejecutan sin una medición uniforme de su huella ambiental.','Cada contratista reporta con un formato distinto y no todos reportan.','Dificulta cumplir metas ambientales y responder a auditorías.'),
('Sostenibilidad','Poca articulación con las comunidades de las zonas de trabajo','El trabajo con comunidades se hace por proyecto y sin un plan común.','Las observaciones de vecinos se atienden por canales distintos y sin registro único.','Genera conflictos sociales que detienen o encarecen las obras.'),
('Sostenibilidad','Indicadores ESG reportados de forma manual y dispersa','Los datos para reportes ESG se recogen a mano desde varias áreas.','El armado del reporte anual toma varias semanas y requiere correcciones.','Riesgo de errores y poca confianza de inversionistas y evaluadores.'),
('Operaciones','Retrasos en la ejecución de obras','Las obras se terminan después de la fecha comprometida con el cliente.','Una parte importante de los proyectos del año cerró con retraso frente al cronograma.','Más costos, clientes insatisfechos e ingresos que se postergan.'),
('Operaciones','Falta de integración entre sistemas','Los sistemas de las áreas no comparten información entre sí.','Los mismos datos se registran dos o tres veces en sistemas distintos.','Duplica el trabajo y produce cifras que no coinciden.'),
('Operaciones','Procesos internos lentos y burocráticos','Muchos procesos exigen varias firmas y pasos que no agregan valor.','Procesos simples toman semanas por aprobaciones en cadena.','Se pierde agilidad y la gente dedica tiempo a trámites en vez de a resultados.'),
('Operaciones','Emergencias y fugas atendidas con tiempos altos','El tiempo entre el aviso de una fuga y la llegada de la cuadrilla es mayor al esperado.','En horas punta, el tiempo de llegada supera la meta en varios distritos.','Riesgo para las personas, daño a la imagen y sanciones del regulador.'),
('Operaciones','Exceso de reportes manuales','Los reportes operativos se arman a mano cada semana.','Los equipos dedican varias horas semanales a copiar datos entre archivos.','Horas perdidas, errores y decisiones con información atrasada.'),
('Comercial','Conexiones nuevas que demoran en concretarse','Desde que el cliente acepta hasta que se concreta la conexión pasan demasiados días.','Los tiempos reales de conexión superan los comprometidos en la oferta.','Se caen ventas y baja la meta de nuevos clientes.'),
('Comercial','Falta de seguimiento a prospectos','Los contactos interesados no tienen seguimiento ordenado en el tiempo.','Una parte de los prospectos no recibe una segunda llamada.','Oportunidades de venta que se enfrían.'),
('Comercial','Metas comerciales poco alineadas con la capacidad operativa','Las metas de venta se fijan sin considerar cuántas conexiones se pueden ejecutar.','Los picos de venta superan la capacidad de las cuadrillas.','Atrasos, reclamos y frustración en los equipos comercial y operativo.'),
('Seguridad Integral','Daños a la red por terceros durante excavaciones','Las excavaciones de terceros dañan las tuberías de la red.','Se registran incidentes de daños por terceros todos los meses.','Riesgo de accidentes, interrupción del servicio y costos de reparación.'),
('Seguridad Integral','Cumplimiento desigual de normas de seguridad en contratistas','Los contratistas no siguen las mismas reglas de seguridad en todas las obras.','Las inspecciones encuentran observaciones repetidas en el mismo tipo de trabajo.','Aumenta la probabilidad de accidentes y de sanciones.'),
('Seguridad Integral','Reporte tardío de incidentes','Los incidentes se informan horas o días después de ocurrir.','Reportes de incidentes llegan al área de Seguridad fuera del plazo establecido.','Se pierde la oportunidad de prevenir y de investigar a tiempo.'),
('Finanzas','Datos poco confiables para decidir','Las cifras que usan las áreas para decidir cambian según el sistema de donde salen.','En cada cierre se concilian diferencias entre reportes que deberían coincidir.','Decisiones basadas en información dudosa.'),
('Finanzas','Cierres contables con muchas tareas manuales','El cierre mensual depende de archivos y ajustes manuales.','El cierre toma más días de los previstos por correcciones de último minuto.','Retrasa los reportes de gestión y aumenta el riesgo de error.'),
('Finanzas','Presupuestos que se desvían sin alertas tempranas','No hay alertas que avisen cuando un gasto se aleja de lo presupuestado.','Las desviaciones se descubren al cierre del mes y no cuando ocurren.','Sobrecostos que ya no se pueden corregir a tiempo.');
```

Para cargar los dolores reales más adelante: `delete from public.dolores;` (solo si aún no hay votos) y luego un nuevo `insert`. Si ya hay votos, usa `update public.dolores set activo = false` en los que quieras retirar.

---

## 4. Referencia: lógica clave del frontend

Si Lovable no implementa algo como se pidió, pega este fragmento y dile "usa esta lógica".

```ts
// Selección: máximo 8, bloqueo de no marcados, desmarcar permitido
const MAX = 8;
const [selected, setSelected] = useState<Set<number>>(new Set());
const toggle = (id: number) =>
  setSelected(prev => {
    const next = new Set(prev);
    if (next.has(id)) next.delete(id);
    else if (next.size < MAX) next.add(id);
    return next;
  });
const isLocked = (id: number) => selected.size >= MAX && !selected.has(id);

// Envío
await supabase.rpc("enviar_seleccion", {
  p_participante: participanteId,
  p_dolores: Array.from(selected),
});

// Ranking en tiempo real (solo si ya envió; la BD lo exige igual)
useEffect(() => {
  const load = async () => {
    const { data: av } = await supabase.rpc("obtener_avance");
    if (av?.[0]) { setCompletos(av[0].completaron); setTotal(av[0].total_esperado); }
    if (yaEnvio) {
      const { data } = await supabase.rpc("obtener_ranking", { p_participante: participanteId });
      setRanking(data ?? []);
    }
  };
  load();
  const ch = supabase.channel("avance")
    .on("postgres_changes", { event: "*", schema: "public", table: "participantes" }, load)
    .subscribe();
  return () => { supabase.removeChannel(ch); };
}, [yaEnvio]);

// Pestañas: solo se muestran cuando yaEnvio === true

// Top 8 y animación de reordenado (filas de altura fija = ROW_H)
const ROW_H = 68;
// cada fila: style={{ transform: `translateY(${index * ROW_H}px)`, transition: "transform .7s" }}
// verde si index < 8 && puntos > 0
```

---

## 5. SQL adicional para el gráfico de resultados

Ejecútalo DESPUÉS del SQL del panel (`PROMPT_LOVABLE_PANEL.md`, sección 2), porque usa la tabla `orden_final`. Reemplaza `obtener_ranking` para que también devuelva las dimensiones de impacto y la posición final.

```sql
alter table public.dolores add column if not exists dimensiones text[] default '{}';
alter table public.dolores add column if not exists fuente int;

drop function if exists public.obtener_ranking(uuid);
create or replace function public.obtener_ranking(p_participante uuid)
returns table (
  id int, area text, texto text, definicion text, evidencia text, impacto text,
  dimensiones text[], puntos int, posicion bigint, pos_final int
)
language plpgsql
security definer
set search_path = public
as $$
begin
  if not exists (select 1 from public.participantes where participantes.id = p_participante and completo) then
    raise exception 'El ranking se habilita después de enviar tu selección';
  end if;
  return query
    select d.id, d.area, d.texto, d.definicion, d.evidencia, d.impacto, d.dimensiones,
           count(v.dolor_id)::int,
           row_number() over (order by count(v.dolor_id) desc, d.id),
           (select o.posicion from public.orden_final o where o.dolor_id = d.id)
    from public.dolores d
    left join public.votos v on v.dolor_id = d.id
    where d.activo
    group by d.id, d.area, d.texto, d.definicion, d.evidencia, d.impacto, d.dimensiones
    order by 9;
end;
$$;
grant execute on function public.obtener_ranking(uuid) to anon, authenticated;
```

En el frontend: `posicion` es la posición por votos y `pos_final` es la del orden por consenso (puede ser nulo). El Top 8 por votación se congela cuando completaron todos; un dolor es amarillo si no está en ese Top 8, está en el orden final y su `pos_final` es menor que su `posicion`. Para que el panel solo pinte de amarillo lo que el facilitador arrastró, guarda además esa marca (opcional): `alter table public.orden_final add column if not exists movido boolean default false;` y envíala en `guardar_orden_final`.
