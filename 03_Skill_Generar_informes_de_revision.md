# Skill: Generar informes de revisión

## Objetivo

Organizar los hallazgos del análisis en un informe claro y trazable para una persona responsable, y generar un **CSV separado de registros que requieren corrección**. Ambos se guardan aparte del inventario original cuando la interfaz lo permite.

## Entradas

- Hallazgos y lista por registro del Skill «Analizar inventarios TI»: fila o registro, campo, regla, clasificación y, en duplicados, motivo.
- Hoja y pestaña leídas (o archivo adjunto) y método de acceso usado.
- Avisos de lectura y criterios pendientes por reglas ausentes.
- Formato y ubicación de guardado, si la persona usuaria los indica y la interfaz los permite.
- El inventario original (hoja de Google Sheets o archivo), solo para conservar sus columnas y valores en el CSV.

## Estructura del informe

1. **Datos de la revisión:** hoja o archivo revisado, pestaña leída, método de acceso (conector de Google Drive o archivo adjunto), campos obligatorios, reglas de formato y campos de comparación de duplicados aplicados.
2. **Limitaciones y avisos:** hoja, pestaña, archivo o columna no accesibles o no legibles; reglas pendientes.
3. **Hallazgos confirmados por comprobación mecánica:** fila o registro, campo, regla y descripción.
4. **Casos que requieren juicio humano**, incluidos los posibles duplicados, con las filas implicadas y el motivo de la coincidencia.
5. **Salidas generadas:** nombre del CSV de registros por corregir, número de registros incluidos y estado de entrega de cada archivo (ver «Estado de entrega»).
6. **Nota de control:** el inventario original no se ha modificado; la validación y las correcciones corresponden a una persona.

## Especificación del CSV de registros que requieren corrección

- **Nombre:** archivo nuevo y distinto del inventario original, por ejemplo `inventario_registros_por_corregir.csv`.
- **Filas:** una por cada registro con uno o más hallazgos por campos vacíos, formatos inesperados o posibles duplicados. Ninguna fila se repite.
- **Columnas originales:** todas las del inventario, con sus valores sin cambios, para poder identificar y corregir el registro.
- **Columnas añadidas (mínimo):**
  - `Problema detectado`: todos los problemas del registro y la regla que los generó.
  - `Campos que requieren revisión`: todos los campos afectados.
- **Varios problemas en un registro:** se reúnen en esas columnas, sin duplicar la fila.
- **Posibles duplicados:** se incluye cada fila implicada y `Problema detectado` explica el motivo (por ejemplo, que comparte identificador con otra fila, indicando cuál).
- **Datos:** no se completa, corrige ni reemplaza ningún valor. Los campos vacíos siguen vacíos.

## Procedimiento

1. Comprobar que cada hallazgo tiene fila o registro y regla asociada.
2. Separar hallazgos mecánicos y casos de juicio humano.
3. Redactar el informe con la estructura indicada, sin presentar como incorrecto lo que solo requiere revisión.
4. Construir el CSV a partir de la lista por registro, según la especificación anterior.
5. Comprobar el CSV antes de entregarlo:
   - Contiene todos los registros afectados y ninguno sin hallazgos.
   - No hay filas repetidas.
   - Las columnas y valores originales coinciden con el inventario.
   - Todas las filas de cada posible duplicado están presentes con su motivo.
6. Intentar guardar informe y CSV como archivos nuevos, en el formato y la ubicación acordados. **No escribir en Google Drive salvo que la persona usuaria indique una ubicación y lo autorice expresamente.**
7. Declarar el **estado de entrega** de cada archivo (ver siguiente sección).

## Estado de entrega

La respuesta indica siempre, para el informe y para el CSV, una de estas dos situaciones:

- **Archivo creado y disponible para descarga:** se indica el nombre y dónde encontrarlo.
- **No se pudo crear o descargar el archivo:** se indica la limitación, no se afirma que se guardó y se entrega el contenido en un **bloque separado** (el CSV en un bloque de código con el nombre exacto con el que debe guardarse) para que la persona lo guarde manualmente.

## Límites

- No modificar ni eliminar el inventario original ni la hoja de Google Sheets.
- No corregir, completar ni reemplazar datos, ni en el informe ni en el CSV.
- No inventar valores ni reglas; si falta una, indicarlo como pendiente.
- Explicar la regla de cada señalamiento.
- No afirmar que se guardó un archivo en una ubicación si no se ha podido hacer.
- No escribir en Drive sin autorización expresa y ubicación indicada.
- Dejar la validación y las correcciones en manos de una persona.

## Pendientes de definir

- Delimitador, codificación y entrecomillado del CSV.
- Separador entre problemas dentro de una celda.
- Si se añade una columna con el número de fila original.
- Tratamiento de hojas con varias pestañas.
- Qué se entrega si no hay hallazgos.
- Si se permite guardar las salidas en Google Drive y en qué carpeta.

Hasta que se definan, el agente debe indicar qué criterio aplicó en cada caso o preguntar.

## Resultado

- Informe de revisión trazable, con la hoja y pestaña leídas.
- CSV de registros que requieren corrección, con las columnas originales más `Problema detectado` y `Campos que requieren revisión`.
- Estado de entrega de cada archivo y, si procede, el contenido en bloques separados para guardarlo manualmente.
