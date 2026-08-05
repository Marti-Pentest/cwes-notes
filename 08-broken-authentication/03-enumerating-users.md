# Módulo 08 — Broken Authentication

## Sección 3/14: Enumerating Users

> [!NOTE]
> **User enumeration** ocurre cuando una app responde distinto ante inputs de autenticación válidos vs inválidos, permitiendo construir una lista de usuarios reales sin conocer sus passwords.

## 🎯 Por qué los usernames importan

> [!TIP]
> Los usuarios suelen reutilizar el mismo username en múltiples servicios (web, FTP, RDP, SSH) — enumerar exitosamente reduce el trabajo del atacante en ataques posteriores.

## 📌 User enumeration no siempre es "un bug"

> [!WARNING]
> Incluso WordPress permite enumeration por diseño (mensajes de error distintos para username inexistente vs password incorrecto). Un buscador de usuarios en una app de chat también "enumera", pero es funcionalidad central del producto.

> [!TIP]
> Mitigación práctica: usar email en vez de username para el login.

## 🛠️ Enumerando vía diferencias en mensajes de error

- Username inválido → `Unknown user`
- Username válido, password inválido → `Invalid credentials`

```bash
ffuf -w /opt/useful/seclists/Usernames/xato-net-10-million-usernames.txt \
  -u http://172.17.0.2/index.php -X POST \
  -H "Content-Type: application/x-www-form-urlencoded" \
  -d "username=FUZZ&password=invalid" \
  -fr "Unknown user"
```

## 🛠️ User enumeration vía side-channel attacks

> [!WARNING]
> Si la app hace un lookup en base de datos solo para usernames válidos, esa diferencia puede generar un tiempo de respuesta distinto, incluso si el mensaje de error es idéntico.

> [!TIP]
> Este tipo de enumeration por timing se cubre en detalle en el módulo Whitebox Attacks.

## 🧠 Quiz de repaso

<details>
<summary>¿Por qué la reutilización de usernames hace que la enumeración sea más valiosa de lo que parece?</summary>

Porque un username confirmado como válido tiene alta probabilidad de ser válido también en otros servicios de la organización o de terceros, convirtiendo una lista de usernames en un insumo reutilizable para múltiples vectores de ataque posteriores.

</details>

<details>
<summary>¿Por qué un ataque de side-channel por timing funciona aunque el mensaje de error sea idéntico?</summary>

Porque el tiempo de respuesta es una señal independiente del contenido del mensaje. Si la app ejecuta un paso adicional (consulta a DB) solo cuando el username existe, ese trabajo extra se traduce en latencia mensurable.

</details>

> [!NOTE]
> **Ejercicio práctico (lab)** — El lab pide enumerar un usuario válido. Metodología: `ffuf` con wordlist de usernames, filtrando el mensaje de error de "usuario desconocido". No se documenta el username específico, solo el enfoque.

## 🔗 Relacionado
- [02 — Attacks on Authentication](02-attacks-on-authentication.md)
- [04 — Brute-Forcing Passwords](04-brute-forcing-passwords.md)

#cwes #modulo08 #broken-authentication #user-enumeration #ffuf #side-channel
