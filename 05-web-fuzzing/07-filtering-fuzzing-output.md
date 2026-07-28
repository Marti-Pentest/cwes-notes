# Módulo 05 — Web Fuzzing

## Sección 7/12: Filtering Fuzzing Output

> [!NOTE]
> Los fuzzers generan una cantidad enorme de datos por naturaleza. Sin filtrado, el ruido (típicamente `404 Not Found`) ahoga los resultados relevantes. Cada herramienta ofrece su propio sistema de filtros — por código de estado, tamaño, cantidad de palabras/líneas, tiempo de respuesta o regex — y combinarlos bien es lo que hace la diferencia entre una salida inútil y una que resalta exactamente lo que importa.

## 🛠️ Gobuster

> [!WARNING]
> Solo en modo `dir`: las flags `-s` y `-b` solo están disponibles en el modo de fuzzing de directorios (`dir`), no en `vhost` o `dns`.

| Flag | Descripción | Escenario |
|---|---|---|
| `-s` (include) | Incluye solo los códigos de estado especificados (separados por coma) | Buscar redirects: `-s 301,302,307` |
| `-b` (exclude) | Excluye los códigos especificados | Excluir 404: `-b 404` |
| `--exclude-length` | Excluye respuestas con tamaños específicos (soporta rangos) | Excluir vacías/404: `--exclude-length 0,404` |

```bash
gobuster dir -u http://example.com/ -w wordlist.txt -s 200,301 --exclude-length 0
```

## 🛠️ FFUF

Sistema de filtrado altamente customizable, con matchers (`-m*`, incluir) y filtros (`-f*`, excluir):

| Flag | Descripción | Escenario |
|---|---|---|
| `-mc` (match code) | Incluye solo los códigos especificados (soporta rangos: `400-499`). Default: `200-299,301,302,307,401,403,405,500` | `-mc 200` para aislar solo respuestas OK |
| `-fc` (filter code) | Excluye los códigos especificados | `-fc 404` para quitar ruido |
| `-fs` (filter size) | Excluye por tamaño de respuesta (soporta rangos) | `-fs 0-1023` para descartar respuestas chicas |
| `-ms` (match size) | Incluye solo el tamaño especificado | `-ms 3456` para un backup de tamaño conocido |
| `-fw` (filter words) | Excluye por cantidad de palabras | `-fw 219` |
| `-mw` (match words) | Incluye solo por cantidad de palabras | `-mw 5-10` para mensajes de error cortos |
| `-fl` (filter lines) | Excluye por cantidad de líneas | `-fl 10` |
| `-ml` (match lines) | Incluye solo por cantidad de líneas | `-ml 20` |
| `-mt` (match time) | Incluye según tiempo de respuesta (TTFB) | `-mt >500` para detectar respuestas lentas |

```bash
# Combinando filtros
ffuf -u http://example.com/FUZZ -w wordlist.txt -mc 200 -fw 427 -ms >500
ffuf -u http://example.com/FUZZ -w wordlist.txt -fc 404,401,302
ffuf -u http://example.com/FUZZ.bak -w wordlist.txt -fs 0-10239 -ms 10240-102400
ffuf -u http://example.com/FUZZ -w wordlist.txt -mt >500
```

## 🛠️ wenum

| Flag | Descripción | Escenario |
|---|---|---|
| `--hc` / `--sc` | Hide/show code | `--hc 400` oculta bad requests; `--sc 200` muestra solo OK |
| `--hl` / `--sl` | Hide/show length (líneas) | Ocultar errores verbosos con muchas líneas |
| `--hw` / `--sw` | Hide/show word count | `--sw 5-10` para mensajes cortos |
| `--hs` / `--ss` | Hide/show size (bytes) | `--hs 10000` para descartar archivos grandes |
| `--hr` / `--sr` | Hide/show regex (sobre el body) | `--sr "admin\|password"` para encontrar contenido específico |
| `--filter` / `--hard-filter` | Filtro general show/hide vía regex; el "hard" además evita post-procesamiento por plugins | `--filter "Login"` muestra solo matches; `--hard-filter "Login"` los oculta |

