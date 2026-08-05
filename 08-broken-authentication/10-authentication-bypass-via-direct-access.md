# Módulo 08 — Broken Authentication

## Sección 10/14: Authentication Bypass via Direct Access

> [!NOTE]
> Una variante sutil: la app redirige al login (302) pero no detiene la ejecución del script, filtrando contenido protegido en el body de esa misma respuesta.

## 🛠️ El bug de fondo

```php
if(!$_SESSION['active']) {
    header("Location: index.php");
}
```

> [!WARNING]
> `header("Location: ...")` no detiene la ejecución del script. Sin `exit;`, el resto del código sigue ejecutándose y su output se agrega al body — aunque el status sea 302 y el navegador siga la redirección sin mostrarlo.

## 🛠️ Explotación

1. Activar Intercept en Burp
2. Navegar a `/admin.php`
3. Interceptar la respuesta
4. Cambiar el status code de `302 Found` a `200 OK`
5. Forward

El navegador ya no sigue la redirección y renderiza el contenido protegido que ya estaba en el body.

## 🛠️ Fix correcto

```php
if(!$_SESSION['active']) {
    header("Location: index.php");
    exit;
}
```

## 🧠 Quiz de repaso

<details>
<summary>¿Por qué un 302 con contenido protegido en el body es una vulnerabilidad real si el navegador no lo muestra?</summary>

Porque el comportamiento del navegador es solo una convención de UX del cliente, no una garantía de seguridad. Cualquier herramienta que no siga automáticamente redirects puede acceder directamente a ese contenido.

</details>

<details>
<summary>¿Por qué cambiar el status a 200 es suficiente para que se muestre el contenido?</summary>

Porque el comportamiento del navegador ante una respuesta está determinado en gran parte por el status code. Un 3xx le indica "no proceses este body". Al cambiar a 200, se le indica "esta es la respuesta final, procésala normalmente".

</details>

> [!NOTE]
> **Ejercicio práctico (lab)** — El lab pide aplicar esta técnica para evadir autenticación. Metodología: interceptar la respuesta con Burp y cambiar el código de 302 a 200.

## 🔗 Relacionado
- [09 — Vulnerable Password Reset](09-vulnerable-password-reset.md)
- [11 — Authentication Bypass via Parameter Modification](11-authentication-bypass-via-parameter-modification.md)

#cwes #modulo08 #broken-authentication #authentication-bypass #burp #direct-access
