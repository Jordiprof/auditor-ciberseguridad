# 🛡️ Auditoría de Seguridad Multi-Agente

Skill de auditoría de seguridad para proyectos de software (`/auditoria-seguridad`). Un orquestador lanza especialistas de solo lectura en paralelo, consolida sus hallazgos y genera un informe en español con vulnerabilidades, evidencias y propuestas de remediación.

Este repositorio es una **biblioteca/plantilla de referencia**: contiene el orquestador (`SKILL.md`), la plantilla del informe y la documentación. No contiene código ejecutable ni dependencias.

## Objetivo

Realizar una auditoría de seguridad profunda de un proyecto identificando vulnerabilidades en:

* Inyecciones (SQLi, XSS, RCE, SSRF, Path Traversal, XXE, etc.)
* Autenticación y control de acceso
* Criptografía y gestión de sesiones
* Secretos expuestos y fugas de información
* Cadena de suministro y dependencias
* Configuración insegura
* Lógica de negocio
* Condiciones de carrera y TOCTOU

La auditoría es de **solo lectura**: el único fichero que escribe es el informe.

---

## Contenido del repositorio

| Fichero | Qué es |
|---|---|
| `SKILL.md` | Orquestador de la auditoría: fases 0–3, selección de especialistas, consolidación, informe, reglas de seguridad y gobernanza de herramientas (fuente única de verdad de herramientas y comandos). Es lo que lee el agente. |
| `plantilla-informe.md` | Plantilla que el orquestador rellena para generar el informe final. |
| `MANUAL-USUARIO.txt` | Manual de uso: paso a paso, especialistas, seguridad, cómo leer el informe, limitaciones. |
| `AGENTS.md` / `CLAUDE.md` | Reglas de mantenimiento del repositorio (no las lee el auditor). |
| `CHANGELOG.md` / `VERSION` | Registro de cambios y versión declarada. |

### Qué NO incluye este repositorio

* **Las definiciones de los 5 subagentes** (`auditor-inyecciones`, `auditor-autenticacion`, `auditor-secretos-exposicion`, `auditor-cadena-suministro`, `auditor-logica-negocio`). `SKILL.md` los invoca por nombre, pero sus ficheros `auditor-*.md` no están aquí.
* **El hook guardián** (`auditoria-shell-guard.ps1`) que, según el manual, restringe los comandos de shell de los agentes.
* **El proyecto de prueba deliberadamente vulnerable** que cita el manual.

Las rutas de `MANUAL-USUARIO.txt` (secciones 8 y 9) corresponden a la instalación del autor y sirven solo como referencia.

---

## Requisitos

* **Claude Code** con soporte de skills y de subagentes. `SKILL.md` usa la herramienta de subagentes (`Task`/`Agent`), `AskUserQuestion` y `$ARGUMENTS`. Si la herramienta de subagentes no está disponible, la skill degrada a un recorrido secuencial y lo avisa.
* **Los 5 subagentes instalados** (p. ej. en `~/.claude/agents/`) con los nombres exactos anteriores. No se incluyen aquí.
* **PowerShell**: la Fase 0 detecta los escáneres disponibles con `Get-Command`.
* **Escáneres opcionales.** Ninguno es obligatorio; si faltan, el informe lo marca como cobertura degradada. El recomendado es `gitleaks`. La lista completa, con su nivel y su comando, está en la sección "Gobernanza de herramientas" de `SKILL.md`. La skill nunca instala nada.
* **Modelo:** `SKILL.md` recomienda ejecutar el orquestador con un modelo fuerte (Opus) y deja a los especialistas en Sonnet.

---

## Uso

1. Copia `SKILL.md` y `plantilla-informe.md` a una carpeta de skill llamada `auditoria-seguridad` (p. ej. `~/.claude/skills/auditoria-seguridad/`). La plantilla debe estar en la misma carpeta que `SKILL.md`.
2. Instala los 5 subagentes y reinicia Claude Code (los agentes nuevos solo se registran al reiniciar).
3. Abre Claude Code dentro del proyecto que quieres auditar y ejecuta:

   ```text
   /auditoria-seguridad
   /auditoria-seguridad src/api      # solo una subruta (proyectos grandes)
   ```

