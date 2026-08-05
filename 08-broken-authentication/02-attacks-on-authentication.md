# Módulo 08 — Broken Authentication

## Sección 2/14: Attacks on Authentication

> [!NOTE]
> Cada categoría de autenticación (knowledge, ownership, inherence) tiene un perfil de riesgo distinto. El módulo se enfoca en knowledge-based por ser la más común y atacable.

## 📌 Atacando Knowledge-based Authentication

> [!WARNING]
> Sufre de depender de información estática que puede obtenerse, adivinarse o forzarse por fuerza bruta.

## 📌 Atacando Ownership-based Authentication

> [!TIP]
> Resistente a phishing y password-guessing, ya que depende de un objeto físico.

> [!WARNING]
> Vectores específicos: ataques físicos (robo/clonación, ej. clonar una badge NFC en un espacio público) y ataques criptográficos contra el algoritmo del token.

## 📌 Atacando Inherence-based Authentication

> [!TIP]
> Conveniente — no requiere recordar passwords ni cargar tokens.

> [!WARNING]
> Riesgo crítico: compromiso irreversible. A diferencia de knowledge/ownership, los datos biométricos no se pueden cambiar una vez filtrados.

> [!NOTE]
> Caso real: la brecha de BioStar 2 (2019) expuso huellas dactilares y patrones faciales de usuarios de smart locks — sin posibilidad de "cambiar" ese dato comprometido.

## 🧠 Quiz de repaso

<details>
<summary>¿Por qué el módulo se enfoca principalmente en knowledge-based authentication?</summary>

Porque es el método más extendido en aplicaciones web y estructuralmente el más atacable — depende de información que puede filtrarse, adivinarse, o forzarse mediante técnicas automatizadas.

</details>

<details>
<summary>¿Por qué el compromiso de datos biométricos se describe como "irreversible"?</summary>

Porque un dato biométrico es una característica física fija — no es un secreto que se pueda regenerar como una contraseña nueva. Una vez expuesto, permanece comprometido de forma permanente para esa persona.

</details>

## 🔗 Relacionado
- [01 — What is Authentication](01-what-is-authentication.md)

#cwes #modulo08 #broken-authentication #knowledge-based #ownership-based #inherence-based
