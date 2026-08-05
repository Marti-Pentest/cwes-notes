# Módulo 08 — Broken Authentication

## Sección 8/14: Default Credentials

> [!NOTE]
> Muchas apps se instalan con credenciales por defecto que deben cambiarse tras la configuración inicial. Testear esto es parte estándar de la metodología OWASP.

> [!TIP]
> Credenciales comunes por defecto según OWASP: `admin` y `password`.

## 🛠️ Fuentes para buscar credenciales por defecto

- [CIRT.net](https://www.cirt.net/passwords)
- [SecLists Default Credentials](https://github.com/danielmiessler/SecLists/tree/master/Passwords/Default-Credentials)
- [SCADAPASS](https://github.com/scadastrangelove/SCADAPASS/tree/master)

## 🛠️ Búsqueda dirigida

Buscar `<producto> default credentials` a menudo lleva directo a la documentación oficial de instalación. Ejemplo: BookStack documenta `admin@admin.com:password`.

> [!TIP]
> Muchos proyectos documentan públicamente sus credenciales por defecto porque se espera que el admin las cambie tras el setup.

## 🧠 Quiz de repaso

<details>
<summary>¿Por qué testear credenciales por defecto es uno de los primeros pasos recomendados?</summary>

Porque es la vía de menor esfuerzo y mayor probabilidad de éxito cuando aplica — no requiere generar wordlists ni evadir protecciones, solo probar combinaciones conocidas.

</details>

<details>
<summary>¿Qué tienen en común CIRT.net, SecLists y la documentación oficial como fuentes?</summary>

Explotan el mismo hecho: las credenciales por defecto no son un secreto a descubrir — son valores conocidos y a menudo documentados públicamente como parte del proceso de instalación estándar.

</details>

## 🔗 Relacionado
- [07 — Weak Brute-Force Protection](07-weak-brute-force-protection.md)
- [09 — Vulnerable Password Reset](09-vulnerable-password-reset.md)

#cwes #modulo08 #broken-authentication #default-credentials #osint
