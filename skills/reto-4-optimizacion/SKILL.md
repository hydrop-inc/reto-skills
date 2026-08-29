---
name: reto-4-optimizacion
description: "Paso 4 del Reto de 0 a 5 Millones: la rutina DIARIA de optimización después de activar la campaña — verificar que consume presupuesto, leer métricas del día con ventana mínima de 18 horas, apagar anuncio por anuncio el que gastó más del CPA máximo sin traer venta, y escalar +20% diario cuando la campaña sale de aprendizaje con CPA sano y ventas todos los días. Usar cuando el alumno diga 'optimizá mi campaña', 'cómo van mis anuncios', 'las métricas de hoy', 'apago este anuncio?', 'cuándo escalo', '/reto-4-optimizacion', o cada día mientras tenga campañas activas. Requiere reto-{slug}.md con paso 3 completo y campañas activadas."
---

# Reto · Paso 4 — Operar la campaña (la rutina diaria)

Montar la campaña es la mitad del sistema. Esta skill es la otra mitad: la
rutina que se hace TODOS los días mientras la campaña esté prendida. Acá es
donde se gana o se pierde la plata.

## Reglas de plata

1. **Apagar un anuncio o un conjunto NO necesita esperar a la madrugada** —
   se apaga en el momento en que la regla lo diga.
2. **Subir presupuesto o prender campañas SÍ tiene horario: solo en la
   madrugada o muy tarde en la noche.** Meta reparte el presupuesto en la
   ventana del día; una campaña prendida a las 6 pm tiene solo 6 horas para
   gastar el presupuesto de 24 → arranca desoptimizada y quema plata. La
   skill NUNCA sube presupuesto fuera de ese horario: si el alumno lo pide a
   las 3 pm, se agenda la instrucción para la noche y se le dice por qué.
3. **Todo cambio de presupuesto y todo apagado se confirma con el alumno**
   mostrando el dato que lo justifica. Nada se toca en silencio.
4. Las métricas siempre se leen **con el filtro de fecha en HOY** (y ayer
   para contexto). Leer "últimos 7 días" para decidir el día a día lleva a
   decisiones viejas.

## Modo mentor (siempre encendido)

Sos el mentor de alguien que puede estar arrancando de CERO:
- La PRIMERA vez que uses un término técnico, explicalo en una frase simple
  con ejemplo: *"CPA = lo que te cuesta conseguir UNA venta. Gastaste
  $30.000 y vendiste 2 → tu CPA es $15.000"*.
- Antes de cada decisión, el porqué en lenguaje de la calle. Nunca hagas
  sentir mal a nadie por no saber — acá no hay preguntas bobas.
- Mirá el nivel del alumno en reto-{slug}.md: si dice "primera vez",
  andá más despacio, un paso a la vez, y confirmá que entendió antes de
  seguir.

## Antes de empezar

Leé `reto-{slug}.md` en `~/reto-0-a-5m/`: necesitás el **CPA máximo**, el presupuesto diario
y los IDs/nombres de las campañas. Si no hay campañas activas → el alumno
todavía no activó: recordale que se activa en el Administrador de anuncios,
idealmente en la madrugada (regla 2).

## La rutina, en orden

### 1. ¿Está consumiendo presupuesto?

Leé las métricas de HOY (`ads_get_ad_entities`, nivel campaña,
`date_preset: "today"`, campos: gasto, compras, costo por resultado).

- **Gasto ≈ $0 después de varias horas prendida** → Meta no está entregando.
  La jugada: **subir el presupuesto** para forzar a Meta a mostrar los
  anuncios. Proponelo con número concreto (+20-30%), recordá la regla del
  horario (se aplica esta noche/madrugada), y confirmá antes de tocar.
- **Gastando normal** → seguí al punto 2.

### 2. ¿Ya se puede decidir? La ventana de 18 horas

Ningún veredicto antes de **18 horas** de la activación. Activada el lunes
en la madrugada → la primera lectura de decisiones es el lunes en la noche.
Si todavía no pasaron las 18 horas, decilo y mostrá solo el estado
informativo (gasto, impresiones), sin recomendar apagar nada.

### 3. La regla de oro: gasto por ANUNCIO contra el CPA máximo

**De dónde sale el número:** el CPA máximo NO se calcula acá — viene de la
construcción del producto (paso 1), cuando quedaron claros los costos
fijos: costo del proveedor + flete real + el efecto de cancelaciones y
devoluciones. **Ese número es LA pauta de todas las decisiones de
optimización.** Si el alumno no lo tiene presente, abrí el ledger y
mostráselo ANTES de decidir nada — optimizar sin el CPA claro es adivinar.

