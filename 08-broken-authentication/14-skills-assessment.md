# Módulo 08 — Broken Authentication

## Sección 14/14: Skills Assessment

> [!NOTE]
> Assessment final que requiere combinar múltiples técnicas del módulo en cadena, contra una app con mecanismo de autenticación "nuevo" y política de passwords reforzada.

> [!NOTE]
> **Metodología general aplicada (sin flag):**
> 1. Reconocimiento del flujo de autenticación completo: login, password reset, 2FA, manejo de sesión
> 2. User enumeration por diferencias de mensajes de error
> 3. Brute-force acotado por la política de passwords anunciada
> 4. Revisar cada paso adicional del flujo por separado (2FA, preguntas de seguridad, consistencia de parámetros)
> 5. Analizar el token de sesión: múltiples muestras, patrones de baja entropía o datos codificados sin protección
> 6. Revisar bypass directo: acceso a endpoints protegidos sin autenticar, manejo de parámetros como user_id

> [!TIP]
> Este assessment integra prácticamente todo el módulo porque no se sabe de antemano cuál eslabón de la cadena de autenticación es el débil.

No se documentan la flag ni la vulnerabilidad específica del assessment, respetando los términos de HTB Academy.

## 🔗 Relacionado
- [03 — Enumerating Users](03-enumerating-users.md)
- [04 — Brute-Forcing Passwords](04-brute-forcing-passwords.md)
- [06 — Brute-Forcing 2FA Codes](06-brute-forcing-2fa-codes.md)
- [09 — Vulnerable Password Reset](09-vulnerable-password-reset.md)
- [11 — Authentication Bypass via Parameter Modification](11-authentication-bypass-via-parameter-modification.md)
- [12 — Attacking Session Tokens](12-attacking-session-tokens.md)
- [13 — Further Session Attacks](13-further-session-attacks.md)

#cwes #modulo08 #broken-authentication #skills-assessment
