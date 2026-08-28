---
name: reto-2-creativos
description: "Paso 2 del Reto de 0 a 5 Millones: arma el banco publicitario del producto — 5 imágenes de anuncio generadas con IA en Oktopus, banco de videos investigando la Biblioteca de Anuncios de Meta en OTROS países para modelar (no robar), y el archivo de copies con AIDA y PAS más los gatillos de título y descripción. Usar cuando el alumno diga 'armá mis creativos', 'busca videos del producto', 'los copies', 'banco de anuncios', '/reto-2-creativos', o después de completar reto-1-producto. Este paso NO gasta plata en pauta. Requiere el archivo reto-{slug}.md; si no existe, correr /reto-0-requisitos."
---

# Reto · Paso 2 — Banco de creativos y copies

Acá se construye TODO lo que la campaña va a necesitar, sin gastar un peso de
pauta. Al terminar, el alumno tiene una carpeta ordenada con imágenes, videos
y un archivo de copies listo.

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

Leé `reto-{slug}.md` en `~/reto-0-a-5m/`. Sin paso 1 completo →
`/reto-1-producto` primero. **El slug es el mismo del archivo que estás
leyendo** — usalo idéntico para las carpetas.

Creá (si no existe) la estructura y decísela al alumno:

```
~/reto-0-a-5m/reto-productos/{slug}/
├── imagenes-anuncio/
├── videos/
└── copies.md
```

## 1. Las 5 imágenes de anuncio (Oktopus, estilo ads)

Las imágenes de anuncio NO son las de la página: se generan con estilo
publicitario (texto encima, badges). `okto_product_images_generate` con
`purpose: "ads"` y `set: "custom"` con estos 5 tipos — los que más venden:

`["urgency", "socialproof", "transformation", "comparison", "problem"]`

Tarda 3-6 minutos — avisá que no cierre la ventana. Verificá con
`okto_product_images_get` pasando `purpose: "ads"`: devuelve las URLs
públicas de cada imagen. **Descargá cada una a `imagenes-anuncio/`** (curl a
archivo) y verificá que quedaron 5 archivos con peso real antes de marcar
nada. Si una descarga falla, reintentá; si el banco tiene menos de 5, la
generación se cortó por el límite del plan — decilo tal cual.

## 2. El banco de videos

Preguntá primero: *"¿Ya tenés videos del producto (propios o descargados)?"*

**Si ya tiene** → que los ponga en `videos/` y seguí al punto 3.

**Si no tiene**, investigá la Biblioteca de Anuncios de Meta con
`ads_library_search` (MCP oficial de Meta):

- Buscá el nombre del producto **país por país, en OTROS países** distintos
  al del alumno (si vende en Colombia: México, España, Ecuador, Perú, Chile).
  **Regla del reto: se modelan competidores de OTROS países** — se estudia lo
  que ya funciona afuera, no se le copia al vecino.
- La herramienta devuelve hasta 50 anuncios por llamada: quién lo anuncia,
  desde cuándo corre y el link de cada anuncio. NO devuelve el video en sí.
- **Verificá que sea EXACTAMENTE el mismo producto** (mismo aparato, misma
  presentación) — mismo nombre no siempre es mismo producto. Si dudás,
  mostrale los links al alumno para que confirme con sus ojos.
- La señal de producto ganador: un anunciante con MUCHOS anuncios casi
  iguales del mismo producto (aparece repetido 8-10 veces) y campañas
  corriendo hace semanas. Eso es plata invertida sostenida.

Entregale una lista corta y accionable:

```
{Anunciante} — {país} — corriendo desde {fecha} — {N} anuncios del producto
→ {link del anuncio}
```

Que abra los links y ESTUDIE los 3-5 mejores. Y acá la verdad importante:
**modelar no es descargar el archivo del competidor** — es quedarse con la
estructura (gancho → problema → demostración → oferta) y hacer la versión
propia. Los caminos reales para llenar `videos/`:

- **Grabar su versión con el celular** siguiendo esa estructura cuando le
  llegue el producto (pedirse una unidad a sí mismo es jugada estándar).
- **Pedirle material al proveedor en Dropi** — la ficha del producto suele
  traer videos y fotos descargables del proveedor.
