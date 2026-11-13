# Relevamiento Situacional de Personas en Situación de Discapacidad · Río Cuarto 2026

> **Prueba piloto preliminar · Confidencial**

Formulario web accesible para el **autocenso** de personas en situación de discapacidad que residen en la ciudad de Río Cuarto (Córdoba, Argentina). Es una iniciativa del Gobierno de Río Cuarto, con el soporte técnico y metodológico de la Dirección de Estadística, Control de Calidad y Procesos.

El relevamiento busca conocer las condiciones de vida, la accesibilidad, los apoyos, la salud, la educación, el trabajo y la participación de las personas en situación de discapacidad, para planificar políticas y programas con evidencia. Tiene dos usos: **estadístico** (diagnóstico agregado) y **de gestión** (orientación y seguimiento individualizado de programas y prestaciones).

El formulario implementa la versión 3 del instrumento, con los ajustes validados durante su desarrollo.

---

## Características

- **Un solo archivo HTML.** No necesita instalación, servidor ni dependencias externas: funciona abriéndolo en cualquier navegador moderno, en la computadora o en el celular.
- **Un formulario por hogar, varias personas.** Los datos del hogar se piden una sola vez (Parte A). Los bloques de cada persona en situación de discapacidad se repiten tantas veces como personas haya en el hogar (Parte B).
- **Estándares internacionales según la edad.**
  - 18 años o más: Lista breve de preguntas sobre la discapacidad del Grupo de Washington (WG-SS).
  - 2 a 17 años: Módulo sobre el funcionamiento en niños y niñas de UNICEF y el Grupo de Washington (ver *Pendientes*).
  - Menores de 2 años: se incorporan por CUD, trámite o diagnóstico.
- **Rutas automáticas por edad.** La edad se calcula a partir de la fecha de nacimiento y define qué bloques se muestran: Educación desde los 3 años, Trabajo desde los 14 y Talleres inclusivos desde los 16.
- **Lógica condicional.** Las preguntas de seguimiento aparecen solo cuando corresponden, y las respuestas que quedan ocultas se borran.
- **Validaciones.** DNI, fechas, teléfono y correo; coherencia entre la cantidad de personas del hogar (P9 y P10) y la convivencia (P22); control de documentos repetidos dentro del hogar; obligatoriedad solo donde corresponde.
- **Consentimiento informado obligatorio.** Se muestra una única vez al inicio y sin aceptarlo no se puede avanzar. Se registran la fecha, la hora y la versión del texto.
- **Buscador de calles.** Incluye las 839 calles de la ciudad. La búsqueda no distingue mayúsculas, acentos, Ñ ni Ü. La altura se pide en un campo aparte.
- **Listado oficial de barrios** (54 barrios).
- **Constancia final** con código único, fecha y hora, y los contactos que corresponden según las respuestas. Se puede imprimir o guardar en PDF.

## Accesibilidad

Diseñado con criterios de **WCAG 2.1 nivel AA**:

- Compatible con lectores de pantalla (etiquetas, grupos de opciones, mensajes de error asociados a cada campo y avisos en vivo).
- Navegación completa con teclado, incluido el buscador de calles (flechas, Enter y Escape).
- Botones para agrandar la letra y alternar el contraste; modo oscuro automático.
- Resumen de errores con enlaces directos a cada pregunta.
- Sin límite de tiempo.
- Diseño adaptable a celulares.

## Protección de datos

El relevamiento **no es anónimo**: es nominativo y se hace con el consentimiento de la persona o de su representante, conforme a la **Ley 25.326** de Protección de los Datos Personales y a la **Ordenanza 162/25**.

Las respuestas se separan en **dos capas**, vinculadas por un identificador interno:

| Capa | Contenido | Uso |
|---|---|---|
| **Registro** | Nombre, documento, domicilio, teléfono, correo y datos de quien completa | Evitar duplicados y hacer la gestión individual. Acceso restringido. |
| **Estadística** | Todas las demás respuestas, sin identificadores (el barrio es el único dato territorial) | Indicadores y tablero, con publicación solo agregada y un corte mínimo de 5 casos. |

En esta versión **el formulario no envía ni guarda datos en ningún servidor**: al finalizar, las respuestas se descargan como un archivo JSON con las dos capas separadas.

## Uso

1. Abrir `index.html` en un navegador, o publicarlo con GitHub Pages (el formulario está en `index.html` para que Pages lo muestre como página principal).
2. Para identificar el canal de difusión sin preguntarlo (P2), agregar el parámetro `canal` al enlace o al código QR de cada canal:

   ```
   https://<usuario>.github.io/<repositorio>/?canal=centro-de-salud
   ```

### Configuración

Los listados editables están agrupados en el objeto `LISTAS`, al comienzo del código:

| Clave | Contenido |
|---|---|
| `canasta` | Valor y fuente de la Canasta Básica Total usada en P16 |
| `barrios` | Listado oficial de barrios (P6) |
| `calles` | Listado de calles para el buscador (P5) |
| `canalDerechos` | Canal para ejercer los derechos de acceso, rectificación y supresión |
| `inclusion` | Contactos del Departamento de Inclusión |

Los programas municipales y sus contactos están en `PROGRAMAS`.

### Formato de salida

```json
{
  "meta": { "codigo": "RSD-XXXX-XXXX", "fecha_hora": "…", "version_formulario": "v3" },
  "registro": { "id_interno": "…", "domicilio": "…", "personas": [ … ] },
  "estadistica": { "id_interno": "…", "hogar": { … }, "personas": [ … ] }
}
```

Los nombres de las variables siguen el diccionario de variables del Anexo III. Para cada persona se agregan variables calculadas: `edad`, `criterio_inclusion`, `vigencia_cud`, `tipo_disc_multiple` y `apoyo_recibe`.

## Estructura del formulario

| Parte | Bloques |
|---|---|
| **Inicio** | Bienvenida, ayuda e información, consentimiento informado (P1) |
| **A · Una vez por hogar** | 1 · Canal de llegada y quién completa · 2 · Identificación del hogar · 3 · Vivienda y condiciones de vida |
| **B · Por cada persona** | 4 · Datos de la persona · 5 · Dificultades en actividades cotidianas · 6 · Autopercepción y CUD · 7 · Apoyos y cuidados · 8 · Salud · 9 · Educación · 10 · Trabajo y protección social · 11 · Accesibilidad, barreras y movilidad · 12 · Participación, programas municipales y discriminación |
| **C · Cierre** | 13 · Comentario final y constancia |

## Pendientes

- [ ] Conectar el formulario a un almacenamiento cifrado y habilitar el envío de la constancia por correo.
- [ ] Cargar el texto literal del módulo infantil de UNICEF y el Grupo de Washington, una vez obtenida la autorización de reproducción. Hasta entonces, los bloques 5B y 5C muestran solo un resumen.
- [ ] Completar los datos marcados como `[COMPLETAR]`: código, vigencia, fechas de recepción y canal de derechos.
- [ ] Definir la región de referencia de la canasta y su ajuste por tamaño del hogar.
- [ ] Validar el texto del consentimiento con el área de Legales.
- [ ] Realizar la prueba piloto con personas en distintas situaciones de discapacidad.

## Marco normativo

- Ley Nacional 25.326 de Protección de los Datos Personales
- Ley 26.378: Convención sobre los Derechos de las Personas con Discapacidad
- Ordenanza 1047/18 y su modificatoria 458/26
- Ordenanza 162/25

---

*Documento interno de trabajo. Prueba piloto preliminar y confidencial: no difundir.*
