# Módulo 08 — Broken Authentication

## Sección 1/14: What is Authentication

> [!NOTE]
> **Autenticación** es el proceso de verificar que una entidad es quien dice ser. **Autorización**, en cambio, determina qué recursos puede acceder una vez verificada su identidad. Son conceptos distintos y secuenciales: primero se autentica, luego se autoriza. Este módulo se enfoca en atacar mecanismos de autenticación, no en autorización.

## 📊 Autenticación vs Autorización

| | Autenticación | Autorización |
|---|---|---|
| Qué verifica | Identidad | Nivel de acceso |
| Requiere | Credenciales | — |
| Momento | Ocurre antes | Ocurre después |
| Se basa en | Prueba de identidad | Políticas de acceso |

> [!TIP]
> Los formularios de login (usuario + contraseña) son el método de autenticación más extendido en aplicaciones web.

## 📌 Las 3 categorías de métodos de autenticación

| Categoría | Se basa en | Ejemplos |
|---|---|---|
| **Knowledge** | Algo que el usuario sabe | Password, PIN, respuesta a pregunta de seguridad |
| **Ownership** | Algo que el usuario tiene | ID card, security token, app de autenticación |
| **Inherence** | Algo que el usuario es/hace | Huella dactilar, patrón facial, voz |

> [!TIP]
> La biometría es efectiva porque los factores de inherencia están intrínsecamente ligados al individuo.

## 📌 Single-Factor vs Multi-Factor Authentication (MFA)

- **Single-factor**: depende de un único método
- **Multi-factor (MFA)**: combina múltiples métodos, típicamente de categorías distintas
- **2FA**: caso particular de MFA con exactamente dos factores

## 🧠 Quiz de repaso

<details>
<summary>¿Por qué es importante distinguir autenticación de autorización antes de estudiar ataques de "Broken Authentication"?</summary>

Porque son fallas de naturaleza distinta: un fallo de autenticación permite a un atacante hacerse pasar por un usuario legítimo o evadir la identificación, mientras que un fallo de autorización permite a un usuario ya autenticado acceder a recursos que no debería (como IDOR). El módulo se enfoca específicamente en romper o evadir la verificación de identidad en sí.

</details>

<details>
<summary>¿Por qué combinar dos factores de la misma categoría ofrece menos seguridad que combinar categorías distintas?</summary>

Porque ambos factores dentro de la misma categoría comparten el mismo tipo de debilidad estructural. Combinar categorías distintas obliga al atacante a comprometer dos vectores fundamentalmente diferentes — robar una contraseña no le da automáticamente acceso al dispositivo físico que genera el TOTP.

</details>

## 🔗 Relacionado
- [02 — Attacks on Authentication](02-attacks-on-authentication.md)

#cwes #modulo08 #broken-authentication #fundamentos #mfa
