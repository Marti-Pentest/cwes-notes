# Módulo 07 — Server-side Attacks

## Sección 7/19: Template Engines

> [!NOTE]
> Un **template engine** combina plantillas predefinidas con datos generados dinámicamente para producir respuestas — el caso típico es un sitio con headers/footers compartidos. Ejemplos populares: **Jinja** y **Twig**.

## 🛠️ Cómo funciona el "rendering"

Un template engine recibe: la plantilla (con marcadores predefinidos) y valores (pares clave-valor). El proceso de combinarlos se llama **rendering**.

### Ejemplo básico (sintaxis Jinja)

```jinja2
Hello {{ name }}!
```

Con `name="vautia"`, produce: `Hello vautia!`

### Ejemplo con lógica (loops)

```jinja2
{% for name in names %}
Hello {{ name }}!
{% endfor %}
```

Con `names=["vautia", "21y4d", "Pedant"]`, produce:
```
Hello vautia!
Hello 21y4d!
Hello Pedant!
```

> [!TIP]
> El hecho de que la sintaxis de template soporte lógica de programación (loops, condiciones, y acceso a objetos/funciones del lenguaje subyacente) es precisamente lo que hace peligrosa la inyección de plantillas — no es solo "insertar texto", es potencialmente ejecutar lógica arbitraria del motor si el atacante logra inyectar su propia sintaxis de template.

## 🧠 Quiz de repaso

<details>
<summary>¿Por qué el hecho de que un template engine soporte loops y condicionales es relevante para entender el riesgo de SSTI?</summary>

Porque si el motor solo pudiera sustituir variables por valores estáticos, la superficie de ataque sería mínima. Pero al soportar construcciones de control de flujo y acceso a objetos/métodos del lenguaje subyacente, la sintaxis de template se acerca a un lenguaje de programación en miniatura. Si un atacante logra que su input sea interpretado como sintaxis de template, no solo inserta texto: ejecuta lógica dentro del motor, lo cual puede escalar hasta ejecución de código arbitrario.

</details>

## 🔗 Relacionado
- [06 — Preventing SSRF](06-preventing-ssrf.md)
- [08 — Introduction to SSTI](08-introduction-to-ssti.md)

#cwes #modulo07 #server-side-attacks #ssti #template-engines #jinja #twig
