# IT AssetCheck — Descripción del proyecto

IT AssetCheck es un agente creado con Codex para revisar inventarios de activos TI almacenados en archivos CSV o Excel. Detecta registros con información incompleta o inconsistente, señala posibles duplicados y genera un informe de revisión y un CSV de registros que requieren corrección, para que una persona responsable revise los hallazgos y aplique las correcciones.

> Nota: las secciones «Problema», «Justificación» y «Alternativas consideradas» desarrollan el contexto a partir del objetivo y los límites del proyecto; no recogen datos de ningún inventario real.

## 1. Problema

Los inventarios de activos TI se mantienen en archivos CSV o Excel y pueden contener registros con campos vacíos, formatos inesperados o filas posiblemente duplicadas. Revisarlos a mano es lento, los hallazgos suelen quedar sin referencia clara a la fila afectada ni a la regla que los originó, y quien corrige necesita tener a mano los registros afectados con sus datos originales.

## 2. Justificación

- Una revisión sistemática reduce el riesgo de pasar por alto registros incompletos, inconsistentes o duplicados.
- Cada hallazgo es verificable: indica fila o registro y regla aplicada.
- Un CSV con solo los registros afectados y los motivos facilita que una persona los localice y corrija sin recorrer todo el inventario.
- Una persona valida y corrige, por lo que el control sobre el inventario no se delega.

## 3. Alternativas consideradas

| Alternativa | Observación |
|---|---|
| Revisión manual | Posible, pero lenta y poco repetible. |
| Corrección automática de datos | Descartada: el alcance prohíbe modificar el inventario y completar o inventar valores. |
| Solo informe, sin CSV | Descartada: el informe no consolida los registros con sus datos originales para corregirlos. |
| Revisión asistida con informe trazable y CSV de registros por corregir (elegida) | Automatiza la detección, mantiene la trazabilidad y deja la decisión en una persona. |

## 4. Solución elegida

El agente lee el inventario que la persona usuaria proporciona o autoriza, aplica únicamente las reglas que ella indica y entrega:

1. Un **informe de revisión** con los hallazgos, separando los confirmados por una comprobación mecánica de los que requieren juicio humano.
2. Un **CSV separado de registros que requieren corrección**, con una fila por cada registro afectado y sus valores originales.

## 5. Plataforma agéntica

El agente se crea con **Codex** y se usa desde ChatGPT. La configuración (proyecto, instrucciones, archivos y Skills) la realiza una persona manualmente; las funciones disponibles, incluida la creación y descarga de archivos, dependen de la interfaz y el plan.

## 6. Skills

1. **Analizar inventarios TI** (`02_Skill_Analizar_inventarios_TI.md`): lee tablas, detecta campos incompletos, formatos inesperados y posibles duplicados, y deja los hallazgos asociados a cada fila o registro.
2. **Generar informes de revisión** (`03_Skill_Generar_informes_de_revision.md`): organiza los hallazgos en un informe trazable, genera el CSV de registros por corregir y los guarda aparte cuando la interfaz lo permite.

## 7. Conector o método de acceso

**No hay conector ni integración configurados.** No existe acceso a carpetas locales ni a sistemas externos. El método de acceso es **adjuntar manualmente** el inventario (CSV o Excel) en la conversación. Cualquier otro método debe configurarse y documentarse aquí antes de afirmar que existe.

## 8. Entradas

- Inventario CSV o Excel adjunto o autorizado por la persona usuaria.
- Campos obligatorios, indicados por la persona usuaria.
- Reglas de formato, indicadas por la persona usuaria.
- Campos que se comparan para detectar posibles duplicados, indicados por la persona usuaria.
- Formato y ubicación de guardado de las salidas, cuando la interfaz lo permita.

No se suponen campos, valores válidos ni reglas no indicados. Si falta una regla necesaria, se pregunta o se señala como pendiente en el resultado.

## 9. Salidas

### 9.1 Informe de revisión

Trazable, con hallazgos asociados a fila o registro y a la regla que los generó (estructura en `03_Skill_Generar_informes_de_revision.md`).

### 9.2 CSV de registros que requieren corrección

Archivo nuevo y con nombre distinto al del inventario (por ejemplo `inventario_registros_por_corregir.csv`):

- Una fila por cada registro que presenta uno o más hallazgos por campos vacíos, formatos inesperados o posibles duplicados.
- Conserva todas las columnas y valores originales del inventario.
- Añade, como mínimo, `Problema detectado` y `Campos que requieren revisión`.
- Si un registro tiene varios problemas, se reúnen en esas columnas sin duplicar la fila.
- Para posibles duplicados, incluye cada fila implicada y explica el motivo (por ejemplo, que comparte identificador con otra fila).
- No completa, corrige ni reemplaza datos.

### 9.3 Estado de entrega

El agente indica claramente si el entorno permite crear y descargar cada archivo. Si no puede crearlo, no afirma haberlo guardado: entrega el contenido CSV en un bloque separado para guardarlo manualmente.

### 9.4 Avisos

Archivo, hoja o columna no legibles y reglas ausentes.

## 10. Reglas de control

- Trabajar solo con archivos proporcionados o autorizados.
- No modificar ni eliminar el inventario original; guardar el CSV como archivo nuevo con nombre distinto.
- No inventar, completar, corregir ni reemplazar valores, tampoco en el CSV.
- No presentar como incorrecto un dato que solo requiere revisión.
- Explicar la regla que generó cada señalamiento y asociarlo a su fila o registro.
- Aplicar únicamente las reglas de validación indicadas por la persona usuaria.
- Avisar si no se puede leer el archivo, una hoja o una columna.
- No afirmar que se guardó un archivo que no se pudo crear.
- Dejar la validación y las correcciones finales en manos de una persona.

## 11. Decisiones pendientes de definir

- Delimitador, codificación y entrecomillado del CSV de salida.
- Separador dentro de las celdas que agrupan varios problemas.
- Si el CSV incluye una columna con el número de fila original.
- Tratamiento de libros Excel con varias hojas.
- Qué se entrega cuando no hay hallazgos.
- Formato y ubicación por defecto del informe.
