# IT AssetCheck — Guía de pruebas

Esta guía explica cómo probar el agente con un inventario de ejemplo y cómo comprobar que el informe y el CSV de registros por corregir son correctos. **No contiene datos del inventario de prueba ni resultados de pruebas ejecutadas.**

> **Pendiente:** el CSV de ejemplo no estaba disponible al preparar esta guía, por lo que su nombre, columnas y casos concretos no se documentan. Complétalos en las secciones marcadas como «Completar» a partir del archivo real.

## 1. Requisitos previos

- Proyecto configurado manualmente en ChatGPT/Codex (ver `README.md`).
- El CSV de ejemplo, con **datos ficticios o anonimizados**. No uses información personal real.
- Los campos obligatorios, las reglas de formato y los campos de comparación de duplicados que se aplicarán.

## 2. Casos que debe contener el CSV de ejemplo

Debería incluir al menos uno de cada tipo. **Completar** la columna «Filas» con los números de fila reales:

| Tipo de caso | Qué debería ocurrir | Filas (completar) |
|---|---|---|
| Campo obligatorio vacío | Hallazgo mecánico; la fila aparece en el CSV. | |
| Formato que no cumple una regla indicada | Hallazgo mecánico con la regla; la fila aparece en el CSV. | |
| Posible duplicado | Todas las filas implicadas aparecen en el CSV, con el motivo; clasificado como juicio humano. | |
| Registro con varios problemas | Aparece **una sola vez** en el CSV, con todos los problemas y campos reunidos. | |
| Registro sin problemas | No aparece en el CSV. | |

## 3. Cómo hacer la prueba

1. Abre una conversación en el proyecto configurado.
2. Adjunta el CSV de ejemplo.
3. Envía la plantilla de la sección 4 con tus campos y reglas.
4. Espera el informe y el CSV de registros por corregir.
5. Lee el **estado de entrega**: si el agente dice que creó los archivos, comprueba que puedes descargarlos; si dice que no pudo, comprueba que entrega el CSV en un bloque separado con el nombre exacto y que **no** afirma haberlo guardado.

## 4. Plantilla de solicitud

```text
Revisa el inventario adjunto.
Archivo: [nombre del archivo]. Hoja (solo Excel): [nombre o "todas"].
Campos obligatorios: [campo 1, campo 2, ...].
Reglas de formato: [campo: regla] (una por línea; o "ninguna").
Campos para detectar posibles duplicados: [campos; si no los indico, pregúntame].
Salida: informe de revisión y CSV de registros que requieren corrección
(inventario_registros_por_corregir.csv), con una fila por registro afectado,
las columnas originales, "Problema detectado" y
"Campos que requieren revisión".
Indica si pudiste crear y ofrecer ambos archivos para descarga; si no,
entrega el CSV en un bloque separado.
No modifiques el inventario original, no completes ni corrijas valores
y explica la regla de cada hallazgo.
```

## 5. Cómo comprobar que el resultado es correcto

**Informe**

- [ ] Cada hallazgo apunta a una fila y un campo que existen en el archivo.
- [ ] Cada campo vacío señalado está realmente vacío en el original.
- [ ] Cada formato señalado incumple de verdad la regla que indicaste.
- [ ] Los posibles duplicados coinciden en los campos indicados como causa.
- [ ] Los casos dudosos y los duplicados figuran como «requieren juicio humano», no como errores confirmados.
- [ ] El agente no aplicó reglas que no indicaste; si faltaba alguna, la marcó como pendiente o preguntó.

**CSV de registros por corregir**

- [ ] Contiene una fila por cada registro afectado por campos vacíos, formatos inesperados o posibles duplicados.
- [ ] No contiene registros sin hallazgos.
- [ ] Ninguna fila está repetida; el registro con varios problemas aparece una sola vez.
- [ ] Conserva todas las columnas originales y sus valores exactos (compara varias filas con el original).
- [ ] Incluye `Problema detectado` y `Campos que requieren revisión`.
- [ ] En cada posible duplicado están todas las filas implicadas y el motivo explica la coincidencia (por ejemplo, identificador compartido, indicando con qué fila).
- [ ] Ningún valor fue completado, corregido ni reemplazado; los campos vacíos siguen vacíos.
- [ ] El recuento de filas del CSV coincide con el número de registros afectados que tú identificaste.

**Estado de entrega y archivo original**

- [ ] El agente indicó claramente si pudo crear y ofrecer para descarga cada archivo.
- [ ] Si no pudo, no afirmó haberlo guardado y entregó el contenido en un bloque separado.
- [ ] El inventario original no ha cambiado (compara tamaño, fecha de modificación o suma de verificación antes y después).
- [ ] El informe y el CSV son archivos nuevos, con nombre distinto del original.

## 6. Prueba de límites (opcional)

- Pide una revisión sin indicar campos obligatorios ni de comparación de duplicados: el agente debería preguntar o marcar los criterios como pendientes, no suponerlos.
- Pide que «corrija» o «complete» valores en el CSV: debería negarse y recordar que lo hace una persona.
- Adjunta un archivo ilegible o con una hoja inaccesible: debería avisar de qué no pudo leer.
- Prueba con un inventario sin hallazgos: comprueba qué entrega el agente y anótalo, porque ese comportamiento está pendiente de definir.

## 7. Registro de resultados (completar tras ejecutar)

| Fecha | Archivo | ¿Se crearon los archivos descargables? | Resultado | Observaciones |
|---|---|---|---|---|
| | | | | |
