---
name: reto-diagnostico
description: "Skill de servicio del Reto de 0 a 5 Millones: lee los archivos reto-{slug}.md del alumno y le dice en qué paso quedó cada producto, qué está verificado de verdad y cuál es el siguiente comando. Usar cuando el alumno diga 'dónde iba', 'qué me falta', 'se me cerró la ventana', 'estado de mis productos', '/reto-diagnostico', o cuando retome después de días sin trabajar. También sirve para detectar productos a medio montar."
---

# Reto · Diagnóstico — ¿Dónde iba?

Modo mentor: explicá cada término técnico la primera vez, en una frase
simple. Si el ledger dice "primera vez", andá más despacio.

Buscá todos los archivos `reto-*.md` en `~/reto-0-a-5m/` (y de paso en la
carpeta actual, por si el alumno los creó antes de la convención). Para cada uno, leé el progreso y VERIFICÁ
lo verificable en vivo — no te fíes solo de lo que dice el archivo:

- Paso 1 marcado ✅ → confirmá con `okto_landings_list` que la landing sigue
  publicada, y si hay URL, que responde.
- Paso 2 marcado ✅ → verificá que `reto-productos/{slug}/` contiene las 5
  imágenes, `copies.md` y (si declaró videos) los archivos de video.
  Carpeta vacía o incompleta = ⏳ con el detalle de qué falta.
- Paso 3 marcado ✅ → confirmá con el MCP de Meta que las campañas existen y
  mostrá su estado real (pausada / activa / rechazada) y el gasto si ya corre.

Mostrá una tabla, un producto por fila:

| Producto | País | Paso 0 | Paso 1 | Paso 2 | Paso 3 | Siguiente |
|---|---|---|---|---|---|---|
| {nombre} | CO | ✅ | ✅ | ⏳ videos | — | `/reto-2-creativos` |

Y debajo, para el producto más avanzado, el "siguiente paso" en una frase
con el comando exacto.

Si NO hay ningún archivo `reto-*.md` → el alumno no ha empezado:

> Todavía no tenés productos en el reto. Escribí `/reto-0-requisitos` para
> arrancar con tu primer producto.

Si un archivo dice ✅ pero la verificación en vivo falla (landing caída,
campaña que no existe), decilo claro y ofrecé el paso para repararlo. Nunca
reportes verde algo que no comprobaste en vivo.

Si hay campañas ACTIVAS, agregá el mini-informe de operación:
- Gasto acumulado vs presupuesto diario (¿ya llegó a 3-4× sin ROAS 1? →
  recordá la regla de matar).
- Recordatorio: el ROAS de Meta es valor de pedido, no plata cobrada —
  cruzar con entregas reales en Dropi.
- Y el empujón: la rutina diaria de campañas activas es
  `/reto-4-optimizacion` — si el último registro del ledger tiene más de un
  día, recordáselo.

Si el alumno reporta un problema DE Oktopus (algo que no carga, un error de
la plataforma), canalizalo con `okto_support_ask` — es el canal oficial de
soporte y responde en la misma conversación. La documentación viva está en
oktopus.lat/docs.
