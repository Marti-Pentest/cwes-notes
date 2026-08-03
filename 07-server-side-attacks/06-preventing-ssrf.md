# Módulo 07 — Server-side Attacks

## Sección 6/19: Preventing SSRF

> [!NOTE]
> Las mitigaciones contra SSRF se implementan en dos capas: **aplicación** (validar/restringir qué orígenes y esquemas de URL se aceptan) y **red** (firewall + segmentación).

## 🛠️ Mitigaciones a nivel aplicación

- **Whitelist de orígenes remotos**: validar contra una lista blanca explícita
- **Restricción del esquema/protocolo de URL**: no debe aceptarse libremente del input — debe estar hardcodeado o validado contra whitelist
- **Sanitización de input**

## 🛠️ Mitigaciones a nivel red

- **Reglas de firewall restrictivas**: bloquean requests salientes hacia sistemas destino potencialmente interesantes
- **Segmentación de red**: limita qué sistemas internos son alcanzables desde el servidor vulnerable

> [!TIP]
> Referencia adicional: [OWASP SSRF Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Server_Side_Request_Forgery_Prevention_Cheat_Sheet.html)

## 🧠 Quiz de repaso

<details>
<summary>¿Por qué restringir solo el origen permitido no es suficiente si no se restringe también el esquema de la URL?</summary>

Porque el esquema de la URL determina el tipo de operación que se realiza, no solo hacia dónde. Aunque el dominio esté validado, si el atacante puede seguir eligiendo el esquema (file:// en vez de https://), puede desviar la funcionalidad hacia operaciones completamente distintas a la intención original, incluso apuntando al mismo host o a localhost.

</details>

<details>
<summary>¿Por qué la segmentación de red se considera una mitigación válida contra SSRF si el problema está en el código?</summary>

Porque la defensa en profundidad no depende de una sola capa siendo perfecta. Aunque la vulnerabilidad de código siga existiendo, si la segmentación de red hace que el servidor no tenga ruta hacia sistemas sensibles, un atacante que explote la SSRF no podrá alcanzarlos de todos modos — reduciendo drásticamente el impacto sin necesidad de parchear el código inmediatamente.

</details>

## 🔗 Relacionado
- [02 — Introduction to SSRF](02-introduction-to-ssrf.md)
- [05 — Blind SSRF](05-blind-ssrf.md)
- [04 — Exploiting SSRF](04-exploiting-ssrf.md)

#cwes #modulo07 #server-side-attacks #ssrf #prevention #whitelist
