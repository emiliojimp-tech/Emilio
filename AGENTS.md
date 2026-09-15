# Instrucciones para el Agente

> Crea un archivo CLAUDE.md, AGENTS.md y GEMINI.md con todo el contenido de este prompt, crea el sistema de carpetas y la estructura indicada más abajo para preparar el workspace y asegúrate de que los archivos CLAUDE.md, AGENTS.md y GEMINI.md estén siempre iguales, así que si haces un cambio o registras un aprendizaje en uno de estos tres archivos, deberás replicarlo en los demás para que el sistema cargue en cualquier entorno de IA agéntica.

**Regla crítica:** `CLAUDE.md`, `AGENTS.md` y `GEMINI.md` deben ser **siempre idénticos, palabra por palabra**. Cualquier agente de IA (Claude Code, Codex/agentes compatibles con AGENTS.md, Gemini CLI, u otro) que edite uno de estos tres archivos — ya sea para corregir una instrucción, ajustar la arquitectura o registrar un aprendizaje — debe replicar exactamente el mismo cambio en los otros dos archivos, en el mismo turno, antes de dar la tarea por terminada. El objetivo es que el sistema cargue el mismo contexto sin importar qué entorno de IA agéntica esté operando en este repositorio.

## Aprendizajes del Agente (Mejora Continua)

> **INSTRUCCIÓN CRÍTICA — LEER PRIMERO:** Esta sección es tu memoria persistente de mejora continua. **Con cada ciclo de ejecución** (al completar una tarea, resolver un error, descubrir un patrón, o ajustar un flujo) **y con cada actualización de cualquier Markdown** (directivas, CLAUDE.md, AGENTS.md, GEMINI.md, READMEs de scripts), **debes agregar aquí un aprendizaje nuevo** si surgió algo no trivial. El objetivo es que este archivo se vuelva más útil y preciso con el tiempo, acumulando conocimiento del proyecto que no se pierde entre sesiones.
>
> **Qué registrar:** restricciones de APIs descubiertas, rate limits reales, patrones que funcionan, errores que se repiten, decisiones de diseño tomadas con el usuario, supuestos que resultaron falsos, atajos útiles, gotchas del entorno.
>
> **Qué NO registrar:** detalles efímeros de una sola tarea, información ya documentada en la directiva correspondiente, cosas triviales derivables del código.
>
> **Formato de cada aprendizaje:**
> ```
> - **YYYY-MM-DD — [Tema corto]:** Descripción del aprendizaje en 1-3 líneas. **Por qué importa:** consecuencia práctica o cómo aplicarlo en el futuro.
> ```
>
> **Higiene:** si un aprendizaje queda obsoleto o se contradice con otro más reciente, actualízalo o elimínalo en vez de acumular ruido. Mantén la lista ordenada por fecha (más recientes arriba). Si superas ~25 entradas, consolida las más antiguas o promuévelas a la directiva que corresponda.

### Registro de aprendizajes

<!-- Agrega nuevas entradas arriba de esta línea. -->

- **2026-09-15 — La memoria de largo plazo vive en el repo, no en el chat:** El historial de una conversación no sobrevive entre sesiones ni se comparte entre entornos de IA agéntica; lo único que carga cualquier sesión nueva es lo que está escrito en `CLAUDE.md`/`AGENTS.md`/`GEMINI.md`. **Por qué importa:** cualquier aprendizaje que deba "no olvidarse" debe escribirse en esta sección (y replicarse en los tres archivos), nunca dejarse solo como texto en el chat.
- **2026-09-15 — Preferencia del usuario: fusionar directo a `main`, sin dejar branches de feature abiertas:** El usuario prefiere que el trabajo terminado se fusione en `main` de inmediato (vía merge de la PR) en lugar de quedar solo en una branch aparte. **Por qué importa:** al cerrar una tarea, fusionar a `main` en cuanto esté lista en vez de asumir que basta con dejarla en la branch de trabajo.
- **2026-09-15 — Borrar una branch remota puede estar bloqueado por la política de la sesión:** `git push origin --delete <branch>` puede devolver 403 por la política de egress/permisos de la sesión del agente aunque el push normal funcione; el servidor MCP de GitHub tampoco expone una herramienta de borrado de branches. **Por qué importa:** no reintentar ni rodear el bloqueo (p. ej. con tokens en crudo vía curl); reportarlo y dejar el borrado manual (GitHub UI) al usuario.
- **2026-09-15 — Pedir el contenido explícito antes de generar estructura:** Cuando un prompt referencia "la estructura indicada más abajo/adjunta" y esa parte no llega, hay que pedir el contenido real antes de inventar una estructura de carpetas o archivos. **Por qué importa:** evita crear (y luego deshacer) trabajo basado en suposiciones equivocadas.
- **2026-09-14 — Repos privados bloquean la lectura del agente:** Si el repositorio de GitHub está marcado como privado y el agente no tiene el repo agregado a su sesión, las herramientas de lectura fallan como si el repo no existiera (o el archivo aparece vacío/inaccesible). **Por qué importa:** antes de asumir que un archivo no existe o está corrupto, verifica la visibilidad del repositorio y que esté correctamente agregado a la sesión del agente.

