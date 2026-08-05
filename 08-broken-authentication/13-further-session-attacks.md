# Módulo 08 — Broken Authentication

## Sección 13/14: Further Session Attacks

> [!NOTE]
> Dos vectores adicionales: **Session Fixation** (la app no rota el token tras login) e **Improper Session Timeout** (sesiones que nunca expiran).

## 🛠️ Session Fixation

> [!WARNING]
> Ocurre cuando la app no asigna un nuevo token tras autenticación exitosa.

**Flujo del ataque:**
1. Atacante obtiene un token válido (ej. `a1b2c3d4e5f6`) y hace logout
2. Envía a la víctima un link tipo `http://vulnerable.htb/?sid=a1b2c3d4e5f6`
3. Víctima se autentica — como la app no rota el token, usa el mismo que el atacante conoce
4. Atacante secuestra la sesión con el token conocido

> [!TIP]
> Mitigación: generar un token nuevo y aleatorio inmediatamente después de una autenticación exitosa.

## 🛠️ Improper Session Timeout

> [!WARNING]
> Sin timeout definido, un token sigue siendo válido indefinidamente.

> [!TIP]
> No hay un valor universal — depende del contexto de negocio (datos de salud → minutos; red social → horas).

## 🧠 Quiz de repaso

<details>
<summary>¿Por qué rotar el token tras login es suficiente contra Session Fixation, y no "usar tokens más aleatorios"?</summary>

Porque Session Fixation no depende de que el token sea predecible — el problema es que el atacante ya lo conoce de antemano. Ningún nivel de aleatoriedad soluciona esto; la única defensa es invalidar el token pre-existente y generar uno nuevo al login.

</details>

<details>
<summary>¿Por qué un timeout largo amplifica el impacto de otras vulnerabilidades del módulo?</summary>

Porque no es una vulnerabilidad independiente, sino un multiplicador del impacto temporal de cualquier compromiso de sesión ya logrado por otros medios.

</details>

## 🔗 Relacionado
- [12 — Attacking Session Tokens](12-attacking-session-tokens.md)
- [11 — Authentication Bypass via Parameter Modification](11-authentication-bypass-via-parameter-modification.md)

#cwes #modulo08 #broken-authentication #session-fixation #session-timeout
