# Módulo 08 — Broken Authentication

## Sección 11/14: Authentication Bypass via Parameter Modification

> [!NOTE]
> Si la autenticación depende de la presencia/valor de un parámetro HTTP, un atacante puede manipularlo para evadir autenticación o escalar privilegios. Relacionado con IDOR.

## 🛠️ Investigando el rol del parámetro

Tras login, la app redirige a `/admin.php?user_id=183`. Remover el parámetro rompe el acceso **incluso con sesión válida**.

> [!WARNING]
> Esto revela que la app usa `user_id` como parte de su lógica de autenticación/autorización, no solo como dato informativo.

## 🛠️ Explotando

```
GET /admin.php?user_id=183
```

> [!WARNING]
> Si se puede adivinar/brute-forcear el `user_id` de un administrador, especificarlo otorga privilegios administrativos sin conocer sus credenciales.

## 📌 Otras vías de bypass no cubiertas en este módulo

- Type juggling → Whitebox Attacks
- Inyección → Injection Attacks / SQL Injection Fundamentals
- Logic bugs → Parameter Logic Bugs

## 🧠 Quiz de repaso

<details>
<summary>¿Por qué remover el parámetro rompiendo el acceso es la pista clave?</summary>

Porque revela que el servidor usa ese parámetro para su decisión de autenticación, en vez de derivar completamente la identidad desde el estado de sesión server-side.

</details>

<details>
<summary>¿Por qué se describe como "relacionada con IDOR" en vez de idéntica?</summary>

Porque un IDOR clásico ocurre después de una autenticación legítima. Aquí el fallo ocurre en la propia decisión de si el usuario está autenticado y con qué privilegios — mezclando autenticación y autorización en el mismo punto.

</details>

> [!NOTE]
> **Ejercicio práctico (lab)** — Autenticar con htb-stdnt/AcademyStudent!, identificar user_id, confirmar su rol removiéndolo, y brute-forcear/adivinar el ID de un admin.

## 🔗 Relacionado
- [10 — Authentication Bypass via Direct Access](10-authentication-bypass-via-direct-access.md)
- [04 — Brute-Forcing Passwords](04-brute-forcing-passwords.md)

#cwes #modulo08 #broken-authentication #authentication-bypass #idor #parameter-modification
