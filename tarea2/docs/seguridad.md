# 6. Seguridad


*Tarea 2 · Administración de Proyectos de Software (1151055) · UAM Azcapotzalco · 26-O*

## 6.1 Riesgos

**Instrucciones escondidas en el contenido de un archivo.** El modelo no separa bien las órdenes de la persona de los datos que lee. Un archivo puede contener texto pensado para manipularlo ("ignora lo anterior y haz X"); esto se conoce como **inyección indirecta de instrucciones** [2][6]. En MCP existe además una variante: instrucciones ocultas en la **descripción de una herramienta** (*tool poisoning*), que el modelo lee pero la persona normalmente no ve [7].

**Acceso fuera del directorio autorizado.** Si el servidor valida mal las rutas, el modelo podría llegar a archivos que no se le autorizaron. Esto ocurrió en el propio servidor de sistema de archivos con una comparación por prefijo (CVE-2025-53110) y un fallo con enlaces simbólicos (CVE-2025-53109) [1].

**Escritura o borrado no deseados.** Las herramientas de escritura pueden ejecutarse por un error del modelo o inducidas por una inyección, y modificar o eliminar archivos importantes [5][7].

## 6.2 Mitigaciones

| Mitigación | En qué consiste | Fuente |
|---|---|---|
| **Confirmación humana** | Que una persona pueda negar cada llamada a herramienta, sobre todo las que modifican archivos | [7] |
| **Alcance limitado** | Autorizar solo una carpeta creada para la tarea, nunca la raíz del disco ni la carpeta de usuario | [5] |
| **Solo lectura** | Cuando no hace falta escribir, montar el directorio en modo solo lectura (por ejemplo con Docker y la opción `ro`) | [5] |
| **Revisar lo que expone el servidor** | Tratar las definiciones de herramientas como código, fijar versiones y usar solo servidores confiables | [7] |
| **Mantener actualizado** | Usar una versión del servidor que incluya las correcciones de seguridad conocidas | [1] |

Dos advertencias de la documentación de VS Code ilustran por qué esto importa: los servidores MCP locales pueden ejecutar código arbitrario en la máquina [3], y VS Code no pide confirmación para las herramientas marcadas como de solo lectura [4]. Por eso conviene revisar también qué marca un servidor como "solo lectura".

## 6.3 Lo que hice en esta tarea

- **Carpeta dedicada.** El servidor solo tiene autorizada `C:\mcp-tarea2`, creada para esta tarea. No autoricé la raíz del disco ni mi carpeta de usuario.
- **Confirmación humana.** Claude Desktop agrupa las herramientas del servidor y marca las 10 de solo lectura como «Requiere aprobación» ([captura](../img/07b-herramientas-lista.png)); las otras 4, de escritura y eliminación, aparecen en un grupo aparte ([captura](../img/07e-herramientas-escritura.png)). Al usar una herramienta, el cliente mostró una ventana con *Denegar*, *Permitir siempre* y *Permitir una vez* ([captura](../img/07d-aprobacion-herramienta.png), [otra](../img/09-leer-archivo.png)).
- **Versión del servidor.** 1.0.0 ; debe ser igual o posterior a la 2025.7.1, que corrigió las vulnerabilidades de salida del directorio [1].
- **Prueba del límite** ([captura](../img/13-limite.png)). Le pedí leer `C:\Windows\System32\drivers\etc\hosts`, fuera de la carpeta autorizada. La llamada a la herramienta apareció como «Fallido» y Claude respondió que el servidor solo tiene acceso a `C:\mcp-tarea2` y sus subcarpetas, por lo que rechazó la ruta. Lo que lo impidió fue la restricción de directorios del propio servidor [5], no un criterio del modelo: la herramienta se invocó y falló.

## Referencias

[1] Cymulate Research Labs. (2025). *EscapeRoute: Breaking the scope of Anthropic's Filesystem MCP Server (CVE-2025-53109 & CVE-2025-53110)*. Cymulate. https://cymulate.com/blog/cve-2025-53109-53110-escaperoute-anthropic/

[2] Greshake, K., Abdelnabi, S., Mishra, S., Endres, C., Holz, T., & Fritz, M. (2023). *Not what you've signed up for: Compromising real-world LLM-integrated applications with indirect prompt injection* (arXiv:2302.12173). arXiv. https://arxiv.org/abs/2302.12173

[3] Microsoft. (s. f.-a). *Add and manage MCP servers in VS Code*. Visual Studio Code Docs. https://code.visualstudio.com/docs/copilot/customization/mcp-servers

[4] Microsoft. (s. f.-b). *MCP developer guide*. Visual Studio Code Docs. https://code.visualstudio.com/docs/copilot/guides/mcp-developer-guide

[5] Model Context Protocol. (s. f.). *Filesystem MCP Server* [Repositorio]. GitHub. https://github.com/modelcontextprotocol/servers/tree/main/src/filesystem

[6] OWASP Foundation. (2025). *LLM01:2025 Prompt injection*. OWASP Gen AI Security Project. https://genai.owasp.org/llmrisk/llm01-prompt-injection/

[7] Willison, S. (2025, 9 de abril). *Model Context Protocol has prompt injection security problems*. Simon Willison's Weblog. https://simonwillison.net/2025/Apr/9/mcp-prompt-injection/

## Declaración de uso de inteligencia artificial
Utilicé Claude (Anthropic) para realizar esta investigación.