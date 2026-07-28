# Módulo 05 — Web Fuzzing

## Sección 3/12: Directory and File Fuzzing

> [!NOTE]
> Las apps web suelen tener directorios y archivos no enlazados ni visibles desde la UI. Estos recursos ocultos pueden contener info sensible (backups, configs, logs), contenido desactualizado con vulnerabilidades conocidas, entornos de desarrollo/staging, o funcionalidades no documentadas. El **directory & file fuzzing** sondea sistemáticamente estos recursos probando nombres candidatos y analizando las respuestas del servidor.

## 🎯 Por qué importa encontrar assets ocultos

- Las áreas ocultas suelen tener **menos controles de seguridad** que los componentes públicos → blanco prioritario
- Incluso sin hallar una vuln directa, la info obtenida (stack tecnológico, estructura) es valiosa para fases posteriores del pentest
- Da una imagen más completa de la superficie de ataque de la aplicación

## 📌 Wordlists

> [!TIP]
> **SecLists** — La colección de wordlists más completa y usada es [SecLists](https://github.com/danielmiessler/SecLists). En Pwnbox está en `/usr/share/seclists/` (todo minúscula); en otras distros puede aparecer como `SecLists` — si un comando falla, verificar el path primero.

Las herramientas (ffuf, wfuzz, etc.) no traen wordlists integradas — están diseñadas para trabajar con archivos de wordlist externos.

| Wordlist | Uso |
|---|---|
| `Discovery/Web-Content/common.txt` | Propósito general, buen punto de partida |
| `Discovery/Web-Content/DirBuster-2007_directory-list-2.3-medium.txt` | Enfocado en directorios, más extenso |
| `Discovery/Web-Content/raft-large-directories.txt` | Colección masiva de nombres de directorios |
| `Discovery/Web-Content/big.txt` | Wordlist masiva, directorios + archivos combinados |

## 🛠️ Cómo funciona ffuf

1. **Wordlist**: se provee un archivo con nombres candidatos
2. **URL con keyword `FUZZ`**: placeholder donde se insertan las entradas de la wordlist
3. **Requests**: ffuf itera la wordlist, reemplaza `FUZZ` y envía requests HTTP
4. **Response Analysis**: analiza status codes, content length, etc. y filtra según criterio

```
http://localhost/FUZZ
```

## 🛠️ Directory Fuzzing

```bash
ffuf -w /usr/share/seclists/Discovery/Web-Content/DirBuster-2007_directory-list-2.3-medium.txt -u http://IP:PORT/FUZZ
```

- `-w`: path a la wordlist
- `-u`: URL base, con `FUZZ` como placeholder

Un `301 (Moved Permanently)` en un directorio descubierto es indicador de un directorio válido — punto de entrada potencial para seguir investigando.

## 🛠️ File Fuzzing

Una vez encontrado un directorio, se profundiza buscando archivos específicos dentro (o en la raíz).

> [!NOTE]
> Extensiones comunes a fuzzear: `.php` (server-side scripting) · `.html` (páginas) · `.txt` (logs/info simple) · `.bak` (backups — puede exponer credenciales de DB o API keys si se combina con el nombre de un archivo real, ej. `config.php.bak`) · `.js` (funcionalidad dinámica)

```bash
ffuf -w /usr/share/seclists/Discovery/Web-Content/common.txt -u http://IP:PORT/w2ksvrus/FUZZ -e .php,.html,.txt,.bak,.js -v
```

- `-e`: lista de extensiones a probar contra cada palabra de la wordlist
- `-v`: verbose, muestra el detalle de cada match

Un `200 OK` con contenido en el body indica un archivo real encontrado (ej. `index.html` como página default del directorio, o archivos con nombres menos obvios que ameritan revisión manual).

## 🧠 Quiz de repaso

<details>
<summary>¿Cuál es la diferencia práctica entre directory fuzzing y file fuzzing?</summary>

Directory fuzzing busca carpetas/rutas ocultas usando una wordlist de nombres de directorio contra la URL raíz. File fuzzing va un paso más allá: una vez identificado un directorio, se fuzzea dentro de él (o en la raíz) buscando archivos concretos, normalmente combinando una wordlist de nombres con una lista de extensiones (`-e`) para cubrir más superficie con menos esfuerzo.

</details>

<details>
<summary>¿Por qué un archivo .bak es especialmente interesante durante el fuzzing?</summary>

Porque suele ser una copia de seguridad de un archivo real (ej. `config.php.bak`) que el servidor no procesa como código ejecutable, sino que lo sirve como texto plano — exponiendo su contenido fuente, que puede incluir credenciales de base de datos, API keys u otra info sensible que en el archivo original estaría oculta por la ejecución del intérprete.

</details>

> [!NOTE]
> **Ejercicio práctico (lab)** — La sección incluye un lab con el path `webfuzzing_hidden_path` donde se pide fuzzear primero carpetas y luego archivos para encontrar una flag. Metodología aplicada: 1) directory fuzzing sobre `/webfuzzing_hidden_path/` con una wordlist tipo `common.txt` o `directory-list-medium`, 2) una vez identificado el subdirectorio válido (código 200/301), file fuzzing dentro de él con extensiones relevantes (`.txt`, `.php`, `.bak`, etc.) hasta dar con el archivo que expone la flag. No se documenta la respuesta exacta del lab, solo el enfoque.

## 🔗 Relacionado
- [01 — Introduction](01-introduction.md)
- [02 — Tooling](02-tooling.md)
- [04 — Recursive Fuzzing](04-recursive-fuzzing.md)

#cwes #modulo05 #web-fuzzing #directory-fuzzing #file-fuzzing #ffuf #seclists
