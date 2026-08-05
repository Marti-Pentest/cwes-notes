# Módulo 08 — Broken Authentication

## Sección 5/14: Brute-Forcing Password Reset Tokens

> [!NOTE]
> Los flujos de password recovery dependen de un token de un solo uso. Si tiene poco espacio de valores posibles, puede brute-forcearse — permitiendo tomar control de la cuenta de otro usuario.

## 🛠️ Flujo típico

Usuario olvida password → solicita reset → recibe token → usa el token → cambia password.

## 🎯 Identificando tokens débiles

```
http://weak_reset.htb/reset_password.php?token=7351
```

> [!WARNING]
> Token de 4 dígitos → solo 10,000 valores posibles — trivialmente brute-forceable.

## 🛠️ Explotando el token débil

```bash
seq -w 0 9999 > tokens.txt
```

- `-w`: rellena con ceros a la izquierda, crítico para que el formato coincida exactamente

```bash
ffuf -w ./tokens.txt -u http://weak_reset.htb/reset_password.php?token=FUZZ -fr "The provided token is invalid"
```

> [!TIP]
> Para targetear a un usuario específico, primero hay que disparar un password reset para ese usuario (para que exista un token activo), y luego brute-forcear ese token.

## 🧠 Quiz de repaso

<details>
<summary>¿Por qué el flag -w de seq es crítico y no solo cosmético?</summary>

Porque el fuzzing depende de que el valor coincida exactamente con el formato esperado. Si la app genera tokens con padding de ceros pero el wordlist no los tiene, el candidato nunca coincidirá aunque el valor numérico sea idéntico.

</details>

<details>
<summary>¿Por qué es necesario disparar un reset antes de brute-forcear el token?</summary>

Porque el token no es una clave estática — se genera y asocia a una solicitud de reset activa. Sin una solicitud pendiente, no habría ningún token activo contra el cual el brute-force pudiera tener éxito.

</details>

> [!NOTE]
> **Ejercicio práctico (lab)** — El lab incluye preguntas conceptuales y un ejercicio de tomar control de otra cuenta explotando esta debilidad. Metodología: `seq -w`, disparar reset para el objetivo, brute-force con ffuf.

## 🔗 Relacionado
- [03 — Enumerating Users](03-enumerating-users.md)
- [04 — Brute-Forcing Passwords](04-brute-forcing-passwords.md)
- [06 — Brute-Forcing 2FA Codes](06-brute-forcing-2fa-codes.md)

#cwes #modulo08 #broken-authentication #password-reset #brute-force #ffuf #otp
