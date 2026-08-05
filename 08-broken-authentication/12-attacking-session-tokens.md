# Módulo 08 — Broken Authentication

## Sección 12/14: Attacking Session Tokens

> [!NOTE]
> Si un session token no tiene suficiente entropía, puede brute-forcearse. Si contiene datos codificados (no cifrados) sin protección, puede falsificarse directamente.

## 🛠️ Brute-Force de session tokens débiles

### Token corto
```
Set-Cookie: session=a5fd
```

### Token largo con partes estáticas
Capturando múltiples tokens:
```
2c0c58b27c71a2ec5bf2b4b6e892b9f9
2c0c58b27c71a2ec5bf2b4546092b9f9
```

> [!WARNING]
> 28 de 32 caracteres son idénticos — solo 4 son realmente aleatorios. Entropía efectiva mucho menor que la longitud sugiere.

### Identificador incremental
```
141233, 141234, 141237, 141238, 141240
```

> [!TIP]
> Capturar múltiples tokens es esencial para detectar patrones de baja entropía.

## 🛠️ Session tokens predecibles: datos codificados sin protección

### Base64
```bash
echo -n dXNlcj1odGItc3RkbnQ7cm9sZT11c2Vy | base64 -d
# user=htb-stdnt;role=user
```

> [!WARNING]
> No hay firma ni cifrado que impida modificar y re-codificar:
```bash
echo -n 'user=htb-stdnt;role=admin' | base64
# dXNlcj1odGItc3RkbnQ7cm9sZT1hZG1pbg==
```

### Hex (mismo principio)
```bash
echo -n 'user=htb-stdnt;role=admin' | xxd -p
```

> [!WARNING]
> Encoding vs Encryption: si el token está cifrado (no solo codificado), atacarlo en blackbox es mucho más difícil sin acceso al código fuente.

## 🧠 Quiz de repaso

<details>
<summary>¿Por qué un único token no basta para evaluar si es vulnerable a brute-force?</summary>

Porque solo comparando múltiples tokens se puede identificar qué porciones cambian (parte aleatoria) y cuáles permanecen idénticas (padding estático sin entropía real).

</details>

<details>
<summary>¿Por qué un token codificado es más débil que uno cifrado?</summary>

Porque el encoding es reversible y público — cualquiera puede decodificar y re-codificar datos modificados sin secreto alguno. El cifrado requiere una clave para generar tokens válidos.

</details>

> [!NOTE]
> **Ejercicio práctico (lab)** — Obtener acceso admin autenticando como htb-stdnt. Metodología: decodificar el token (Base64/hex), modificar el rol a admin, re-codificar.

## 🔗 Relacionado
- [05 — Brute-Forcing Password Reset Tokens](05-brute-forcing-password-reset-tokens.md)
- [11 — Authentication Bypass via Parameter Modification](11-authentication-bypass-via-parameter-modification.md)
- [13 — Further Session Attacks](13-further-session-attacks.md)

#cwes #modulo08 #broken-authentication #session-tokens #entropy #base64 #cookie-forging
