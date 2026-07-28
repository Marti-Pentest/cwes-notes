# Módulo 05 — Web Fuzzing

## Sección 9/12: Web APIs

> [!NOTE]
> Una **Web API** es un conjunto de reglas que permite que distintas aplicaciones se comuniquen entre sí sobre la web, actuando como puente entre un servidor (datos/funcionalidad) y un cliente (browser, app móvil, otro servidor). Entender las diferencias entre los estilos de API (REST, SOAP, GraphQL) y entre una API y un web server tradicional es clave para adaptar el enfoque de fuzzing.

## 📌 Tipos de Web APIs

### REST (Representational State Transfer)

Estilo arquitectónico stateless cliente-servidor. Usa métodos HTTP estándar (`GET`, `POST`, `PUT`, `DELETE`) para operaciones CRUD sobre recursos identificados por URLs únicas. Intercambia datos típicamente en JSON o XML.

```http
GET /users/123
```

### SOAP (Simple Object Access Protocol)

Protocolo más formal y estandarizado. Define mensajes en XML encapsulados en "envelopes" SOAP, transmitidos sobre HTTP o SMTP. Incluye funciones nativas de seguridad, confiabilidad y manejo transaccional — apto para aplicaciones enterprise que requieren integridad de datos estricta.

```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:tem="http://tempuri.org/">
   <soapenv:Header/>
   <soapenv:Body>
      <tem:GetStockPrice>
         <tem:StockName>AAPL</tem:StockName>
      </tem:GetStockPrice>
   </soapenv:Body>
</soapenv:Envelope>
```

### GraphQL

A diferencia de REST (múltiples endpoints por recurso), GraphQL expone **un único endpoint** donde el cliente pide exactamente los datos que necesita mediante un lenguaje de query flexible. Elimina el over-fetching/under-fetching típico de REST. Su tipado fuerte e introspección facilitan evolucionar la API sin romper clientes existentes.

```graphql
query {
  user(id: 123) {
    name
    email
  }
}
```

## 🎯 Ventajas de las Web APIs

- Exponen funcionalidades específicas a usuarios externos u otras apps → reutilización de código y "mashups"
- Facilitan integrar servicios de terceros (login social, pagos, mapas) sin reinventar la rueda
- Son la base de las arquitecturas de **microservicios** — apps monolíticas divididas en servicios independientes que se comunican vía APIs bien definidas, mejorando escalabilidad y resiliencia

## 📊 Web Server vs API

| Característica | Web Server | API |
|---|---|---|
| Propósito | Servir contenido estático/dinámico (HTML, CSS, páginas generadas server-side) | Permitir que apps se comuniquen entre sí, intercambien datos y disparen acciones |
| Comunicación | HTTP con browsers | HTTP, HTTPS, SOAP u otros protocolos según la API |
| Formato de datos | HTML, CSS, JS | JSON, XML u otros según especificación |
| Interacción del usuario | Directa, vía browser | Indirecta — la app usa la API en nombre del usuario |
| Acceso | Generalmente público | Público, privado (uso interno) o de partner (acceso restringido) |
| Ejemplo | Visitar `https://www.example.com` y recibir HTML/CSS/JS renderizado | Una app de clima consultando una API de clima detrás de escena, sin que el usuario interactúe directamente con la API |

> [!TIP]
> Implicancia para el fuzzing: al fuzzear una API, el foco cambia — en vez de buscar directorios/archivos ocultos como en un web server tradicional, el objetivo pasa a ser **endpoints de la API y sus parámetros**, prestando especial atención al formato de datos usado en requests y responses (JSON, XML, GraphQL queries).

## 🧠 Quiz de repaso

<details>
<summary>¿Por qué GraphQL "elimina" el problema de over-fetching/under-fetching que sí existe en REST?</summary>

En REST, cada endpoint devuelve una estructura de datos fija definida por el servidor — si el cliente necesita solo el nombre de un usuario, igual puede recibir todo el objeto usuario completo (over-fetching), o si necesita datos de varios recursos relacionados, puede necesitar múltiples requests a distintos endpoints (under-fetching, resuelto con más llamadas). GraphQL invierte el control: el cliente especifica exactamente qué campos quiere en una sola query contra un único endpoint, y el servidor devuelve solo esos campos — eliminando la necesidad de sobre-pedir o hacer múltiples round-trips.

</details>

<details>
<summary>¿Por qué el enfoque de fuzzing cambia al pasar de un web server tradicional a una API?</summary>

Porque la superficie de ataque es distinta: un web server tradicional expone contenido navegable (páginas, directorios, archivos estáticos), por lo que el fuzzing busca rutas/archivos ocultos. Una API, en cambio, no tiene "páginas" para navegar — su superficie son los endpoints (definidos por la especificación de la API) y los parámetros/campos que aceptan en el body o query, en formatos estructurados como JSON, XML o queries GraphQL. Por eso el fuzzing de APIs se enfoca en descubrir endpoints documentados/no documentados y en fuzzear los parámetros dentro de esos formatos estructurados, en vez de nombres de archivo o directorio genéricos.

</details>

## 🔗 Relacionado
- [05 — Parameter and Value Fuzzing](05-parameter-value-fuzzing.md)
- [10 — Identifying Endpoints](10-identifying-endpoints.md)
- [11 — API Fuzzing](11-api-fuzzing.md)

#cwes #modulo05 #web-fuzzing #web-apis #rest #soap #graphql
