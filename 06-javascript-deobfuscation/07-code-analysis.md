# Módulo 06 — JavaScript Deobfuscation

## Sección 7/11: Code Analysis

> [!NOTE]
> Una vez deofuscado el código, el siguiente paso es el **análisis funcional**: leer línea por línea qué hace, investigando las funciones/objetos nativos de JavaScript que no se conozcan (con Google/MDN), para entender el propósito real de la lógica — incluso si esa funcionalidad no está expuesta todavía en la interfaz visible de la página.

## 🛠️ Código deofuscado a analizar

```javascript
'use strict';
function generateSerial() {
  var xhr = new XMLHttpRequest;
  var url = "/serial.php";
  xhr.open("POST", url, true);
  xhr.send(null);
};
```

## 📌 Desglose línea por línea

- **`var xhr = new XMLHttpRequest;`** — crea un objeto `XMLHttpRequest`, el mecanismo nativo de JS para manejar requests HTTP
- **`var url = "/serial.php";`** — define la URL destino. Al ser una ruta relativa, se asume que apunta al **mismo dominio** donde vive el script
- **`xhr.open("POST", url, true);`** — abre un request HTTP con método `POST` hacia esa URL (el tercer argumento `true` indica que es asíncrono)
- **`xhr.send(null);`** — envía el request, sin body (`null`)

## 🎯 Conclusión del análisis

`generateSerial()` simplemente envía un `POST` vacío a `/serial.php`, sin enviar ni recibir datos explícitamente en el código visible.

> [!TIP]
> Como no hay ningún elemento HTML visible (botón, formulario) que llame a `generateSerial()`, es probable que los desarrolladores hayan dejado esta función implementada pero sin conectar a la interfaz — pensada para uso futuro.

> [!WARNING]
> Si el endpoint server-side (`/serial.php`) ya está activo y responde aunque el frontend no lo use todavía, se puede estar frente a **funcionalidad no lanzada** — este tipo de código suele tener menos testing y más probabilidad de bugs o vulnerabilidades.

## 🧠 Quiz de repaso

<details>
<summary>¿Por qué es útil investigar (googlear) funciones nativas del lenguaje como XMLHttpRequest en vez de asumir su comportamiento?</summary>

Porque el análisis de código depende de entender con precisión qué hace cada línea, y asumir el comportamiento de una API sin confirmarlo puede llevar a conclusiones erróneas sobre la funcionalidad real del script — por ejemplo, no sería obvio a simple vista que el tercer argumento de `xhr.open()` controla si el request es síncrono o asíncrono. Verificar en fuentes confiables (MDN, documentación oficial) es parte normal y necesaria del proceso de análisis.

</details>

<details>
<summary>¿Por qué encontrar una función JS que apunta a un endpoint sin uso visible en la UI es relevante desde una perspectiva de seguridad?</summary>

Porque sugiere la existencia de funcionalidad server-side que puede estar activa y accesible, pero que no pasó por el mismo proceso de testing, revisión o hardening que las funciones expuestas públicamente en la interfaz. Replicar manualmente el request permite verificar si ese endpoint "oculto" responde y qué hace, lo cual puede revelar funcionalidad no documentada con vulnerabilidades no descubiertas aún.

</details>

## 🔗 Relacionado
- [06 — Deobfuscation](06-deobfuscation.md)
- [08 — HTTP Requests](08-http-requests.md)

#cwes #modulo06 #javascript-deobfuscation #code-analysis #xmlhttprequest
