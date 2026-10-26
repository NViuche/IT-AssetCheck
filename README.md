# IT AssetCheck — Revisión de inventarios TI

IT AssetCheck es un agente creado con Codex para revisar inventarios de activos TI. El inventario se proporciona como **hoja de Google Sheets** a través del **conector de Google Drive**, en modo de solo lectura (también se admiten CSV y Excel adjuntados a mano). El agente detecta registros con información incompleta o inconsistente, señala posibles duplicados y genera dos salidas separadas: un **informe de revisión** y un **CSV de registros que requieren corrección**, para que una persona responsable revise y corrija.

## Qué problema resuelve

Los inventarios de activos TI pueden contener campos vacíos, formatos inesperados o filas posiblemente duplicadas. Revisarlos a mano es lento y difícil de repetir con el mismo criterio. IT AssetCheck automatiza la detección, asocia cada hallazgo con su fila y con la regla que lo generó, y consolida los registros afectados en un CSV para facilitar su corrección. Las correcciones las hace siempre una persona, y la hoja original nunca se modifica.

## Requisitos

- Acceso a ChatGPT con Codex (o a Claude) con posibilidad de crear un proyecto y cargar archivos.
- El **conector de Google Drive activado y autorizado en tu cuenta**. Lo conectas tú manualmente; ver «Acceso al inventario».
- El inventario en una **hoja de Google Sheets** a la que tu cuenta de Google tenga acceso de lectura.
- Los **campos obligatorios** y las **reglas de formato** del inventario, definidos por la persona usuaria. El agente no los supone.
- Para detectar posibles duplicados, los **campos que se comparan** (por ejemplo, un identificador), indicados por la persona usuaria.

> Las opciones exactas del menú, los nombres de las funciones y la disponibilidad del conector de Google Drive dependen de la interfaz y del plan. Compruébalas en tu cuenta antes de configurar.

## Estructura de archivos

| Archivo | Contenido |
|---|---|
| `README.md` | Esta guía. |
| `01_Descripcion_del_proyecto.md` | Problema, justificación, alternativas, solución, plataforma, Skills, conector, entradas, salidas y reglas de control. |
| `02_Skill_Analizar_inventarios_TI.md` | Skill «Analizar inventarios TI». |
| `03_Skill_Generar_informes_de_revision.md` | Skill «Generar informes de revisión». |
| `04_Guia_de_pruebas.md` | Cómo probar el agente y el conector, y verificar el informe y el CSV. |
| `sources/inventario_prueba_IT_AssetCheck.csv` | Inventario de prueba. |
| `LICENSE` | Licencia MIT. |
| `.gitignore` | Archivos que no se suben al repositorio. |

## Acceso al inventario: conector de Google Drive (Google Sheets)

**Cómo funciona**

1. Indicas al agente qué hoja revisar: enlace, identificador o título exacto del archivo, y la pestaña si hay varias.
2. El agente localiza esa hoja con el conector de Google Drive y la **lee en modo de solo lectura**. Si la búsqueda devuelve varios archivos, te pide confirmar cuál antes de analizar.
3. Analiza solo esa hoja y entrega el informe y el CSV de registros por corregir como archivos nuevos y separados.

**Lo que el agente no hace**

- No modifica, reemplaza ni elimina la hoja original ni ningún otro archivo de tu Drive.
- No revisa otros archivos de tu Drive que no le hayas indicado o autorizado.
- No guarda las salidas en Drive salvo que le indiques una ubicación y lo autorices expresamente.

**Qué configuras tú manualmente**

1. Conectar Google Drive en tu cuenta (ChatGPT o Claude), iniciando sesión en Google y aceptando los permisos. Este paso lo haces tú; el agente no introduce credenciales.
2. Comprobar que tu cuenta de Google puede abrir la hoja del inventario.
3. En cada conversación, indicar la hoja, la pestaña, los campos obligatorios, las reglas de formato y los campos de comparación de duplicados.

**Qué está verificado y qué no**

- Verificado: en una sesión de Claude, el conector de Google Drive ofrece herramientas para buscar archivos, leer hojas de cálculo de Google y exportar su contenido como texto o CSV.
- **No verificado:** la lectura de un inventario real, el comportamiento con varias pestañas, los límites de tamaño y la disponibilidad exacta del conector en ChatGPT/Codex según tu plan. Se comprueba con `04_Guia_de_pruebas.md`.

