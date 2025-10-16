<!-- Copilot / AI agent instructions for the "Paper" repository -->
### Objetivo breve
Ayuda a editar y mantener el manuscrito LaTeX de la investigación: archivo principal `main.tex`, bibliografía en `refs.bib` y figuras en `figures/`. Prioriza correcciones tipográficas, actualizaciones de referencias, ajuste de figuras/tablas y cambios estructurales en el texto. Evita alterar contenido científico sin confirmación del autor.

### Comportamiento esperado
- Haz cambios mínimos y explícitos: ofrece diffs claros y comenta la intención en el commit/message.  
- Cuando propongas reescrituras largas del texto (p. ej. secciones completas), sugiere una versión y pide confirmación antes de reemplazar.
- No introduzcas referencias nuevas en `refs.bib` sin verificar formato BibTeX y campos obligatorios (author, title, year, journaltitle/url cuando aplique).

### Cómo compilar (comandos útiles)
El proyecto usa `biblatex` con backend `biber` (ver `\usepackage[backend=biber,...]{biblatex}` en `main.tex`). Comandos típicos para compilar desde la raíz del proyecto:

```bash
latexmk -pdf main.tex    # preferible si está disponible
pdflatex main.tex
biber main
pdflatex main.tex
pdflatex main.tex
```

Si `latexmk` no está disponible, usa la secuencia pdflatex -> biber -> pdflatex x2 mostrada arriba.

### Estructura y convenciones del repositorio
- `main.tex`: archivo principal; contiene el preámbulo con paquetes (utf8, fontenc, babel en inglés), bibliografía via `\addbibresource{refs.bib}` y figuras referenciadas desde `figures/`.
- `refs.bib`: base BibTeX usada por `biber`.
- `figures/`: imágenes nombradas como `Figure 1.jpg`, ..., `Figure 6.jpg`; tablas como `Table N.xlsx` (fuentes de datos). Mantén referencias a las figuras con rutas relativas `figures/<name>`.
- Evita renombrar las figuras salvo que actualices todas las referencias en `main.tex`.

### Patrones y ejemplos detectados
- Bibliografía: `biblatex` + `biber`. Evita cambios a `\usepackage[backend=biber,...]{biblatex}` salvo para corregir opciones.
- Figuras: insertadas con `\includegraphics[width=\textwidth]{figures/Figure 1.jpg}` (ejemplo en `main.tex`), formatos JPG son aceptados; si aportas SVG/PNG, conserva la extensión y actualiza la referencia.
- Tablas: datos fuente en `figures/Table N.xlsx`. Si creas tablas LaTeX, sigue estilo `booktabs` (ya usado) e intenta mantener líneas de separación mínimas.

### Ediciones frecuentes y cómo realizarlas correctamente
- Añadir/actualizar una referencia: modifica `refs.bib` con entradas completas; luego recompila usando `biber main` y pdflatex. Comprueba que la clave no duplique otra existente.
- Actualizar una figura: añade el archivo en `figures/`, referencia con el nombre exacto en `main.tex`, y verifica tamaño/posición (usa `[width=\textwidth]` o `\centering`).
- Cambios en el preámbulo (paquetes): restringidos — reporta al autor si se requiere añadir paquetes grandes (TikZ/pgfplots ya presentes).

### Qué evitar
- No cambies el idioma global (`\usepackage[english]{babel}`) ni la codificación sin confirmación.  
- No elimines archivos auxiliares generados (`.aux`, `.bbl`, `.blg`, `.fls`) en commits; estos pueden ser ignorados por .gitignore, pero no los recrees manualmente.
- No infieras resultados experimentales ni modifiques cifras/valores numéricos en tablas/figuras sin una fuente clara.

### Indicaciones para commits y mensajes
- Usa mensajes claros y enfocados, por ejemplo: "Fix figure path and caption for Figure 3" o "Add BibTeX entry for Isensee2021 and update citation".
- Si el cambio es sugerido pero no seguro, crea un PR (o un patch) y marca en el cuerpo qué necesita confirmación del autor.

### Archivos clave a revisar para contexto
- `main.tex` — estructura del documento y comandos de compilación.
- `refs.bib` — formato y entradas bibliográficas.
- `figures/` — imágenes y datos tabulares.

### Preguntas que debes hacer al autor antes de cambios grandes
1. ¿Aceptar reescrituras largas de secciones (sí/no)?
2. ¿Quieres que migrar las tablas desde XLSX a tablas LaTeX embebidas?
3. ¿Prefieres usar `latexmk` en CI o un script Makefile/CI específico?

Si algo no es claro, crea un issue enumerando la duda y proponiendo 1–2 alternativas.

---
Si quieres que refine esto con comandos de CI (GitHub Actions) para compilación PDF automática o reglas de formato, indícalo y lo añado.
