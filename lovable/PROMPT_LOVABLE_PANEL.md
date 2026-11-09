# Panel de ranking en vivo (facilitador): prompt y código para Lovable

Segunda plataforma, pensada para quien dirige la sesión. Solo abre el enlace y ve el ranking en vivo de las calificaciones que ponen las personas. Cuando todas envían su calificación (nota de 1 a 5 a cada dolor), se habilita **Modificar ranking** para reordenar por consenso.

Boceto para comparar: `mockup/panel-ranking.html`.

Esta plataforma usa **el mismo proyecto de Supabase** que la primera (`PROMPT_LOVABLE.md`): mismas tablas `dolores`, `participantes`, `calificaciones`, `config`. Puede ser el mismo proyecto de Lovable con una ruta nueva (`/panel`) o un proyecto aparte con las mismas credenciales de Supabase.

Orden de uso:
1. Si aún no lo hiciste, ejecuta el SQL de la primera plataforma.
2. Ejecuta el **SQL adicional** (sección 2) en Supabase.
3. Pega el **Prompt principal** (sección 1) en Lovable y adjunta `mockup/logo-calidda-turquesa.png`.
4. Abre el panel con el enlace secreto: `https://TU-APP/panel?k=LA_CLAVE` (la clave que definas en la sección 2).

---

## 1. Prompt principal (pegar en Lovable)

