# IT AssetCheck — Revisión de inventarios TI

IT AssetCheck es un agente creado con Codex para revisar inventarios de activos TI almacenados en archivos CSV o Excel. Detecta registros con información incompleta o inconsistente, señala posibles duplicados y genera dos salidas separadas: un **informe de revisión** y un **CSV de registros que requieren corrección**, para que una persona responsable revise y corrija.

## Qué problema resuelve

Los inventarios de activos TI pueden contener campos vacíos, formatos inesperados o filas posiblemente duplicadas. Revisarlos a mano es lento y difícil de repetir con el mismo criterio. IT AssetCheck automatiza la detección, asocia cada hallazgo con su fila y con la regla que lo generó, y consolida los registros afectados en un CSV para facilitar su corrección. Las correcciones las hace siempre una persona.

## Requisitos

- Acceso a ChatGPT con Codex y con la posibilidad de crear un proyecto, cargar archivos y adjuntar archivos a una conversación.
- Un inventario en **CSV o Excel** que la persona usuaria pueda adjuntar.
- Los **campos obligatorios** y las **reglas de formato** del inventario, definidos por la persona usuaria. El agente no los supone.
- Para detectar posibles duplicados, los **campos que se comparan** (por ejemplo, un identificador), indicados por la persona usuaria.

> Las opciones exactas del menú, los nombres de las funciones y la disponibilidad de cada una dependen de la interfaz y del plan de ChatGPT. Compruébalas en tu cuenta antes de configurar.

## Estructura de archivos

| Archivo | Contenido |
|---|---|
| `README.md` | Esta guía. |
| `01_Descripcion_del_proyecto.md` | Problema, justificación, alternativas, solución, plataforma, Skills, método de acceso, entradas, salidas y reglas de control. |
| `02_Skill_Analizar_inventarios_TI.md` | Skill «Analizar inventarios TI». |
| `03_Skill_Generar_informes_de_revision.md` | Skill «Generar informes de revisión». |
| `04_Guia_de_pruebas.md` | Cómo probar el agente y verificar el informe y el CSV. |
| `sources/inventario_prueba_IT_AssetCheck.csv` | Inventario de prueba. |

## Qué debe configurarse manualmente en ChatGPT

El paquete son documentos; **no se instala ni se activa por sí solo**. Una persona debe hacer manualmente lo siguiente:

1. Crear el proyecto o agente en ChatGPT/Codex.
2. Pegar las instrucciones del proyecto, incluida la salida adicional del CSV (ver `01_Descripcion_del_proyecto.md`, secciones «Entradas», «Salidas» y «Reglas de control»).
3. Cargar estos archivos como archivos del proyecto, o dar de alta cada Skill con el contenido de `02_...` y `03_...`, según lo que permita la interfaz.
4. En cada conversación, adjuntar el inventario que se quiere revisar.
5. Indicar los campos obligatorios, las reglas de formato y los campos de comparación de duplicados en cada solicitud.
6. Guardar manualmente los resultados en la ubicación deseada si la interfaz solo permite descargarlos o solo muestra el contenido.

**Pendiente de confirmar por quien configure el proyecto:** si su cuenta permite al agente crear y descargar archivos (informe y CSV).

## Acceso a archivos: no hay integración con carpetas locales

Este proyecto **no incluye ninguna integración ni conector configurado** para acceder a carpetas locales ni a sistemas externos. El método de acceso es **adjuntar manualmente** el archivo en la conversación. Si en el futuro se configura otro método, debe documentarse en `01_Descripcion_del_proyecto.md`.

## Pasos de uso

1. Abre una conversación en el proyecto.
2. Adjunta el inventario (CSV o Excel).
3. Indica los campos obligatorios, las reglas de formato y los campos de comparación de duplicados.
4. Pide la revisión con la plantilla de `04_Guia_de_pruebas.md`.
5. Lee en la respuesta si el entorno **pudo crear y ofrecer para descarga** el informe y el CSV. Si no pudo, el contenido llega en bloques separados y debes guardarlo tú con el nombre indicado.
6. Contrasta los hallazgos con el inventario original y aplica tú las correcciones.

## Ejemplo de solicitud de prueba

```text
Revisa el inventario adjunto. Es un archivo de prueba.
Campos obligatorios: [lista de campos].
Reglas de formato: [regla por campo, o "ninguna"].
Campos para detectar posibles duplicados: [campos].
Genera el informe de revisión y el CSV de registros por corregir
(inventario_registros_por_corregir.csv). Indica si pudiste crear
y ofrecer los archivos para descarga.
No modifiques el inventario ni completes valores.
```

## Resultados esperados

- **Informe de revisión** con datos de la revisión, limitaciones, hallazgos confirmados mecánicamente, casos que requieren juicio humano y nota de control.
- **CSV de registros que requieren corrección** (por ejemplo `inventario_registros_por_corregir.csv`):
  - Una fila por cada registro con uno o más hallazgos (campos vacíos, formatos inesperados o posibles duplicados), sin duplicar filas.
  - Todas las columnas y valores originales, sin cambios.
  - Como mínimo, las columnas añadidas `Problema detectado` y `Campos que requieren revisión`, que reúnen todos los problemas del registro.
  - Para posibles duplicados, cada fila implicada con el motivo (por ejemplo, que comparte identificador con otra fila).
- **Estado de entrega** de cada archivo: si el entorno pudo crearlo y ofrecerlo para descarga, o si se entrega su contenido en un bloque separado. El agente no afirma haber guardado un archivo que no creó.
- **Avisos** si no se puede leer un archivo, hoja o columna, o si falta una regla.

## Limitaciones y reglas de control

- Solo revisa archivos que la persona usuaria proporciona o autoriza.
- No modifica ni elimina el inventario original; el CSV es un archivo nuevo con nombre distinto.
- No inventa, completa, corrige ni reemplaza valores, tampoco en el CSV.
- Solo aplica las reglas indicadas por la persona usuaria; si falta una, la señala como pendiente o pregunta.
- No presenta como incorrecto un dato que solo requiere revisión.
- Explica la regla de cada señalamiento y lo asocia a su fila o registro.
- No hay acceso a carpetas locales ni a sistemas externos.
- Las correcciones finales las valida y aplica una persona.
- Este paquete no incluye resultados de pruebas ejecutadas.

## Decisiones pendientes de definir

- Delimitador, codificación y entrecomillado del CSV de salida.
- Separador dentro de las celdas que agrupan varios problemas.
- Si el CSV debe incluir una columna con el número de fila original.
- Tratamiento de libros Excel con varias hojas (un CSV por hoja o uno consolidado).
- Qué se entrega cuando no hay ningún hallazgo (CSV solo con cabecera o ningún CSV).
- Formato y ubicación por defecto del informe.
