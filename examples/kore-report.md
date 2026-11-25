# Reporte de ejemplo — KÓRE Studio

> ⚠️ **SEÑAL SIMULADA, NO EVIDENCIA DE USUARIOS REALES.** Generado con `virtual-user-test`.

**Config:**
- `url`: http://localhost:3001/ (home 3D de KÓRE Studio, Córdoba AR)
- `total`: 40 personas
- `navPercent`: 20% (8 personas navegaron con browser real)
- `segments`: dueño de pyme cordobesa, gerente de marketing, founder tech, freelancer diseñador, director de ecommerce, dueño de negocio tradicional

---

## 📊 Conversión estimada: 37,5% (15/40)

| Verdict | Cantidad | % |
|---|---|---|
| buy | 1 | 2,5% |
| contact | 14 | 35% |
| unsure | 14 | 35% |
| leave | 11 | 27,5% |

**Conversión = buy + contact = 15/40 = 37,5%**

---

## 🔥 Top 9 fricciones (detectadas en navegación real)

1. **El sitio se cayó durante la navegación.** Al llegar al capítulo 09 apareció un overlay "Console Error" de Next.js (`LiquidMetalNavbar.js`) y luego `ERR_CONNECTION_REFUSED`. El servidor se reinició. Para dos personas fue el golpe final: `wouldContinue: false`.
2. **CTA ausente durante el 80% del recorrido.** El botón de WhatsApp está en el hero y reaparece recién en el capítulo 09 (~90% del scroll, ~45 s de recorrido). En medio, solo el navbar.
3. **Navbar engañoso.** "Inicio / Servicios / Trabajos / Estudio / Contacto" apuntan todos a `#h3d-contenido`. "Trabajos" no lleva al portfolio real (que sí existe en el capítulo 08).
4. **Lenguaje técnico / en inglés.** "Del stack a la interfaz", "Build / Fix / Service / Learn", "Automatizaciones", "SEO local" y un **"Subscribe to newsletter" en el pie** con campo "tu@negocio.com" — confunden a perfiles de bajo techComfort y a negocios tradicionales, que no saben si eso es para contactar o para suscribirse.
5. **H1 sin espacios en el DOM.** `innerText` devuelve "PROBLEMASDENEGOCIO.SOLUCIONESATUMEDIDA." (y "CINCOPASOS,UNDESTINO."), visualmente están bien pero lectores de pantalla y copiado sufren.

**Fricciones adicionales del scoring:** la experiencia 3D no es scannable (exige scroll para revelar todo), el tono oscuro y precios en USD no transmiten calidez a negocios locales, y el portfolio no muestra "antes y después" concreto. Los precios aparecen sin detalle de qué incluye cada uno ni cómo pedir presupuesto concreto.

**Fricción de interacción detectada por 2 personas más:** hacer scroll al 30% las dejó todavía en el hero (capítulo 01). La progresión por capítulos no se percibe predecible, lo que con attention spans de 35-55 s se siente como espera. El hint "DESPLAZATE PARA RECORRER LA PIEZA" no dice cuánto falta ni qué hay al final.

**Nota de navegación:** de 6 personas que navegaron, 3 dijeron `wouldContinue: true` (con reservas) y 3 `false` (2 por la caída del sitio, 1 por no encontrar portfolio de su rubro). Todas encontraron el CTA del hero en ~5 s y todas coincidieron en que desaparece hasta el ~90%.

6. **Scroll imprevisible.** Es velocity-driven con snap: un gesto de 2300 px aterrizó en 1229 y luego siguió a 1917; otro de 2900 px saltó del capítulo 03 directo al footer (~7983 px). No se puede recorrer el embudo de forma lineal.
7. **Badge "5 Issues / 7 Issues" de Next.js visible** en la esquina inferior durante todo el recorrido: señal de sitio sin terminar frente a quien evalúa al proveedor.
8. **Sin analítica ni UTMs.** No hay GA4/GTM/Plausible/PostHog/Clarity en el DOM; los links `wa.me` van al mismo número sin parámetros de campaña. Quien mide tráfico pagado ve todo como "directo". Además dos nombres distintos ("Agendar diagnóstico estratégico" y "Contame tu proyecto") para el mismo destino generan ruido de embudo.
9. **El sol del render 3D quema "KÓRE STUDIO" en el cierre**, dejándolo casi ilegible.

**Actualización 2026-11-25:** la caída se reprodujo en una verificación posterior: `net::ERR_CONNECTION_REFUSED` al intentar navegar de nuevo. El servidor de desarrollo no se mantuvo estable bajo navegación concurrente. En producción esto sería un bloqueador total de conversión.

**Nota final de navegación (8/8 personas):** 4 dijeron `wouldContinue: true` (con reservas) y 4 `false` (2 por la caída, 1 por no ver portfolio de su rubro, 1 por falta de tracking y UTMs). Todas encontraron el CTA del hero en ~5 s y todas coincidieron en que desaparece hasta el ~90%. Una persona hizo click real en "Contame tu proyecto" y se abrió wa.me correctamente.

---

## ✅ Top 5 recomendaciones

