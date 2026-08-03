# Módulo 07 — Server-side Attacks

## Sección 5/19: Blind SSRF

> [!NOTE]
> En **blind SSRF**, la respuesta del recurso obtenido internamente no se refleja al atacante — solo se puede confirmar que el request ocurrió. Esto restringe drásticamente el impacto, aunque igual se puede extraer información indirecta a partir de diferencias en el comportamiento de la app.

## 🛠️ Identificando Blind SSRF

Se confirma la SSRF igual que antes (servidor propio + listener), pero al apuntar la app a sí misma, la respuesta **no** contiene el HTML esperado — solo un mensaje genérico. Esa ausencia de reflejo confirma que es ciega.

## 🛠️ Explotando Blind SSRF: diferencias de comportamiento como canal de información

### Port scanning ciego

- Puerto cerrado → `Something went wrong!`
- Puerto abierto (responde HTTP válido) → mensaje distinto

> [!WARNING]
> Si un servicio interno está abierto pero no responde con HTTP válido (ej. MySQL), la app puede no distinguirlo de un puerto cerrado — el port scanning ciego solo detecta confiablemente servicios que responden como HTTP.

### Enumeración de archivos (ciega)

Misma lógica: no se puede leer el contenido, pero sí detectar existencia si el mensaje de error difiere entre archivo existente e inexistente.

> [!TIP]
> La ceguera no elimina el vector, solo lo limita: se puede enumerar puertos abiertos, enumerar archivos existentes, e interactuar a ciegas con aplicaciones internas probando payloads comunes sin poder ver la respuesta.

## 🧠 Quiz de repaso

<details>
<summary>¿Por qué el impacto de una SSRF ciega es generalmente menor que uno no ciego?</summary>

Porque el valor real de una SSRF no ciega está en la capacidad de leer el contenido de la respuesta del recurso interno, permitiendo exfiltrar datos directamente. En una SSRF ciega, el servidor hace el mismo request, pero el atacante nunca ve el resultado — solo puede inferir información binaria (sí/no) a partir de diferencias de comportamiento observables externamente, lo cual es mucho más lento y limitado.

</details>

<details>
<summary>¿Por qué el mismo principio sirve tanto para port scanning como para enumeración de archivos en un contexto blind?</summary>

Porque en ambos casos se explota aprovechando que la aplicación maneja de forma distinta dos posibles resultados de una operación internamente, y ese manejo distinto se filtra hacia la respuesta visible externamente (un mensaje de error genérico distinto según el caso). El patrón subyacente es el mismo: encontrar cualquier diferencia observable que correlacione con un estado interno binario, y usarla como canal de información indirecto.

</details>

> [!NOTE]
> **Ejercicio práctico (lab)** — El lab pide explotar la SSRF ciega para identificar puertos abiertos además del 80. Metodología: automatizar requests con distintos puertos, filtrando por el mensaje de error específico de "puerto cerrado" para aislar los puertos abiertos. No se documenta el puerto específico del lab, solo el enfoque.

## 🔗 Relacionado
- [03 — Identifying SSRF](03-identifying-ssrf.md)
- [04 — Exploiting SSRF](04-exploiting-ssrf.md)
- [06 — Preventing SSRF](06-preventing-ssrf.md)

#cwes #modulo07 #server-side-attacks #ssrf #blind-ssrf
