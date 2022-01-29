# Tesis semáforos inteligentes

## Tabla de contenido

- [Acerca de](#acerca_de)
- [Instalacion](#instalacion)
- [Uso](#uso)
- [Extensiones recomendadas](#extensiones)

## Acerca de <a name = "acerca_de"></a>
Este documento es mi tesis de licenciatura en Ingeniería en Sistemas Computacionales. Está conformada por sencillos archivos en [markdown](https://www.markdownguide.org/getting-started/) que se traspilan a [LaTeX](https://www.latex-project.org/) al guardar usando [Pandoc](https://pandoc.org/). Luego esos archivos son leídos por una plantilla que implementa el formato requerido por el Instituto Tecnológico de Mérida (hecha usando como base [esta plantilla](https://www.overleaf.com/latex/templates/tesis-de-licenciatura-instituto-tecnologico-de-morelia/cxgbymjjxrvy)) y finalmente se pueden compilar a un archivo PDF usando cualquier compilador de LaTeX.


## Instalación <a name = "instalacion"></a>

Estas instrucciones resultarán en una copia del proyecto listo para trabajar y capaz de compilarse en un archivo PDF.

### Prerrequisitos
Se puede ejecutar el proyecto tanto en Windows como en Linux, pero la mayoría de los detalles y comandos funcionarán únicamente para Linux, ya que esta documentación fue escrita durante una instalación sobre Debian usando [WSL](https://docs.microsoft.com/en-us/windows/wsl/).

1. Instalar [Visual Studio Code](https://code.visualstudio.com/).

2. Instalar un compilador de LaTeX. Se recomienda TeX Live. [Descarga para windows](https://www.tug.org/texlive/windows.html). 
En Ubuntu/Debian: `sudo apt install texlive-full`. Nota: la instalación de todos los paquetes puede tomar mucho tiempo.

3. Instalar [pandoc](https://pandoc.org/installing.html).

### Configuración del proyecto

1. Clonar este proyecto y abrirlo en Visual Studio Code.

```
git clone https://github.com/gggiovanny/TesisSemaforosInteligentes
cd TesisSemaforosInteligentes
code .  
```

2. Aceptar instalar las extensiones recomendadas para el entorno de trabajo cuando el editor lo pregunte. Si por alguna razón no se aceptaron, se pueden encontrar en la [tabla adjunta](#extensiones) o el ID de los paquetes en el archivo `.vscode/extensions.json`, y se pueden instalar manualmente buscándolas en el menú de extensiones de VS Code.

## Uso <a name = "uso"></a>

1. Abrir `ITMtesis.tex` para que se inicialice la extensión LaTeX Workshop.
2. Abrir la extensión LaTeX Workshop (que se encuentra en la barra lateral de VS Code) y dar clic en _Build LaTeX project_. Esto usará la receta por defecto. Yo he tenido buenos resultados con latexmk (la primera opción).
3. Un indicador se verá en la barra inferior, y si no ocurre ningún error de compilación, ésta se actualizará a una ✅. Si esto ocurre, ¡todo está listo para trabajar!
4. Para ver una versión en PDF del proyecto, basta con dar clic en _View LaTeX PDF_ en la pestaña de la extensión LaTeX Workshop.
5. El flujo de trabajo es el siguiente: editar los capítulos (cap_X.md) usando Markdown. Estos automáticamente se traspilan a LaTeX al guardar con la ayuda de la extensión Save And Run. (para más detalles, analizar el contenido de `.vscode/settings.json`).
6. El proceso anterior desencadenará en la extensión LaTeX Workshop compile nuevamente el proyecto, y el resultado se puede visualizar en el PDF generado.

# Solucionar comunes <a name = "errores"></a>
Para ver los errores de compilación, bastará con dar clic en el botón "Open compiler log" en la notificación que la extensión LaTeX Workshop muestra. En su defecto, también se puede abrir el archivo `build/ITMtesis.log` y buscar la palabra _error_ dentro.

- Al cambiar entre sistemas operativos, pueden ocurrir errores de compilación por inconsistencias en el formato de las rutas. Actualmente el proyecto está configurado para Linux, pero si se cambia de entorno, basta con cambiar la ruta en `graphicspath` en el inicio de todos los archivos Markdown.

## Extensiones recomendadas <a name = "extensiones"></a>

| Nombre                                      |                                Uso                                |
| ------------------------------------------- | :---------------------------------------------------------------: |
| LaTeX Workshop                              |                  Integración de LaTeX en VS Code                  |
| Markdown All In One                         |                  Soporte y atajos para Markdown                   |
| Action Buttons                              |             Ejecutar comandos desde la barra inferior             |
| Save And Run                                |              Ejecutar comandos al guardar un archivo              |
| LaTeX – LanguageTool grammar/spell checking |                Verificación de ortografía y estilo                |
| vscode-drawio                               |                 Herramienta para hacer diagramas                  |
| change-case                                 | Utilidad para cambiar palabras entre mayúsculas, minúsculas, etc. |