```
Crea la ruta /panel de la aplicación "Priorización de dolores" (React + TypeScript + Tailwind + Supabase). Es el panel del facilitador: SOLO muestra el ranking en vivo y, cuando todos terminan de calificar, permite reordenarlo por consenso. No hay pantalla de acceso ni formulario: se entra con un enlace que lleva una clave en la URL (/panel?k=CLAVE). Lee esa clave con useSearchParams y envíala en cada llamada. Si falta o es incorrecta, muestra un mensaje "Enlace no válido" y nada más.

DISEÑO
- Fondo BLANCO (#FFFFFF). Tarjetas blancas con borde fino #BFE3EC. Color de acción: azul petróleo #0B6E99 (hover #07556F).
- Tipografías: "Outfit" para títulos y "Figtree" para texto.
- Arriba a la izquierda: "SESIÓN DE ESTRATEGIA" en mayúsculas, pequeño, azul petróleo; debajo el título "Ranking en vivo de dolores". Arriba a la derecha, un indicador de estado en forma de píldora: "En vivo" (celeste con punto verde que parpadea), "Votación completa" (verde claro), "Modo edición" (amarillo claro) u "Orden final guardado" (verde claro).
- Al pie, centrado, el logo de Cálidda en turquesa (te adjunto el archivo), a 150 px de ancho.
- Debe verse bien en celular y computadora.

TARJETA DE PARTICIPACIÓN
- Borde grueso de 3 px en azul petróleo y sombra suave azulada. Título "Participación completada", a la derecha "X de 30 · N%" y una barra de progreso (pasa a verde al llegar al 100%). Debajo, una línea con la última persona que envió: "Nombre envió su selección".
- X sale de la función RPC `obtener_avance()` (completaron y total_esperado).

RANKING (lista de TODOS los dolores)
- Cada fila, de altura fija (66 px, separación 74 px): posición, el área en letras pequeñas arriba (mayúsculas, azul petróleo), el texto del dolor, una barrita proporcional (puntaje / (5 × total esperado)) y a la derecha el PUNTAJE en grande (la suma de las notas de 1 a 5) con el promedio debajo en letra pequeña ("prom. 4.3").
- Sobre la lista, una fila de chips con "Mayor puntaje: 135" (verde claro), "Menor puntaje: 63" y "Máximo posible: 150 (30 personas × 5)".
- Mientras se vota, el orden es por puntaje, es decir, la suma de las notas (mayor a menor; empate por número de dolor) y se actualiza en tiempo real con una animación suave (transform translateY con transición de 0,7 s). Los 8 primeros se ven en VERDE (#1FA463, fondo #E1F5EA); los demás, en blanco con borde gris azulado.
- Al pasar el mouse sobre el texto de un dolor aparece una tarjeta flotante (borde azul petróleo de 2 px, máximo 360 px) con el área, el título y tres bloques: "Definición del dolor", "Evidencia" e "Impacto" (columnas `definicion`, `evidencia`, `impacto` de `dolores`). Se posiciona debajo de la fila o arriba si no hay espacio, y se cierra al sacar el mouse o hacer scroll.
- Leyenda sobre la lista: verde = "Top 8 por puntaje", amarillo = "Subió por consenso", gris azulado = "Resto de dolores".

CUANDO TODOS TERMINAN (completaron >= total_esperado)
- El mensaje cambia a "Todos completaron su selección. Ya puedes modificar el ranking según el consenso." y aparece el botón "Modificar ranking".
- En ese momento se CONGELA quién es el Top 8 por puntaje: los 8 dolores que quedaron arriba por puntos. Ese conjunto no cambia aunque se muevan las filas.

PESTAÑAS DEL PANEL
- Justo debajo de la tarjeta "Participación completada" hay dos pestañas, iguales en estilo a las de la plataforma de calificación: "Ranking en vivo" (la lista de los dolores, con el modo edición) y "Gráfico de resultados". La pestaña activa va rellena azul petróleo (#0B6E99) con texto blanco; la inactiva, blanca con borde #BFE3EC. Por defecto está activa "Ranking en vivo".
- La barra de acciones (mensaje, "Modificar ranking", "Guardar orden final"…) sigue visible en ambas pestañas.

PESTAÑA "Gráfico de resultados" (es la MISMA que ven las personas en la plataforma de calificación)
- Se actualiza en tiempo real con cada voto, con cada arrastre en el modo edición (aunque todavía no se haya guardado) y con cada orden final guardado.
- Chips de estado arriba: "Participación X de 30", el estado ("En vivo", "Votación completa", "Modo edición" en amarillo u "Orden final guardado" en verde) y, si hay dolores sumados, "Consenso · se suman N dolores". Leyenda: verde = "Más votados", amarillo = "Se suma por consenso".
- Verde (#1FA463): los 8 de mayor puntaje (el Top 8 por puntaje, congelado cuando completaron todos). Amarillo (#E0A800, fondo #FFF3BF): los dolores que no estaban en ese Top 8 y que el facilitador arrastró hacia arriba (posición actual mejor que la que tenía por puntaje). Los amarillos se SUMAN a los 8 verdes; los verdes nunca desaparecen.
- Gráfico 1, "Dolores por área": una barra apilada por área (segmento verde y segmento amarillo) con el total a la derecha, de mayor a menor, contando solo los 8 verdes y los amarillos sumados.
- Gráfico 2, "Mapa de calor del Top 8": una fila por dolor (primero los 8 verdes numerados 1 a 8; debajo los amarillos con el signo "+") por 7 columnas de dimensiones de impacto (Económico/financiero, Cliente, Operativo, Estratégico, Personas y capacidades, Riesgo/cumplimiento, Otras; salen de la columna `dimensiones`). Celdas verdes en las filas verdes, amarillas en las amarillas, gris muy claro donde no aplica y franjas grises si el dolor no tiene dimensiones. Al final, una fila "Total" con un número grande por columna (cuántos dolores tocan esa dimensión) y, si hay amarillos, una línea pequeña "verdes + amarillos" (por ejemplo "4 + 2"). Con scroll horizontal en celular.
- No incluyas otros gráficos.
- Los datos son los mismos que devuelve `obtener_panel` (necesita la columna `dimensiones`; ver la sección 5 de `PROMPT_LOVABLE.md`: agrega `dimensiones` al `returns table` y al `select` de `obtener_panel`).

MODO EDICIÓN (botón "Modificar ranking")
- Estado "Modo edición". Aparecen botones: "Guardar orden final" (verde), "Volver al orden por puntaje" y "Cancelar". El texto de ayuda dice: "Arrastra cualquier dolor hacia arriba o abajo y suéltalo en su nueva posición."
- NO hay botones de flecha. El reordenamiento es SOLO arrastrando: con el mouse, el usuario presiona sobre CUALQUIER parte de la fila de un dolor (cursor "grab"), la arrastra y la suelta en la posición que quiera. Debe poder llevar el dolor 15 al puesto 1 o el 1 al 30 en un solo movimiento. Cada fila muestra a la derecha un asa ⋮⋮ solo como pista visual.
- Mientras se arrastra: la fila arrastrada sigue al puntero (sin transición, con sombra más fuerte y por encima de las demás) y las otras filas se reacomodan en vivo con animación. La página hace auto-scroll rápido cuando el puntero se acerca a los 130 px del borde superior o inferior de la pantalla, con velocidad proporcional a la cercanía al borde (hasta 38 px por cuadro), para poder cruzar los 30 dolores sin soltar.
- En celular (dedo), la fila se arrastra tomándola desde el asa ⋮⋮ (touch-action: none en el asa; el resto de la fila deja hacer scroll normal).
- Usa pointer events (pointerdown, pointermove, pointerup) con setPointerCapture. La posición destino es clamp(round(y / 74)) donde y es la posición vertical del puntero relativa a la lista.
- REGLA DE COLORES (importante):
  * Los 8 dolores del Top 8 por puntaje SIEMPRE se quedan en verde, estén en la posición que estén. Mover otros dolores NO cambia el color de esos 8.
  * Un dolor que NO estaba en el Top 8 por puntaje, que el usuario ARRASTRÓ él mismo y que ahora está en una posición mejor que la que tenía por puntaje se pinta AMARILLO (#FFF3BF, borde #E0A800, texto #6B4E00): significa que ahora se está considerando. Los dolores que solo se corrieron un puesto porque otro pasó por encima NO se pintan ni llevan texto de movimiento: solo los que se arrastraron.
  * Si un amarillo se vuelve a bajar hasta su posición original (o más abajo), vuelve al estilo normal. Guarda en el estado la lista de ids "movidos a mano" (se vacía con "Volver al orden por puntaje" y "Cancelar").
  * En cada fila que se arrastró muestra un texto pequeño junto al área: "▲ subió de #15" o "▼ bajó de #3" (la posición original por votos).
  * Los puntajes nunca cambian al mover filas: son las calificaciones reales.
- "Guardar orden final": guarda el orden en Supabase con `guardar_orden_final`, sale del modo edición y el estado pasa a "Orden final guardado". Cualquiera que abra el panel después ve ese orden guardado (con los mismos colores) y en tiempo real si se vuelve a guardar. Debe existir "Modificar de nuevo".
- "Volver al orden por puntaje": deja las filas en el orden por puntaje y borra las marcas de "movido a mano" (sin guardar todavía).
- "Cancelar": descarta los cambios hechos desde que se entró al modo edición.

DATOS (Supabase)
- Ranking: RPC `obtener_panel(p_clave text)`.
- Avance: RPC `obtener_avance()`.
- Guardar: RPC `guardar_orden_final(p_clave text, p_orden int[])` (lista de ids de dolores en el orden final, del primero al último).
- Borrar el orden guardado: RPC `borrar_orden_final(p_clave text)`. Úsala solo si el facilitador quiere quitar el orden guardado y mostrar de nuevo el orden por puntaje (ofrece un botón secundario "Quitar orden guardado" visible solo cuando ya hay un orden guardado). Para reemplazarlo por otro basta con volver a llamar a `guardar_orden_final`.
- Tiempo real: suscríbete a cambios de las tablas `participantes` y `orden_final` y vuelve a pedir `obtener_panel` y `obtener_avance`.
- Si `pos_final` viene con valores, el orden mostrado es ese; si no, el orden es por votos.

Accesibilidad: botones con aria-label, foco visible, contraste suficiente.
```

