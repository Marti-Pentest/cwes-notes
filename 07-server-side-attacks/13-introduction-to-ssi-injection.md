# Módulo 07 — Server-side Attacks

## Sección 13/19: Introduction to SSI Injection

> [!NOTE]
> **SSI (Server-Side Includes)** genera contenido dinámico dentro de páginas HTML estáticas mediante directivas embebidas. **SSI Injection** ocurre cuando un atacante logra inyectar sus propias directivas en un archivo que el servidor procesa y ejecuta.

## 📌 Identificando el uso de SSI

> [!WARNING]
> La extensión de archivo no es concluyente. Extensiones típicas: `.shtml`, `.shtm`, `.stm`, pero los servidores pueden configurarse para procesar directivas SSI en cualquier extensión.

## 📌 Sintaxis

```
<!--#nombre param1="valor1" param2="valor2" -->
```

## 🛠️ Directivas comunes

| Directiva | Función | Ejemplo |
|---|---|---|
| `printenv` | Imprime variables de entorno | `<!--#printenv -->` |
| `config` | Cambia configuración de SSI | `<!--#config errmsg="Error!" -->` |
| `echo` | Imprime el valor de una variable | `<!--#echo var="DOCUMENT_NAME" -->` |
| `exec` | Ejecuta un comando | `<!--#exec cmd="whoami" -->` |
| `include` | Incluye un archivo del web root | `<!--#include virtual="index.html" -->` |

> [!WARNING]
> `exec` es la más peligrosa por diseño — ejecuta comandos del sistema directamente. Si un atacante controla el contenido de una directiva `exec`, el resultado es RCE directo.

## 🎯 Cómo ocurre la inyección

1. **Upload de archivos vulnerable**: subir un archivo con directivas SSI maliciosas al web root
2. **Escritura de input de usuario a un archivo en el web root**: sin sanitizar, el input puede convertirse en una directiva ejecutable

## 🧠 Quiz de repaso

<details>
<summary>¿Por qué no se puede confiar solo en la extensión para determinar si un servidor procesa SSI?</summary>

Porque la configuración del servidor web es lo que realmente determina qué extensiones se procesan como SSI — un administrador puede configurar el servidor para interpretar directivas SSI incluso en archivos .html normales.

</details>

<details>
<summary>¿Qué tienen en común los dos escenarios típicos de SSI Injection?</summary>

Ambos comparten el mismo requisito estructural: que el contenido controlado por el atacante termine ubicado dentro de un archivo que el servidor sirve y procesa como SSI. La diferencia está solo en el mecanismo de entrada.

</details>

## 🔗 Relacionado
- [12 — SSTI Tools of the Trade & Preventing SSTI](12-ssti-tools-preventing.md)
- [14 — Exploiting SSI Injection](14-exploiting-ssi-injection.md)

#cwes #modulo07 #server-side-attacks #ssi-injection #fundamentos
