---
name: reto-3-campana
description: "Paso 3 del Reto de 0 a 5 Millones: sube la campaña de Meta con la estructura probada del reto — campaña de Ventas CBO optimizada a compra, un solo conjunto abierto con Advantage+, exclusiones geográficas de Colombia, 3-5 anuncios del banco, y la campaña TEST#2 con los creativos restantes. Todo nace PAUSADO: el alumno activa a mano. Usar cuando el alumno diga 'subí la campaña', 'lancemos los anuncios', 'montá la pauta', '/reto-3-campana', o después de completar reto-2-creativos. Requiere el archivo reto-{slug}.md con pasos 1 y 2 completos."
---

# Reto · Paso 3 — La campaña

## Reglas de plata — LEER PRIMERO, van por encima de todo

1. **TODO nace PAUSADO.** El MCP oficial de Meta crea campañas, conjuntos y
   anuncios SIEMPRE en estado PAUSED — no existe un parámetro `status`, no
   intentes pasarlo. Confirmá el estado releyendo cada objeto después de
   crearlo. El alumno los activa CON SU MANO en el Administrador de
   anuncios. Esta skill JAMÁS activa nada que gaste.
2. **El presupuesto en la API de Meta va en subunidades de la moneda.** En
   la mayoría (COP, MXN, PEN, USD…) son centavos: $30.000 COP = `3000000`.
   **EXCEPCIÓN: CLP y PYG van en unidades enteras** ($15.000 CLP =
   `15000`). Equivocarse acá crea campañas 100 veces más caras o más
   baratas SIN ningún error visible. Por eso, siempre:
   - Confirmar la moneda real de la cuenta ANTES de calcular (del ledger;
     si no está, verificarla en vivo o pedirle al alumno que la lea en su
     Administrador de anuncios — NUNCA asumir).
   - Después de crear, RELEER la campaña y mostrarle al alumno el
     presupuesto que Meta guardó, en plata normal ("$30.000 COP por día").
   - **PASÓ DE VERDAD (28-ago-2026):** en una corrida real la campaña quedó
     creada con **$3.000.000 COP diarios en vez de $30.000** — cien veces el
     presupuesto, sin ningún error en pantalla. Por eso la relectura NO es
     opcional: si el número mostrado no coincide con lo acordado, se corrige
     AHÍ MISMO (`ads_update_entity`, campo `daily_budget`, en la subunidad
     correcta de la moneda) antes de dar cualquier otro paso. Una campaña
     con presupuesto errado no se deja viva NI pausada.
3. **Inicio SIEMPRE programado: el día siguiente a las 4:44 a.m.** (hora
   local del alumno). Las campañas nunca arrancan inmediatamente — se crean
   con `start_time` = mañana 4:44 a.m., para que Meta tenga la ventana del
   día completa. Así el alumno puede activarlas a cualquier hora sin
   arrancar desoptimizado: la entrega empieza sola a las 4:44. Verificar el
   inicio programado en la relectura, igual que el presupuesto.
4. **El MCP oficial NO tiene modo de prueba (dry_run).** La validación es en
   dos tiempos: (a) mostrar el plan completo al alumno y esperar su OK antes
   de crear NADA; (b) después de crear, releer cada objeto y confirmar
   presupuesto, inicio programado y estado antes de seguir.
5. Nunca inventar que algo quedó creado: cada objeto se confirma releyendo
   su ID en Meta.

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

**Diccionario rápido** (usalo cuando cada término aparezca por primera vez):

| Término | En cristiano |
|---|---|
| Campaña | La caja grande: define el objetivo y cuánta plata por día |
| CBO | Presupuesto a nivel campaña: le das la plata a la campaña y Meta la reparte sola entre tus anuncios — por eso el conjunto no lleva presupuesto propio |
| Advantage+ | Le permite a Meta ampliar el público más allá de lo definido si encuentra compradores afuera |
| Conjunto (ad set) | La caja del medio: define A QUIÉN se le muestran los anuncios |
| Creativo | La pieza que la gente ve: el video o la imagen |
| CPA | Lo que te cuesta conseguir UNA venta con publicidad |
| CPA máximo | Tu tope: una venta más cara que esto te hace perder plata |
| ROAS | Por cada peso que pusiste, cuántos volvieron en pedidos |
| Pixel | El chismoso: le cuenta a Meta quién compró en tu página para buscar más gente así |
| Fase de aprendizaje | Los primeros 4-7 días en los que Meta está aprendiendo a quién mostrarle tu anuncio |

## Antes de empezar

Leé `reto-{slug}.md` en `~/reto-0-a-5m/`. Exigí: landing publicada con pixel verificado ✅,
banco de creativos ✅, CPA máximo anotado. Falta algo → mandá al paso que
corresponde. Verificá con `ads_get_ad_accounts` que la cuenta sigue ACTIVE
y con método de pago.

