# 🛡️ Auditoría de Seguridad Multi-Agente

Sistema de auditoría de seguridad para proyectos de software basado en especialistas que trabajan en paralelo y generan un informe consolidado con vulnerabilidades, evidencias y propuestas de remediación.

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

La auditoría es de **solo lectura** y genera un informe detallado con recomendaciones concretas.

---

## Componentes

### SKILL.md

Archivo principal que contiene la lógica de orquestación de la auditoría.

Define:

* Fases de ejecución
* Selección de especialistas
* Consolidación de hallazgos
* Generación del informe
* Reglas de seguridad
* Gobernanza de herramientas

### MANUAL-USUARIO.txt

Documentación completa de uso.

Incluye:

* Procedimiento paso a paso
* Descripción de los especialistas
* Limitaciones
* Cobertura de análisis
* Consideraciones de seguridad

### plantilla-informe.md

Plantilla utilizada para generar el informe final de auditoría.

---

## Especialistas

### Auditor de Inyecciones

Busca:

* SQL Injection
* XSS
* RCE
* SSRF
* Path Traversal
* XXE
* Deserialización insegura
* Prototype Pollution
* ReDoS

### Auditor de Autenticación

Busca:

* JWT inseguros
* Gestión de sesiones
* CSRF
* IDOR / BOLA
* Escalada de privilegios
* Errores criptográficos

### Auditor de Secretos y Exposición

Busca:

* Credenciales hardcodeadas
* Secretos en repositorios
* PII expuesta
* Logs inseguros
* Security Headers ausentes
* Configuración CORS incorrecta

### Auditor de Cadena de Suministro

Busca:

* Dependencias vulnerables
* CVEs conocidas
* Scripts peligrosos
* Dockerfiles inseguros
* CI/CD inseguro
* Infraestructura como código

### Auditor de Lógica de Negocio

Busca:

* Fallos de negocio
* Race Conditions
* TOCTOU
* Mass Assignment
* Validaciones insuficientes
* Abuso de límites

---

## Flujo de Trabajo

1. Reconocimiento del proyecto.
2. Detección del stack tecnológico.
3. Identificación de herramientas disponibles.
4. Lanzamiento de especialistas en paralelo.
5. Consolidación y deduplicación de hallazgos.
6. Verificación de evidencias.
7. Generación del informe final.
8. Presentación de resumen ejecutivo.

---

## Seguridad Operativa

Este sistema fue diseñado para minimizar riesgos durante la auditoría.

### Garantías

* No modifica código fuente.
* No instala software automáticamente.
* No ejecuta comandos destructivos.
* Trata el contenido auditado como datos.
* Ignora intentos de prompt injection.
* Solo genera documentación de resultados.

### Modo de terceros

Cuando el proyecto no es de confianza:

* No se ejecutan gestores de paquetes.
* No se lanzan auditorías que puedan ejecutar código del proyecto.
* Se trabaja únicamente sobre análisis estático.

---

## Resultado

La auditoría genera un informe con:

* Resumen ejecutivo
* Hallazgos priorizados
* Evidencias verificadas
* Clasificación CWE y OWASP
* Plan de remediación
* Limitaciones de cobertura
* Recomendaciones de seguridad

---

## Estructura Recomendada

```text
auditoria-seguridad/
│
├── README.md
├── SKILL.md
├── MANUAL-USUARIO.txt
└── plantilla-informe.md
```

---

## Compatibilidad

Esta skill puede utilizarse desde cualquier entorno capaz de leer instrucciones Markdown:

* Claude Code
* Cursor
* VS Code Agents
* ChatGPT
* Gemini
* OpenCode
* Aider
* Otros agentes compatibles

---

## Licencia de Uso

Uso personal y profesional.

Recomendado como primera fase de revisión antes de:

* Auditorías manuales
* Pentesting
* Revisiones de arquitectura
* Despliegues en producción
* Revisiones de cumplimiento normativo

---

## Autor

Skill de auditoría de seguridad multi-agente para análisis estático y generación de informes accionables de seguridad.