- **Videos que ya tenga** de otras fuentes propias.

⚠️ Si hoy no hay videos, NO es bloqueante: el paso 2 se puede cerrar solo
con las 5 imágenes, marcando videos como "pendiente" en el ledger. La
campaña del paso 3 acepta imagen o video — se arranca con imágenes y los
videos entran en la segunda tanda.

**TikTok**: buscar a mano el nombre del producto + "review" en TikTok — guía:
los videos con más comentarios de compra son los que muestran el problema en
los primeros 2 segundos. **Pinterest** sirve solo como referencia visual de
fotos, no tiene biblioteca de anuncios.

**Apify (opcional, solo si el alumno lo pide):** para barridos grandes de la
Biblioteca de Anuncios existe Apify — da US$5 de crédito al mes gratis, sin
tarjeta. Si el alumno quiere ese nivel, guialo: crear cuenta en apify.com,
conectar el MCP de Apify, y usar el actor de Facebook Ad Library. No es
requisito del reto: la búsqueda de arriba alcanza para empezar.

## 3. El archivo de copies

Escribí `copies.md` con TODO lo que la campaña necesita. Usá el nombre del
producto, su beneficio central y el dolor que resuelve (están en la landing).

### Estructura del anuncio en Meta (así se llena):

- **Texto principal** (el largo, arriba del video) → acá va la estructura
  de copywriting.
- **Título** → gatillo de prueba social o urgencia.
- **Descripción** → el otro gatillo + contra entrega.

### Los textos largos — 2 con AIDA y 2 con PAS:

**AIDA** (Atención → Interés → Deseo → Acción): gancho que detiene el
scroll → dato o beneficio que engancha → cómo se ve su vida con el producto
→ orden clara de compra con el CTA.

**PAS** (Problema → Agitar → Solución): nombrar el dolor exacto → hacerlo
sentir (qué pasa si sigue así) → el producto como salida + CTA.

Reglas de los textos: lenguaje de la calle del país, frases cortas, cero
tecnicismos, emojis con moderación, y SIEMPRE cerrar con contra entrega
("pagas cuando te llega") — es el quitamiedos #1 en LATAM. Ojo: esta guía
te habla de vos, pero **TUS anuncios se escriben como habla el cliente de
tu país** (Colombia: tú/usted). No mezcles.

### Títulos y descripciones — 5 de cada uno, con gatillos:

Prueba social: "Más de {N} unidades vendidas", "⭐⭐⭐⭐⭐ Clientes felices".
Urgencia/escasez: "Últimas unidades disponibles", "Solo por hoy: envío gratis".
Contra entrega: "Paga al recibir en tu casa 🚚".

⚠️ Regla de honestidad: los números de prueba social deben ser sostenibles —
estos productos ya venden en el mercado, por eso se eligen productos
validados en el paso del cazador. Si un número no es defendible, cambialo
por uno que sí.

## 4. La regla del carácter (leésela textual al alumno)

> Hay dos tipos de personas frente a la IA: las que hacen lo que la IA dice,
> y las que le dicen a la IA qué hacer. **Nada de lo que acabo de generar se
> publica tal cual.** Leé los copies en voz alta, mirá las imágenes, y pedime
> cambios con criterio: "el gancho no detiene a nadie", "esa imagen no se ve
> real", "hablale a una mamá de 45, no a un joven". Entre más vueltas le des
> con carácter, mejor queda. Iterá conmigo AHORA, antes de gastar un peso.

Iterá lo que pida. Solo cuando el alumno diga que está conforme, cerrá.

## 5. Cerrar

Checklist de salida (mostrala):
- [ ] 5 imágenes de anuncio descargadas en `imagenes-anuncio/`
- [ ] Videos en `videos/` — o marcados "pendiente" en el ledger (no bloquea)
- [ ] `copies.md` con 4 textos largos + 5 títulos + 5 descripciones, iterados

Actualizá el ledger (paso 2 ✅, ruta de la carpeta) y cerrá con:

> **Tu banco de {producto} está completo. Cuando quieras subir la campaña,
> escribí `/reto-3-campana`. Vas a necesitar decidir cuánta plata vas a
> invertir por día — pensalo: la regla es el 20% del precio de tu producto,
> mínimo $30.000 COP diarios.**
