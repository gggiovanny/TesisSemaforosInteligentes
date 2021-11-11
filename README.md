# Tesis semáforos inteligentes

## Table of Contents

- [Acerca de](#acerca_de)
- [Instalacion](#instalacion)
- [Uso](#uso)
- [Extensiones recomendadas](#extensiones)

## Acerca de <a name = "acerca_de"></a>
Este documento es mi tesis de licenciatura en Ingeniería en Sistemas Computacionales. Está conformada por sencillos archivos en [markdown](https://www.markdownguide.org/getting-started/) que se traspilan a LaTeX al guardar usando [Pandoc](https://pandoc.org/). Luego esos archivos son leídos por una plantilla que implementa el formato requerido por el Instituto Tecnológico de Mérida (hecha usando como base [esta plantilla](https://www.markdownguide.org/getting-started/)) y finalmente se pueden compilar a un archivo PDF usando cualquier compilador de LaTeX.


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

Continuará...

## Uso <a name = "uso"></a>

Continuará...


## Extensiones recomendadas <a name = "extensiones"></a>

| Nombre                                     |                                Uso                                 |
| ------------------------------------------ | :----------------------------------------------------------------: |
| LaTeX Workshop                             |                  Integración de LaTeX en VS Code                   |
| Markdown All In One                        |                   Soporte y atajos para Markdown                   |
| Action Buttons                             |             Ejecutar comandos desde la barra inferior              |
| Save And Run                               |              Ejecutar comandos al guardar un archivo               |
| LTeX – LanguageTool grammar/spell checking |                Verificación de ortografía y estilo                 |
| vscode-drawio                              |                  Herramienta para hacer diagramas                  |
| change-case                                | Utilidad para cambiar palabras  entre mayúsculas, minúsculas, etc. |