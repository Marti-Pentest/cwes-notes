# Módulo 06 — JavaScript Deobfuscation

## Sección 4/11: Basic Obfuscation

> [!NOTE]
> La ofuscación rara vez se hace a mano — se usan herramientas automatizadas. Esta sección cubre dos niveles básicos: **minificación** (reduce legibilidad, casi sin ofuscar realmente) y **packing** (ofuscación real, aunque reversible).

## 🛠️ Código de ejemplo (cleartext)

```javascript
console.log('HTB JavaScript Deobfuscation Module');
```

Se puede probar en [JSConsole](https://jsconsole.com) — pegar y ejecutar, imprime el mensaje directamente.

## 📌 Minificación (Minifying)

Comprime todo el código en una sola línea (a menudo muy larga), reduciendo espacios/saltos de línea. Es más útil en código largo.

> [!TIP]
> Herramienta de ejemplo: [javascript-minifier.com](https://javascript-minifier.com/). Suele guardarse con extensión `.min.js`.

> [!NOTE]
> La minificación no es exclusiva de JavaScript — se aplica también a CSS, HTML, etc.

> [!WARNING]
> Minificación ≠ ofuscación real. Reduce legibilidad por compactación, pero no reescribe la lógica ni oculta nombres/strings — es trivial de revertir con un formateador/beautifier estándar.

## 📌 Packing

Ofuscación más agresiva. Convierte todas las palabras y símbolos del código en una lista/diccionario, y usa una función con firma reconocible para reconstruir el código original en tiempo de ejecución.

```javascript
eval(function(p,a,c,k,e,d){e=function(c){return c};if(!''.replace(/^/,String)){while(c--){d[c]=k[c]||c}k=[function(e){return d[e]}];e=function(){return'\w+'};c=1};while(c--){if(k[c]){p=p.replace(new RegExp('\b'+e(c)+'\b','g'),k[c])}}return p}('5.4(\'3 2 1 0\');',6,6,'Module|Deobfuscation|JavaScript|HTB|log|console'.split('|'),0,{}))
```

Ejecutado en JSConsole, produce el mismo output que el código original en cleartext.

> [!TIP]
> Cómo identificar un "packer": la firma reconocible es la función inicial con **seis argumentos**: `function(p,a,c,k,e,d)`. La combinación de letras puede variar entre distintos packers, pero el patrón (dictionary + reconstrucción vía `eval`) es la pista más confiable.

> [!WARNING]
> Aunque reduce mucho la legibilidad, las **strings principales siguen visibles en cleartext** dentro del dictionary, lo cual puede revelar parte de la funcionalidad del código incluso sin deofuscarlo del todo.

## 🧠 Quiz de repaso

<details>
<summary>¿Por qué la minificación no debería considerarse una técnica de ofuscación real?</summary>

Porque su único efecto es comprimir el código eliminando espacios y saltos de línea innecesarios, sin alterar nombres de variables/funciones, sin reescribir la lógica, y sin ocultar ningún string. Un beautifier/formateador estándar revierte completamente el efecto visual de la minificación, dejando el código tan legible como el original. La ofuscación real busca dificultar la comprensión de la lógica en sí, no solo su presentación visual.

</details>

<details>
<summary>Si un packer deja las strings principales visibles en texto plano, ¿qué tan efectivo es realmente como técnica de ofuscación?</summary>

Es efectivo para dificultar la lectura de la estructura lógica del código (nombres de variables, orden de operaciones, flujo de control quedan ocultos detrás de la función de reconstrucción), pero tiene una limitación clara: cualquiera que inspeccione el dictionary de strings puede ver fragmentos de las palabras/valores originales usados en el código, lo cual puede dar pistas sobre su propósito general. Por eso existen formas de ofuscación más robustas, cubiertas en "Advanced Obfuscation".

</details>

## 🔗 Relacionado
- [03 — Code Obfuscation](03-code-obfuscation.md)
- [05 — Advanced Obfuscation](05-advanced-obfuscation.md)

#cwes #modulo06 #javascript-deobfuscation #minification #packing #obfuscation