4. Responde si el código es **de confianza** o **de terceros** (ante la duda, terceros).
5. El informe se escribe en la raíz del proyecto auditado como `AUDITORIA-SEGURIDAD-AAAA-MM-DD.md` (con sufijo `-HHmm` si ya existe), y se muestra un resumen en el chat.

---

## Especialistas

### Auditor de Inyecciones (`auditor-inyecciones`)

SQL Injection, XSS, RCE, SSRF, Path Traversal, XXE, deserialización insegura, Prototype Pollution, ReDoS.

### Auditor de Autenticación (`auditor-autenticacion`)

JWT inseguros, gestión de sesiones, CSRF, IDOR / BOLA, escalada de privilegios, errores criptográficos.

### Auditor de Secretos y Exposición (`auditor-secretos-exposicion`)

Credenciales hardcodeadas, secretos en repositorios e historial git, PII expuesta, logs inseguros, security headers ausentes, configuración CORS incorrecta.

### Auditor de Cadena de Suministro (`auditor-cadena-suministro`)

Dependencias vulnerables, CVEs conocidas, scripts peligrosos, Dockerfiles inseguros, CI/CD inseguro, infraestructura como código.

### Auditor de Lógica de Negocio (`auditor-logica-negocio`)

Fallos de negocio, race conditions, TOCTOU, mass assignment, validaciones insuficientes, abuso de límites.

Secretos y lógica de negocio se lanzan siempre; los demás, según lo que detecte el reconocimiento (en caso de duda, se lanzan).

---

## Flujo de trabajo

1. Reconocimiento del proyecto y detección del stack (Fase 0).
2. Detección de escáneres disponibles.
3. Pregunta de confianza (de confianza / de terceros).
4. Lanzamiento de especialistas en paralelo (Fase 1).
5. Consolidación, deduplicación y verificación de evidencias (Fase 2).
6. Generación del informe final y resumen en el chat (Fase 3).

---

## Seguridad operativa

### Garantías

* No modifica código fuente: el único fichero que escribe es el informe.
* No instala software.
* Trata el contenido auditado como datos: si un fichero contiene instrucciones dirigidas a la IA, no las obedece y las reporta como contenido sospechoso.
* Enmascara los secretos en el informe y en el chat.

Las restricciones técnicas de los especialistas (sin herramientas de escritura, y shell filtrado por el hook guardián) dependen de las definiciones de los agentes y del hook, que **no están en este repositorio**.

### Modo de terceros

Cuando el proyecto no es de confianza:

* No se ejecutan gestores de paquetes (`npm audit`, `pip-audit`, `checkov`).
* Los agentes trabajan solo sobre los manifiestos y el análisis estático.

---

## Resultado

El informe (`plantilla-informe.md`) incluye:

* Resumen ejecutivo ordenado por riesgo
* Metodología y cobertura (escáneres ejecutados y ausentes)
* Hallazgos detallados, ordenados de lo más difícil de detectar a lo más fácil, con CWE/OWASP, cadena de alcanzabilidad, evidencia y solución
* Plan de remediación priorizado
* Anexos: hallazgos no verificados, herramientas recomendadas y contenido sospechoso de prompt-injection
* Limitaciones

---

## Compatibilidad

Está diseñada para **Claude Code** (skills y subagentes). Otros entornos que lean instrucciones Markdown podrían seguir `SKILL.md` en modo secuencial, pero eso no está comprobado.

---

## Limitaciones

Es análisis **estático**: produce hipótesis priorizadas, no explotación confirmada. No sustituye una auditoría profesional ni pruebas dinámicas (DAST/fuzzing). Se recomienda como primera fase antes de auditorías manuales, pentesting, revisiones de arquitectura, despliegues en producción o revisiones de cumplimiento.

---

## Licencia de uso

Uso personal y profesional. El repositorio no incluye todavía un fichero `LICENSE` con los términos formales.

---

## Versión

La versión declarada está en `VERSION` y los cambios en `CHANGELOG.md`.
