# Módulo 06 — JavaScript Deobfuscation

## Sección 8/11: HTTP Requests

> [!NOTE]
> Tras analizar el código y entender que `generateSerial()` envía un `POST` vacío a `/serial.php`, el siguiente paso lógico es **replicar ese request manualmente** con `curl` para observar la respuesta real del servidor.

> [!TIP]
> El módulo remite al módulo **Web Requests** de HTB Academy para más detalle sobre `cURL` y requests web en general.

## 🛠️ Request GET básico

```bash
curl http://SERVER_IP:PORT/
```

Devuelve el HTML de la página en texto plano.

## 🛠️ Request POST

```bash
curl -s http://SERVER_IP:PORT/ -X POST
```

- `-X POST`: fuerza el método HTTP a `POST`
- `-s` (silent): reduce el ruido de la salida

## 🛠️ Request POST con datos

```bash
curl -s http://SERVER_IP:PORT/ -X POST -d "param1=sample"
```

- `-d`: adjunta datos al body del POST, como pares `param=valor`

> [!NOTE]
> Como `generateSerial()` usa `xhr.send(null)` (sin body), el request equivalente con curl para replicar exactamente esa llamada sería un `POST` **sin** `-d` — apuntando directamente al endpoint identificado en el análisis de código.

## 🧠 Quiz de repaso

<details>
<summary>¿Por qué es valioso replicar con curl un request que ya identificamos en el código JavaScript?</summary>

Porque el código JavaScript solo describe la petición que el navegador enviaría, pero no garantiza cómo responde el servidor ante ella — replicarlo manualmente con curl permite observar directamente el comportamiento real del backend. Además, permite interactuar con el endpoint de forma controlada, probando variaciones que el código original nunca contempló.

</details>

<details>
<summary>¿Por qué el flag -s en curl es útil durante este tipo de análisis?</summary>

Porque al automatizar o repetir múltiples requests durante un análisis, la barra de progreso y estadísticas de transferencia que curl muestra por defecto se vuelven ruido que dificulta leer rápidamente la respuesta real del servidor. Con `-s`, la salida queda limpia y es mucho más fácil de inspeccionar visualmente o de procesar con otras herramientas.

</details>

> [!NOTE]
> **Ejercicio práctico (lab)** — El lab pide enviar un `POST` a `/serial.php` y observar la respuesta. Metodología: `curl -s http://SERVER_IP:PORT/serial.php -X POST`, replicando el request identificado en `generateSerial()`, y leer el contenido devuelto por el servidor. No se documenta la respuesta exacta del lab, solo el enfoque.

## 🔗 Relacionado
- [07 — Code Analysis](07-code-analysis.md)
- [09 — Decoding](09-decoding.md)

#cwes #modulo06 #javascript-deobfuscation #curl #http-requests #post
