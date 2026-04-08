# career-ops -- Modos en español (`modes/es/`)

Esta carpeta contiene los modos career-ops en español para candidatos que buscan empleo en mercados hispanohablantes o que trabajan con ofertas de empleo en español.

## ¿Cuándo usar estos modos?

Usa `modes/es/` si se cumple al menos una de estas condiciones:

- Postulas principalmente a **ofertas de empleo en español** (InfoJobs, LinkedIn ES, Indeed ES, portales de empleo locales)
- Tu **CV está en español** o alternas entre ES e EN según la oferta
- Necesitas respuestas y cartas de motivación en **español tech natural**, no traducido por máquina
- Necesitas manejar **conceptos del mercado laboral hispanohablante**: convenio colectivo, nómina, pagas extra, periodo de prueba, preaviso, contrato indefinido/temporal, seguridad social, IRPF, teletrabajo

Si la mayoría de tus ofertas son en inglés, usa los modos estándar en `modes/`. Los modos ingleses son el default y están orientados al mercado norteamericano (US/Canada).

## ¿Cómo activar?

### Opción 1 -- Por sesión

Dile a Claude al inicio de la sesión:

> "Usa los modos en español bajo `modes/es/`."

Claude leerá los archivos de esta carpeta en lugar de `modes/`.

### Opción 2 -- Permanente

Añade en `config/profile.yml`:

```yaml
language:
  primary: es
  modes_dir: modes/es
```

Recuérdaselo a Claude en tu primera sesión ("Mira en `profile.yml`, tengo `language.modes_dir` configurado"). Claude usará automáticamente los modos en español.

## ¿Qué modos están incluidos?

Esta carpeta incluye el set completo de modos career-ops en español:

| Archivo | Equivalente EN | Propósito |
|---------|----------------|-----------|
| `_shared.md` | `modes/_shared.md` | Contexto compartido, arquetipos, reglas globales |
| `offer.md` | `modes/offer.md` | Evaluación completa de una oferta (Bloques A-F) |
| `compare.md` | `modes/compare.md` | Comparación multi-oferta (scoring matrix) |
| `contact.md` | `modes/contact.md` | LinkedIn outreach: encontrar contactos + mensaje |
| `apply.md` | `modes/apply.md` | Asistente de aplicación en vivo |
| `pipeline.md` | `modes/pipeline.md` | Inbox de URLs / Second Brain |
| `auto-pipeline.md` | `modes/auto-pipeline.md` | Pipeline completo automático |
| `batch.md` | `modes/batch.md` | Procesamiento masivo de ofertas |
| `scan.md` | `modes/scan.md` | Escáner de portales |
| `pdf.md` | `modes/pdf.md` | Generación de PDF ATS-optimizado |
| `deep.md` | `modes/deep.md` | Research profundo de empresa |
| `tracker.md` | `modes/tracker.md` | Tracker de aplicaciones |
| `training.md` | `modes/training.md` | Evaluación de formación |
| `project.md` | `modes/project.md` | Evaluación de proyecto portfolio |

## ¿Qué se mantiene en inglés?

Voluntariamente no traducido porque es vocabulario tech estándar:

- `cv.md`, `pipeline`, `tracker`, `report`, `score`, `archetype`, `proof point`
- Nombres de herramientas (`Playwright`, `WebSearch`, `WebFetch`, `Read`, `Write`, `Edit`, `Bash`)
- Valores de estado en el tracker (`Evaluated`, `Applied`, `Interview`, `Offer`, `Rejected`)
- Snippets de código, rutas, comandos

Los modos usan español tech natural, como se habla en equipos de engineering: texto corrido en español, términos técnicos en inglés donde es lo habitual. No forzamos traducciones de "Pipeline" a "Tubería" ni de "Deploy" a "Despliegue aplicativo".

## Vocabulario de referencia

| Inglés | Español (en esta codebase) |
|--------|---------------------------|
| Job posting | Oferta de empleo |
| Application | Candidatura / Aplicación |
| Cover letter | Carta de motivación |
| Resume / CV | CV / Currículum |
| Salary | Salario / Sueldo |
| Compensation | Compensación / Paquete retributivo |
| Skills | Habilidades / Competencias |
| Interview | Entrevista |
| Hiring manager | Hiring manager / Responsable de contratación |
| Recruiter | Recruiter / Reclutador |
| AI | IA / Inteligencia Artificial |
| Requirements | Requisitos |
| Career history | Trayectoria profesional |
| Notice period | Preaviso |
| Probation | Periodo de prueba |
| Vacation | Vacaciones |
| Permanent employment | Contrato indefinido |
| Fixed-term contract | Contrato temporal |
| Freelance | Freelance / Autónomo |
| Collective agreement | Convenio colectivo |
| Social security | Seguridad Social |

## Contribuir

Si quieres mejorar una traducción o añadir un modo:

1. Abre un Issue con la propuesta (ver `CONTRIBUTING.md`)
2. Respeta el vocabulario de arriba para mantener el tono consistente
3. Traduce de forma idiomática -- no hagas traducciones literales
4. Mantén los elementos estructurales (Bloques A-F, tablas, bloques de código, instrucciones de herramientas)
5. Testea con una oferta real en español antes de enviar el PR
