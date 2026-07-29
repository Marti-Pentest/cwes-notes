# Módulo 06 — JavaScript Deobfuscation

## Sección 9/11: Decoding

> [!NOTE]
> El código ofuscado suele contener bloques de texto **codificados** que se decodifican en tiempo de ejecución. Esta sección cubre los 3 métodos de encoding más comunes: **Base64**, **Hex** y **Caesar/ROT13** — cómo reconocerlos y cómo codificar/decodificar cada uno.

## 📌 Base64

Reduce el uso de caracteres especiales — cualquier input, incluso binario, se representa solo con caracteres alfanuméricos más `+` y `/`.

> [!TIP]
> Cómo identificarlo: solo caracteres alfanuméricos + `+`/`/`, y su rasgo más distintivo: **padding con `=`**. La longitud del string codificado siempre es múltiplo de 4.

```bash
# Encode
echo https://www.hackthebox.eu/ | base64
# → aHR0cHM6Ly93d3cuaGFja3RoZWJveC5ldS8K

# Decode
echo aHR0cHM6Ly93d3cuaGFja3RoZWJveC5ldS8K | base64 -d
# → https://www.hackthebox.eu/
```

## 📌 Hex

Codifica cada carácter según su valor hexadecimal en la tabla ASCII (`a` = `61`, `b` = `62`, etc.). Tabla completa: `man ascii`.

> [!TIP]
> Cómo identificarlo: solo usa 16 caracteres posibles: `0-9` y `a-f`.

```bash
# Encode
echo https://www.hackthebox.eu/ | xxd -p
# → 68747470733a2f2f7777772e6861636b746865626f782e65752f0a

# Decode
echo 68747470733a2f2f7777772e6861636b746865626f782e65752f0a | xxd -p -r
# → https://www.hackthebox.eu/
```

## 📌 Caesar / ROT13

Cifrado clásico: desplaza cada letra un número fijo de posiciones. ROT13 (el más común) desplaza 13 posiciones.

> [!TIP]
> Cómo identificarlo: aunque parece aleatorio, cada carácter mapea siempre al mismo carácter sustituto — ej. `http://www` se convierte en `uggc://jjj` en ROT13, y ese patrón repetitivo puede dar la pista.

```bash
# Encode
echo https://www.hackthebox.eu/ | tr 'A-Za-z' 'N-ZA-Mn-za-m'
# → uggcf://jjj.unpxgurobk.rh/

# Decode (mismo comando — ROT13 es simétrico)
echo uggcf://jjj.unpxgurobk.rh/ | tr 'A-Za-z' 'N-ZA-Mn-za-m'
# → https://www.hackthebox.eu/
```

> [!TIP]
> Alternativa online: herramientas como [rot13.com](https://rot13.com) permiten codificar/decodificar sin usar terminal.

## 📌 Otros tipos de encoding

> [!TIP]
> Cuando no se reconoce el encoding, herramientas como Cipher Identifier ayudan a determinar automáticamente qué tipo de encoding se está usando.

> [!WARNING]
> Encoding vs Encriptación: muchas herramientas de ofuscación van un paso más allá y usan **encriptación** (requiere una clave para revertir), no solo encoding. Esto puede volver el código extremadamente difícil de revertir, especialmente si la clave de desencriptado no está embebida en el propio script.

## 🧠 Quiz de repaso

<details>
<summary>¿Por qué el padding con = es la pista más confiable para identificar Base64?</summary>

Porque muchos otros encodings también producen strings alfanuméricos, así que ese rasgo por sí solo es ambiguo. El padding, en cambio, es una regla estructural específica de Base64: la longitud del string codificado siempre debe ser múltiplo de 4, y cuando no lo es naturalmente, se completa con uno o dos signos `=` al final. Ver ese patrón específico es una señal mucho más confiable de que se trata de Base64.

</details>

<details>
<summary>¿Por qué encoding y encriptación no deberían tratarse como sinónimos al analizar código ofuscado?</summary>

Porque el encoding es una transformación reversible sin necesidad de ningún secreto — cualquiera que reconozca el método puede revertirlo con las herramientas estándar. La encriptación, en cambio, requiere una clave para revertir la transformación; si esa clave no está presente en el script, ningún análisis del código por sí solo permite recuperar el valor original. Confundir ambos lleva a subestimar cuánto esfuerzo (o si es siquiera posible) revertir un bloque de texto ofuscado.

</details>

> [!NOTE]
> **Ejercicio práctico (lab)** — El lab pide determinar el tipo de encoding usado en la respuesta obtenida en la sección anterior y decodificarla, para luego enviarla como parámetro `serial` en un nuevo `POST` a `serial.php` y obtener la flag. Metodología: identificar el encoding por sus características (el padding `=` sugiere Base64), decodificar con `base64 -d`, y reenviar el resultado con `curl -X POST -d "serial=..."`. No se documenta el valor decodificado ni la flag del lab, solo el enfoque.

## 🔗 Relacionado
- [08 — HTTP Requests](08-http-requests.md)
- [05 — Advanced Obfuscation](05-advanced-obfuscation.md)

#cwes #modulo06 #javascript-deobfuscation #base64 #hex #rot13 #encoding
