# Módulo 06 — JavaScript Deobfuscation

## Sección 2/11: Source Code

> [!NOTE]
> La mayoría de los sitios web combinan HTML (estructura), CSS (diseño) y JavaScript (funcionalidad). Aunque todo el código corre client-side y el browser lo renderiza sin que lo notemos, entender la funcionalidad de una página empieza por revisar su **código fuente** — HTML, CSS y JS incluidos.

## 🛠️ Ver el código fuente de una página

```
CTRL + U
```

Abre la vista `view-source:http://SERVER_IP:PORT` con el HTML completo de la página.

> [!TIP]
> Vale la pena leer los comentarios del HTML — los desarrolladores a veces dejan ahí información sensible que puede ser aprovechada (rutas internas, credenciales de prueba, notas de debugging, etc.).

## 📌 CSS: interno vs externo

**Interno** (dentro del mismo HTML, entre `<style>`):

```html
<style>
    *,
    html {
        margin: 0;
        padding: 0;
        border: 0;
    }
    h1 {
        font-size: 144px;
    }
</style>
```

**Externo** (archivo `.css` separado, referenciado en el `<head>`):

```html
<head>
    <link rel="stylesheet" href="style.css">
</head>
```

## 📌 JavaScript: interno vs externo

Mismo concepto que CSS — puede estar entre `<script>` en el HTML, o en un archivo `.js` externo referenciado así:

```html
<script src="secret.js"></script>
```

> [!WARNING]
> Al abrir el archivo `.js` referenciado, el código puede aparecer completamente incomprensible, por ejemplo envuelto en `eval(function(p,a,c,k,e,d){...})`. Esto es **ofuscación de código** — el tema central del resto del módulo: qué es, cómo se hace y dónde se usa.

```javascript
eval(function (p, a, c, k, e, d) { e = function (c) { ... } ... }('...SNIP...'.split('|'), 0, {}))
```

## 🧠 Quiz de repaso

<details>
<summary>¿Por qué revisar el código fuente HTML es un primer paso útil incluso antes de tocar JavaScript o CSS?</summary>

Porque el HTML es el punto de entrada que referencia todo lo demás — ahí se ven los `<script src="...">` y `<link rel="stylesheet" href="...">` que apuntan a los archivos externos de JS y CSS, además de posibles comentarios dejados por developers con información relevante. Sin mirar primero el HTML, no se sabría siquiera dónde buscar la lógica funcional de la página (archivo externo vs código inline).

</details>

> [!NOTE]
> **Ejercicio práctico (lab)** — El lab pide repetir el proceso mostrado (ver código fuente con `CTRL+U`, identificar el script externo referenciado, y revisarlo) para encontrar una flag secreta. Metodología: abrir la vista de código fuente de la página, localizar el `<script src="...">`, abrir ese archivo `.js` y revisar su contenido (aunque esté ofuscado, en esta sección basta con inspeccionar el código fuente, sin deofuscar todavía). No se documenta la flag específica del lab, solo el enfoque.

## 🔗 Relacionado
- [01 — Introduction](01-introduction.md)

#cwes #modulo06 #javascript-deobfuscation #html #css #javascript #source-code
