# Módulo 05 — Web Fuzzing

## Sección 12/12: Skills Assessment

> [!NOTE]
> Assessment práctico final del módulo. No introduce teoría nueva — evalúa la aplicación combinada de todo lo cubierto: directory/file fuzzing, recursive fuzzing, parameter fuzzing (GET/POST), vhost/subdomain fuzzing, filtrado de output, validación responsable de hallazgos, e identificación/fuzzing de endpoints de API.

> [!TIP]
> Wordlist a usar: todo el assessment puede resolverse con `common.txt` de SecLists, en `/usr/share/seclists/Discovery/Web-Content/common.txt` (Pwnbox) o desde el repo de GitHub de SecLists.

> [!NOTE]
> **Metodología general aplicada (sin flag):**
> 1. Reconocimiento inicial del target (directory/file fuzzing con `ffuf`/`gobuster`/`feroxbuster`)
> 2. Si aparece estructura anidada → recursive fuzzing con límite de profundidad razonable
> 3. Identificar parámetros GET/POST expuestos y fuzzear sus valores
> 4. Revisar si hay vhosts o subdominios relevantes
> 5. Filtrar el output agresivamente (`-fc`, `-mc`, `--hc`, etc.) para no perder la señal en el ruido
> 6. Validar cualquier hallazgo sospechoso de forma responsable (headers antes que contenido)
> 7. Si hay componente de API, identificar endpoints (documentados y ocultos) y fuzzear parámetros
>
> No se documenta la flag específica del assessment, respetando los términos de HTB Academy.

## 🔗 Relacionado
- [03 — Directory and File Fuzzing](03-directory-file-fuzzing.md)
- [04 — Recursive Fuzzing](04-recursive-fuzzing.md)
- [05 — Parameter and Value Fuzzing](05-parameter-value-fuzzing.md)
- [06 — Virtual Host and Subdomain Fuzzing](06-vhost-subdomain-fuzzing.md)
- [07 — Filtering Fuzzing Output](07-filtering-fuzzing-output.md)
- [08 — Validating Findings](08-validating-findings.md)
- [11 — API Fuzzing](11-api-fuzzing.md)

#cwes #modulo05 #web-fuzzing #skills-assessment
