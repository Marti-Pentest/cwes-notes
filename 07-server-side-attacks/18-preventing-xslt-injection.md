# Módulo 07 — Server-side Attacks

## Sección 18/19: Preventing XSLT Injection

> [!NOTE]
> La prevención sigue el principio central del módulo: no insertar input de usuario en los datos XSL antes del procesamiento. Cuando hay que reflejar valores del usuario, la clave es aplicar encoding correcto según el formato de salida.

## 🛠️ Medida principal: encoding según el output

Si el XSLT genera HTML, **HTML-encoding** el input antes de insertarlo previene la inyección:
- `<` → `&lt;`
- `>` → `&gt;`

> [!WARNING]
> El encoding depende del formato de salida real — un output distinto podría requerir un esquema de encoding diferente.

## 🛠️ Medidas de hardening adicionales

- **Ejecutar el procesador XSLT con privilegios mínimos**
- **Deshabilitar funciones PHP dentro de XSLT**
- **Mantener la librería XSLT actualizada**

## 🧠 Quiz de repaso

<details>
<summary>¿Por qué el encoding de caracteres es distinto de la sanitización genérica?</summary>

Porque el encoding no bloquea o elimina caracteres, sino que los transforma en una representación equivalente sin significado sintáctico especial en el contexto de salida — preservando el dato original mientras neutraliza su capacidad de ser interpretado como código.

</details>

<details>
<summary>¿Por qué deshabilitar funciones PHP en XSLT es tan directamente relevante?</summary>

Porque el salto de LFI limitado a RCE completo dependió enteramente de que el procesador tuviera habilitada la extensión php:function. Sin esa capacidad, un atacante quedaría limitado a lo que las funciones nativas de XSLT permiten, sin acceso al universo completo de funciones de PHP.

</details>

## 🔗 Relacionado
- [16 — Intro to XSLT Injection](16-intro-to-xslt-injection.md)
- [17 — Exploiting XSLT Injection](17-exploiting-xslt-injection.md)

#cwes #modulo07 #server-side-attacks #xslt #xslt-injection #prevention #html-encoding
