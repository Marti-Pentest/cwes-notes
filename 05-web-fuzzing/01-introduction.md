# Módulo 05 — Web Fuzzing

## Sección 1/12: Introduction

> [!NOTE]
> El **web fuzzing** es una técnica de testing automatizado que envía inputs inesperados, malformados o aleatorios a una aplicación web para detectar vulnerabilidades que el testing manual podría pasar por alto. Es una pieza clave del arsenal de cualquier pentester porque automatiza la búsqueda de puntos débiles simulando ataques reales.

> [!TIP]
> **Fuzzing vs Brute-forcing** — Aunque se usan como sinónimos, no son lo mismo:
> - **Fuzzing**: red amplia, prueba datos malformados, caracteres inválidos, combinaciones sin sentido para ver cómo reacciona la app ante lo inesperado.
> - **Brute-forcing**: enfoque dirigido, prueba sistemáticamente muchas posibilidades para un valor específico (password, ID) usando diccionarios predefinidos.
>
> Analogía: fuzzing es tirarle de todo a una puerta cerrada (llaves, destornilladores, un pato de goma) para ver si algo la abre. Brute-forcing es probar cada combinación de un llavero hasta encontrar la correcta.

## 🎯 Por qué hacer fuzzing en apps web

| Beneficio | Detalle |
|---|---|
| Descubre vulnerabilidades ocultas | Detecta comportamientos inesperados que el testing tradicional no cubre |
| Automatiza el testing de seguridad | Ahorra tiempo/recursos; el equipo se enfoca en analizar resultados |
| Simula ataques reales | Replica técnicas de atacantes antes de que exploten algo real |
| Refuerza la validación de inputs | Clave para prevenir SQLi y XSS |
| Mejora la calidad del código | El feedback del fuzzing ayuda a escribir código más robusto |
| Seguridad continua | Se integra en pipelines CI/CD para testing regular |

## 📌 Conceptos esenciales

| Concepto | Descripción | Ejemplo |
|---|---|---|
| Wordlist | Diccionario/lista de palabras, nombres de archivo, directorios o valores de parámetros usados como input | Genérico: `admin`, `login`, `backup`, `config` · Específico de la app: `productID`, `addToCart` |
| Payload | El dato real enviado a la app durante el fuzzing | `' OR 1=1 --` (SQLi) |
| Response Analysis | Examinar las respuestas de la app (códigos, mensajes de error) para detectar anomalías | Normal: `200 OK` · Posible SQLi: `500 Internal Server Error` con mensaje de DB |
| Fuzzer | Herramienta que automatiza el envío de payloads y el análisis de respuestas | `ffuf`, `wfuzz`, Burp Suite Intruder |
| False Positive | Resultado marcado como vulnerabilidad que en realidad no lo es | `404 Not Found` para un directorio inexistente |
| False Negative | Vulnerabilidad real que el fuzzer no detecta | Un fallo de lógica sutil en un flujo de pago |
| Fuzzing Scope | Las partes específicas de la app que se están targeteando | Solo la página de login o un endpoint de API puntual |

## 🔗 Relacionado
- [02 — Tooling](02-tooling.md)
- [05 — Parameter and Value Fuzzing](05-parameter-value-fuzzing.md)

#cwes #modulo05 #web-fuzzing #fundamentos
