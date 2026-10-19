# 7. Casos de uso

*Tarea 2 · Administración de Proyectos de Software (1151055) · UAM Azcapotzalco · 26-O*

## 7.1 Herramientas actuales que usan MCP

| Herramienta | Qué es | Cómo usa MCP |
|---|---|---|
| **Claude Desktop / Claude Code** | Aplicaciones de IA de Anthropic | Actúan como *hosts* MCP: crean un cliente por cada servidor. Claude Desktop lanza el servidor de archivos como proceso local por stdio [5] |
| **Visual Studio Code con GitHub Copilot (modo agente)** | Editor de código con asistente de IA | Los servidores MCP se configuran, por ejemplo, en `.vscode/mcp.json`; sus herramientas se usan en modo agente [2][3] |
| **Google Antigravity** | Entorno de desarrollo (IDE) de Google | Se conecta a servidores MCP, por ejemplo servidores remotos de Google Cloud como el de BigQuery [1] |

**Precisión de nombres:** **Qwen** es una *familia de modelos* de lenguaje, no una plataforma ni un entorno de desarrollo; **Google Antigravity** sí es un entorno de desarrollo (IDE).

## 7.2 Cómo editan repositorios completos sin subir archivos a mano

Antes se copiaba y pegaba el código en un chat. Con MCP el flujo cambia:

1. La herramienta (el host) corre en tu computadora, con tu proyecto abierto.
2. Un servidor MCP, como el de sistema de archivos, ofrece al modelo operaciones sobre la carpeta del proyecto: listar, leer, buscar, editar y mover archivos [4][5].
3. El modelo decide qué herramienta usar; el host la ejecuta localmente y le devuelve el resultado, normalmente pidiendo tu aprobación en las acciones que modifican archivos [5][6].
4. Así el modelo trabaja directamente sobre el repositorio real, sin que tengas que subir archivos uno por uno.

La misma idea aplica a servidores que envuelven otros servicios (bases de datos, repositorios remotos, etc.), que el modelo puede combinar con las operaciones de archivos [1].

## 7.3 Mi caso

Elegí **Claude Desktop** en Windows 11, por tres razones. Primero, el servidor se agrega con un archivo JSON de configuración (`claude_desktop_config.json`), algo fácil de documentar y de reproducir en una máquina limpia. Segundo, su interfaz muestra el conector `filesystem`, la lista de sus herramientas y el permiso de cada una, así que se puede comprobar y enseñar en vivo que el cliente reconoce el servidor ([conector](../img/07c-conector-filesystem.png), [herramientas](../img/07b-herramientas-lista.png)). Tercero, antes de ejecutar una herramienta muestra una ventana con las opciones *Denegar*, *Permitir siempre* y *Permitir una vez* ([captura](../img/07d-aprobacion-herramienta.png)), que es la confirmación humana que se recomienda para este tipo de herramientas [6].

## Referencias

[1] Google Cloud. (s. f.). *Use MCP servers*. Google Cloud Documentation. https://docs.cloud.google.com/data-cloud-extension/antigravity/use-mcp-servers

[2] Microsoft. (s. f.-a). *Add and manage MCP servers in VS Code*. Visual Studio Code Docs. https://code.visualstudio.com/docs/copilot/customization/mcp-servers

[3] Microsoft. (s. f.-b). *MCP developer guide*. Visual Studio Code Docs. https://code.visualstudio.com/docs/copilot/guides/mcp-developer-guide

[4] Model Context Protocol. (s. f.). *Filesystem MCP Server* [Repositorio]. GitHub. https://github.com/modelcontextprotocol/servers/tree/main/src/filesystem

[5] Model Context Protocol. (2026). *Architecture overview*. https://modelcontextprotocol.io/docs/2026-07-28/learn/architecture

[6] Willison, S. (2025, 9 de abril). *Model Context Protocol has prompt injection security problems*. Simon Willison's Weblog. https://simonwillison.net/2025/Apr/9/mcp-prompt-injection/

## Declaración de uso de inteligencia artificial
Utilicé Claude (Anthropic) para realizar esta investigación.