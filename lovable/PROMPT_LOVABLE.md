# Plataforma de priorización de dolores: prompt y código para Lovable

Orden de uso:
1. Pega el **Prompt principal** (sección 1) en Lovable.
2. Cuando Lovable pida conectar Supabase, conéctalo y ejecuta el **SQL** (sección 2) en el SQL Editor.
3. Pega el **Prompt de datos** (sección 3) para cargar los 30 dolores de ejemplo.
4. Adjunta `mockup/logo-calidda.webp` en el chat de Lovable junto al prompt principal.
5. Compara con el boceto en `mockup/priorizacion-dolores.html`.

---

## 1. Prompt principal (pegar en Lovable)

```
Crea una aplicación web en español llamada "Priorización de dolores", con React + TypeScript + Tailwind y Supabase (base de datos + Realtime). Es para que unas 30 personas elijan en tiempo real los dolores más importantes de la compañía.

DISEÑO
- Fondo turquesa corporativo de Cálidda (#32C5DB) en toda la app, tarjetas blancas con sombra suave y esquinas redondeadas.
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

Pantalla de selección (única visible hasta enviar):
- Lista de todos los dolores (30 de ejemplo, luego serán más). Cada fila muestra número, texto y a la derecha un botón tipo casilla (checklist) cuadrado, celeste oscuro, con check blanco al marcarse.
- Se pueden marcar como máximo 8. Al llegar a 8, todas las filas NO marcadas quedan bloqueadas (opacidad baja, cursor not-allowed, sin poder hacer clic). Las marcadas siguen activas: si la persona desmarca una, se desbloquean las demás y puede elegir otra.
- Barra fija abajo con el contador "5 / 8", un mensaje de ayuda ("Te faltan 3 para poder enviar") y el botón "Enviar selección", habilitado solo cuando hay exactamente 8.
- Al enviar: se guardan los votos, se marca al participante como "completó", se bloquea toda la lista (solo se puede enviar una vez), se muestra el aviso "Tu selección fue enviada" y RECIÉN ENTONCES aparecen las dos pestañas "Mi selección" y "Ranking en vivo", cambiando automáticamente a "Ranking en vivo". Si la persona recarga la página y ya había enviado, entra directo con las pestañas visibles.

Pestaña "Ranking en vivo" (solo existe después de enviar):
- Lista de TODOS los dolores ordenados de mayor a menor cantidad de puntos. Los puntos son cuántas personas lo marcaron (si 30 lo marcaron, tiene 30; si 15, tiene 15).
- Cada fila: posición, texto del dolor, una barrita proporcional (puntos / total esperado) y los puntos a la derecha en grande.
- Los 8 primeros llevan el estilo verde. Cuando un dolor sube al top 8, toma el verde; el que sale del top 8 pierde el verde.
- Cuando cambian los puntos, las filas se reordenan con una animación suave (usar transform translateY con transición, filas de altura fija). El empate se desempata por número de dolor.
- Se actualiza en tiempo real con Supabase Realtime sin recargar la página: suscríbete a cambios en `votos` y `participantes` y vuelve a calcular.
- Leyenda: verde = "Top 8 priorizados", gris azulado = "Fuera del top 8".

DATOS (Supabase)
- Usa las tablas del SQL que te voy a pasar: `dolores`, `participantes`, `votos`, `config`.
- NO leas `votos` desde el frontend. El ranking se pide con la función RPC `obtener_ranking(p_participante uuid)`, que solo responde si ese participante ya envió. El avance se pide con `obtener_avance()`.
- Para el tiempo real, suscríbete a cambios en `participantes` (cada vez que alguien envía cambia su fila) y vuelve a llamar a `obtener_ranking` y `obtener_avance`.
- El envío debe ser una sola llamada a la función RPC `enviar_seleccion(p_participante uuid, p_dolores int[])`, que valida que sean exactamente 8 dolores distintos y que el participante no haya enviado antes.

Accesibilidad: botones con role="checkbox" y aria-checked, foco visible, contraste suficiente.
```

---

## 2. SQL para Supabase (SQL Editor)

```sql
-- Dolores a priorizar
create table public.dolores (
  id serial primary key,
  texto text not null,
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
returns table (id int, texto text, puntos int, posicion bigint)
language plpgsql
security definer
set search_path = public
as $$
begin
  if not exists (select 1 from public.participantes where participantes.id = p_participante and completo) then
    raise exception 'El ranking se habilita después de enviar tu selección';
  end if;
  return query
    select d.id, d.texto, count(v.dolor_id)::int,
           row_number() over (order by count(v.dolor_id) desc, d.id)
    from public.dolores d
    left join public.votos v on v.dolor_id = d.id
    where d.activo
    group by d.id, d.texto
    order by 4;
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
create policy "leer config" on public.config for select using (true);

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
Inserta en la tabla `dolores` estos 30 dolores de ejemplo (Fenómeno del Niño en el Perú, desde la mirada de los pobladores). Después los reemplazaré por los reales, así que la app debe leer siempre la lista desde la tabla y no tener textos fijos en el código.
```

```sql
insert into public.dolores (texto) values
('Pérdida de viviendas por huaicos e inundaciones'),
('Destrucción de cultivos y pérdida de cosechas'),
('Carreteras y puentes colapsados que dejan aislados a los centros poblados'),
('Falta de agua potable segura tras el desborde de ríos'),
('Brotes de dengue por acumulación de agua estancada'),
('Pérdida de ganado y animales de crianza'),
('Pérdida de ingresos y de empleo en el comercio local'),
('Colegios inundados o dañados que interrumpen las clases'),
('Centros de salud dañados o inaccesibles'),
('Alertas tempranas que no llegan a tiempo'),
('Falta de refugios y albergues adecuados'),
('Demora en la llegada de la ayuda humanitaria'),
('Desborde de ríos por falta de defensas ribereñas'),
('Cortes prolongados de luz y de telecomunicaciones'),
('Pérdida de documentos de identidad y de propiedad'),
('Escasa limpieza de quebradas y drenajes antes de la temporada'),
('Viviendas construidas en zonas de alto riesgo'),
('Aumento de precios de alimentos y productos básicos'),
('Enfermedades respiratorias y diarreicas en la población'),
('Pérdida de embarcaciones y paralización de la pesca artesanal'),
('Deudas y falta de acceso a créditos para reconstruir'),
('Demora en la reconstrucción y en la entrega de bonos'),
('Colapso de desagües y del alcantarillado'),
('Estrés, ansiedad y afectación emocional de las familias'),
('Falta de información clara sobre qué hacer en una emergencia'),
('Falta de maquinaria y equipos para responder rápido'),
('Riesgo para niños, adultos mayores y personas con discapacidad'),
('Daño a canales y a la infraestructura de riego'),
('Acumulación de basura y residuos después del desastre'),
('Descoordinación entre autoridades locales, regionales y nacionales');
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