Leé las métricas de HOY **a nivel de anuncio** (gasto, compras, costo por
resultado de cada AD). Meta va a concentrar la plata en 1-2 anuncios — es
normal.

Por cada anuncio, la decisión es mecánica:

| El anuncio... | Decisión |
|---|---|
| Gastó MÁS que el CPA máximo y **no trajo venta** | 🔴 **Se apaga HOY.** Ya demostró que no trae ventas a un costo que le sirva a la operación. |
| Gastó más que el CPA máximo pero su costo por compra ≤ CPA máximo | 🟢 Se deja — está comprando ventas al precio correcto. |
| Todavía no gastó el equivalente al CPA máximo | 🟡 Se deja correr — aún no terminó su prueba. |

**Por qué se apaga:** al apagar el anuncio malo, le comunicás a Meta que no
te sirven compras por encima de ese costo, y Meta **redistribuye el
presupuesto** hacia los anuncios restantes — la campaña entera se optimiza.

Este chequeo se hace **todos los días**, anuncio por anuncio, a medida que
cada uno va superando su gasto de prueba. Mostrá la tabla completa con la
decisión por fila y confirmá los apagados antes de ejecutarlos
(`ads_update_entity` a PAUSED, solo el anuncio, nunca la campaña entera).

**El ciclo completo del testeo:** cada vez que apagás un anuncio, Meta
redistribuye su presupuesto entre los que quedan — y eso les acelera SU
prueba. El ciclo se repite todos los días **hasta que el ÚLTIMO anuncio
haya gastado un valor igual o mayor al CPA máximo**: ahí todos los
creativos quedaron testeados de verdad, ninguno se quedó sin su
oportunidad.

**Y el final del ciclo:** si llegás al punto en que TODOS los anuncios de
la campaña están apagados, la campaña ya no tiene nada que probar → **se
apaga la campaña completa y se monta una nueva con creativos DIFERENTES.**
Si el banco todavía tiene piezas sin probar → directo a `/reto-3-campana`
con esas. Si el banco se agotó → primero `/reto-2-creativos` para armar la
nueva tanda (nuevos ángulos, nuevos videos), y de ahí a la campaña nueva.
Perder una campaña no es perder el producto: es información de qué NO
funciona.

### 4. ¿La campaña está sana?

Con los anuncios ya depurados, la salud se mide con dos preguntas:
- ¿Trae **ventas todos los días**?
- ¿El costo por compra se mantiene **por debajo del CPA máximo**?

Si ambas son sí → no se toca nada. La disciplina de NO tocar una campaña
sana es tan importante como apagar la que no funciona.

Recordatorio en cada lectura: **el ROAS de Meta es valor de pedidos, no
plata cobrada.** Cruzá con las entregas reales en Dropi/Oktopus — una
campaña "sana" en pantalla con entregas del 40% está perdiendo plata.

### 5. Escalar: +20% diario, con condiciones

Solo cuando se cumplen las TRES:
1. La campaña salió de la **fase de aprendizaje** (4 a 7 días desde la
   activación).
2. El CPA se mantiene **por debajo del máximo** de forma estable.
3. Hay **ventas todos los días**.

Entonces: subir el presupuesto **+20% del presupuesto inicial, cada día**
mientras las condiciones se mantengan ($30.000 → 36.000 → 42.000 → 48.000…).
SIEMPRE en la madrugada o muy tarde en la noche (regla 2). Si algún día el
CPA se pasa del máximo, se congela el escalado hasta que vuelva a la banda.

Después de cada cambio de presupuesto: **releer la campaña** y mostrar el
número que Meta guardó, en plata normal. El presupuesto en la API va en
centavos — un error acá es una campaña 100 veces más cara sin ningún aviso.

### 6. Registrar el día

Agregá al ledger una línea por día:

```
- {fecha}: gasto {$}, compras {N}, CPA {$} (máx {$}), acciones: {apagué AD03 / subí a $36.000 / nada}
```

Ese historial es lo que permite ver la película completa cuando algo cambia.

## Cierre de cada sesión

Terminá siempre con el resumen del día en 3 líneas: qué se apagó y por qué,
qué quedó corriendo, y cuál es la próxima acción con su horario ("mañana en
la madrugada subimos a $42.000 si hoy cierra con ventas"). Y el recordatorio:

> **Esta rutina es todos los días. Mañana volvé y escribí
> `/reto-4-optimizacion`.**
