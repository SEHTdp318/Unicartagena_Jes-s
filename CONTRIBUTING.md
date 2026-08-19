# Cómo contribuir

¡Gracias por tu interés en mejorar esta bóveda académica!

## Formas de contribuir

### 1. Reportar errores / sugerencias
- Abre un **Issue** en GitHub con:
  - Título claro: [Materia] Descripción breve
  - Descripción del problema/mejora
  - Archivos afectados (si aplica)

### 2. Proponer cambios (Pull Request)
1. Haz **fork** del repo
2. Crea una rama: git checkout -b feat/nombre-mejora
3. Haz tus cambios (ver guía de estilo abajo)
4. Commit: git commit -m "feat(materia): descripción clara"
4. Push: git push origin feat/nombre-mejora
5. Abre **Pull Request** → describe qué y por qué

### 3. Añadir contenido a materias
- Cada materia tiene su carpeta en materias/ (o raíz)
- Sigue la estructura:
  `
  materia/
  ├── README.md          # Resumen + estado
  ├── cronograma.md      # Fechas, temas, entregas
  ├── apuntes/           # .md por tema
  ├── recursos.md        # Enlaces, biblio, videos
  └── evidencias/        # PDFs, capturas (opcional)
  `

## Guía de estilo

### Archivos Markdown
- **UTF-8 sin BOM** (obligatorio)
- **Nombres de archivo**: kebab-case.md (ej: cronograma.md, 	ema-1-variables.md)
- **Encabezados**: # H1, ## H2, ### H3 (jerarquía)
- **Tablas**: usa pipes | alineadas
- **Checklists**: - [ ] Pendiente, - [x] Hecho
- **Enlaces**: [texto](url) — evita URLs crudas

### Commits (Conventional Commits)
`
tipo(ámbito): descripción breve

Tipos:
  feat     # nueva funcionalidad/contenido
  fix      # corrección
  docs     # solo documentación
  style    # formato, sin cambio lógico
  refactor # reestructuración
  chore    # mantenimiento (gitignore, deps, etc.)

Ejemplos:
  feat(algoritmos): añadir apuntes de recursividad
  fix(readme): corregir encoding en tabla de horarios
  docs(python): añadir guía de virtualenv
`

### Estructura de carpetas (respetar)
- Nombres: kebab-case (sin acentos, sin espacios)
- Profundidad máx: 3 niveles
- Un .md por tema/concepto atómico

## Revisión
- Todos los PRs requieren **1 aprobación** antes de merge
- CI debe pasar (markdownlint, links rotos)
- No merges directos a main — solo via PR

## Código de conducta
- Sé respetuoso y constructivo
- Enfoque en el contenido, no en la persona
- Español como idioma principal

---

¿Dudas? Abre un Issue con label question.
