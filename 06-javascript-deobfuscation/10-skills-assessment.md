# Módulo 06 — JavaScript Deobfuscation

## Sección 10/11: Skills Assessment

> [!NOTE]
> Assessment práctico que simula un pentest real: un servidor web con JavaScript y APIs cuya funcionalidad hay que descubrir. Integra todo el flujo del módulo de punta a punta — desde ubicar el JS en el HTML hasta decodificar un valor final para obtener la flag.

> [!NOTE]
> **Metodología general aplicada (sin flags):**
> 1. **Ubicar el JS**: revisar el código fuente HTML (`CTRL+U`) para identificar el archivo `.js` referenciado (`<script src="...">`)
> 2. **Ejecutarlo tal cual**: correr el script (o observar su comportamiento en el navegador) para ver si produce algún resultado directo
> 3. **Deofuscar**: como el código está ofuscado, aplicar las técnicas del módulo — beautify + herramienta de unpacking (o sustituir `eval` por `console.log`) para recuperar el código legible y extraer variables relevantes (ej. una variable `flag`)
> 4. **Analizar y replicar**: leer la lógica deofuscada para entender qué requests hace (típicamente vía `XMLHttpRequest`), y replicar esa llamada manualmente con `curl` para obtener un valor devuelto por el servidor (una "secret key")
> 5. **Decodificar y cerrar el ciclo**: identificar el tipo de encoding de esa key (Base64/Hex/ROT13), decodificarla, y reenviarla como parámetro en un nuevo `POST` para obtener la flag final

> [!TIP]
> Todo el assessment se resuelve con las mismas herramientas ya vistas: vista de código fuente del navegador, un beautifier/unpacker online, `curl` para replicar requests, y `base64`/`xxd`/`tr` (o Cipher Identifier) para el decoding final.

No se documentan las flags ni respuestas exactas del assessment, respetando los términos de HTB Academy — solo la metodología aplicada.

## 🔗 Relacionado
- [02 — Source Code](02-source-code.md)
- [06 — Deobfuscation](06-deobfuscation.md)
- [07 — Code Analysis](07-code-analysis.md)
- [08 — HTTP Requests](08-http-requests.md)
- [09 — Decoding](09-decoding.md)

#cwes #modulo06 #javascript-deobfuscation #skills-assessment