## 1. El presupuesto decide cuántos anuncios (no al revés)

Preguntá cuánto va a invertir POR DÍA. Las reglas del reto:

- **Presupuesto diario = el MAYOR entre el 20% del precio del producto y
  $30.000 COP** (o su equivalente en la moneda del alumno). Producto de
  $99.900 → el 20% da $19.980, pero el mínimo manda: **$30.000/día**.
  Producto de $199.900 → $40.000/día.
- **Cada creativo necesita ~$6.000 COP/día para que Meta lo pruebe de
  verdad.** Presupuesto ÷ 6.000 = cuántos anuncios subir. **En el reto: entre
  3 y 5.** Con $30.000 → 5 anuncios.
- **TOPE DURO: con $30.000/día el máximo es 5 anuncios por campaña. NUNCA
  más.** ¿El alumno quiere más anuncios? Dos caminos: subir el presupuesto
  (~$6.000 por anuncio adicional) o guardar esas piezas para TEST#2 y la
  siguiente tanda. Más anuncios con el mismo presupuesto no prueba más —
  reparte migajas.

Explicale por qué: Meta no reparte la plata entre anuncios — elige uno en las
primeras horas y le mete casi todo. Subir 10 anuncios con $10.000 no prueba
10: prueba 1, mal. Y recordale su **CPA máximo** del ledger: *"cada venta te
puede costar hasta {cpa}. Más que eso, es pérdida."*

⚠️ Cuenta publicitaria NUEVA: recomendá arrancar con 3 anuncios, no 5. Meta
revisa con lupa las cuentas recién creadas y muchos anuncios de golpe el
primer día aumentan el riesgo de rechazo o restricción.

## 2. La estructura exacta (no negociable)

```
CAMPAÑA  "{DD/MM} | {PRODUCTO} VENTAS CBO | TEST#1"
  · Objetivo: Ventas (OUTCOME_SALES) · CBO con el presupuesto diario
  · Puja: mayor volumen (sin límite de costo)
  · Inicio: PROGRAMADO para el día siguiente, 4:44 a.m. hora local
  · El {DD/MM} del nombre = la fecha de INICIO (mañana), no la de hoy
  └── CONJUNTO  "Open - [1 Clic]"  (uno solo, sin presupuesto propio)
        · Optimización: conversiones (compra en el sitio web) con el pixel
          del ledger y evento COMPRA
        · País del alumno · edad 18-65 · todos los géneros
        · CERO intereses · Advantage+ audience ENCENDIDO
        · Ubicaciones automáticas (no tocar plataformas)
        └── ANUNCIOS  AD01, AD02, AD03 (… hasta AD05)
              · Creativo: imagen o video del banco (mezcla libre)
              · Texto/título/descripción: del copies.md
              · Destino: la URL de la landing
```

**Exclusiones geográficas para Colombia (SIEMPRE, como regiones/estados, no
ciudades — salvo Tumaco que sí es ciudad):**

| Excluir | Key de Meta |
|---|---|
| Amazonas | 716 |
| Chocó | 725 |
| Guaviare | 727 |
| Guainía | 728 |
| San Andrés y Providencia | 738 |
| Vaupés | 743 |
| Vichada | 744 |
| Tumaco (ciudad, Nariño) | 480911 |

Son zonas donde la entrega contra pago no funciona o la devolución es
altísima. **La tabla es la referencia esperada, no la fuente de verdad:
después de crear el conjunto, RELEÉ su targeting y mostrale al alumno los
NOMBRES de las regiones excluidas que Meta devolvió** — si un nombre no
coincide con la tabla, una key quedó corrida y se corrige antes de seguir.

**Si el alumno NO vende en Colombia:** revisá la cobertura de entrega de su
país con las herramientas de cobertura de Oktopus (`okto_dropi_coverage_*`)
— las zonas sin cobertura contra entrega son las zonas a excluir. Si no hay
dato claro, regla de fallback: lanzar a todo el país y en la revisión de
las 48 horas excluir las zonas que concentren cancelaciones, preguntando
además en la comunidad del reto por la lista de su país.

## 3. Mostrar el plan ANTES de crear

Armá el resumen en lenguaje humano y esperá el OK:

> Voy a crear: campaña "{nombre}" con ${X} diarios ({moneda}), 1 conjunto
> abierto a todo {país} (menos {exclusiones}), {N} anuncios con estos
> creativos: {lista}. TODO PAUSADO — no gasta nada hasta que vos la
> actives. ¿La creo?

## 4. Crear, en este orden, releyendo cada paso

