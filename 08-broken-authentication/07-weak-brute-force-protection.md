# Módulo 08 — Broken Authentication

## Sección 7/14: Weak Brute-Force Protection

> [!NOTE]
> Los dos mecanismos más comunes contra brute-force son rate limiting y CAPTCHAs. Ambos pueden implementarse de forma débil.

## 🛠️ Rate Limits

> [!TIP]
> Controla la cantidad de requests en una ventana de tiempo, protegiendo contra sobrecarga y brute-force.

> [!WARNING]
> Muchas implementaciones identifican al atacante por IP, pero con middleboxes delante, algunas apps confían en headers como `X-Forwarded-For`.

> [!WARNING]
> Bypass: `X-Forwarded-For` es un header arbitrario que el cliente puede setear. Un atacante puede randomizarlo en cada intento, evadiendo el límite. Caso real: CVE-2020-35590.

## 🛠️ CAPTCHAs

> [!TIP]
> Fuerza que los requests provengan de humanos, convirtiendo el brute-force en tarea manual.

> [!WARNING]
> Fallo común: exponer la solución del CAPTCHA en la respuesta (HTML/campo oculto), anulando su propósito.

> [!WARNING]
> Otras formas de evasión: extensiones de browser dedicadas, solvers open-source, herramientas de IA con reconocimiento de imágenes/voz.

> [!NOTE]
> Trade-off: los CAPTCHAs presentan desafíos de accesibilidad para usuarios con discapacidades.

## 🧠 Quiz de repaso

<details>
<summary>¿Por qué confiar en X-Forwarded-For es fundamentalmente distinto de confiar en la IP de origen real?</summary>

Porque la IP de origen TCP es difícil de falsificar sin romper la conexión, mientras que X-Forwarded-For es solo un campo de texto arbitrario que el cliente controla por completo.

</details>

<details>
<summary>¿Por qué exponer la solución de un CAPTCHA es más grave que un CAPTCHA "fácil" de resolver por IA?</summary>

Porque un CAPTCHA fácil sigue exigiendo recursos computacionales no triviales para automatizar. Exponer la solución elimina la necesidad de resolver absolutamente nada, anulando el mecanismo entero.

</details>

## 🔗 Relacionado
- [06 — Brute-Forcing 2FA Codes](06-brute-forcing-2fa-codes.md)
- [04 — Brute-Forcing Passwords](04-brute-forcing-passwords.md)

#cwes #modulo08 #broken-authentication #rate-limiting #captcha #x-forwarded-for