**Alternativa sin conector:** si el conector no está disponible o no tienes permisos, puedes adjuntar manualmente el inventario en formato CSV o Excel en la conversación. No hay acceso a carpetas locales ni a ningún otro sistema externo.

## Pasos de uso

1. Conecta Google Drive en tu cuenta (una sola vez).
2. Abre una conversación en el proyecto.
3. Indica la hoja de Google Sheets del inventario y su pestaña.
4. Indica los campos obligatorios, las reglas de formato y los campos de comparación de duplicados.
5. Pide la revisión con la plantilla de `04_Guia_de_pruebas.md`.
6. Lee en la respuesta si el agente **pudo acceder a la hoja** y si **pudo crear y ofrecer para descarga** el informe y el CSV. Si no pudo, el contenido llega en bloques separados y debes guardarlo tú con el nombre indicado.
7. Contrasta los hallazgos con la hoja original y aplica tú las correcciones.

## Ejemplo de solicitud de prueba

```text
Revisa el inventario de Google Sheets: [enlace o título exacto], pestaña [nombre].
Es un archivo de prueba. Usa solo lectura.
Campos obligatorios: [lista de campos].
Reglas de formato: [regla por campo, o "ninguna"].
Campos para detectar posibles duplicados: [campos].
Genera el informe de revisión y el CSV de registros por corregir
(inventario_registros_por_corregir.csv). Indica si pudiste acceder a la hoja
y si pudiste crear y ofrecer los archivos para descarga.
No modifiques la hoja ni completes valores.
```

## Resultados esperados

- **Informe de revisión** con datos de la revisión (incluida la hoja y pestaña leídas), limitaciones, hallazgos confirmados mecánicamente, casos que requieren juicio humano y nota de control.
- **CSV de registros que requieren corrección** (por ejemplo `inventario_registros_por_corregir.csv`):
  - Una fila por cada registro con uno o más hallazgos (campos vacíos, formatos inesperados o posibles duplicados), sin duplicar filas.
  - Todas las columnas y valores originales, sin cambios.
  - Como mínimo, las columnas añadidas `Problema detectado` y `Campos que requieren revisión`, que reúnen todos los problemas del registro.
  - Para posibles duplicados, cada fila implicada con el motivo (por ejemplo, que comparte identificador con otra fila).
- **Estado de entrega** de cada archivo: si el entorno pudo crearlo y ofrecerlo para descarga, o si se entrega su contenido en un bloque separado. El agente no afirma haber guardado un archivo que no creó.
- **Avisos** si no se puede acceder a la hoja, pestaña o columna, o si falta una regla.

## Limitaciones y reglas de control

- Solo revisa la hoja o el archivo que la persona usuaria proporciona o autoriza.
- Accede a Google Drive en solo lectura; no modifica ni elimina la hoja original. El CSV es un archivo nuevo con nombre distinto.
- No inventa, completa, corrige ni reemplaza valores, tampoco en el CSV.
- Solo aplica las reglas indicadas por la persona usuaria; si falta una, la señala como pendiente o pregunta.
- No presenta como incorrecto un dato que solo requiere revisión.
- Explica la regla de cada señalamiento y lo asocia a su fila o registro.
- Avisa si el conector no está disponible, si no tiene permiso sobre la hoja o si no puede leer una pestaña o columna; no inventa su contenido.
- No hay acceso a carpetas locales ni a otros sistemas externos.
- Las correcciones finales las valida y aplica una persona.
- Este paquete no incluye resultados de pruebas ejecutadas.

## Decisiones pendientes de definir

- Delimitador, codificación y entrecomillado del CSV de salida.
- Separador dentro de las celdas que agrupan varios problemas.
- Si el CSV debe incluir una columna con el número de fila original.
- Cómo se numera una fila de la hoja (propuesta: número de fila visible en Google Sheets, con la cabecera como fila 1).
- Tratamiento de hojas con varias pestañas (una pestaña por revisión, un CSV por pestaña o uno consolidado).
- Qué se entrega cuando no hay ningún hallazgo (CSV solo con cabecera o ningún CSV).
- Si se permite guardar las salidas en Google Drive y en qué carpeta (por defecto, no).
- Formato y ubicación por defecto del informe.
- Comportamiento del conector con hojas grandes, fórmulas o celdas combinadas.