Con el MCP oficial de Meta Ads:
1. **Subir los creativos** con `ads_creative_upload_media`: por URL pública
   directa (links de Drive/Dropbox NO sirven) o, para imágenes locales, el
   modo de archivo local. Video desde archivo local:
   `ads_creative_upload_video`.
   - ⚠️ **Los videos procesan en segundo plano**: esperá a que
     `ads_get_ad_videos` muestre el video listo (ready) antes de crear el
     creativo.
   - ⚠️ **Todo creativo de video EXIGE una miniatura** (imagen): subí un
     frame del video como imagen y usá su hash.
2. `ads_create_campaign` — objetivo OUTCOME_SALES, `buying_type: AUCTION`
   (obligatorio), CBO con el presupuesto en la subunidad correcta de la
   moneda (regla 2) → guardar el ID y confirmar que nació PAUSED.
3. `ads_create_ad_set` — "Open - [1 Clic]", `billing_event: IMPRESSIONS`
   (obligatorio), optimización a compra con el pixel del ledger y evento
   COMPRA, targeting del punto 2 con las exclusiones.
4. `ads_create_creative` + `ads_create_ad` por cada anuncio (AD01…), con la
   página de Facebook e Instagram del ledger.
   - **Cada AD lleva un combo distinto de copy**: AD01 = AIDA#1 + título#1 +
     descripción#1, AD02 = PAS#1 + título#2 + descripción#2, y así — se
     prueba copy Y creativo a la vez. Anotá en el ledger qué combo lleva
     cada AD.
5. **Releer la campaña completa** y mostrar: nombre, presupuesto en plata
   normal, conjunto (con los nombres de las exclusiones que Meta devolvió),
   anuncios y su estado. Si Meta rechaza un anuncio en revisión, decirlo tal
   cual con el motivo.

## 5. TEST#2 — la segunda mitad del banco

TEST#2 **no es una copia**: es otra campaña idéntica en configuración
(presupuesto, targeting, destino) pero con los creativos QUE NO entraron en
TEST#1 (los siguientes del banco) y nombres de anuncio nuevos (AD04, AD05…
o AD06 en adelante). **Cada anuncio conserva su combo de copy por anuncio**
— lo que cambia entre campañas es la pieza (video/imagen), no el sistema de
combos. Así Meta prueba el doble de piezas sin que compitan por el mismo
presupuesto.

Crearla igual que la primera, nombre "...| TEST#2", TODO PAUSADO.

⚠️ Avisá textual: *"si activás las DOS campañas, tu gasto diario es el DOBLE
del presupuesto ({2X} por día). Si tu plata alcanza para una sola, activá
TEST#1 y guardá TEST#2 para la semana siguiente."*

## 6. La entrega y las reglas de después

Actualizá el ledger (paso 3 ✅, IDs y nombres de campañas, presupuesto,
fecha) y entregá el cierre:

> **Tus dos campañas de {producto} están creadas, PAUSADAS y programadas
> para arrancar MAÑANA a las 4:44 a.m. Para activarlas: Administrador de
> anuncios → seleccioná la campaña → botón de encendido. Activálas hoy
> mismo tranquilo: no gastan un peso hasta las 4:44 — así Meta arranca con
> el día completo por delante.**
>
> Las reglas de operación, grabátelas:
> 1. **48 horas sin tocar nada.** Apagar un anuncio el primer día es apagarlo
>    antes de que Meta decidiera si le apostaba.
> 2. **Regla de matar:** ROAS = valor de los pedidos ÷ plata gastada en
>    pauta (ROAS 1 = en pantalla recuperaste lo invertido). Si gastaste 3-4
>    veces tu presupuesto diario y el ROAS no llega a 1 → se apaga. Sin
>    discusión y sin esperanza.
> 3. ROAS arriba de 2 → se deja correr. Arriba de 4 → se escala.
> 4. **El ROAS de Meta NO es tu plata.** Es valor de pedidos atribuidos. En
>    contra entrega solo cobrás lo que se ENTREGA: un ROAS de 2,4 en pantalla
>    con 60% de entrega es ~1,4 real, antes del producto y el flete. Cruzalo
>    SIEMPRE con las entregas reales en Dropi/Oktopus.
> 5. Tu CPA máximo es {cpa}: si el costo por compra lo pasa, volvé acá y
>    revisamos juntos qué está fallando (creativo, precio o landing).
> 6. **El arranque a las 4:44 a.m. no es capricho:** Meta reparte el
>    presupuesto en la ventana del día. Una campaña que arranca a las 6 pm
>    solo tiene 6 horas para gastar lo de 24 y nace desoptimizada. Por eso
>    todas quedan programadas de madrugada — y si algún día creás una a
>    mano, programala igual.
>
> **Desde mañana, tu rutina diaria es `/reto-4-optimizacion`.** Ahí se
> decide qué anuncio se apaga, cuándo se escala y cómo se cuida tu plata.
