# Plantillas de ingeniería con Marp

Este repositorio reúne presentaciones, apuntes y recursos sobre temas de ingeniería, escritos principalmente en **Markdown** y preparados para generar diapositivas con [Marp](https://marp.app/). La idea es combinar la sencillez de Markdown con una presentación visual cuidada, inspirada en documentos técnicos y en el estilo de trabajo de LaTeX.

Los materiales pueden servir como punto de partida para clases, exposiciones y documentación técnica. Las plantillas se pueden adaptar, ampliar y reutilizar en nuevos temas.

## Contenido

Los materiales están organizados por carpetas temáticas. Entre los temas disponibles se encuentran:

- **Filtros digitales:** conceptos y diseño de filtros FIR e IIR.
- **Wavelets:** transformadas, análisis de señales y ejemplos de aplicación.
- **TinyML:** aprendizaje automático en sistemas embebidos.
- **Escritura de tesis:** borradores y recursos relacionados con la preparación de documentos académicos.
- **Proyectos ISB:** material asociado a proyectos.

Las carpetas pueden incluir archivos Markdown (`.md`), imágenes, temas CSS, notebooks y exportaciones PDF o HTML. Los recursos específicos de una presentación suelen estar junto a su archivo Markdown para facilitar su edición y portabilidad.

## Requisitos

- [Visual Studio Code](https://code.visualstudio.com/) y la extensión **Marp for VS Code** para editar y previsualizar las presentaciones.
- Opcionalmente, [Node.js](https://nodejs.org/) para exportar desde la línea de comandos con Marp CLI.

## Uso en Visual Studio Code

1. Clona o descarga este repositorio.
2. Abre en Visual Studio Code la carpeta de una presentación.
3. Abre el archivo `.md` y utiliza la vista previa de Markdown/Marp.
4. Si la presentación usa un tema CSS propio, comprueba que esté registrado en la configuración de Marp de esa carpeta. Por ejemplo:

   ```json
   {
     "markdown.marp.themes": [
       "./themes/beam.css",
       "./dsp-theme.css"
     ]
   }
   ```

5. Desde la vista previa de Marp, utiliza las opciones de exportación disponibles para generar el formato que necesites.

La configuración de temas y los nombres de archivo pueden variar entre carpetas. Consulta los archivos `.vscode/settings.json` y el encabezado YAML del Markdown antes de cambiar un tema.

## Exportar desde la terminal

Con Node.js instalado, se puede ejecutar Marp CLI con `npx` para exportar una presentación. Desde la raíz del repositorio, por ejemplo:

```bash
npx @marp-team/marp-cli "1_CLASES_VARIOS/FILTROS/sesion_filtros.md" --pdf
```

Para generar un archivo HTML:

```bash
npx @marp-team/marp-cli "1_CLASES_VARIOS/FILTROS/sesion_filtros.md" --html
```

Para otros formatos y opciones, consulta la [documentación de Marp CLI](https://github.com/marp-team/marp-cli). La exportación puede requerir dependencias adicionales del entorno, especialmente para PDF.

## Estructura de una presentación

Una presentación Marp comienza con metadatos YAML entre delimitadores `---`. Por ejemplo:

```markdown
---
marp: true
theme: beam
paginate: true
size: 16:9
---

# Título de la presentación

## Subtítulo o curso

---

# Segunda diapositiva

- Ideas principales
- Ejemplos y resultados
```

Cada separador `---` crea una nueva diapositiva. Se pueden usar encabezados, listas, tablas, bloques de código, imágenes y expresiones matemáticas para construir material técnico:

```markdown
La transformada discreta de Fourier se define como:

$$
X[k] = \sum_{n=0}^{N-1} x[n] e^{-j 2\pi kn/N}
$$
```

Para añadir una imagen, utiliza una ruta relativa al archivo Markdown:

```markdown
![Diagrama del sistema](images/diagrama.png)
```

Así, la presentación y sus recursos se mantienen juntos y es más sencillo mover o compartir la carpeta.

## Recomendaciones para contribuir

- Mantén cada presentación y sus recursos en la carpeta del tema correspondiente.
- Utiliza rutas relativas para imágenes y otros archivos locales.
- Conserva los metadatos de Marp al inicio del documento y verifica que el tema indicado exista.
- Prefiere nombres de archivo descriptivos y evita duplicar recursos cuando puedan reutilizarse.
- Previsualiza las diapositivas y revisa la exportación final antes de compartirla.
- Indica las fuentes de imágenes, datos y material de terceros cuando corresponda.

## Licencia y atribuciones

Antes de reutilizar o distribuir el contenido, verifica las licencias y atribuciones de los materiales incluidos, en especial imágenes, temas y recursos de terceros. Si se define una licencia para este repositorio, añádela aquí y asegúrate de que sea compatible con esos recursos.
