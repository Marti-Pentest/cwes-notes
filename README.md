# cwes-notes

Notas de estudio personales para el path **HTB Certified Web Exploitation Specialist (CWES)** — metodología, comandos y conceptos propios. No incluye soluciones de labs, flags, ni contenido transcrito del curso.

## 📊 Roadmap de módulos

| # | Módulo | Categoría | Dificultad | Tier | Estado |
|---|--------|-----------|------------|------|--------|
| 01 | Web Requests | General | Fundamental | Tier 0 | ✅ |
| 02 | Introduction to Web Applications | General | Fundamental | Tier 0 | ✅ |
| 03 | Using Web Proxies | Offensive | Easy | Tier II | ✅ |
| 04 | Information Gathering - Web Edition | Offensive | Easy | Tier II | ✅ |
| 05 | [Web Fuzzing](05-web-fuzzing/) | Offensive | Easy | Tier 0 | ✅ |
| 06 | [JavaScript Deobfuscation](06-javascript-deobfuscation/) | Defensive | Easy | Tier 0 | ✅ |
| 07 | Cross-Site Scripting (XSS) | Offensive | Easy | Tier II | ✅ |
| 08 | SQL Injection Fundamentals | Offensive | Medium | Tier 0 | ✅ |
| 09 | SQLMap Essentials | Offensive | Easy | Tier II | ✅ |
| 10 | Command Injections | Offensive | Medium | Tier II | ✅ |
| 11 | File Upload Attacks | Offensive | Medium | Tier II | ✅ |
| 12 | [Server-side Attacks](07-server-side-attacks/) | Offensive | Medium | Tier II | ✅ |
| 13 | Login Brute Forcing | Offensive | Easy | Tier II | ✅ |
| 14 | [Broken Authentication](08-broken-authentication/) | Offensive | Medium | Tier II | ✅ |
| 15 | Web Attacks | Offensive | Medium | Tier II | ✅ |
| 16 | File Inclusion | Offensive | Medium | Tier 0 | ✅ |
| 17 | Attacking GraphQL | Offensive | Medium | Tier II | 🟡 |
| 18 | API Attacks | Offensive | Medium | Tier II | 🟡 |
| 19 | Attacking Common Applications | Offensive | Medium | Tier II | ✅ |
| 20 | Bug Bounty Hunting Process | General | Easy | Tier I | 🟡 |

**Leyenda:** ✅ Completado · 🟡 Pendiente / en progreso · ⏳ No iniciado

> [!NOTE]
> Los módulos completados (✅) fueron cursados previamente en el marco de la certificación CPTS y comparten contenido con el path CWES. No se documentan con notas propias en este repo salvo repaso puntual; el foco de este repositorio son los módulos pendientes.

## 📁 Estructura del repo

```
cwes-notes/
├── README.md
├── 05-web-fuzzing/
│   ├── 01-nombre-seccion.md
│   └── ...
├── 06-javascript-deobfuscation/
│   └── ...
├── 12-server-side-attacks/
│   └── ...
├── 14-broken-authentication/
│   └── ...
├── 17-attacking-graphql/
│   └── ...
├── 18-api-attacks/
│   └── ...
└── 20-bug-bounty-hunting-process/
    └── ...
```

Cada carpeta corresponde a un módulo del path CWES. Cada archivo `.md` dentro documenta una sección o subsección de ese módulo, numerada en orden de aparición en HTB Academy.

## ✍️ Formato usado

Cada nota sigue esta estructura:

- **Concepto principal** — resumen de la idea clave de la sección
- **Comandos / sintaxis** — bloques de código con la sintaxis relevante (bash, SQL, HTTP, etc.)
- **Ejemplos prácticos / detección** — casos de uso o cómo identificar el patrón en la práctica
- **Quiz de repaso** — preguntas plegables (`<details>`) cuando la sección de HTB incluía preguntas
- **Relacionado** — links a otras notas del repo conectadas por tema

Los diagramas de flujo (kill chains, arquitecturas, ciclos de vida de ataque) se documentan con [Mermaid](https://mermaid.js.org/), soportado nativamente por GitHub.

## ⚠️ Disclaimer

- Estas son notas de estudio personales y resúmenes propios, no transcripción del curso de HTB Academy.
- No se incluyen respuestas exactas de labs, flags, ni walkthroughs completos de ejercicios pagos.
- Se documenta la metodología y los comandos utilizados, no la solución literal de ningún ejercicio.
- Este repositorio respeta los Términos de Servicio de Hack The Box respecto al contenido de HTB Academy.

## 🎓 Sobre CWES

**HTB Certified Web Exploitation Specialist** es una certificación práctica que evalúa habilidades de pentesting de aplicaciones web y bug bounty hunting a nivel intermedio, incluyendo la capacidad de evaluar el riesgo de una aplicación, servicio o API y redactar un reporte de nivel profesional.
