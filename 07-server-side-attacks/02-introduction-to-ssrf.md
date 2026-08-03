# Módulo 07 — Server-side Attacks

## Sección 2/19: Introduction to SSRF

> [!NOTE]
> **SSRF** (parte del OWASP Top 10) ocurre cuando una aplicación web obtiene recursos remotos (típicamente vía una URL) basándose en input suministrado por el usuario, y un atacante logra coaccionar al servidor para que haga requests a URLs arbitrarias que él controla.

## 🎯 Por qué importa el esquema/protocolo de la URL

Si la app confía en un esquema de URL suministrado por el usuario, el atacante puede manipular ese esquema para lograr comportamiento adicional no deseado.

| Esquema | Función | Uso en explotación de SSRF |
|---|---|---|
| `http://` / `https://` | Obtiene contenido vía requests HTTP/S | Evadir WAFs, acceder a endpoints restringidos, o alcanzar servicios en la red interna |
| `file://` | Lee un archivo del sistema de archivos local | Leer archivos locales del servidor web (equivalente a un LFI) |
| `gopher://` | Envía bytes arbitrarios a la dirección especificada | Enviar requests HTTP POST con payloads arbitrarios, o comunicarse con otros servicios como SMTP o bases de datos |

> [!WARNING]
> A diferencia de `http://`/`https://` o `file://`, `gopher://` permite construir **bytes arbitrarios** en la conexión — esto habilita al atacante a "hablar" con protocolos completamente distintos al HTTP a través del mismo punto de entrada SSRF.

> [!TIP]
> El módulo remite a **Modern Web Exploitation Techniques** para técnicas avanzadas de explotación de SSRF (bypass de filtros, DNS rebinding) — no cubiertas en este módulo introductorio.

## 🧠 Quiz de repaso

<details>
<summary>¿Por qué SSRF puede tener "consecuencias devastadoras" aunque a primera vista parezca solo "el servidor hace un request extra"?</summary>

Porque el servidor típicamente tiene acceso a una red interna, credenciales, o servicios que no están expuestos públicamente — el atacante, al forzar al servidor a hacer el request en su nombre, hereda esa posición de red privilegiada sin necesidad de estar físicamente dentro de esa red. Esto puede traducirse en acceso a metadatos de infraestructura cloud, paneles de administración internos, bases de datos sin autenticación expuesta solo a la red interna, o servicios que confían implícitamente en requests que vienen "desde el propio servidor".

</details>

<details>
<summary>¿Cuál es la diferencia práctica entre restringir solo el dominio de destino vs restringir también el esquema de la URL en una mitigación de SSRF?</summary>

Restringir solo el dominio no previene que un atacante use un esquema distinto a http/https para el mismo host o para alcanzar otros recursos — como `file://` para leer archivos locales del propio servidor. Una mitigación robusta necesita validar explícitamente qué esquemas están permitidos (idealmente solo `https://` con una allowlist estricta de dominios), no asumir que controlar el dominio es suficiente cuando el esquema también es controlable por el atacante.

</details>

## 🔗 Relacionado
- [01 — Introduction](01-introduction.md)
- [03 — Identifying SSRF](03-identifying-ssrf.md)

#cwes #modulo07 #server-side-attacks #ssrf #owasp-top10
