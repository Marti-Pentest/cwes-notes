# Módulo 05 — Web Fuzzing

## Sección 11/12: API Fuzzing

> [!NOTE]
> El **API fuzzing** aplica los mismos principios del fuzzing tradicional (enviar inputs inesperados o inválidos) pero adaptados a la estructura y protocolos específicos de las Web APIs. El objetivo es disparar errores, crashes o comportamiento inesperado que revele fallas de validación, inyección, o problemas de autenticación/autorización.

## 🛠️ Tipos de modificaciones en API fuzzing

- Alterar valores de parámetros
- Modificar headers del request
- Cambiar el orden de los parámetros
- Introducir tipos de datos o formatos inesperados

## 🎯 Por qué fuzzear APIs

- **Descubrir vulnerabilidades ocultas**: endpoints/parámetros no documentados susceptibles a ataques
- **Testear robustez**: verificar que la API maneje input malformado sin crashear ni exponer datos
- **Automatizar el testing**: probar todas las combinaciones manualmente es inviable
- **Simular ataques reales**: replicar las acciones de un atacante antes de que ocurran de verdad

## 📌 3 tipos principales de API Fuzzing

| Tipo | Foco | Vulnerabilidades que expone |
|---|---|---|
| **Parameter Fuzzing** | Query params, headers, request bodies | Inyección (SQLi, command injection), XSS, parameter tampering |
| **Data Format Fuzzing** | Estructura/contenido/encoding de JSON, XML | Errores de parseo, buffer overflows, mal manejo de caracteres especiales |
| **Sequence Fuzzing** | Orden y timing de requests entre endpoints interconectados | Race conditions, IDOR (Insecure Direct Object References), bypass de autorización |

## 🛠️ Explorando la API objetivo

Muchas APIs autogeneran documentación vía Swagger/OpenAPI, normalmente en `/docs`. Ejemplo de endpoints documentados típicos:

- `GET /` — Read Root
- `GET /items/{item_id}` — Read Item
- `DELETE /items/{item_id}` — Delete Item
- `PUT /items/{item_id}` — Update Item
- `POST /items/` — Create Or Update Item

> [!WARNING]
> Endpoints ocultos ("hidden"): aunque la documentación liste N endpoints, pueden existir otros no documentados intencionalmente — para funciones internas, por "seguridad por oscuridad" (mal enfoque), o simplemente porque están en desarrollo y no listos para exposición pública.

## 🛠️ Fuzzing de endpoints con una herramienta dedicada

```bash
git clone https://github.com/PandaSt0rm/webfuzz_api.git
cd webfuzz_api
pip3 install -r requirements.txt
python3 api_fuzzer.py http://IP:PORT
```

Salida esperada (resumen):

```
Status code counts:
404: 4727
200: 2
405: 1
Found valid endpoints:
- http://localhost:8000/cz...
- http://localhost:8000/docs
Unusual status codes:
405: http://localhost:8000/items
```

> [!TIP]
> Interpretando los códigos:
> - `404` masivo: ruido esperado, endpoints inexistentes
> - `200` en un endpoint que no aparece en la documentación (`/cz...`): endpoint **oculto** descubierto
> - `405 Method Not Allowed` en `/items`: el endpoint existe pero se está usando el método HTTP incorrecto (ej. GET en vez de POST) — indicador de que vale la pena reintentar con otros métodos

Validación del endpoint oculto con curl:

```bash
curl http://localhost:8000/cz...
```

## 🎯 Fuzzing de parámetros dentro de endpoints (más allá del descubrimiento)

Una vez identificados los endpoints, fuzzear sus parámetros puede exponer:

- **Broken Object-Level Authorization**: manipular valores de parámetro para acceder sin permiso a objetos/recursos de otros usuarios
- **Broken Function Level Authorization**: manipular parámetros para invocar funciones que el usuario no debería poder ejecutar
- **SSRF (Server-Side Request Forgery)**: inyectar valores maliciosos que hagan que el servidor realice requests no intencionados a recursos internos/externos

> [!NOTE]
> Profundizar más: estas vulnerabilidades específicas de APIs se cubren en profundidad en el módulo **API Attacks** (pendiente en el roadmap de CWES).

## 🧠 Quiz de repaso

<details>
<summary>¿Por qué un 405 Method Not Allowed es un hallazgo interesante durante el fuzzing de endpoints, y no solo ruido a descartar?</summary>

Un `404` indica que el recurso no existe; un `405`, en cambio, confirma que el endpoint **sí existe** en el servidor, pero el método HTTP usado en el request no es el que ese endpoint acepta. Esto es información valiosa: revela la existencia de un endpoint real (aunque no se haya llegado a interactuar con él correctamente) y sugiere probar otros métodos HTTP (POST, PUT, DELETE) contra esa misma ruta para descubrir su funcionalidad real — en vez de descartarlo como fallo, hay que tratarlo como pista de un endpoint activo.

</details>

<details>
<summary>¿Por qué la seguridad por oscuridad (ocultar un endpoint de la documentación) no es una defensa real?</summary>

Porque ocultar un endpoint de la documentación pública no impide que siga siendo alcanzable en el servidor — solo reduce la probabilidad de que un usuario legítimo lo encuentre por accidente. Un atacante con herramientas de fuzzing puede descubrirlo igual mediante fuerza bruta contra nombres comunes o patrones. La única defensa real es aplicar los controles de autenticación/autorización adecuados sobre el endpoint en sí, independientemente de si está documentado o no.

</details>

> [!NOTE]
> **Ejercicio práctico (lab)** — El lab pide identificar el valor devuelto por el endpoint no documentado que el fuzzer descubre. Metodología: clonar y correr `api_fuzzer.py` contra el target, identificar en la salida el endpoint con `200 OK` que no aparece en `/docs`, y hacer `curl` contra él para leer la respuesta JSON con el valor/flag. No se documenta el valor exacto del lab, solo el enfoque.

## 🔗 Relacionado
- [09 — Web APIs](09-web-apis.md)
- [10 — Identifying Endpoints](10-identifying-endpoints.md)
- [05 — Parameter and Value Fuzzing](05-parameter-value-fuzzing.md)

#cwes #modulo05 #web-fuzzing #api-fuzzing #owasp-api #ssrf #idor
