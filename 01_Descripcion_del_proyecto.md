# IT AssetCheck — Descripción del proyecto

IT AssetCheck es un agente creado con Codex para revisar inventarios de activos TI. El inventario se proporciona como hoja de Google Sheets mediante el conector de Google Drive, en solo lectura; también se admiten archivos CSV o Excel adjuntados manualmente. El agente detecta registros con información incompleta o inconsistente, señala posibles duplicados y genera un informe de revisión y un CSV de registros que requieren corrección, para que una persona responsable revise los hallazgos y aplique las correcciones.

> Nota: las secciones «Problema», «Justificación» y «Alternativas consideradas» desarrollan el contexto a partir del objetivo y los límites del proyecto; no recogen datos de ningún inventario real.

## 1. Problema

Los inventarios de activos TI se mantienen en hojas de cálculo o archivos CSV/Excel y pueden contener registros con campos vacíos, formatos inesperados o filas posiblemente duplicadas. Revisarlos a mano es lento, los hallazgos suelen quedar sin referencia clara a la fila afectada ni a la regla que los originó, y quien corrige necesita tener a mano los registros afectados con sus datos originales. Además, pasar el inventario como archivo adjunto en cada revisión añade pasos manuales y versiones duplicadas.

## 2. Justificación

- Una revisión sistemática reduce el riesgo de pasar por alto registros incompletos, inconsistentes o duplicados.
- Cada hallazgo es verificable: indica fila o registro y regla aplicada.
- Un CSV con solo los registros afectados y los motivos facilita que una persona los localice y corrija sin recorrer todo el inventario.
- Leer la hoja directamente desde Google Sheets evita exportar y adjuntar copias, y la lectura de solo lectura protege el inventario original.
- Una persona valida y corrige, por lo que el control sobre el inventario no se delega.

## 3. Alternativas consideradas

| Alternativa | Observación |
|---|---|
| Revisión manual | Posible, pero lenta y poco repetible. |
| Corrección automática de datos | Descartada: el alcance prohíbe modificar el inventario y completar o inventar valores. |
| Solo informe, sin CSV | Descartada: el informe no consolida los registros con sus datos originales para corregirlos. |
| Adjuntar manualmente un CSV o Excel en cada revisión | Se mantiene como alternativa cuando el conector no está disponible o no hay permisos; no es el método principal. |
| Revisión asistida leyendo Google Sheets con informe trazable y CSV de registros por corregir (elegida) | Automatiza la detección, mantiene la trazabilidad y deja la decisión en una persona. |

## 4. Solución elegida

El agente localiza la hoja de Google Sheets que la persona usuaria indica, la lee en solo lectura, aplica únicamente las reglas que ella indica y entrega:

1. Un **informe de revisión** con los hallazgos, separando los confirmados por una comprobación mecánica de los que requieren juicio humano.
2. Un **CSV separado de registros que requieren corrección**, con una fila por cada registro afectado y sus valores originales.

## 5. Plataforma agéntica

El agente se crea con **Codex** y se usa desde ChatGPT; el paquete también puede usarse desde Claude. La configuración (proyecto, instrucciones, archivos, Skills y conexión de Google Drive) la realiza una persona manualmente; las funciones disponibles, incluida la creación y descarga de archivos y la disponibilidad del conector, dependen de la interfaz y el plan.

## 6. Skills

1. **Analizar inventarios TI** (`02_Skill_Analizar_inventarios_TI.md`): localiza y lee el inventario (Google Sheets, CSV o Excel), detecta campos incompletos, formatos inesperados y posibles duplicados, y deja los hallazgos asociados a cada fila o registro.
2. **Generar informes de revisión** (`03_Skill_Generar_informes_de_revision.md`): organiza los hallazgos en un informe trazable, genera el CSV de registros por corregir y los guarda aparte cuando la interfaz lo permite.

## 7. Conector o método de acceso

**Método principal: conector de Google Drive, con Google Sheets como formato del inventario.**

- El conector lo activa y autoriza la persona usuaria en su propia cuenta, iniciando sesión en Google. El agente no introduce ni gestiona credenciales.
- Se usa **solo lectura**: buscar el archivo indicado, leer su contenido y, si hace falta, exportarlo como texto o CSV.
- El agente solo accede a la hoja que la persona indica (enlace, identificador o título exacto) o autoriza. Si la búsqueda devuelve varios archivos, pide confirmar cuál antes de analizar.
- No modifica ni elimina la hoja original ni ningún otro archivo de Drive.
- Escribir las salidas en Drive **no es el comportamiento por defecto**: solo se haría con una ubicación indicada y autorizada expresamente por la persona usuaria.

