# Directivas

Carpeta de la **Capa 1 (Directiva)** de la arquitectura de 3 capas descrita en `CLAUDE.md` / `AGENTS.md` / `GEMINI.md`.

Cada archivo `.md` aquí es un SOP (procedimiento operativo estándar): define objetivo, entradas, herramientas/scripts de `execution/` a usar, salidas esperadas y casos extremos, en lenguaje natural.

Reglas:
- No crear ni sobreescribir directivas sin preguntar al usuario, salvo instrucción explícita.
- Actualizar una directiva existente cuando se descubran restricciones de API, mejores enfoques o errores comunes (ver "Ciclo de Auto-corrección" en CLAUDE.md).
- Nombrar cada directiva según la tarea que describe, p. ej. `scrape_website.md`.
