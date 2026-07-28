# Módulo 05 — Web Fuzzing

## Sección 6/12: Virtual Host and Subdomain Fuzzing

> [!NOTE]
> **Vhosting** permite servir múltiples sitios/dominios desde un mismo servidor/IP, identificados por el header `Host` de cada request. **Subdominios** son extensiones jerárquicas de un dominio principal, resueltas vía DNS a IPs específicas. Ambos amplían la superficie de ataque más allá de lo que se ve en la URL principal, y ambos son fuzzeables con Gobuster.

## 📊 Vhosts vs Subdominios

| Característica | Virtual Hosts | Subdominios |
|---|---|---|
| Identificación | Header `Host` en el request HTTP | Registros DNS que apuntan a IPs específicas |
| Propósito | Alojar múltiples sitios en un solo servidor | Organizar secciones/servicios dentro de un sitio |
| Riesgo de seguridad | Vhosts mal configurados pueden exponer apps internas o data sensible | Subdomain takeover si los registros DNS quedan mal gestionados |

## 🛠️ Gobuster — capacidades generales

Gobuster no se limita a directorios/archivos: también hace fuzzing de subdominios y vhosts (manipulando el header `Host`).

## 🛠️ Vhost Fuzzing con Gobuster

Primero, agregar el vhost objetivo al `/etc/hosts` local:

```bash
echo "IP inlanefreight.htb" | sudo tee -a /etc/hosts
```

Comando de fuzzing:

```bash
gobuster vhost -u http://inlanefreight.htb:81 -w /usr/share/seclists/Discovery/Web-Content/common.txt --append-domain
```

- `vhost`: modo de fuzzing de virtual hosts
- `-u`: URL base del servidor target
- `-w`: wordlist con nombres candidatos de vhost
- `--append-domain`: **clave** — agrega el dominio base a cada palabra de la wordlist, generando el `Host` header completo (ej. `admin.inlanefreight.htb`) en vez de solo la palabra suelta

> [!TIP]
> Cómo funciona internamente: Gobuster toma cada palabra de la wordlist, le agrega el dominio base, y envía un request HTTP al target con ese `Host` header modificado. Analiza las respuestas (status code, tamaño) para identificar vhosts válidos que podrían no estar documentados públicamente.

Salida esperada:

```
Found: admin.inlanefreight.htb:81 Status: 200 [Size: 100]
```

> [!WARNING]
> **Interpretando resultados** — No todos los `Found` son igual de relevantes. Un `200 OK` indica un vhost válido y accesible — ahí es donde hay que enfocar la atención, filtrando el ruido de otros códigos (400, etc.) que pueden aparecer por comportamiento por defecto del servidor.

## 🛠️ Subdomain Fuzzing con Gobuster

```bash
gobuster dns -d inlanefreight.com -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt
```

- `dns`: modo de fuzzing DNS/subdominios
- `-d`: dominio target
- `-w`: wordlist de nombres de subdominio candidatos (ej. top 5000 más comunes)

Internamente, Gobuster genera nombres de subdominio combinando la wordlist con el dominio target, e intenta resolverlos vía DNS. Si resuelve a una IP, se considera válido.

```
Found: www.inlanefreight.com
Found: blog.inlanefreight.com
```

> [!WARNING]
> **Cambio de sintaxis en versiones recientes** — En versiones más nuevas de Gobuster, `-d` pasó a controlar el **delay** entre requests, no el dominio. Para especificar el dominio target hay que usar `--do` o `--domain`. Verificar siempre la versión instalada antes de asumir la sintaxis.

## 🧠 Quiz de repaso

<details>
<summary>¿Por qué es imprescindible el flag --append-domain en el vhost fuzzing y qué pasaría sin él?</summary>

Porque la validación del vhost depende del `Host` header completo (ej. `admin.inlanefreight.htb`), no solo de la palabra suelta de la wordlist. Sin `--append-domain`, Gobuster enviaría el header `Host: admin` en vez de `Host: admin.inlanefreight.htb`, lo cual no coincidiría con ningún vhost configurado en el servidor — el fuzzing fallaría en detectar vhosts reales aunque estén presentes.

</details>

<details>
<summary>¿Cuál es la diferencia práctica entre fuzzear vhosts y fuzzear subdominios con Gobuster?</summary>

El vhost fuzzing manipula el header `Host` en requests HTTP contra una única IP/servidor conocido, buscando qué nombres de host ese servidor reconoce y sirve contenido distinto para. El subdomain fuzzing, en cambio, hace consultas DNS reales contra el sistema de nombres de dominio para ver qué subdominios *resuelven* a una IP — no depende de un servidor HTTP específico, sino de la configuración DNS del dominio, por lo que puede revelar infraestructura (IPs) que ni siquiera está corriendo un servidor web accesible directamente.

</details>

> [!NOTE]
> **Ejercicio práctico (lab)** — El lab pide: 1) fuzzear vhosts contra el target con `common.txt` para hallar uno con prefijo `web-`, y 2) fuzzear subdominios de `inlanefreight.com` con `subdomains-top1million-5000.txt` para hallar uno con prefijo `su`. Metodología: `gobuster vhost -u ... -w common.txt --append-domain` filtrando por `Status: 200`, y `gobuster dns -d inlanefreight.com -w subdomains-top1million-5000.txt` (o `--do`/`--domain` según versión) revisando los `Found:`. No se documentan los valores exactos hallados, solo el enfoque.

## 🔗 Relacionado
- [05 — Parameter and Value Fuzzing](05-parameter-value-fuzzing.md)
- [02 — Tooling](02-tooling.md)
- [07 — Filtering Fuzzing Output](07-filtering-fuzzing-output.md)

#cwes #modulo05 #web-fuzzing #vhost #subdomain #gobuster #dns