1. **Estabilizar el runtime** antes de cualquier optimización de conversión: el error de Next.js con `LiquidMetalNavbar.js` tiró el sitio en plena navegación. Un sitio que se cae convierte 0.
2. **CTA persistente** (sticky, arriba o abajo) durante todo el scroll, no solo en el hero y el cierre.
3. **Navbar funcional**: mapear cada item a su capítulo real (`#h3d-ch-04` para Servicios, `#h3d-ch-08` para Trabajos, etc.) o cambiar los labels a anclas reales.
4. **Bajar el nivel técnico del copy** en el hero: reemplazar "Del stack a la interfaz" por "Hacemos que tu web se vea profesional y te traiga consultas". Dejar "stack/build/fix" para el capítulo 04 (STACK), donde sí tiene contexto.
5. **Arreglar el H1** para que `innerText` tenga espacios reales (agrupar caracteres por palabra con `white-space: nowrap` y espacios literales entre palabras).
6. **Sacar o traducir el "Subscribe to newsletter" del pie.** Si no hay newsletter real, reemplazarlo por un "Escribinos por WhatsApp" con el mismo número; si la hay, etiquetarla en español y explicar para qué es.
7. **Hacer visible la progresión.** El hint debería indicar capítulo actual y total (ej. "01 / 10") o una barra de progreso, para que el scroll se sienta Responsive y no como espera a ciegas.
8. **Desglosar qué incluye cada precio.** Agregar bajo cada item de STACK una línea concreta ("incluye: diseño + desarrollo + publicación + 30 días de ajustes") para convertir el precio en presupuesto comprensible.
9. **Domar el scroll y sacar el badge de issues.** Limitar el velocity-driven a un capítulo por gesto (o snap suave a capítulos) y desactivar el indicator de dev Issues en producción.
10. **Agregar analítica y UTMs.** Al menos Plausible o GA4, y parámetros de campaña en los links wa.me; unificar los dos CTAs bajo un mismo nombre.

---

## 📈 Desglose por segmento (scoring)

| Segmento | Contactan / Compran | Leave | Unsure |
|---|---|---|---|
| Dueño de pyme cordobesa | alta | media | media |
| Gerente de marketing | media-alta | baja | media |
| Founder tech | media | media | alta |
| Freelancer diseñador | media-alta (valora identidad y casos reales) | media | media |
| Director de ecommerce | baja (sin analítica ni UTMs es un bloqueador) | alta | media |
| Dueño de negocio tradicional | baja | alta | media |

> El segmento tradicional es el que peor convierte: lenguaje técnico + estética fría + precios en USD. El de ecommerce se cae por la falta de tracking. El de diseño valora la identidad pero penaliza el menú roto.

---

## 💬 Quotes representativos (en carácter)

> "Me gustó que el botón de WhatsApp estuviera apenas entré, pero después me encontré con un montón de palabras en inglés y de 'automatizaciones' que no me dicen nada de lo que yo hago con las manos. Buscaba ver cosas lindas y hechas a mano, no un manual de tecnología. Y cuando el sitio se me rompió con un error raro, ahí sí me desanimé del todo." — Persona 5, dueña de negocio tradicional

> "Yo lo que quería era ver en tres segundos quiénes son y el botón de WhatsApp bien grande, y eso al principio estaba. Pero después viene todo eso de 'stack', 'build', 'automatizaciones' que no entiendo, y el menú no me lleva a ningún lado. Encima se me cayó la página con un error. Para mí, ahí me bajo." — Persona 6, dueño de negocio tradicional

> "Mirá, al principio vi el botoncito de WhatsApp y me pareció bien, pero después me quedé en la oscuridad y no sabía si había más o ya estaba todo. Las palabras se me juntan y el menú no me lleva a ningún lado. Igual el WhatsApp funciona y al final lo encontré, así que le doy una oportunidad." — Persona 1, dueño de pyme cordobesa

> "El mensaje de problemas de negocio y soluciones está bien, y los pasos de trabajo se entienden. Pero entre tanta animación y letras pegadas, no termino de ver la seriedad que uno espera de un estudio. El WhatsApp al final funciona, pero me gustaría que estuviera presente en todo el recorrido." — Persona 2, gerente de marketing

> "La identidad está muy bien resuelta: se nota que hay criterio detrás y que los casos son reales, no stock. Pero me molesta que 'Inicio', 'Servicios' y 'Trabajos' no me lleven a ningún lado, y que el contacto desaparezca justo cuando quiero ver el portfolio." — Persona 7, freelancer diseñadora

> "El hero comunica bien y el WhatsApp está a mano, pero necesito ver cómo mido las campañas: no hay ningún tag y los links no tienen UTM, así que todo el tráfico va a llegar como 'directo'. Además 'Agendar diagnóstico' y 'Contame tu proyecto' son el mismo número con dos nombres distintos; eso me genera ruido en el funnel." — Persona 8, directora de ecommerce

> (Del scoring) "Los precios están claros y el proceso en 5 pasos me da confianza, pero necesito ver un caso parecido al mio antes de escribirle por WhatsApp." — Persona 12, gerente de marketing

---

## ⚠️ Caveats

- Resultados **simulados**: los verdicts son predicciones de un modelo, no comportamiento observado de humanos.
- La tasa de conversión real depende de tráfico, precio, competencia y confianza; este número es solo una señal direccional.
- 8 personas navegaron de verdad; 32 fueron scored en batch contra capturas. Los friction points de navegación real tienen más peso.
- El error de Next.js observado puede ser específico del entorno de desarrollo (dev server con HMR). Verificar en producción antes de concluir.
- No se inventaron testimonios, métricas, clientes ni certificaciones. Los precios citados están en el sitio (USD 1.900 / 2.900 / 4.900).
