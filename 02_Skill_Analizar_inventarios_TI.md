# Skill: Analizar inventarios TI

## Objetivo

Leer un inventario de activos TI en CSV o Excel y detectar campos incompletos, formatos inesperados y posibles duplicados, asociando cada hallazgo con su fila o registro y con la regla que lo generó. El resultado se organiza **por registro**, de modo que el Skill «Generar informes de revisión» pueda consolidar el CSV de registros que requieren corrección.

## Entradas

- Archivo de inventario (CSV o Excel) adjunto o autorizado por la persona usuaria.
- Campos obligatorios, indicados por la persona usuaria.
- Reglas de formato por campo, indicadas por la persona usuaria.
- Campos que se comparan para detectar posibles duplicados (por ejemplo, un identificador), indicados por la persona usuaria.
- En Excel, las hojas a revisar, si se indican.

No se suponen campos, valores válidos, reglas ni campos de comparación que no se hayan indicado.

## Procedimiento

1. Identificar el archivo, su formato y, en Excel, sus hojas.
2. Confirmar campos obligatorios, reglas de formato y campos de comparación de duplicados. Si falta alguno necesario, preguntar o dejar constancia de la limitación como pendiente; no suponerlo.
3. Leer la tabla conservando las columnas y los valores originales tal cual. Si no se puede leer el archivo, una hoja o una columna, avisarlo indicando cuál.
4. Comprobar los criterios de revisión (ver siguiente sección).
5. Registrar cada hallazgo con fila o registro, campo afectado, tipo, regla que lo generó y descripción.
6. Clasificar cada hallazgo como confirmado por comprobación mecánica o como caso que requiere juicio humano.
7. **Agrupar los hallazgos por registro.** Un registro con varios hallazgos aparece una sola vez, con todos sus problemas y todos los campos afectados reunidos.
8. **Para posibles duplicados**, incluir todas las filas implicadas y anotar el motivo de la coincidencia (por ejemplo, que comparte identificador con otra fila, indicando cuál).
9. Verificar que todo registro afectado por campos vacíos, formatos inesperados o posibles duplicados quede incluido en la lista por registro.
10. Entregar la lista por registro al Skill «Generar informes de revisión».

## Criterios de revisión

| Criterio | Qué se comprueba | Clasificación habitual |
|---|---|---|
| Campo incompleto | Un campo indicado como obligatorio está vacío. | Mecánica |
| Formato inesperado | El valor no cumple la regla de formato indicada para ese campo. | Mecánica |
| Posible duplicado | Filas que coinciden en los campos de comparación indicados; se explica la coincidencia. | Juicio humano |

Solo se aplican los criterios para los que la persona usuaria ha dado campos o reglas. Si falta una regla, el criterio correspondiente se marca como **pendiente** en el resultado en lugar de suponerlo.

## Límites

- Trabajar solo con archivos proporcionados o autorizados.
- No modificar ni eliminar el inventario original.
- No inventar, completar, corregir ni reemplazar valores.
- No presentar como incorrecto un dato que solo requiere revisión.
- No aplicar reglas que no se hayan indicado.
- Dejar la validación y las correcciones en manos de una persona.

## Resultado

- Lista de hallazgos con, para cada uno: fila o registro, campo afectado, tipo, regla que lo generó y clasificación.
- **Lista por registro afectado**, con los valores originales intactos, todos los problemas del registro agrupados, los campos que requieren revisión y, en los posibles duplicados, el motivo y las filas relacionadas.
- Avisos de lectura (archivo, hoja o columna no legible) y criterios pendientes por reglas ausentes.
