# Módulo 05 — Web Fuzzing

## Sección 5/12: Parameter and Value Fuzzing

> [!NOTE]
> Los **parámetros** son las "variables" que viajan entre el navegador y el servidor, tanto en la URL (**GET**) como en el body del request (**POST**). El fuzzing de parámetros y valores busca manipular esos inputs para descubrir cómo la app procesa datos inesperados — y así detectar vulnerabilidades como XSS, SQLi, accesos no autorizados o funcionalidades ocultas.

## 📌 GET Parameters

Visibles directamente en la URL, después de `?`, separados por `&`:

```
https://example.com/search?query=fuzzing&category=security
```

- `query` = "fuzzing", `category` = "security"
- Son como postales abiertas: cualquiera que vea la URL ve su contenido
- Se usan principalmente para acciones que no cambian el estado del servidor (buscar, filtrar)

## 📌 POST Parameters

Viajan en el body del request, no en la URL — como un sobre cerrado. Preferidos para datos sensibles (credenciales, info personal, financiera).

> [!TIP]
> Flujo de un POST:
> 1. **Data Collection**: se recolectan los datos del formulario
> 2. **Encoding**: se codifican como `application/x-www-form-urlencoded` (pares clave-valor con `&`, igual que GET pero en el body) o `multipart/form-data` (cuando se suben archivos)
> 3. **HTTP Request**: se arma el POST con los datos codificados en el body
> 4. **Server-Side Processing**: el servidor decodifica y procesa según su lógica

```http
POST /login HTTP/1.1
Host: example.com
Content-Type: application/x-www-form-urlencoded

username=your_username&password=your_password
```

## 🎯 Por qué importan los parámetros para fuzzing

- Alterar un product ID en una URL de carrito puede revelar errores de pricing o acceso no autorizado a órdenes de otros usuarios
- Modificar un parámetro oculto puede desbloquear funciones administrativas
- Inyectar código malicioso en un parámetro de búsqueda puede exponer XSS o SQLi

## 🛠️ Fuzzing de parámetros GET con wenum

Primero, reconocimiento manual con curl para entender el comportamiento del endpoint:

```bash
curl http://IP:PORT/get.php
# → Invalid parameter value / x:

curl http://IP:PORT/get.php?x=1
# → Invalid parameter value / x: 1
```

El servidor reconoce el parámetro `x` pero indica que el valor es inválido — señal de que hay una validación específica de valor que vale la pena automatizar.

```bash
wenum -w /usr/share/seclists/Discovery/Web-Content/common.txt --hc 404 -u "http://IP:PORT/get.php?x=FUZZ"
```

- `-w`: path a la wordlist
- `--hc 404`: oculta (hide code) las respuestas con status 404, ya que wenum loggea todo por defecto
- `x=FUZZ`: placeholder del valor a fuzzear

Un `200 OK` entre las respuestas (en vez del `Invalid parameter value` repetido) indica el valor correcto encontrado. Se valida accediendo directamente a esa URL con el valor hallado.

## 🛠️ Fuzzing de parámetros POST con ffuf

El reconocimiento manual usa `-d` para enviar un body vacío:

```bash
curl -d "" http://IP:PORT/post.php
# → Invalid parameter value / y:
```

Automatización con ffuf:

```bash
ffuf -u http://IP:PORT/post.php -X POST -H "Content-Type: application/x-www-form-urlencoded" -d "y=FUZZ" -w /usr/share/seclists/Discovery/Web-Content/common.txt -mc 200 -v
```

- `-X POST`: método HTTP
- `-H`: header de content-type explícito, necesario para que el server interprete el body como form-urlencoded
- `-d "y=FUZZ"`: el payload va en el body, no en la URL — a diferencia del fuzzing GET
- `-mc 200`: matcher, solo muestra respuestas con código 200

> [!WARNING]
> **Contexto real vs lab** — En un escenario real no habría flags explícitas en la respuesta — identificar el valor válido requeriría un análisis más fino de las respuestas (diferencias sutiles de tamaño, tiempo de respuesta, contenido parcial, etc.), no solo un `200 OK` obvio como en el ejercicio.

## 🧠 Quiz de repaso

<details>
<summary>¿Por qué se usa -d en ffuf para fuzzear POST pero se pone el FUZZ en la URL para GET?</summary>

Porque la ubicación del parámetro cambia según el método: en GET, el valor viaja como parte de la query string de la URL, así que el placeholder `FUZZ` se coloca ahí (`?x=FUZZ`). En POST, el valor viaja en el body del request, no en la URL, por lo que `-d "y=FUZZ"` le indica a ffuf que debe insertar el payload dentro del body, no en la URL — y por eso también se necesita el header `Content-Type: application/x-www-form-urlencoded` para que el servidor sepa interpretarlo correctamente.

</details>

<details>
<summary>¿Qué indica el flag --hc 404 en wenum y por qué es útil?</summary>

`--hc` significa "hide code" — le dice a la herramienta que oculte de la salida las respuestas con ese código de estado (404 en este caso). Es útil porque, sin filtrar, wenum muestra el resultado de cada request que hace, incluyendo cientos o miles de intentos fallidos; ocultar los códigos "ruido" (como 404 en directory fuzzing, o el código de "valor inválido" en parameter fuzzing) permite que los resultados realmente interesantes destaquen visualmente en la salida.

</details>

> [!NOTE]
> **Ejercicio práctico (lab)** — El lab pide fuzzear un parámetro GET (`x` en `get.php`) y uno POST (`y` en `post.php`) para encontrar sendas flags. Metodología: reconocimiento manual con `curl` para entender la respuesta ante valores inválidos/vacíos, luego automatización — `wenum` con `--hc 404` para GET, `ffuf` con `-X POST -d` para POST — usando `common.txt` como wordlist, filtrando por el código de respuesta que indica éxito (`200`). No se documentan los valores/flags exactos, solo el enfoque.

## 🔗 Relacionado
- [03 — Directory and File Fuzzing](03-directory-file-fuzzing.md)
- [04 — Recursive Fuzzing](04-recursive-fuzzing.md)
- [02 — Tooling](02-tooling.md)
- [06 — Virtual Host and Subdomain Fuzzing](06-vhost-subdomain-fuzzing.md)

#cwes #modulo05 #web-fuzzing #parameter-fuzzing #get #post #wenum #ffuf
