# Módulo 06 — JavaScript Deobfuscation

## Sección 6/11: Deobfuscation

> [!NOTE]
> Así como existen herramientas para ofuscar código automáticamente, existen herramientas para revertir ese proceso. El flujo típico tiene dos pasos distintos: primero **beautify** (dar formato legible a código minificado) y luego **deofuscar** propiamente (revertir la lógica de ofuscación, no solo el formato).

## 🛠️ Paso 1: Beautify (dar formato)

El código minificado está todo en una línea. Darle formato no lo deofusca, solo lo hace visualmente legible.

**Con Browser Dev Tools (Firefox):**
```
CTRL + SHIFT + Z   → abre el debugger
```
Al abrir el script, el botón `{ }` (Pretty Print) reformatea el código a indentación estándar.

**Con herramientas online:**
- [Prettier Playground](https://prettier.io/playground/)
- [Beautifier.io](https://beautifier.io/)

> [!WARNING]
> Si el código fue minificado y ofuscado, el beautifier solo mejora la indentación — la lógica sigue oculta detrás del dictionary y la función de reconstrucción. Hace falta un paso adicional de deofuscación real.

## 🛠️ Paso 2: Deofuscar (revertir la lógica)

Herramienta de ejemplo: [UnPacker](https://matthewfl.com/unPacker.html) — especializada en revertir el packing tipo `eval(function(p,a,c,k,e,d){...})`.

> [!TIP]
> No dejar líneas vacías antes del script al pegarlo en la herramienta — puede afectar el proceso de deofuscación y dar resultados inexactos.

Resultado tras deofuscar:

```javascript
function generateSerial() {
  var xhr = new XMLHttpRequest;
  var url = "/serial.php";
  xhr.open("POST", url, true);
  xhr.send(null);
};
```

> [!TIP]
> Técnica alternativa manual: en vez de depender solo de una herramienta externa, se puede localizar el **valor de retorno** al final de la función packer y reemplazar su ejecución (`eval(...)`) por un `console.log(...)` — esto imprime el código ya reconstruido en cleartext sin ejecutarlo.

## 🎯 Cuando las herramientas automáticas no alcanzan

> [!WARNING]
> A medida que el código se vuelve más ofuscado/codificado (especialmente con obfuscadores custom), las herramientas automáticas dejan de ser suficientes. En esos casos hace falta **reverse engineering manual**.

> [!NOTE]
> El módulo menciona el curso **Secure Coding 101** como referencia para deofuscación avanzada y reverse engineering de JavaScript más en profundidad.

## 🧠 Quiz de repaso

<details>
<summary>¿Cuál es la diferencia conceptual entre "beautify" y "deofuscar" un script?</summary>

Beautify solo reformatea la presentación visual del código (indentación, saltos de línea) sin tocar su lógica — útil cuando el código está minificado pero no ofuscado. Deofuscar, en cambio, revierte activamente la transformación lógica aplicada por el obfuscador, recuperando la lógica legible del programa, no solo su formato.

</details>

<details>
<summary>¿Por qué reemplazar eval(...) por console.log(...) al final de un packer es una técnica útil y relativamente segura?</summary>

Porque el packer construye el código original como un string dentro de la variable `p` y luego lo ejecuta con `eval(p)`. Si en vez de ejecutar ese string se lo imprime con `console.log(p)`, se obtiene el código reconstruido en texto plano y legible, sin correr efectivamente la lógica potencialmente maliciosa o desconocida que contiene. Es una forma segura de "leer" el resultado de la deofuscación sin arriesgarse a ejecutar código no confiable.

</details>

> [!NOTE]
> **Ejercicio práctico (lab)** — El lab pide deofuscar `secret.js` para obtener el contenido de una flag. Metodología: tomar el código ofuscado (tipo packing), pasarlo por una herramienta de unpacking (o aplicar la técnica manual de sustituir `eval` por `console.log`), y luego leer/ejecutar la lógica resultante (que hace un request POST a un endpoint) para obtener el valor de la flag. No se documenta la flag específica del lab, solo el enfoque.

## 🔗 Relacionado
- [05 — Advanced Obfuscation](05-advanced-obfuscation.md)
- [02 — Source Code](02-source-code.md)

#cwes #modulo06 #javascript-deobfuscation #deobfuscation #unpacking #reverse-engineering
