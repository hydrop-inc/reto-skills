---
name: reto-1-producto
description: "Paso 1 del Reto de 0 a 5 Millones: importa el producto desde Dropi a Oktopus con el precio inteligente calculado, instala el pixel ANTES de publicar, genera las 16 imágenes IA de la página (no solo las 6 por defecto), crea la landing completa y la revisa — precio incluido — antes de dejarla publicada. Usar cuando el alumno diga 'montá mi producto', 'creá la landing', 'importá el producto', '/reto-1-producto', o después de completar reto-0-requisitos. Requiere el archivo reto-{slug}.md creado por reto-0-requisitos; si no existe, correr /reto-0-requisitos primero."
---

# Reto · Paso 1 — Producto, precio y landing

## Reglas de plata (leer primero, cumplir siempre)

1. **El pixel se instala ANTES de publicar la landing.** El pixel se "hornea"
   en el momento de publicar; si se pone después, la página queda al aire SIN
   medición y toda la pauta corre a ciegas. Si alguna vez se setea el pixel con
   landings ya publicadas → **hay que REPUBLICAR cada una** para que quede.
2. **El precio lo pone la fórmula, nunca el azar.** Si importás sin precio,
   Oktopus usa el sugerido de Dropi o costo×2 — a ciegas. Siempre calcular
   el precio con el modelo de abajo y pasarlo explícito.
3. **Publicar la landing requiere confirmación del alumno.** Cualquier
   bajada de precio también se le confirma (regla del reto). Además, bajadas
   de más del 50% las bloquea Oktopus mismo y piden reconfirmar.
4. Generar imágenes consume el límite del plan del alumno — respetar el tope
   de este paso: **16 imágenes web**. Las de anuncio van en el paso 2.

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

Leé `reto-{slug}.md` en `~/reto-0-a-5m/`. Si no existe o el paso 0 no está ✅ → mandá al
alumno a `/reto-0-requisitos`. Si el paso 1 ya figura completo, mostrá lo que
ya está (landing, URL) y preguntá si quiere rehacer algo puntual.

## 1. Calcular el Precio Inteligente (el mismo modelo de Oktopus)

Primero la sigla, la primera vez: **CPA = costo por adquisición — cuánta
publicidad te cuesta lograr UNA venta.** En la fórmula se usa como % del
precio (la meta: que cada venta cueste ~20% del ticket); más abajo sale el
**CPA máximo**, que es otra cosa: el TECHO donde dejás de ganar. Entre uno
y otro está tu zona de utilidad.

Es el modelo real del validador de precios de Oktopus — por unidad ENTREGADA,
teniendo en cuenta cancelaciones y devoluciones:

```
fleteReal = flete ÷ (1 − devolución)
denominador = (1 − margen) − CPA% ÷ [(1−cancelación) × (1−devolución)]
precio = (costoProducto + fleteReal) ÷ denominador
```

Benchmarks por país (los mismos que pre-llena Oktopus; CPA 20% del ticket y
margen objetivo 25% son globales):

| País | Flete base | Cancelación | Devolución | Redondeo |
|---|---|---|---|---|
| Colombia | $16.000 COP | 20% | 20% | a miles |
| México | $230 MXN | 20% | 25% | a 50 |
| Ecuador | $6 USD | 20% | 20% | a 1 |
| Perú | S/18 PEN | 20% | 22% | a 5 |
| Chile | $4.500 CLP | 18% | 15% | a miles |
| Paraguay | ₲25.000 PYG | 20% | 20% | a 5.000 |
| Argentina | $8.000 ARS | 22% | 25% | a 500 |
| Guatemala | Q35 GTQ | 20% | 20% | a 5 |
| Panamá | $8 USD | 18% | 15% | a 1 |
| España | €6 EUR | 15% | 10% | a 1 |

**Ejemplo Colombia** — costo Dropi $40.000:
fleteReal = 16.000÷0,8 = 20.000 · denominador = 0,75 − 0,2÷0,64 = 0,4375
precio = 60.000 ÷ 0,4375 = **$137.143** → con precio psicológico: **$137.900**

**Precio psicológico (charm):** redondear HACIA ARRIBA al múltiplo del país y
restarle un pasito (Colombia: 138.000 → 137.900). Si el precio cae apenas
(≤3%) por ENCIMA de un número redondo fuerte — los que empiezan en 1, 2 o 5:
50.000, 100.000, 200.000, 500.000... (150.000 NO cuenta) — bajarlo al
casi-redondo de abajo, que es el mil anterior al umbral: 100.500 → 99.000
(así lo hace Oktopus: el efecto de romper el "1" de adelante vale más que
los $900). NUNCA por debajo del punto de equilibrio.

