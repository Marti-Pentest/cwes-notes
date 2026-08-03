# Módulo 07 — Server-side Attacks

## Sección 16/19: Intro to XSLT Injection

> [!NOTE]
> **XSLT** transforma documentos XML combinando datos del XML con una plantilla XSL. **XSLT Injection** ocurre cuando input de usuario se inserta en los datos XSL antes de que el procesador genere el output — el mismo patrón conceptual visto en SSTI.

## 📌 Documento XML de ejemplo

```xml
<?xml version="1.0" encoding="UTF-8"?>
<fruits>
    <fruit>
        <name>Apple</name>
        <color>Red</color>
        <size>Medium</size>
    </fruit>
</fruits>
```

## 📌 Elementos XSL principales

| Elemento | Función |
|---|---|
| `<xsl:template match="...">` | Define una plantilla aplicada a un path del XML |
| `<xsl:value-of select="...">` | Extrae el valor de un nodo XML |
| `<xsl:for-each select="...">` | Loop sobre nodos que matchean el select |
| `<xsl:sort select="..." order="...">` | Ordena elementos dentro de un loop |
| `<xsl:if test="...">` | Condicional sobre un nodo |

## 🛠️ Ejemplo básico

```xslt
<xsl:stylesheet version="1.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform">
    <xsl:template match="/fruits">
        Here are all the fruits:
        <xsl:for-each select="fruit">
            <xsl:value-of select="name"/> (<xsl:value-of select="color"/>)
        </xsl:for-each>
    </xsl:template>
</xsl:stylesheet>
```

## 🎯 XSLT Injection: el mismo patrón que SSTI, aplicado a XML

> [!WARNING]
> Ocurre cuando input de usuario se inserta en los datos XSL antes de que el procesador genere el output, permitiendo al atacante inyectar elementos XSL adicionales que el procesador ejecutará.

## 🧠 Quiz de repaso

<details>
<summary>¿Qué paralelismo hay entre XSLT Injection y SSTI?</summary>

Ambas comparten la misma causa raíz: una interfaz de "plantilla" que combina una parte estática con datos dinámicos, segura solo si el input del usuario se mantiene estrictamente como dato y nunca se mezcla con la parte que define la lógica de transformación. Si el input termina dentro del XSL en vez de ser solo un valor extraído del XML, el procesador lo interpretará como elementos XSL ejecutables.

</details>

<details>
<summary>¿Por qué xsl:if con su atributo test es relevante para pensar en el riesgo de inyección?</summary>

Porque revela que XSLT soporta lógica condicional evaluada dinámicamente, no es solo sustitución de texto. Si un atacante lograra controlar el contenido del atributo test, podría potencialmente alterar qué lógica se ejecuta durante la transformación.

</details>

## 🔗 Relacionado
- [15 — Preventing SSI Injection](15-preventing-ssi-injection.md)
- [17 — Exploiting XSLT Injection](17-exploiting-xslt-injection.md)
- [08 — Introduction to SSTI](08-introduction-to-ssti.md)

#cwes #modulo07 #server-side-attacks #xslt #xslt-injection #fundamentos
