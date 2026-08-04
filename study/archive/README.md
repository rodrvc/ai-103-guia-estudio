# Archivo

Contenido rotado fuera de la memoria activa. **No se lee salvo petición explícita.**

| Patrón | Origen | Cuándo se archiva |
| --- | --- | --- |
| `SESSION-LOG-<AAAA-MM>.md` | AI-103-SESSION-LOG.md | Sesiones más allá de las 5 últimas |
| `ERROR-LOG-cerrados-<AAAA>Q<n>.md` | AI-103-ERROR-LOG.md | Cuando § Errores cerrados supera 40 filas |

Regla general: ningún archivo de `study/` supera 200 líneas. Al superarlo, compactar o archivar aquí.
