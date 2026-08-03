# Módulo 07 — Server-side Attacks

## Sección 15/19: Preventing SSI Injection

> [!NOTE]
> La prevención combina el principio universal de validar/sanitizar input con medidas específicas: restringir dónde se procesa SSI, y limitar qué directivas están disponibles.

## 🛠️ Medidas de prevención

- **Validación y sanitización de input**
- **Restringir SSI a extensiones específicas** (y directorios específicos)
- **Limitar capacidades de directivas específicas**: ej. deshabilitar `exec` si no se necesita activamente

> [!TIP]
> Las tres medidas siguen la misma lógica: reducir la superficie de ataque en cada capa — qué input puede interpretarse, dónde puede interpretarse SSI, y qué puede hacer SSI una vez interpretado.

## 🧠 Quiz de repaso

<details>
<summary>¿Por qué deshabilitar exec específicamente es una mitigación de alto impacto?</summary>

Porque exec es la única directiva diseñada explícitamente para ejecutar comandos del sistema operativo. Al deshabilitarla, incluso si un atacante logra inyectar otras directivas, el peor escenario se limita a fuga de información en vez de escalar a RCE completo.

</details>

<details>
<summary>¿Por qué restringir extensiones/directorios complementa la sanitización de input?</summary>

Porque actúan en capas distintas: la sanitización previene la inyección en primer lugar, mientras que restringir extensiones/directorios es una medida de contención — incluso si un descuido de código permitiera inyectar una directiva válida, solo se ejecutaría si termina en un archivo/directorio donde el servidor procesa SSI.

</details>

## 🔗 Relacionado
- [13 — Introduction to SSI Injection](13-introduction-to-ssi-injection.md)
- [14 — Exploiting SSI Injection](14-exploiting-ssi-injection.md)

#cwes #modulo07 #server-side-attacks #ssi-injection #prevention
