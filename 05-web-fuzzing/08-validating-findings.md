# Módulo 05 — Web Fuzzing

## Sección 8/12: Validating Findings

> [!NOTE]
> El fuzzing genera muchos leads, pero no todo hallazgo es una vulnerabilidad real — hay **falsos positivos** (anomalías inofensivas que disparan la detección del fuzzer sin representar una amenaza real). La **validación** es el paso que confirma si un hallazgo es genuino, evalúa su impacto y permite reproducirlo de forma consistente para documentarlo o mitigarlo.

## 🎯 Por qué validar

- **Confirmar vulnerabilidades**: distinguir issues reales de falsas alarmas
- **Entender el impacto**: evaluar severidad y consecuencias potenciales
- **Reproducir el issue**: replicar consistentemente para desarrollar una fix
- **Reunir evidencia**: prueba concreta para compartir con developers o stakeholders

## 🛠️ Verificación manual

1. **Reproducir el request**: repetir manualmente (con `curl` o el navegador) el request que disparó la respuesta anómala durante el fuzzing
2. **Analizar la respuesta**: buscar mensajes de error, contenido inesperado o comportamiento fuera de lo normal
3. **Explotación controlada**: si el hallazgo es prometedor, intentar explotarlo en un entorno controlado y con autorización adecuada para evaluar impacto/severidad

> [!WARNING]
> **Responsabilidad ante todo** — El objetivo es reunir evidencia suficiente para convencer a los stakeholders de que la vulnerabilidad existe, sin dañar el sistema productivo ni comprometer datos sensibles. En vez de extraer o modificar datos reales, se apunta a un **PoC (proof of concept)** inofensivo — por ejemplo, ante sospecha de SQLi, extraer solo el string de versión del servidor SQL en vez de intentar volcar datos de usuarios.

## 🎯 Caso de ejemplo: directorio /backup/ con 200 OK

Un directorio de backup accesible es sensible por naturaleza, ya que puede contener:

- **Database dumps**: bases de datos completas, credenciales, info personal
- **Archivos de configuración**: API keys, claves de cifrado, settings sensibles
- **Código fuente**: revela vulnerabilidades o detalles de implementación

### Confirmar el directory listing

```bash
curl http://IP:PORT/backup/
```

Si el servidor devuelve un listado HTML de archivos (`Index of /backup/`), queda confirmado el directory listing.

### Validar sin exponer contenido sensible: analizar headers

> [!TIP]
> Enfoque responsable: en vez de descargar/leer el contenido del archivo sospechoso, se examinan los **headers de la respuesta** — suficiente para confirmar el riesgo sin arriesgar exposición de datos sensibles.

```bash
curl -I http://IP:PORT/backup/password.txt
```

```
HTTP/1.1 200 OK
Content-Type: text/plain;charset=utf-8
Content-Length: 171
...
```

- **`Content-Type`**: indica el tipo de archivo (ej. `application/sql` para un dump de DB, `application/zip` para un backup comprimido)
- **`Content-Length`**: si es mayor a 0, el archivo tiene contenido real. Un archivo de `Content-Length: 0` puede ser sospechoso por su sola presencia, pero no representa un riesgo directo al estar vacío

Con el nombre del archivo (`password.txt`), su presencia en un directorio de backup y un `Content-Length` mayor a cero, se reúne evidencia sólida del riesgo sin necesidad de abrir el archivo.

## 🧠 Quiz de repaso

<details>
<summary>¿Por qué preferir analizar headers (curl -I) en vez de descargar el archivo completo al validar un hallazgo sensible?</summary>

Porque el objetivo de la validación es confirmar la existencia y el riesgo de la vulnerabilidad, no explotarla al máximo. Descargar y leer un archivo como `password.txt` implicaría acceder a datos potencialmente sensibles de terceros (credenciales reales, por ejemplo), lo cual puede ser innecesario, éticamente cuestionable, y en muchos contextos (bug bounty, pentest con reglas de engagement) directamente una violación de los términos del compromiso. Los headers (`Content-Type`, `Content-Length`) ya aportan evidencia suficiente — tipo de archivo y que tiene contenido real — sin necesidad de leer el dato en sí.

</details>

<details>
<summary>Si un fuzzer marca un directorio /backup/ con 200 OK pero un archivo dentro tiene Content-Length: 0, ¿significa que no hay riesgo?</summary>

No necesariamente elimina el riesgo, pero reduce su severidad inmediata. Un archivo vacío no puede filtrar datos porque no tiene contenido, pero su sola presencia (el nombre del archivo, la estructura del directorio) puede seguir siendo información valiosa — por ejemplo, revela que existe un proceso de backup, convenciones de nombres de archivo, o la posibilidad de que en otro momento (backups rotativos, por ejemplo) sí haya contenido real accesible. La validación debe documentar esto como una observación de menor severidad, no descartarla por completo.

</details>

> [!NOTE]
> **Ejercicio práctico (lab)** — El lab pide fuzzear con `DirBuster-2007_directory-list-2.3-medium.txt` para hallar un directorio oculto, y luego validar responsablemente un archivo `.tar.gz` dentro de ese directorio analizando su header `Content-Length`. Metodología: `ffuf`/`gobuster` con la wordlist indicada para dar con el directorio → `curl -I` sobre el archivo `.tar.gz` hallado para leer el header sin descargar el contenido. No se documenta el valor exacto de `Content-Length` del lab, solo el enfoque.

## 🔗 Relacionado
- [07 — Filtering Fuzzing Output](07-filtering-fuzzing-output.md)
- [03 — Directory and File Fuzzing](03-directory-file-fuzzing.md)
- [05 — Parameter and Value Fuzzing](05-parameter-value-fuzzing.md)

#cwes #modulo05 #web-fuzzing #validation #curl #false-positives #responsible-disclosure
