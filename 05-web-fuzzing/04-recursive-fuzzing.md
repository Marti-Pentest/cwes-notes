# Módulo 05 — Web Fuzzing

## Sección 4/12: Recursive Fuzzing

> [!NOTE]
> El **recursive fuzzing** automatiza el fuzzing de estructuras de directorios anidadas: en vez de fuzzear manualmente cada nivel descubierto, la herramienta detecta un directorio válido y automáticamente lanza un nuevo proceso de fuzzing dentro de ese directorio, repitiendo el ciclo hasta alcanzar un límite de profundidad o quedarse sin más directorios válidos.

## 🎯 Cómo funciona (3 pasos)

1. **Fuzzing inicial**: arranca en el directorio raíz (`/`), envía requests con la wordlist y busca respuestas exitosas (ej. `200 OK`, `301`) que indiquen la existencia de un directorio.
2. **Descubrimiento y expansión**: al encontrar un directorio válido (ej. `admin`), no solo lo registra — crea una nueva rama agregando ese nombre a la URL base (`http://localhost/admin/`) y arranca un nuevo ciclo de fuzzing ahí (`http://localhost/admin/FUZZ`).
3. **Profundidad iterativa**: el proceso se repite para cada directorio descubierto, expandiéndose cada vez más profundo hasta llegar a un límite de profundidad configurado o no encontrar más directorios.

> [!TIP]
> Analogía: es como un árbol: el web root es el tronco, cada directorio descubierto es una rama. El recursive fuzzing explora sistemáticamente cada rama hasta llegar a las hojas (archivos) o a un punto de corte predefinido.

## 🎯 Por qué usarlo

- **Eficiencia**: automatiza el descubrimiento de directorios anidados sin intervención manual
- **Exhaustividad**: explora sistemáticamente cada rama, reduciendo el riesgo de pasar por alto assets ocultos
- **Menos esfuerzo manual**: no hace falta re-lanzar el fuzzer para cada nuevo directorio hallado
- **Escalabilidad**: clave en aplicaciones grandes donde la exploración manual sería impracticable

## 🛠️ Recursive fuzzing con ffuf

```bash
ffuf -w /usr/share/seclists/Discovery/Web-Content/DirBuster-2007_directory-list-2.3-medium.txt -ic -v -u http://IP:PORT/FUZZ -e .html -recursion
```

- `-recursion`: activa el fuzzing recursivo — al hallar un directorio, lanza automáticamente un nuevo job sobre `http://.../directorio_encontrado/FUZZ`
- `-ic` (ignore comments): ignora líneas de la wordlist que empiezan con `#`, evitando tratarlas como inputs válidos
- `-e .html`: extensión a probar en cada nivel

**Flujo del ejemplo:** el fuzzing arranca en la raíz → descubre `level1` (`301`) → se encola un nuevo job sobre `level1/FUZZ` → dentro de `level1` se descubren `level2` y `level3` (nuevas ramas encoladas) además de un `index.html` → el fuzzer procesa la cola y encuentra `index.html` en `level2` y en `level3`. El de `level3` destaca por tener un tamaño de archivo mayor al resto — señal de que vale la pena inspeccionarlo, ya que termina conteniendo la flag del ejercicio de ejemplo.

> [!WARNING]
> **Be Responsible** — El recursive fuzzing puede ser muy demandante en recursos, especialmente en aplicaciones grandes. Un exceso de requests puede sobrecargar el servidor target o disparar mecanismos de seguridad (WAF, rate limiting).

## 🛠️ Flags para controlar el impacto

| Flag | Función |
|---|---|
| `-recursion-depth N` | Limita la profundidad máxima de exploración (ej. `2` = directorio inicial + un nivel de subdirectorios) |
| `-rate N` | Controla la cantidad de requests por segundo |
| `-timeout N` | Timeout por request individual, evita que el fuzzer quede colgado en targets no responsivos |

```bash
ffuf -w /usr/share/seclists/Discovery/Web-Content/DirBuster-2007_directory-list-2.3-medium.txt -ic -u http://IP:PORT/FUZZ -e .html -recursion -recursion-depth 2 -rate 500
```

## 🧠 Quiz de repaso

<details>
<summary>¿Qué diferencia hay entre fuzzear manualmente nivel por nivel y usar -recursion?</summary>

Manualmente, hay que tomar cada directorio descubierto y lanzar un nuevo comando de ffuf apuntando a esa ruta. Con `-recursion`, ffuf automatiza ese ciclo: detecta el directorio válido, encola un nuevo job sobre esa ruta y lo ejecuta sin intervención, repitiendo el proceso en cada nivel hasta el límite de profundidad o agotar directorios válidos.

</details>

<details>
<summary>¿Por qué es importante usar -recursion-depth, -rate y -timeout en un fuzzing recursivo real?</summary>

Porque el recursive fuzzing sin límites puede generar un volumen de requests exponencial a medida que se descubren más directorios, lo cual puede sobrecargar el servidor target, degradar su performance o activar mecanismos de defensa (WAF, rate limiting, bloqueo de IP). Estas flags permiten acotar el alcance (profundidad), la velocidad (rate) y evitar que el fuzzer quede bloqueado esperando respuestas de un target no responsivo (timeout).

</details>

> [!NOTE]
> **Ejercicio práctico (lab)** — El lab pide fuzzear recursivamente el path `recursive_fuzz` para encontrar una flag. Metodología: usar ffuf con `-recursion` (y opcionalmente `-recursion-depth` para acotar) sobre `http://IP:PORT/recursive_fuzz/FUZZ`, dejando que la herramienta descubra y explore automáticamente los subdirectorios anidados hasta dar con el archivo que contiene la flag. No se documenta la flag específica del lab, solo el enfoque.

## 🔗 Relacionado
- [03 — Directory and File Fuzzing](03-directory-file-fuzzing.md)
- [02 — Tooling](02-tooling.md)
- [05 — Parameter and Value Fuzzing](05-parameter-value-fuzzing.md)

#cwes #modulo05 #web-fuzzing #recursive-fuzzing #ffuf
