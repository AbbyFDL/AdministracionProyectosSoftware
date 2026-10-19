# 2. El problema del aislamiento

*Tarea 2 · Administración de Proyectos de Software (1151055) · UAM Azcapotzalco · 26-O*

## 2.1 Un LLM recibe texto y devuelve texto

Un modelo de lenguaje es, en esencia, una función: recibe una secuencia de tokens y devuelve la continuación más probable [3]. No tiene, por sí mismo, ninguna operación para "abrir un archivo" o "guardar un cambio". Anthropic planteó el problema señalando que incluso los modelos más avanzados están limitados por su aislamiento de los datos, atrapados detrás de silos de información [1].

## 2.2 Razones de arquitectura

- **Corre en un servidor remoto.** El modelo se ejecuta en la infraestructura del proveedor; tu computadora solo le envía mensajes y recibe respuestas.
- **No hay un canal hacia tu disco.** Ese servidor no tiene ninguna conexión con tus carpetas. Lo único que el modelo "ve" es el texto que se le manda en la conversación.
- **Alguien tiene que hacer el trabajo por él.** Para que el modelo actúe sobre archivos, una aplicación intermedia (el *host*) debe leer los archivos, incluir su contenido en el texto, y ejecutar localmente las acciones que el modelo solicite [4].

## 2.3 Razones de seguridad

Aunque se pudiera abrir un canal, hacerlo sin control sería peligroso:

- **Aislamiento.** El modelo no debería tener acceso por defecto a todo lo que tu usuario puede tocar: documentos personales, claves, configuraciones.
- **Consentimiento.** Las acciones sobre tus archivos deben ser aprobadas por una persona. La especificación de MCP recomienda que siempre haya un humano con la posibilidad de negar la ejecución de una herramienta [6].
- **Inyección de instrucciones.** El modelo no distingue de forma fiable entre las órdenes de quien lo usa y el texto de los datos que procesa. Si un documento contiene frases como "ignora lo anterior y haz X", el modelo puede obedecerlas. A esto se le llama **inyección indirecta de instrucciones** [2][5].

## 2.4 Resumen

| Tipo de razón | Pregunta que responde | Ejemplo |
|---|---|---|
| Arquitectura | ¿Puede técnicamente? | El modelo está en un servidor remoto sin conexión a tu disco [1][4] |
| Seguridad: aislamiento | ¿Debería poder tocar todo? | Acceso limitado a una carpeta |
| Seguridad: consentimiento | ¿Quién autoriza cada acción? | Confirmación humana antes de escribir o borrar [6] |
| Seguridad: inyección | ¿Puede un archivo darle órdenes al modelo? | Texto escondido en un documento que el modelo lee [2][5] |

## 2.5 Cómo se resuelve

La solución no es dar acceso total al modelo, sino ponerle un intermediario: el host expone al modelo un conjunto acotado de herramientas, y es el host quien las ejecuta, con permisos y, normalmente, con confirmación del usuario [4][6]. Ese es el papel que cumple MCP, que se explica en los puntos 3 y 4.

## Referencias

[1] Anthropic. (2024, 25 de noviembre). *Introducing the Model Context Protocol*. https://www.anthropic.com/news/model-context-protocol

[2] Greshake, K., Abdelnabi, S., Mishra, S., Endres, C., Holz, T., & Fritz, M. (2023). *Not what you've signed up for: Compromising real-world LLM-integrated applications with indirect prompt injection* (arXiv:2302.12173). arXiv. https://arxiv.org/abs/2302.12173

[3] Jurafsky, D., & Martin, J. H. (2025). *Speech and language processing* (3.ª ed., borrador). Stanford University. https://web.stanford.edu/~jurafsky/slp3/

[4] Model Context Protocol. (2026). *Architecture overview*. https://modelcontextprotocol.io/docs/2026-07-28/learn/architecture

[5] OWASP Foundation. (2025). *LLM01:2025 Prompt injection*. OWASP Gen AI Security Project. https://genai.owasp.org/llmrisk/llm01-prompt-injection/

[6] Willison, S. (2025, 9 de abril). *Model Context Protocol has prompt injection security problems*. Simon Willison's Weblog. https://simonwillison.net/2025/Apr/9/mcp-prompt-injection/

## Declaración de uso de inteligencia artificial
Utilicé Claude (Anthropic) para realizar esta investigación.