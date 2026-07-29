# Módulo 06 — JavaScript Deobfuscation

## Sección 3/11: Code Obfuscation

> [!NOTE]
> La **ofuscación** es una técnica que hace que un script sea mucho más difícil de leer para un humano, manteniendo su funcionalidad técnica intacta (aunque puede volverse más lento). Se logra típicamente con herramientas automáticas que reescriben el código de forma ilegible según su propio diseño.

## 🛠️ Cómo funciona típicamente

Un patrón común: el obfuscador convierte el código en un **diccionario** de todas las palabras/símbolos usados, y luego reconstruye el código original en tiempo de ejecución referenciando ese diccionario (de ahí el patrón típico `eval(function(p,a,c,k,e,d){...}(...,'palabra1|palabra2|...'.split('|'),...))`).

> [!TIP]
> Herramienta de ejemplo: [beautifytools.com/javascript-obfuscator.php](http://beautifytools.com/javascript-obfuscator.php) permite ver en vivo cómo un código JS simple se transforma al ofuscarlo.

## 📌 Por qué JavaScript es especialmente propenso a esto

> [!WARNING]
> Lenguajes interpretados: server-side vs client-side. Python y PHP también son interpretados, pero suelen correr **server-side** — el código nunca llega al usuario final. JavaScript, en cambio, se ejecuta típicamente en el **browser (client-side)**, lo que significa que el código se envía al usuario **en texto plano**, visible para cualquiera. Esta es la razón principal por la que la ofuscación es tan común específicamente en JavaScript.

## 🎯 Casos de uso de la ofuscación

- **Proteger propiedad intelectual**: dificultar la reutilización o copia del código sin permiso
- **Capa de seguridad adicional** en autenticación/cifrado — aunque con matiz importante (ver abajo)
- **Uso malicioso** (el más común en contextos de seguridad): atacantes ofuscan sus scripts maliciosos para evadir la detección de sistemas IDS/IPS

> [!WARNING]
> Autenticación/cifrado client-side no es recomendable. Aunque la ofuscación se use como "capa de seguridad" para lógica de autenticación o cifrado, hacer estas operaciones en el client-side sigue siendo una mala práctica — el código, ofuscado o no, es más vulnerable a ataques al estar expuesto y ejecutarse en un entorno que el atacante controla completamente.

## 🧠 Quiz de repaso

<details>
<summary>¿Por qué la ofuscación es mucho más frecuente en JavaScript que en Python o PHP, si los tres son lenguajes interpretados?</summary>

Porque lo que determina la exposición del código no es si el lenguaje es interpretado o compilado, sino **dónde se ejecuta**. Python y PHP suelen ejecutarse server-side, por lo que el código fuente nunca sale del servidor. JavaScript, en cambio, se ejecuta client-side en el navegador del usuario, lo que obliga a enviar el código fuente completo en texto plano a cualquiera que visite la página. La ofuscación es, en ese contexto, casi la única herramienta disponible para dificultar la lectura de un código que de por sí no se puede ocultar del todo.

</details>

<details>
<summary>¿Por qué usar ofuscación como "capa de seguridad" para autenticación client-side sigue siendo una mala práctica?</summary>

Porque la ofuscación dificulta la lectura humana del código, pero no impide que el código se ejecute exactamente igual — y por lo tanto, un atacante con suficiente tiempo y las herramientas adecuadas puede deofuscarlo y entender su lógica completa. Además, cualquier secreto que viva en el client-side está en un entorno que el atacante controla totalmente, algo que no ocurre con lógica equivalente alojada server-side. La ofuscación agrega fricción, no seguridad real.

</details>

## 🔗 Relacionado
- [02 — Source Code](02-source-code.md)
- [04 — Basic Obfuscation](04-basic-obfuscation.md)

#cwes #modulo06 #javascript-deobfuscation #obfuscation #fundamentos
