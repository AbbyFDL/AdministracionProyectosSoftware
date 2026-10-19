# 3. MCP frente a una API

*Tarea 2 · Administración de Proyectos de Software (1151055) · UAM Azcapotzalco · 26-O*

## 3.1 Qué es una API

Una **API** es un contrato entre programas: define qué operaciones ofrece un sistema y cómo se piden [9]. Quien programa decide de antemano qué operación se llama y en qué momento [2]. Por ejemplo, el código de una aplicación puede estar escrito para llamar siempre a "obtener usuario" y luego a "guardar pedido", en ese orden.

## 3.2 Qué es MCP

El **Model Context Protocol (MCP)** es un protocolo abierto para conectar aplicaciones de IA con herramientas y datos externos [1]. Sus mensajes usan **JSON-RPC 2.0** [4][8]. Un servidor MCP **publica un catálogo de herramientas**; el cliente lo consulta al conectarse, el modelo lo descubre en ese momento y **decide cuál usar** según lo que le pida el usuario [7][8].

Ejemplo simplificado de los dos mensajes principales (la revisión 2026-07-28 añade metadatos como la versión del protocolo en `_meta`) [8]:

```json
{ "jsonrpc": "2.0", "id": 1, "method": "tools/list" }
```

```json
{ "jsonrpc": "2.0", "id": 2, "method": "tools/call",
  "params": { "name": "list_directory", "arguments": { "path": "C:\\mcp-tarea2" } } }
```

## 3.3 Tabla comparativa

| Criterio | API tradicional | MCP |
|---|---|---|
| **Quién decide qué se invoca** | El programador, en el código, de antemano [2] | El modelo, en tiempo de ejecución, entre las herramientas publicadas [7] |
| **Cómo se descubren las capacidades** | Documentación o esquema (por ejemplo OpenAPI) que lee una persona o un generador de código [9] | El cliente pide el catálogo al servidor (`tools/list`) y lo descubre al conectar [8] |
| **Acoplamiento** | Alto: cada aplicación se integra a mano con cada API | Bajo: cualquier cliente compatible usa cualquier servidor compatible [1] |
| **Formato de los mensajes** | Depende de cada API (REST con JSON, GraphQL, SOAP…) [2] | JSON-RPC 2.0 [4][8] |
| **Autenticación y consentimiento** | Lo define cada API (claves, OAuth…); el consentimiento lo resuelve la aplicación | La especificación define autorización para el transporte HTTP y recomienda un humano que pueda negar las llamadas [8][10]. Con stdio, la especificación indica no seguir ese flujo de autorización y tomar las credenciales del entorno del proceso [6] |
| **Reutilización entre aplicaciones** | Baja: la integración se reescribe en cada aplicación | Alta: un mismo servidor sirve a muchos clientes [1][7] |


## 3.4 MCP no sustituye a las APIs

MCP **no reemplaza** a las APIs: casi siempre **envuelve una que ya existe**. Por ejemplo, Google Cloud ofrece servidores MCP para servicios como BigQuery, que ya cuentan con su propia API [3]. El servidor de sistema de archivos, por su parte, envuelve las operaciones del sistema de archivos local [5]. Lo que MCP añade es una capa uniforme para que un modelo descubra y use esas capacidades sin que alguien programe cada integración a mano.


## Referencias

[1] Anthropic. (2024, 25 de noviembre). *Introducing the Model Context Protocol*. https://www.anthropic.com/news/model-context-protocol

[2] Fielding, R. T. (2000). *Architectural styles and the design of network-based software architectures* [Tesis doctoral, University of California, Irvine]. https://ics.uci.edu/~fielding/pubs/dissertation/top.htm

[3] Google Cloud. (s. f.). *Use MCP servers*. Google Cloud Documentation. https://docs.cloud.google.com/data-cloud-extension/antigravity/use-mcp-servers

[4] JSON-RPC Working Group. (2013). *JSON-RPC 2.0 specification*. https://www.jsonrpc.org/specification

[5] Model Context Protocol. (s. f.). *Filesystem MCP Server* [Repositorio]. GitHub. https://github.com/modelcontextprotocol/servers/tree/main/src/filesystem

[6] Model Context Protocol. (2025). *Authorization* (Specification 2025-06-18). https://modelcontextprotocol.io/specification/2025-06-18/basic/authorization

[7] Model Context Protocol. (2026a). *Architecture overview*. https://modelcontextprotocol.io/docs/2026-07-28/learn/architecture

[8] Model Context Protocol. (2026b, 28 de julio). *Specification* (Versión 2026-07-28). https://modelcontextprotocol.io/specification/2026-07-28

[9] OpenAPI Initiative. (2024). *OpenAPI specification v3.1.1*. https://spec.openapis.org/oas/v3.1.1.html

[10] Willison, S. (2025, 9 de abril). *Model Context Protocol has prompt injection security problems*. Simon Willison's Weblog. https://simonwillison.net/2025/Apr/9/mcp-prompt-injection/

## Declaración de uso de inteligencia artificial
Utilicé Claude (Anthropic) para realizar esta investigación.