---

## 2. SQL adicional para Supabase

```sql
-- Clave del facilitador (cámbiala antes de usar)
alter table public.config add column if not exists clave_facilitador text not null default 'CAMBIAR-ESTA-CLAVE';
update public.config set clave_facilitador = 'PON-AQUI-TU-CLAVE' where id = 1;

-- Orden final definido por consenso
create table if not exists public.orden_final (
  dolor_id int primary key references public.dolores(id) on delete cascade,
  posicion int not null
);
alter table public.orden_final enable row level security;
create policy "leer orden final" on public.orden_final for select using (true);
alter publication supabase_realtime add table public.orden_final;

-- Ranking para el panel: solo con la clave del facilitador
create or replace function public.obtener_panel(p_clave text)
returns table (
  id int, area text, texto text, definicion text, evidencia text, impacto text,
  puntos int, promedio numeric, pos_votos bigint, pos_final int
)
language plpgsql
security definer
set search_path = public
as $$
begin
  if not exists (select 1 from public.config where id = 1 and clave_facilitador = p_clave) then
    raise exception 'Clave no válida';
  end if;
  return query
    select d.id, d.area, d.texto, d.definicion, d.evidencia, d.impacto,
           coalesce(sum(c.puntaje), 0)::int,
           round(coalesce(avg(c.puntaje), 0), 1),
           row_number() over (order by coalesce(sum(c.puntaje), 0) desc, d.id),
           (select o.posicion from public.orden_final o where o.dolor_id = d.id)
    from public.dolores d
    left join public.calificaciones c on c.dolor_id = d.id
    where d.activo
    group by d.id, d.area, d.texto, d.definicion, d.evidencia, d.impacto
    order by 9;
end;
$$;

-- Guardar el orden final (solo con clave y solo cuando todos terminaron)
create or replace function public.guardar_orden_final(p_clave text, p_orden int[])
returns void
language plpgsql
security definer
set search_path = public
as $$
declare
  v_total int; v_completos int; v_activos int;
begin
  if not exists (select 1 from public.config where id = 1 and clave_facilitador = p_clave) then
    raise exception 'Clave no válida';
  end if;

  select total_esperado into v_total from public.config where id = 1;
  select count(*) into v_completos from public.participantes where completo;
  if v_completos < v_total then
    raise exception 'Aún no todos completaron su selección (% de %)', v_completos, v_total;
  end if;

  select count(*) into v_activos from public.dolores where activo;
  if array_length(p_orden, 1) <> v_activos
     or (select count(distinct x) from unnest(p_orden) x) <> v_activos
     or exists (select 1 from unnest(p_orden) x where x not in (select id from public.dolores where activo)) then
    raise exception 'El orden debe incluir cada dolor activo exactamente una vez';
  end if;

  delete from public.orden_final;
  insert into public.orden_final (dolor_id, posicion)
  select x, ord from unnest(p_orden) with ordinality as t(x, ord);
end;
$$;

-- Borrar el orden guardado (volver a mostrar el orden por votos)
create or replace function public.borrar_orden_final(p_clave text)
returns void
language plpgsql
security definer
set search_path = public
as $$
begin
  if not exists (select 1 from public.config where id = 1 and clave_facilitador = p_clave) then
    raise exception 'Clave no válida';
  end if;
  delete from public.orden_final;
end;
$$;

grant execute on function public.obtener_panel(text) to anon, authenticated;
grant execute on function public.guardar_orden_final(text, int[]) to anon, authenticated;
grant execute on function public.borrar_orden_final(text) to anon, authenticated;
```

