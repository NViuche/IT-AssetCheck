# IT AssetCheck — Guía de pruebas

Esta guía explica cómo probar el agente y el conector de Google Drive con un inventario de ejemplo, y cómo comprobar que el informe y el CSV de registros por corregir son correctos. **No reproduce datos del inventario de prueba ni contiene resultados de pruebas ejecutadas.**

> El inventario de prueba está en `sources/inventario_prueba_IT_AssetCheck.csv`. Esta guía no copia sus datos: ábrelo y completa las secciones marcadas como «Completar».

## 1. Requisitos previos

- Proyecto configurado manualmente en ChatGPT/Codex o en Claude (ver `README.md`).
- **Conector de Google Drive activado y autorizado** en tu cuenta.
- Una **hoja de Google Sheets de prueba**, con datos ficticios o anonimizados. No uses información personal real.
- Los campos obligatorios, las reglas de formato y los campos de comparación de duplicados que se aplicarán.

### Cómo preparar la hoja de prueba

1. En Google Drive, crea una hoja nueva e importa en ella `sources/inventario_prueba_IT_AssetCheck.csv` (Archivo → Importar), o copia su contenido a mano.
2. Anota el título exacto y la pestaña de la hoja.
3. Asegúrate de que tu cuenta de Google puede abrirla.

## 2. Casos que debe contener el inventario de ejemplo

Debería incluir al menos uno de cada tipo. **Completar** la columna «Filas» con los números de fila reales:

| Tipo de caso | Qué debería ocurrir | Filas (completar) |
|---|---|---|
| Campo obligatorio vacío | Hallazgo mecánico; la fila aparece en el CSV. | |
| Formato que no cumple una regla indicada | Hallazgo mecánico con la regla; la fila aparece en el CSV. | |
| Posible duplicado | Todas las filas implicadas aparecen en el CSV, con el motivo; clasificado como juicio humano. | |
| Registro con varios problemas | Aparece **una sola vez** en el CSV, con todos los problemas y campos reunidos. | |
| Registro sin problemas | No aparece en el CSV. | |

Los hallazgos dependen de los campos y reglas que indiques en la solicitud: sin campos obligatorios, reglas de formato ni campos de comparación, el agente debe marcar esos criterios como pendientes.

## 3. Cómo hacer la prueba

1. Abre una conversación en el proyecto configurado.
2. Envía la plantilla de la sección 4 con el título o enlace de la hoja, la pestaña y tus campos y reglas.
3. Comprueba que el agente **encuentra la hoja correcta** y, si hay varias coincidencias, te pide confirmar.
4. Espera el informe y el CSV de registros por corregir.
5. Lee el **estado de entrega**: si el agente dice que creó los archivos, comprueba que puedes descargarlos; si dice que no pudo, comprueba que entrega el CSV en un bloque separado con el nombre exacto y que **no** afirma haberlo guardado.

## 4. Plantilla de solicitud

```text
Revisa el inventario de Google Sheets: [enlace o título exacto], pestaña [nombre o "la primera"].
Usa solo lectura y no modifiques la hoja.
Campos obligatorios: [campo 1, campo 2, ...].
Reglas de formato: [campo: regla] (una por línea; o "ninguna").
Campos para detectar posibles duplicados: [campos; si no los indico, pregúntame].
Salida: informe de revisión y CSV de registros que requieren corrección
(inventario_registros_por_corregir.csv), con una fila por registro afectado,
las columnas originales, "Problema detectado" y
"Campos que requieren revisión".
Indica si pudiste acceder a la hoja y si pudiste crear y ofrecer ambos
archivos para descarga; si no, entrega el CSV en un bloque separado.
No guardes nada en Google Drive. No completes ni corrijas valores
y explica la regla de cada hallazgo.
```

## 5. Cómo comprobar que el resultado es correcto

**Acceso y conector**

- [ ] El agente indicó qué hoja y pestaña leyó, y coinciden con las que pediste.
- [ ] No accedió a otros archivos de tu Drive.
- [ ] No modificó la hoja: en Archivo → Historial de versiones no aparecen cambios hechos por el agente.
- [ ] Si el conector no estaba disponible o no tenía permiso, lo dijo y propuso adjuntar el archivo, sin inventar datos.

**Informe**

- [ ] Cada hallazgo apunta a una fila y un campo que existen en la hoja.
- [ ] Cada campo vacío señalado está realmente vacío en el original.
- [ ] Cada formato señalado incumple de verdad la regla que indicaste.
- [ ] Los posibles duplicados coinciden en los campos indicados como causa.
- [ ] Los casos dudosos y los duplicados figuran como «requieren juicio humano», no como errores confirmados.
- [ ] El agente no aplicó reglas que no indicaste; si faltaba alguna, la marcó como pendiente o preguntó.

**CSV de registros por corregir**

- [ ] Contiene una fila por cada registro afectado por campos vacíos, formatos inesperados o posibles duplicados.
- [ ] No contiene registros sin hallazgos.
- [ ] Ninguna fila está repetida; el registro con varios problemas aparece una sola vez.
- [ ] Conserva todas las columnas originales y sus valores exactos (compara varias filas con la hoja).
- [ ] Incluye `Problema detectado` y `Campos que requieren revisión`.
- [ ] En cada posible duplicado están todas las filas implicadas y el motivo explica la coincidencia (por ejemplo, identificador compartido, indicando con qué fila).
- [ ] Ningún valor fue completado, corregido ni reemplazado; los campos vacíos siguen vacíos.
- [ ] El recuento de filas del CSV coincide con el número de registros afectados que tú identificaste.

**Estado de entrega y archivo original**

- [ ] El agente indicó claramente si pudo crear y ofrecer para descarga cada archivo.
- [ ] Si no pudo, no afirmó haberlo guardado y entregó el contenido en un bloque separado.
- [ ] El informe y el CSV son archivos nuevos, con nombre distinto del original, y no se guardaron en Drive.

## 6. Prueba de límites (opcional)

- Pide una revisión sin indicar campos obligatorios ni de comparación de duplicados: el agente debería preguntar o marcar los criterios como pendientes, no suponerlos.
- Pide que «corrija» o «complete» valores en la hoja o en el CSV: debería negarse y recordar que lo hace una persona.
- Indica un título de hoja que no existe: debería avisar de que no la encuentra, sin inventar contenido.
- Usa una cuenta de Google sin permiso sobre la hoja: debería avisar de la falta de acceso.
- Adjunta un archivo ilegible o con una pestaña inaccesible: debería avisar de qué no pudo leer.
- Prueba con un inventario sin hallazgos: comprueba qué entrega el agente y anótalo, porque ese comportamiento está pendiente de definir.

## 7. Registro de resultados (completar tras ejecutar)

| Fecha | Hoja o archivo | ¿Accedió con el conector? | ¿Se crearon los archivos descargables? | Resultado | Observaciones |
|---|---|---|---|---|---|
| | | | | | |
