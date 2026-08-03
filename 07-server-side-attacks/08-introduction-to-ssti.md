# Módulo 07 — Server-side Attacks

## Sección 8/19: Introduction to SSTI

> [!NOTE]
> **SSTI** ocurre cuando un atacante logra inyectar código de templating dentro de una plantilla que el servidor renderiza. La distinción clave está en dónde termina el input del usuario: si va como *valor*, el motor lo trata como dato inerte; si termina siendo parte del **string de la plantilla misma**, el motor lo interpreta y ejecuta como sintaxis de template.

## 📌 La distinción crítica: valores vs template string

> [!TIP]
> Uso seguro: si el templating está implementado correctamente, el input del usuario siempre se provee como *valor* a la función de rendering — el motor lo inserta sin ejecutar ningún código contenido en ese valor.

> [!WARNING]
> SSTI aparece cuando el atacante controla el **parámetro de la plantilla en sí**, no solo un valor.

## 🎯 Escenarios típicos donde ocurre SSTI

1. **Input insertado en la plantilla antes de renderizar**
2. **Rendering encadenado / múltiple**: si el input insertado en el output del primer rendering termina siendo parte del string de la plantilla en un segundo pase
3. **Apps que permiten modificar/enviar plantillas directamente**

## 🧠 Quiz de repaso

<details>
<summary>¿Por qué el mismo motor de templating puede ser seguro en un caso y vulnerable a SSTI en otro?</summary>

Porque la vulnerabilidad no está en el motor, sino en cómo la aplicación construye la plantilla que le pasa. Si el input siempre viaja como valor separado, el motor nunca lo interpreta como sintaxis. Si la aplicación concatena el input directamente dentro del string que constituye la plantilla, el motor sí lo interpreta como código al renderizar.

</details>

<details>
<summary>¿Por qué el escenario de "rendering múltiple" es un caso particularmente sutil de SSTI?</summary>

Porque en el primer pase, el input pudo haber sido tratado de forma completamente segura. El riesgo aparece si ese output ya renderizado se usa como plantilla de entrada para un segundo rendering: el motor ya no distingue que ese texto fue un valor de usuario en la ronda anterior, simplemente ve un string de plantilla, y si contiene sintaxis válida del motor, la interpretará como tal.

</details>

## 🔗 Relacionado
- [07 — Template Engines](07-template-engines.md)
- [09 — Identifying SSTI](09-identifying-ssti.md)

#cwes #modulo07 #server-side-attacks #ssti #fundamentos
