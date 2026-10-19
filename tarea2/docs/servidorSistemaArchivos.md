# 5. El servidor de sistema de archivos

*Tarea 2 · Administración de Proyectos de Software (1151055) · UAM Azcapotzalco · 26-O*

## 5.1 Qué es (y qué no es)

El servidor de sistema de archivos **no forma parte del protocolo MCP**: es uno de los **servidores de referencia** del proyecto, publicado en el repositorio oficial de servidores y en npm como `@modelcontextprotocol/server-filesystem` [2]. Sirve como ejemplo de cómo se construye un servidor y como herramienta práctica para que un modelo trabaje con archivos locales. En la arquitectura de MCP, cuando el host lo lanza se ejecuta en la misma máquina mediante el transporte stdio [3].

## 5.2 Qué herramientas expone

Según su documentación, ofrece operaciones para leer y escribir archivos, crear y listar directorios, mover o renombrar, buscar y obtener metadatos [2]. Los nombres exactos cambian entre versiones (versiones antiguas usaban `read_file`; las actuales, `read_text_file`) [2].

| Categoría | Herramientas típicas |
|---|---|
| Lectura | `read_text_file`, `read_media_file`, `read_multiple_files` |
| Escritura | `write_file`, `edit_file` |
| Directorios | `create_directory`, `list_directory`, `list_directory_with_sizes`, `directory_tree` |
| Mover / renombrar | `move_file` |
| Búsqueda y metadatos | `search_files`, `get_file_info` |
| Alcance | `list_allowed_directories` |

Esta tabla es una referencia. En mi cliente (Claude Desktop) las herramientas aparecen en dos grupos: **10 de solo lectura** —`Read File` (marcada como *Deprecated*), `Read Text File`, `Read Media File`, `Read Multiple Files`, `List Directory`, `List Directory with Sizes`, `Directory Tree`, `Search Files`, `Get File Info` y `List Allowed Directories`— ([captura](../img/07b-herramientas-lista.png)) y **4 de escritura y eliminación** ([captura](../img/07e-herramientas-escritura.png)). `Write File`, `Edit File`, `Create Directory` y `Move File`.

## 5.3 Cómo los directorios permitidos limitan su alcance

Los directorios permitidos se indican **al iniciar el servidor**, como argumentos de línea de comandos. El servidor solo permite operaciones dentro de esos directorios, y la herramienta `list_allowed_directories` muestra cuáles son [2]. Si arranca sin directorios en los argumentos y el cliente tampoco ofrece *Roots*, el servidor falla al iniciar: necesita al menos un directorio permitido [2]. 

Su documentación describe además que los clientes con soporte de *Roots* pueden reemplazar esa lista de forma dinámica [2]. Ten en cuenta que *Roots* quedó deprecado en la revisión 2026-07-28 de la especificación [4].

Mi configuración permite únicamente la carpeta `C:\mcp-tarea2`.

## 5.4 Qué pasaría sin ese límite

Sin ese límite (o con uno demasiado amplio, como la raíz del disco o toda mi carpeta de usuario), el servidor podría operar con los mismos permisos que mi usuario del sistema (esto es una deducción mía, no una cita): leer, modificar o borrar documentos personales, claves SSH, archivos de configuración o código de otros proyectos. Por eso la consigna pide **nunca** autorizar la raíz del disco ni la carpeta de usuario completa.

El límite también debe estar bien implementado. En 2025 se encontraron en este mismo servidor dos fallos que permitían salirse de la carpeta autorizada: una comparación de rutas por prefijo (CVE-2025-53110) y un problema con enlaces simbólicos (CVE-2025-53109), corregidos en la versión 2025.7.1 [1]. Verifica que tu versión instalada sea igual o posterior a esa.

## 5.5 Respuesta corta para la exposición

> **¿Qué habilita exactamente el servidor de sistema de archivos?** Le da a un modelo, a través de un cliente MCP, un conjunto de herramientas para listar, leer, crear, modificar, mover y buscar archivos, pero únicamente dentro de los directorios que yo autorice al iniciarlo.

## 5.6 Lo que observé en mi demostración

Le pedí buscar los archivos que contuvieran la palabra «MCP» ([captura](../img/12-buscar.png)). Claude explicó que la herramienta de búsqueda del servidor solo busca por **nombre** de archivo, no por contenido, así que leyó los archivos de la carpeta con `Read Multiple Files` y encontró la palabra en `notas.txt` (línea 2) y en `prueba.md` (línea 4). Es un buen ejemplo de cómo se reparten los papeles: el servidor ofrece operaciones simples y el modelo decide cómo combinarlas para resolver la tarea.

## Referencias

[1] Cymulate Research Labs. (2025). *EscapeRoute: Breaking the scope of Anthropic's Filesystem MCP Server (CVE-2025-53109 & CVE-2025-53110)*. Cymulate. https://cymulate.com/blog/cve-2025-53109-53110-escaperoute-anthropic/

[2] Model Context Protocol. (s. f.). *Filesystem MCP Server* [Repositorio]. GitHub. https://github.com/modelcontextprotocol/servers/tree/main/src/filesystem

[3] Model Context Protocol. (2026a). *Architecture overview*. https://modelcontextprotocol.io/docs/2026-07-28/learn/architecture

[4] Model Context Protocol. (2026b). *Key changes* (Specification 2026-07-28, changelog). GitHub. https://github.com/modelcontextprotocol/modelcontextprotocol/blob/main/docs/specification/2026-07-28/changelog.mdx

## Declaración de uso de inteligencia artificial
Utilicé Claude (Anthropic) para realizar esta investigación.