```bash
wenum -w wordlist.txt --sc 200,301,302 -u https://example.com/FUZZ
wenum -w wordlist.txt --hc 404,400,500 -u https://example.com/FUZZ
wenum -w wordlist.txt --sw 5-10 -u https://example.com/FUZZ
wenum -w wordlist.txt --hs 10000 -u https://example.com/FUZZ
wenum -w wordlist.txt --sr "admin\|password" -u https://example.com/FUZZ
```

## 🛠️ Feroxbuster

| Flag | Descripción | Escenario |
|---|---|---|
| `--dont-scan` | Excluye URLs/patrones específicos del escaneo (incluso en recursión) | Excluir `/uploads` si se sabe que solo tiene imágenes |
| `-S`, `--filter-size` | Excluye por tamaño (bytes, soporta rangos) | `-S 1024` para excluir páginas de error de 1KB |
| `-X`, `--filter-regex` | Excluye por regex en body/headers | `-X "Access Denied"` |
| `-W`, `--filter-words` | Excluye por cantidad de palabras | `-W 0-10` |
| `-N`, `--filter-lines` | Excluye por cantidad de líneas | `-N 50-` |
| `-C`, `--filter-status` | Denylist de códigos HTTP | `-C 404,500` |
| `--filter-similar-to` | Excluye páginas similares a una de referencia | `--filter-similar-to error.html` |
| `-s`, `--status-codes` | Allowlist de códigos (default: todos) | `-s 200,204,301,302` |

```bash
feroxbuster --url http://example.com -w wordlist.txt -s 200 -S 10240 -X "error"
```

## 🎯 Demostración: por qué importa filtrar

Muchas herramientas ya aplican un filtro **por defecto** aunque no se especifique explícitamente. Por ejemplo, ffuf sin `-mc` usa como matcher por defecto: `200-299,301,302,307,401,403,405,500`.

Si se fuerza `-mc all` (sin filtrado real), la salida se inunda de `404 Not Found` repetidos, haciendo casi imposible detectar el hallazgo relevante entre el ruido.

```bash
# Sin filtrar de verdad -> ruido de 404 tapa todo
ffuf -u http://IP:PORT/post.php -X POST -H "Content-Type: application/x-www-form-urlencoded" -d "y=FUZZ" -w /usr/share/seclists/Discovery/Web-Content/common.txt -v -mc all
```

## 🧠 Quiz de repaso

<details>
<summary>¿Por qué ffuf trae un matcher por defecto en vez de mostrar todo?</summary>

Porque sin filtrar, la inmensa mayoría de intentos de fuzzing devuelven `404 Not Found` (recurso inexistente), lo cual generaría cientos o miles de líneas irrelevantes que ocultan los pocos resultados realmente valiosos. El matcher por defecto (`200-299,301,302,307,401,403,405,500`) prioriza códigos que típicamente indican un recurso real (exitoso, redirect, o protegido), reduciendo el ruido sin necesidad de configuración manual.

</details>

<details>
<summary>Distintas herramientas usan nomenclaturas distintas para "excluir por código" — ¿qué patrón conceptual comparten?</summary>

Todas implementan el mismo concepto de denylist de status codes: código(s) que, al matchear la respuesta, la excluyen de la salida — típicamente usado para descartar `404` u otros códigos de "no encontrado/error genérico" que generan ruido. La lógica inversa (allowlist, mostrar solo ciertos códigos) también existe en todas. Entender el patrón conceptual (allowlist vs denylist de código, tamaño, palabras, líneas o regex) permite trasladar la lógica de filtrado de una herramienta a otra sin memorizar cada flag desde cero.

</details>

## 🔗 Relacionado
- [02 — Tooling](02-tooling.md)
- [05 — Parameter and Value Fuzzing](05-parameter-value-fuzzing.md)
- [03 — Directory and File Fuzzing](03-directory-file-fuzzing.md)
- [08 — Validating Findings](08-validating-findings.md)

#cwes #modulo05 #web-fuzzing #filtering #ffuf #gobuster #wenum #feroxbuster
