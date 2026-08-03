# Módulo 07 — Server-side Attacks

## Sección 1/19: Introduction to Server-side Attacks

> [!NOTE]
> A diferencia de los ataques **client-side** (que atacan al navegador, ej. XSS), los ataques **server-side** targetean directamente la aplicación o servicio que corre en el servidor. Este módulo cubre cuatro clases de vulnerabilidades server-side: **SSRF**, **SSTI**, **SSI Injection** y **XSLT Server-Side Injection**.

## 📌 Las 4 clases cubiertas en el módulo

### Server-Side Request Forgery (SSRF)

Permite a un atacante manipular la aplicación web para que envíe requests HTTP no autorizados **desde el servidor**. Ocurre típicamente cuando la app hace requests a otros servidores basándose en input del usuario. Explotado con éxito, permite acceder a sistemas internos, evadir firewalls, y extraer información sensible.

### Server-Side Template Injection (SSTI)

Ocurre cuando un atacante puede inyectar código de plantilla (template) en motores de templating server-side que generan contenido dinámico a partir de input del usuario. Puede derivar en fuga de datos e incluso **compromiso total del servidor vía RCE**.

### Server-Side Includes (SSI) Injection

Las directivas **SSI** instruyen al servidor web a incluir contenido dinámicamente (ej. headers/footers comunes), embebidas directamente en archivos HTML. Si un atacante logra inyectar comandos dentro de esas directivas, puede provocar fuga de datos o RCE.

### XSLT Server-Side Injection

**XSLT** transforma documentos XML a otros formatos (ej. HTML). La inyección ocurre cuando un atacante puede manipular las transformaciones XSLT ejecutadas server-side, permitiendo inyectar y ejecutar código arbitrario en el servidor.

> [!TIP]
> Patrón común entre las 4 vulnerabilidades: todas comparten la misma raíz conceptual — el servidor procesa input del usuario como parte de una operación que genera contenido o realiza acciones dinámicamente, y si ese input no se sanitiza correctamente, el atacante puede inyectar su propia lógica en ese proceso server-side.

## 🧠 Quiz de repaso

<details>
<summary>¿Cuál es la diferencia fundamental entre un ataque client-side (como XSS) y uno server-side?</summary>

Un ataque client-side, como XSS, ejecuta código malicioso en el navegador de la víctima — el servidor entrega el payload, pero la explotación ocurre en la máquina del usuario que visita la página. Un ataque server-side, en cambio, explota directamente la lógica que corre en el servidor, afectando la infraestructura y los datos del propio servidor, no la sesión de un usuario particular en su navegador.

</details>

<details>
<summary>¿Por qué SSTI y SSI Injection, aunque técnicamente distintas, comparten un riesgo similar de severidad?</summary>

Porque ambas involucran motores que procesan input de usuario como parte de una sintaxis que el servidor interpreta y ejecuta para generar contenido dinámico — SSTI mediante la sintaxis del motor de templates, SSI mediante directivas embebidas en HTML. En ambos casos, si el input del atacante logra "escapar" del contexto de dato esperado y ser interpretado como sintaxis del motor/directiva, el resultado potencial es el mismo: ejecución de código en el servidor (RCE).

</details>

## 🔗 Relacionado
- [02 — Introduction to SSRF](02-introduction-to-ssrf.md)

#cwes #modulo07 #server-side-attacks #ssrf #ssti #ssi #xslt #fundamentos