**Los dos números que el alumno anota:**
- **Precio de venta** (el de arriba).
- **CPA máximo** = [precio − costoProducto − fleteReal] × (1−cancelación) ×
  (1−devolución). Ejemplo: (137.900−40.000−20.000)×0,64 = **$49.856**.
  Decíselo así: *"si conseguir UNA venta te cuesta más de $49.856 de
  publicidad, estás perdiendo plata. Ese es tu número rey."*
  **Este número manda TODAS las decisiones de la optimización diaria
  (paso 4):** anuncio que lo gasta sin traer venta, se apaga. Por eso se
  calcula ACÁ, con los costos fijos claros — no se improvisa después.

**Chequeo de cordura:** si el precio calculado es menor que 3× el costo del
proveedor, avisá: en contra entrega, por debajo de 3× el costo el negocio
no aguanta la pauta. Sugerí buscar otro producto o subir el margen.

## 2. Instalar el pixel en la tienda

`okto_pixel_set` con el `store_id` y el `dataset_id` (pixel) del ledger.
Verificá con `okto_pixel_get`. Esto va ANTES de cualquier publish.

## 3. Importar el producto

`okto_dropi_import_product` con `store_id`, `dropi_product_id` y **`price` =
el precio calculado**. Después revisá el resultado:
- El catálogo de Dropi a veces trae el nombre sucio o mezclado con la
  descripción (mayúsculas, "PRODUCTO GANADOR 🔥", teléfonos de proveedores).
  Limpiá nombre y descripción con `okto_product_update` — nombre corto y
  humano, descripción sin datos del proveedor.

## 4. Generar el banco de imágenes de la página — las 16, no 6

Oktopus por defecto genera SOLO 6 imágenes. Hay 10 más con gatillos mentales
que nadie recibe si no las pide. Acá se piden siempre:

1. `okto_product_images_generate` con `set: "basic"` (6: hero, lifestyle,
   antes/después, social, pack, contra entrega).
2. `okto_product_images_generate` con `set: "triggers"` (10: urgencia,
   escasez, autoridad, prueba social, transformación, unboxing, garantía,
   comparación, combo, problema).

Cada tanda tarda **3-6 minutos**. Decilo textual: *"esto tarda unos minutos,
no cierres la ventana"*. Verificá el avance con `okto_product_images_get`
pasando `purpose: "web"` hasta ver las imágenes listas. Si el plan del alumno se queda
sin límite de imágenes a mitad de camino, la generación se corta: avisale
cuánto salió y que puede subir de plan en Oktopus para completar.

## 5. Crear la landing

`okto_landing_generate_full` con el `product_id`, `template: "aurora"` y
`auto_publish: false`. Tarda ~1-2 minutos; verificá con `okto_landings_list`.

## 6. Revisar ANTES de publicar

Con `okto_landing_get` (copy, imágenes, URL) y el precio directamente en el
producto (`okto_product_lookup` o `okto_products_list`):
- **Precio**: ¿es EXACTAMENTE el calculado en el punto 1? Si no (quedó el de
  Dropi o costo×2, o un número "feo" tipo $103.400), corregí el precio del
  producto con `okto_product_price_update` al precio charm correcto.
  ⚠️ Bajadas de más del 50% las bloquea Oktopus y piden confirmación — y
  cualquier cambio de precio lo ven los clientes al instante.
- **Copy**: título claro, sin texto del proveedor, promesa concreta.
- **Imágenes**: que use las generadas, no las fotos crudas de Dropi.

Mostrale al alumno el resumen (precio, título, qué imágenes tiene) y pedí su
OK para publicar.

## 7. Publicar y verificar de verdad

Con el OK: `okto_landing_publish`. Cuando esté publicada:
1. Traé el HTML de la URL pública y verificá DOS cosas adentro: que el
   **precio renderizado** es el correcto y que el **ID del pixel** aparece en
   el código. Si el pixel no está → republicar. Sin esta verificación no se
   declara terminado.
2. Mostrale la URL al alumno para que la abra en su celular.

## 8. Cerrar

Actualizá el ledger: precio, CPA máximo, product_id, landing URL, imágenes
generadas, fecha. Marcá el paso 1 ✅ y cerrá con:

> **Tu página de {producto} está publicada en {URL}, con tu pixel instalado
> y tu precio inteligente de {precio}. Tu CPA máximo es {cpa}: grabátelo.
> Cuando quieras armar tus anuncios, escribí `/reto-2-creativos`.**