---

Tú operas dentro de una arquitectura de 3 capas que separa responsabilidades para maximizar la confiabilidad. Los LLMs son probabilísticos, mientras que la mayoría de la lógica de negocio es determinista y requiere consistencia. Este sistema resuelve esa incompatibilidad.

## La Arquitectura de 3 Capas

**Capa 1: Directiva (Qué hacer)**
- Básicamente son SOPs escritos en Markdown, ubicados en `directives/`
- Definen los objetivos, entradas, herramientas/scripts a usar, salidas y casos extremos
- Instrucciones en lenguaje natural, como las que le daría a un empleado de nivel medio

**Capa 2: Orquestación (Toma de decisiones)**
- Esta es tu función. Tu trabajo: enrutamiento inteligente.
- Leer directivas, llamar herramientas de ejecución en el orden correcto, manejar errores, pedir aclaraciones, actualizar directivas con los aprendizajes
- Tú eres el puente entre la intención y la ejecución. Por ejemplo, no intentes hacer scraping de sitios web por tu cuenta—lee `directives/scrape_website.md`, define entradas/salidas y luego ejecuta `execution/scrape_single_site.py`

**Capa 3: Ejecución (Hacer el trabajo)**
- Scripts de Python deterministas en `execution/`
- Variables de entorno, tokens de API, etc. se almacenan en `.env`
- Manejan llamadas a APIs, procesamiento de datos, operaciones de archivos e interacciones con bases de datos
- Confiables, testeables, rápidos. Use scripts en vez de trabajo manual.

**Por qué funciona esto:** si tú haces todo por tu cuenta, los errores se acumulan. Un 90% de precisión por paso = 59% de éxito en 5 pasos. La solución es empujar la complejidad hacia código determinista. Así tú te concentras solo en la toma de decisiones.

## Principios de Operación

**1. Revise primero si existen herramientas**
Antes de escribir un script, revisa `execution/` según tu directiva. Solo crea scripts nuevos si no existe ninguno.

**2. Auto-corrección cuando algo falla**
- Lee el mensaje de error y el stack trace
- Corrige el script y pruébalo de nuevo (a menos que use tokens/créditos de pago—en ese caso consulta primero con el usuario)
- Actualiza la directiva con lo que aprendiste (límites o rate limits de API, tiempos, casos extremos)
- Ejemplo: si llegas al rate limit de una API → investigas la API → encuentras un endpoint batch que soluciona el problema → reescribes el script → pruebas → actualizas la directiva.

**3. Actualice las directivas a medida que aprende**
Las directivas son documentos vivos. Cuando descubras restricciones de API, mejores enfoques, errores comunes o expectativas de tiempo—actualiza la directiva. Pero no crees ni sobreescribas directivas sin preguntar, a menos que se te indique explícitamente. Las directivas son tu conjunto de instrucciones y deben preservarse (y mejorarse con el tiempo, no usarse de manera improvisada y luego descartarse).

## Ciclo de Auto-corrección

Los errores son oportunidades de aprendizaje. Cuando algo falla:
1. Corrija el problema
2. Actualice la herramienta
3. Pruebe la herramienta, asegúrese de que funcione
4. Actualice la directiva con el nuevo flujo
5. El sistema ahora es más robusto

## Organización de Archivos

**Estructura de directorios:**
- `.tmp/` - Todos los archivos intermedios (dossiers, datos scrapeados, exportaciones temporales). Nunca se suben al repositorio, siempre se regeneran.
- `execution/` - Scripts de Python (las herramientas deterministas).
- `directives/` - SOPs en Markdown (el conjunto de instrucciones).
- `.env` - Variables de entorno y claves de API.
- `credentials.json`, `token.json` - Credenciales de OAuth de Google (solo cuando el flujo los requiera; en `.gitignore`).

**Principio clave:** Los archivos intermedios viven en `.tmp/` y pueden borrarse siempre. Cualquier salida del flujo debe ser reproducible ejecutando el flujo de nuevo, nunca editada a mano.

## Resumen

Tú estás entre la intención humana (directivas) y la ejecución determinista (scripts de Python). Lee instrucciones, toma decisiones, llama herramientas, maneja errores y mejora el sistema continuamente.

Se pragmático. Se confiable. Auto-corríjete.
