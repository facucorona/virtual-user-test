# Virtual User Test — AI Skill

[![OpenCode](https://img.shields.io/badge/OpenCode-native-blueviolet?logo=opencode)](https://opencode.ai)
[![Claude Code](https://img.shields.io/badge/Claude%20Code-compatible-orange?logo=anthropic)](https://claude.ai)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![Version](https://img.shields.io/badge/version-1.0.0-brightgreen)](package.json)

> **¿Vende tu sitio?** Simulá decenas o cientos de usuarios virtuales con personalidad real, hacé que una muestra navegue de verdad y estimá la conversión.

Alternate between **real browser navigation** (detecta fricción real) y **mass AI scoring** (escala a cientos de personas). OpenCode-native: usa `browse` + `subagent`, sin workflow engine externo.

---

## ¿Qué hace?

1. **Genera personas** con demografía, presupuesto, tech comfort, attention span, objetivos, red flags y qué las convierte.
2. **Navega el sitio con una muestra real** ( Chromium/Playwright vía `browse`): scroll, clicks, tiempos, CTAs.
3. **Hace scoring masivo** de todas las personas contra el sitio + la fricción real detectada.
4. **Consolida** un reporte: tasa de conversión simulada, top friction points, recomendaciones y quotes.

Todo con una regla de oro: **nunca inventes testimonios, métricas, clientes, certificaciones ni precios**. Solo lo que está en el sitio.

---

## Instalación

### OpenCode (nativo)
```bash
git clone https://github.com/facucorona/virtual-user-test.git ~/.opencode/skills/virtual-user-test
```

### Claude Code
```bash
npx skills add facucorona/virtual-user-test
```
o
```bash
git clone https://github.com/facucorona/virtual-user-test.git ~/.claude/skills/virtual-user-test
```

### Vía marketplace de Claude Code
```
/plugin marketplace add facucorona/virtual-user-test
```

---

## Uso

```
/virtual-user-test url=http://localhost:3001 total=40 navPercent=20 segments="dueño pyme, gerente marketing, founder tech, freelancer diseñador, director ecommerce"
```

O en lenguaje natural:

> "testea si mi sitio vende con 50 usuarios virtuales, que el 20% navegue de verdad"

### Parámetros

| Parámetro | Default | Descripción |
|---|---|---|
| `url` | — | URL del sitio a testear (requerido) |
| `total` | `50` | Cantidad de personas a generar (escalable a 200+) |
| `navPercent` | `15` | % que navega el sitio de verdad (mínimo 3) |
| `segments` | auto | Segmentos a forzar, separados por coma |

---

## Ejemplo de output

```markdown
## 📊 Tasa de conversión simulada: 22% (9/40)

### Top 5 fricciones
1. El CTA principal no es visible sin scroll en mobile
2. No hay precios ni señales de confianza verificables
3. La propuesta de valor es abstracta ("del stack a la interfaz")
4. El portfolio no muestra resultados concretos
5. El footer no tiene un contacto directo directo

### Top 5 recomendaciones
1. Subir el CTA al hero con contraste alto
2. Agregar un contacto claro (WhatsApp) arriba
3. ...

### Segmentos
| Segmento | Conversión |
|---|---|
| Dueño pyme | 30% |
| Gerente marketing | 25% |
| Founder tech | 10% |
```

Ver el [reporte completo de ejemplo](examples/kore-report.md).

---

## Arquitectura

```
Fase 1  → Generación de N personas (1 subagent)
Fase 2  → Captura del sitio: screenshots + texto (1 subagent + browse)
Fase 3  → Muestra real de navegación (N subagents en paralelo)
Fase 4  → Scoring masivo en batches de 25 (1-2 subagents)
Fase 5  → Consolidación del reporte (1 subagent)
```

---

## Costo estimado

| Config | Personas | Navegación real | Tokens aprox. | Tiempo |
|---|---|---|---|---|
| Rápido | 20 | 15% (3) | ~30k | 1-2 min |
| Estándar | 50 | 20% (10) | ~70k | 2-4 min |
| Escalado | 200 | 10% (20) | ~250k | 10-15 min |

---

## Caveats

- Los resultados son **señales simuladas, no evidencia de usuarios reales**.
- Cuantas más personas, más ruido: 8-12 perfiles diversos ya saturan los insights (Nielsen).
- Lo que escala bien es la **diversidad de segmentos**, no la cantidad.
- Requiere acceso a un navegador (browse / Playwright MCP). Si no hay, cae a scoring sobre capturas.

---

## Licencia

MIT — facucorona
