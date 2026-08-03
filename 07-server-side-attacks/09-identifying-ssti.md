# Módulo 07 — Server-side Attacks

## Sección 9/19: Identifying SSTI

> [!NOTE]
> Confirmar SSTI sigue la misma lógica que cualquier vulnerabilidad de inyección: enviar caracteres especiales que rompan la sintaxis del motor. Además hace falta identificar qué motor específico está en uso, ya que la sintaxis y técnicas de explotación varían.

## 🛠️ Confirmando SSTI: string de prueba universal

```
${{<%[%'"}}%\.
```

> [!TIP]
> Combina caracteres con significado sintáctico especial en varios motores populares a la vez. Al violar la sintaxis de casi cualquier motor, genera un error si la app es vulnerable — el mismo principio que inyectar una comilla simple para provocar un error de SQLi.

## 🛠️ Identificando el motor específico: árbol de decisión

1. Inyectar `${7*7}` — si no se ejecuta, pasar al siguiente
2. Inyectar `{{7*7}}` — si se ejecuta (resultado `49`), confirma familia `{{ }}`
3. Inyectar `{{7*'7'}}` — payload decisivo entre Jinja y Twig

> [!TIP]
> `{{7*'7'}}` se comporta distinto según el motor:
> - **Jinja**: produce `7777777` (Python interpreta int * str como repetición de string)
> - **Twig**: produce `49` (PHP hace type coercion de '7' a entero antes de multiplicar)

## 🧠 Quiz de repaso

<details>
<summary>¿Por qué el string de prueba combina tantos caracteres distintos en vez de probar uno por vez?</summary>

Porque el objetivo en esta etapa es confirmar sospecha de vulnerabilidad de la forma más eficiente posible, sin saber de antemano qué motor está en uso. Al combinar caracteres con significado especial en varios motores a la vez, un solo request tiene alta probabilidad de romper la sintaxis de cualquiera de ellos.

</details>

<details>
<summary>¿Por qué {{7*'7'}} es tan efectivo para diferenciar Jinja de Twig?</summary>

Porque no depende de que la aplicación exponga información de versión — aprovecha una diferencia de comportamiento fundamental entre los lenguajes base (Python vs PHP) frente a multiplicar un número por una cadena de texto. Python trata int * str como repetición, PHP coacciona el string a número. Esta diferencia es observable con un único payload corto.

</details>

> [!NOTE]
> **Ejercicio práctico (lab)** — El lab pide identificar el motor de templating usado. Metodología: inyectar el string de prueba genérico, luego seguir el árbol de decisión (`${7*7}` → `{{7*7}}` → `{{7*'7'}}`) hasta identificar el motor. No se documenta el resultado exacto, solo el enfoque.

## 🔗 Relacionado
- [08 — Introduction to SSTI](08-introduction-to-ssti.md)
- [10 — Exploiting SSTI - Jinja2](10-exploiting-ssti-jinja2.md)
- [11 — Exploiting SSTI - Twig](11-exploiting-ssti-twig.md)

#cwes #modulo07 #server-side-attacks #ssti #jinja #twig #identification
