# Módulo 06 — JavaScript Deobfuscation

## Sección 5/11: Advanced Obfuscation

> [!NOTE]
> El packing básico todavía deja strings en cleartext dentro del dictionary. La ofuscación avanzada busca eliminar por completo cualquier rastro legible del código original, usando encoding adicional (Base64), nombres de variables sin sentido, y hasta técnicas extremas como JSFuck.

## 🛠️ Obfuscator.io con String Array Encoding en Base64

Herramienta: [obfuscator.io](https://obfuscator.io)

> [!TIP]
> Antes de ofuscar, cambiar **String Array Encoding** a **Base64** — esto codifica las strings del dictionary en vez de dejarlas en texto plano.

Código original:
```javascript
console.log('HTB JavaScript Deobfuscation Module');
```

Resultado ofuscado (fragmento representativo):
```javascript
var _0x1ec6=['Bg9N','sfrciePHDMfty3jPChqGrgvVyMz1C2nHDgLVBIbnB2r1Bgu='];
(function(_0x13249d,_0x1ec6e5){...}(_0x1ec6,0xb4));
var _0x14f8=function(_0x13249d,_0x1ec6e5){...};
console[_0x14f8('0x0')](_0x14f8('0x1'));
```

Al ejecutarlo en JSConsole, produce el mismo output que el original — la funcionalidad se preserva, pero ya no hay strings visibles en cleartext.

## 🛠️ Ofuscación extrema: JSFuck-style

Existen técnicas aún más agresivas que reescriben el código usando solo un conjunto muy limitado de caracteres (`[`, `]`, `(`, `)`, `!`, `+`), generando código extremadamente largo pero funcionalmente equivalente:

```javascript
[][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+...
```

> [!WARNING]
> Este tipo de ofuscación extrema es fácil de ejecutar (sigue siendo JS válido) pero puede volver la ejecución notablemente **más lenta**.

## 📌 Otros obfuscadores mencionados

- **JJEncode**
- **AAEncode**

> [!WARNING]
> Estos obfuscadores suelen hacer que la ejecución/compilación del código sea muy lenta. No se recomienda su uso salvo por una razón puntual y justificada, como intentar evadir filtros o restricciones web.

## 🧠 Quiz de repaso

<details>
<summary>¿Qué gana la ofuscación al codificar el String Array en Base64, comparado con el packing básico?</summary>

El packing básico deja las strings originales visibles en texto plano dentro del dictionary, lo cual puede revelar pistas sobre la funcionalidad del código con solo mirar esa lista. Al codificar el array de strings en Base64, esas mismas strings quedan representadas como texto codificado sin significado aparente a simple vista — hay que decodificarlas activamente para recuperar los valores originales, lo cual eleva considerablemente el esfuerzo necesario para entender el código sin ejecutarlo.

</details>

<details>
<summary>¿Por qué técnicas como JSFuck no son prácticas para uso general a pesar de ofuscar el código casi al 100%?</summary>

Porque el trade-off entre ofuscación y performance se vuelve extremo: el código resultante, aunque funcionalmente idéntico, se vuelve enormemente más largo y computacionalmente costoso de interpretar. Por eso su uso legítimo se reserva para casos puntuales donde el objetivo específico (como evadir un filtro que bloquea ciertos patrones de caracteres) justifica ese costo.

</details>

## 🔗 Relacionado
- [04 — Basic Obfuscation](04-basic-obfuscation.md)
- [06 — Deobfuscation](06-deobfuscation.md)

#cwes #modulo06 #javascript-deobfuscation #advanced-obfuscation #base64 #jsfuck
