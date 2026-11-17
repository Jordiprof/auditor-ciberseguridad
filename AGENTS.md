# Reglas del repositorio

Este fichero son las **reglas de mantenimiento** del repositorio para cualquier agente o persona que lo edite. **No** son las instrucciones del auditor: el "prompt" que ejecuta el agente auditor es `SKILL.md`.

## Qué es este repositorio

Biblioteca/plantilla de referencia de la skill `auditoria-seguridad` (auditoría de seguridad multi-agente, de solo lectura). No hay código ejecutable, dependencias ni variables de entorno: son ficheros Markdown y texto.

| Fichero | Papel | ¿Lo lee el agente auditor? |
|---|---|---|
| `SKILL.md` | Instrucciones del orquestador (Fases 0–3, gobernanza de herramientas) | Sí |
| `plantilla-informe.md` | Plantilla que el orquestador rellena en la Fase 3 | Sí |
| `MANUAL-USUARIO.txt` | Manual para la persona usuaria | No |
| `README.md` | Presentación del repositorio | No |
| `AGENTS.md`, `CLAUDE.md`, `CHANGELOG.md`, `VERSION`, `.gitignore` | Mantenimiento del repositorio | No |

## Reglas

1. **Idioma:** todo en español (contenido, commits, PR).
2. **Cambiar `SKILL.md` o `plantilla-informe.md` cambia el comportamiento del auditor.** Hazlo solo a propósito, en un commit propio, y anótalo en `CHANGELOG.md`.
3. **Fuente única de verdad de herramientas:** la tabla de "Gobernanza de herramientas" de `SKILL.md`. Si cambias una herramienta o un comando, actualiza en el mismo commit su espejo en `MANUAL-USUARIO.txt` (sección 6). No inventes comandos de instalación.
4. **Coherencia documental:** `README.md` y `MANUAL-USUARIO.txt` no deben contradecir a `SKILL.md`. Si cambias el número o nombre de los especialistas, las fases o los campos del contrato JSON, revisa los tres.
5. **No están en este repositorio:** las definiciones de los subagentes (`auditor-*.md`) ni el hook guardián (`auditoria-shell-guard.ps1`). No afirmes en la documentación que se incluyen.
6. **Repositorio público:** nunca subas secretos, ficheros `.env`, claves (`*.pem`, `*.key`) ni informes reales `AUDITORIA-SEGURIDAD-*.md` (están en `.gitignore`). Los ejemplos de vulnerabilidades deben usar valores ficticios y evidentes.
7. **Sin dependencias ni variables de entorno:** no hay `.env.example` porque nada lee variables de entorno. Si algún día se añade un script que las lea, crea `.env.example` solo con los nombres que se lean.
8. **Versionado:** la versión declarada está en `VERSION`. Los cambios se anotan en `CHANGELOG.md` bajo "Sin publicar"; no inventes versiones publicadas ni fechas.
9. **Git:** nunca se commitea directamente en `main`; trabaja en una rama y abre un PR. Un commit por tema. Nada de `--force`, `rebase`, `amend` ni etiquetas sin autorización expresa.