> Importante: la clave del enlace es lo que evita que los participantes abran el ranking antes de calificar. No compartas el enlace del panel con ellos. `config` tiene RLS activado sin política de lectura para la columna de clave, así que la clave no se puede leer desde el navegador; solo las funciones la validan.

> Nota: el SQL de la primera plataforma ya no deja `config` con lectura pública. Si ejecutaste una versión anterior que sí la tenía, corre `drop policy if exists "leer config" on public.config;` para que la clave no quede expuesta. `obtener_avance()` entrega el total esperado sin leer la tabla.

---

## 3. Referencia: lógica clave del frontend

```ts
const ROW_H = 74, MAX = 8;
const k = new URLSearchParams(location.search).get("k") ?? "";

// 1) Cargar
const { data } = await supabase.rpc("obtener_panel", { p_clave: k });
// data: [{ id, area, texto, definicion, evidencia, impacto, puntos, promedio, pos_votos, pos_final }]  (puntos = suma de notas 1-5; pos_votos = posición por puntaje)

const completo = completaron >= totalEsperado;
const porVotos = [...data].sort((a, b) => a.pos_votos - b.pos_votos);
const top8 = new Set(porVotos.slice(0, MAX).map(d => d.id));   // se congela al completar
const origPos = new Map(porVotos.map((d, i) => [d.id, i]));

// Orden inicial: el guardado si existe, si no el de votos
const inicial = data.some(d => d.pos_final != null)
  ? [...data].sort((a, b) => (a.pos_final ?? 999) - (b.pos_final ?? 999))
  : porVotos;

// 2) Colores (solo después de completar)
const esVerde = (id: number, pos: number) => (completo ? top8.has(id) : pos < MAX && puntos(id) > 0);
const esAmarillo = (id: number, pos: number) => completo && movidos.has(id) && !top8.has(id) && pos < origPos.get(id)!;
// movidos: Set<number> con los ids que el facilitador arrastró a mano

// 3) Mover (al soltar/arrastrar): quitar el id de su lugar e insertarlo en el destino
const moverA = (orden: number[], id: number, destino: number) => {
  const next = orden.filter(x => x !== id); next.splice(destino, 0, id); return next;
};

// 4) Guardar
await supabase.rpc("guardar_orden_final", { p_clave: k, p_orden: orden });

// 5) Tiempo real
supabase.channel("panel")
  .on("postgres_changes", { event: "*", schema: "public", table: "participantes" }, cargar)
  .on("postgres_changes", { event: "*", schema: "public", table: "orden_final" }, cargar)
  .subscribe();

// 6) Arrastre: cada fila usa style={{ transform: `translateY(${pos * ROW_H}px)` }}.
//    En pointermove calcula la nueva posición = clamp(round(y / ROW_H)) y reordena el arreglo en vivo.
//    Haz auto-scroll cuando el puntero esté a menos de 90 px del borde de la pantalla.
```
