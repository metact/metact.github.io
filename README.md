# Metac — Metodologías Activas

**Metac** es un catálogo web de técnicas de aprendizaje activo, pensado para docentes que quieren explorar, guardar y compartir metodologías de forma rápida y sencilla.

🌐 **[metact.github.io](https://metact.github.io)**

---

## Características

- **+100 técnicas** descritas con ámbitos, tags y recursos enlazados
- **Búsqueda** por nombre, descripción y etiquetas
- **Filtrado jerárquico** por bloque principal y ámbito temático
- **Paginación** configurable: 10, 25, 50 o todas las técnicas
- **Favoritos** con categorías personalizables, persistentes en el navegador
- **Compartir** selecciones y colecciones mediante URL
- **Asistente de IA** (NotebookLM) para consultar dudas sobre las técnicas
- **Trilingüe**: castellano, catalán e inglés
- **Responsive**: funciona en móvil, tablet y escritorio

## Tecnología

Aplicación de página única (`index.html`) sin dependencias externas, sin proceso de compilación y sin framework. Todo el CSS y el JavaScript está integrado en un único archivo.

Los datos se cargan desde Google Sheets publicado como CSV, con fallback a los archivos locales `data/metac - es.csv`, `data/metac - ca.csv` y `data/metac - en.csv`.

Para desarrollar localmente basta con abrir `index.html` en el navegador o servir el directorio:

```bash
python3 -m http.server
```

## Estructura

```
index.html          # Aplicación completa (HTML + CSS + JS)
data/
  metac - es.csv    # Datos en castellano
  metac - ca.csv    # Datos en catalán
  metac - en.csv    # Datos en inglés
notebooklm-icon.webp
```

## Licencias

| Componente | Licencia |
|---|---|
| Contenido de las fichas | [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/) |
| Código fuente | [AGPLv3](https://www.gnu.org/licenses/agpl-3.0.html) |
| Archivos CSV | © Juan José de Haro, todos los derechos reservados |

## Autor

**Juan José de Haro** — [bilateria.org](https://bilateria.org)

¿Tienes una sugerencia o has encontrado un error? [Abre un issue](https://github.com/metact/metact.github.io/issues).