**Estado de verificación**

- Verificado: en una sesión de Claude, el conector de Google Drive ofrece herramientas para buscar archivos, leer hojas de cálculo de Google, exportar contenido como texto o CSV y crear archivos.
- No verificado: lectura de un inventario real, comportamiento con varias pestañas, límites de tamaño y disponibilidad exacta en ChatGPT/Codex según el plan.

**Método alternativo:** adjuntar manualmente el inventario en CSV o Excel en la conversación. No existe acceso a carpetas locales ni a otros sistemas externos.

## 8. Entradas

- Referencia a la hoja de Google Sheets del inventario (enlace, identificador o título exacto) y pestaña, si hay varias; o bien un CSV/Excel adjunto.
- Campos obligatorios, indicados por la persona usuaria.
- Reglas de formato, indicadas por la persona usuaria.
- Campos que se comparan para detectar posibles duplicados, indicados por la persona usuaria.
- Formato y ubicación de guardado de las salidas, cuando la interfaz lo permita.

No se suponen campos, valores válidos ni reglas no indicados. Si falta una regla necesaria, se pregunta o se señala como pendiente en el resultado.

## 9. Salidas

### 9.1 Informe de revisión

Trazable, con hallazgos asociados a fila o registro y a la regla que los generó, e indicando la hoja y pestaña leídas (estructura en `03_Skill_Generar_informes_de_revision.md`).

### 9.2 CSV de registros que requieren corrección

Archivo nuevo y con nombre distinto al del inventario (por ejemplo `inventario_registros_por_corregir.csv`):

- Una fila por cada registro que presenta uno o más hallazgos por campos vacíos, formatos inesperados o posibles duplicados.
- Conserva todas las columnas y valores originales del inventario.
- Añade, como mínimo, `Problema detectado` y `Campos que requieren revisión`.
- Si un registro tiene varios problemas, se reúnen en esas columnas sin duplicar la fila.
- Para posibles duplicados, incluye cada fila implicada y explica el motivo (por ejemplo, que comparte identificador con otra fila).
- No completa, corrige ni reemplaza datos.

### 9.3 Estado de entrega

El agente indica claramente si pudo acceder a la hoja y si el entorno permite crear y descargar cada archivo. Si no puede crearlo, no afirma haberlo guardado: entrega el contenido CSV en un bloque separado para guardarlo manualmente.

### 9.4 Avisos

Hoja, pestaña, archivo o columna no accesibles o no legibles; conector no disponible o sin permisos; reglas ausentes.

## 10. Reglas de control

- Trabajar solo con la hoja o el archivo proporcionado o autorizado.
- Acceder a Google Drive únicamente en solo lectura; no modificar ni eliminar la hoja original ni otros archivos.
- No revisar archivos de Drive que no se hayan indicado o autorizado.
- Guardar el CSV y el informe como archivos nuevos con nombre distinto; no escribir en Drive sin autorización expresa.
- No inventar, completar, corregir ni reemplazar valores, tampoco en el CSV.
- No presentar como incorrecto un dato que solo requiere revisión.
- Explicar la regla que generó cada señalamiento y asociarlo a su fila o registro.
- Aplicar únicamente las reglas de validación indicadas por la persona usuaria.
- Avisar si no se puede acceder o leer la hoja, una pestaña o una columna, o si el conector no está disponible; no suponer su contenido.
- No afirmar que se guardó un archivo que no se pudo crear.
- Dejar la validación y las correcciones finales en manos de una persona.

## 11. Decisiones pendientes de definir

- Delimitador, codificación y entrecomillado del CSV de salida.
- Separador dentro de las celdas que agrupan varios problemas.
- Si el CSV incluye una columna con el número de fila original.
- Cómo se numera una fila de la hoja (propuesta: número de fila visible en Google Sheets, con la cabecera como fila 1).
- Tratamiento de hojas con varias pestañas.
- Qué se entrega cuando no hay hallazgos.
- Si se permite guardar las salidas en Google Drive y en qué carpeta.
- Comportamiento del conector con hojas grandes, fórmulas o celdas combinadas.
- Formato y ubicación por defecto del informe.
