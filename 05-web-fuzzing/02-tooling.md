# Módulo 05 — Web Fuzzing

## Sección 2/12: Tooling

> [!NOTE]
> Cuatro herramientas cubren el grueso del fuzzing web: **ffuf** y **wfuzz/wenum** (fuzzing general, muy configurable), **Gobuster** (rápido y simple, bueno para content discovery + subdominios) y **FeroxBuster** (recursivo, escrito en Rust, más "forced browsing" que fuzzer puro). Todas requieren Go y/o Python + pipx como base.

## 🛠️ Instalación base (Go, Python, pipx)

```bash
# Actualizar repos
sudo apt update

# Go
sudo apt install -y golang

# Python
sudo apt install -y python3 python3-pip

# pipx (gestor de apps Python en entornos aislados)
sudo apt install pipx
pipx ensurepath
sudo pipx ensurepath --global

# Verificar instalación
go version
python3 --version
```

> [!TIP]
> ¿Por qué pipx y no pip? `pipx` crea un entorno virtual aislado por cada aplicación Python instalada, evitando conflictos de dependencias entre herramientas. Es ideal para instalar CLI tools como `wenum` sin ensuciar el entorno global de Python.

## 🛠️ FFUF (Fuzz Faster U Fool)

Fuzzer rápido escrito en Go. Ideal para enumeración de directorios/archivos y parámetros.

```bash
go install github.com/ffuf/ffuf/v2@latest
```

| Caso de uso | Descripción |
|---|---|
| Directory & File Enumeration | Identificar rápido directorios y archivos ocultos |
| Parameter Discovery | Encontrar y testear parámetros de la app |
| Brute-Force Attack | Descubrir credenciales u otra info sensible |

## 🛠️ Gobuster

Fuzzer de directorios/archivos conocido por su velocidad y simplicidad.

```bash
go install github.com/OJ/gobuster/v3@latest
```

| Caso de uso | Descripción |
|---|---|
| Content Discovery | Escanear directorios, archivos, virtual hosts |
| DNS Subdomain Enumeration | Identificar subdominios de un dominio target |
| WordPress Content Detection | Wordlists específicas para contenido WordPress |

## 🛠️ FeroxBuster

Tool de content discovery recursivo, escrito en Rust. Más orientado a "forced browsing" que a fuzzing puro.

```bash
curl -sL https://raw.githubusercontent.com/epi052/feroxbuster/main/install-nix.sh | sudo bash -s $HOME/.local/bin
```

| Caso de uso | Descripción |
|---|---|
| Recursive Scanning | Escaneos recursivos para descubrir directorios anidados |
| Unlinked Content Discovery | Contenido no enlazado dentro de la app |
| High-Performance Scans | Rendimiento de Rust para descubrimiento a alta velocidad |

## 🛠️ wfuzz / wenum

`wenum` es el fork mantenido activamente de `wfuzz`. Muy versátil para **parameter fuzzing**. La sintaxis es intercambiable entre ambos.

> [!WARNING]
> `wfuzz` puede venir preinstalado en distros como PwnBox o Kali, pero actualmente tiene complicaciones de instalación. Se recomienda sustituirlo por `wenum`, que sigue la misma sintaxis — los comandos son intercambiables.

```bash
pipx install git+https://github.com/WebFuzzForge/wenum
pipx runpip wenum install setuptools
```

| Caso de uso | Descripción |
|---|---|
| Directory and File Enumeration | Identificar directorios y archivos ocultos |
| Parameter Discovery | Encontrar y testear parámetros |
| Brute-Force Attack | Descubrir credenciales u otra info sensible |

## 🔗 Relacionado
- [01 — Introduction](01-introduction.md)
- [03 — Directory and File Fuzzing](03-directory-file-fuzzing.md)

#cwes #modulo05 #web-fuzzing #tooling #ffuf #gobuster #feroxbuster #wfuzz
