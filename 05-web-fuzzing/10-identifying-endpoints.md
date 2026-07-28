# Módulo 05 — Web Fuzzing

## Sección 10/12: Identifying Endpoints

> [!NOTE]
> Antes de fuzzear una API hay que saber dónde buscar. Cada estilo de API (REST, SOAP, GraphQL) estructura sus endpoints y parámetros de forma distinta, por lo que la estrategia de descubrimiento cambia según el tipo. El punto en común entre los tres: **documentación oficial**, **análisis de tráfico de red** y **fuzzing de nombres de parámetros/operaciones** son las tres vías principales de descubrimiento.

## 📌 REST

Los endpoints son URLs jerárquicas que representan recursos:

```
/users          → colección de usuarios
/users/123      → usuario específico con ID 123
/products       → colección de productos
/products/456   → producto específico con ID 456
```

### Tipos de parámetros

| Tipo | Descripción | Ejemplo |
|---|---|---|
| Query Parameters | Después de `?` en la URL. Filtrado, orden, paginación | `/users?limit=10&sort=name` |
| Path Parameters | Embebidos en la URL. Identifican un recurso específico | `/products/{id}` |
| Request Body Parameters | En el body de `POST`/`PUT`/`PATCH`. Crear/actualizar recursos | `{ "name": "New Product", "price": 99.99 }` |

### Cómo descubrir endpoints/parámetros REST

1. **Documentación de la API**: la vía más confiable. Buscar specs como **Swagger (OpenAPI)** o **RAML**
2. **Análisis de tráfico de red**: Burp Suite o devtools del browser para interceptar requests/responses reales
3. **Parameter Name Fuzzing**: igual que fuzzear directorios/archivos, pero apuntando a nombres de parámetros con `ffuf`/`wfuzz` — útil cuando la documentación es escasa o inexistente

## 📌 SOAP

A diferencia de REST, SOAP típicamente expone **un único endpoint** — la operación específica se define dentro del cuerpo XML del mensaje, no en la URL.

> [!TIP]
> WSDL como mapa de la API: el archivo **WSDL** (Web Services Description Language) describe: operaciones disponibles, parámetros de entrada/salida de cada una, tipos de datos usados, y la URL del endpoint SOAP. Es el recurso más valioso para entender una API SOAP.

Ejemplo de request con parámetros `keywords` y `author` (dejando `genre` fuera, es decir sin filtrar por género):

```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:lib="http://example.com/library">
   <soapenv:Header/>
   <soapenv:Body>
      <lib:SearchBooks>
         <lib:keywords>cybersecurity</lib:keywords>
         <lib:author>Dan Kaminsky</lib:author>
      </lib:SearchBooks>
   </soapenv:Body>
</soapenv:Envelope>
```

### Cómo descubrir endpoints/parámetros SOAP

1. **Análisis del WSDL** (manual o con herramientas de parseo/visualización)
2. **Análisis de tráfico de red**: Wireshark/tcpdump para capturar e inspeccionar mensajes SOAP
3. **Fuzzing de nombres/valores de parámetros**: aunque SOAP tiene estructura bien definida, el fuzzing sigue siendo útil para descubrir operaciones/parámetros no documentados

## 📌 GraphQL

Expone típicamente **un único endpoint** (ej. `/graphql`) para todas las queries y mutations.

### Queries (lectura de datos)

| Componente | Descripción | Ejemplo |
|---|---|---|
| Field | Dato específico a obtener | `name`, `email` |
| Relationship | Conexión entre distintos tipos de datos | `posts` |
| Nested Object | Campo que devuelve otro objeto, permite navegar más profundo | `posts { title, body }` |
| Argument | Modifica el comportamiento (filtrado, paginación) | `posts(limit: 5)` |

```graphql
query {
  user(id: 123) {
    name
    email
    posts(limit: 5) {
      title
      body
    }
  }
}
```

### Mutations (modificación de datos)

| Componente | Descripción | Ejemplo |
|---|---|---|
| Operation | Acción a realizar | `createPost`, `updateUser`, `deleteComment` |
| Argument | Datos de entrada requeridos | `title: "New Post", body: "..."` |
| Selection | Campos a devolver en la respuesta tras la mutation | `id`, `title` |

```graphql
mutation {
  createPost(title: "New Post", body: "This is the content of the new post") {
    id
    title
  }
}
```

### Cómo descubrir queries/mutations en GraphQL

> [!NOTE]
> Introspection: la herramienta más poderosa. El sistema de **introspección** de GraphQL permite enviar una query especial al endpoint que devuelve el **schema completo** de la API: tipos, campos, queries, mutations y argumentos disponibles. Herramientas como **GraphiQL** o **GraphQL Playground** aprovechan esto para ofrecer auto-completado y documentación interactiva.

1. **Introspection**: query especial que revela el schema completo
2. **Documentación**: guías junto a la introspección, explicando el propósito de cada query/mutation
3. **Análisis de tráfico**: capturar requests/responses reales contra el endpoint `/graphql`

> [!WARNING]
> GraphQL es flexible por diseño: no siempre hay un set rígido de queries/mutations documentado. El foco debe estar en entender el **schema subyacente** y cómo el cliente puede construir requests válidos, más que en memorizar una lista fija de operaciones.

## 🧠 Quiz de repaso

<details>
<summary>¿Por qué SOAP y GraphQL comparten la característica de "un único endpoint", pero se descubren de formas tan distintas?</summary>

Ambos concentran toda su funcionalidad en una sola URL, delegando la especificidad de la operación al contenido del mensaje (XML en SOAP, query language en GraphQL) en vez de a la estructura de la URL como en REST. Sin embargo, la forma de descubrir qué operaciones existen difiere radicalmente: SOAP depende de un archivo de definición externo y estático (el WSDL) que hay que analizar aparte, mientras que GraphQL tiene un mecanismo de autodescubrimiento integrado en el propio protocolo (introspección) — se le puede preguntar directamente al servidor qué schema soporta, sin necesidad de un documento externo.

</details>

<details>
<summary>¿Por qué la introspección de GraphQL representa tanto una ventaja para el desarrollo como un riesgo de seguridad?</summary>

Es una ventaja porque permite que herramientas y clientes descubran automáticamente el schema completo (tipos, queries, mutations, argumentos), facilitando el desarrollo y la documentación autogenerada. Pero ese mismo mecanismo, si queda habilitado en producción sin restricciones, le da a un atacante exactamente el mismo mapa completo de la API — incluyendo mutations o campos que el equipo de desarrollo quizás no pretendía exponer públicamente, sin necesidad de fuzzear nada a ciegas.

</details>

## 🔗 Relacionado
- [09 — Web APIs](09-web-apis.md)
- [11 — API Fuzzing](11-api-fuzzing.md)
- [05 — Parameter and Value Fuzzing](05-parameter-value-fuzzing.md)

#cwes #modulo05 #web-fuzzing #rest #soap #graphql #wsdl #introspection
