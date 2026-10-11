# 05. Validación, criterios de éxito, aclaraciones y retroalimentación

**Proyecto:** SGT-SIAAF — Sistema de Gestión del Trámite de Titulación integrado al SIAAF
**Asignatura:** Desarrollo Basado en Plataformas (5.º ciclo, Computación, UNL) — ACD1
**Responsable:** Integrante 5 (responsable de validación)
**Estado:** Versión inicial. La retroalimentación real está **pendiente de la revisión correspondiente**.

> Este documento se apoya en el MVP propuesto en `03-alcance-flujo-sdd.md` (solicitud de aptitud legal previa a la sustentación, Art. 235 del *Reglamento de Régimen Académico de la UNL*, en adelante **RRA-UNL**). Si el equipo cambia el alcance, los criterios deben ajustarse. Ninguna funcionalidad listada aquí está implementada todavía.

---

## 1. Criterios preliminares de éxito

Cada criterio indica cómo se comprobará. Las metas numéricas son **propuestas del equipo** y pueden ajustarse.

| N.º | Criterio | Forma de comprobarlo |
|---|---|---|
| CE-01 | **Registro de solicitudes.** Una solicitud válida puede registrarse y recuperarse. | Prueba de API: crear solicitud y consultarla por su identificador devuelve los mismos datos. |
| CE-02 | **Validación de datos.** Los datos obligatorios faltantes generan errores controlados. | Prueba con campos vacíos o inválidos: respuesta con código 4xx y mensaje por campo, sin error 500. |
| CE-03 | **Gestión de documentos.** Se pueden adjuntar y consultar los cinco documentos requeridos; se rechazan formatos o tamaños no admitidos. | Prueba de carga con PDF válido, archivo no PDF y archivo mayor al límite definido. |
| CE-04 | **Gestión de observaciones.** Una revisión puede dejar observaciones asociadas a una solicitud, visibles para el estudiante. | Prueba: el revisor crea una observación y el estudiante la ve al consultar su solicitud. |
| CE-05 | **Transiciones de estado válidas.** Solo se permiten las transiciones definidas y por el rol autorizado. | Pruebas parametrizadas: transiciones permitidas pasan; transiciones inválidas devuelven error controlado. |
| CE-06 | **Control de acceso.** Las operaciones restringidas se deniegan a usuarios sin permisos; un estudiante no ve solicitudes ajenas. | Matriz rol × endpoint con pruebas automáticas (401/403 según el caso). |
| CE-07 | **Seguimiento.** El estado actual y los cambios relevantes (quién, cuándo, de qué estado a cuál) pueden consultarse. | Prueba: tras tres cambios de estado, el historial devuelve tres registros en orden. |
| CE-08 | **Documentación de la API.** El contrato OpenAPI está publicado y corresponde con los endpoints implementados. | Revisión de la documentación generada frente al catálogo de endpoints. |
| CE-09 | **Pruebas automatizadas.** Existe una suite que cubre los flujos principales y alternativos del backend. | Ejecución de la suite en el repositorio; cobertura mínima propuesta: **70 % en la lógica de negocio** *(a validar con el docente)*. |
| CE-10 | **Reproducibilidad.** Otro integrante puede configurar y ejecutar el proyecto siguiendo el README. | Un integrante ajeno al código levanta el entorno con Docker Compose usando solo el README y registra el resultado. |
| CE-11 | **Trazabilidad SDD.** Cada requisito se vincula con criterio de aceptación, endpoint y prueba. | Revisión de la matriz de trazabilidad: ningún requisito del MVP sin prueba asociada. |
| CE-12 | **Fecha límite informativa.** Al enviar la solicitud se calcula la fecha límite de 8 días laborables (RRA-UNL, Art. 235). | Prueba con fechas fijas, incluyendo fin de semana *(feriados: pendiente de aclarar)*. |

Estos criterios corresponden a lo que el sílabo valora en la Unidad 1: cumplimiento de requisitos, trazabilidad entre especificaciones, código y pruebas, calidad técnica y evidencias en el repositorio.

---

## 2. Supuestos y restricciones

### 2.1 Supuestos

| ID | Supuesto | Tipo | Estado |
|---|---|---|---|
| S-01 | El MVP cubre solo la etapa de **solicitud de aptitud legal** (Art. 235). | Alcance | Por validar con el docente |
| S-02 | Los roles del MVP son estudiante, Secretario/a-Abogado/a, Decano/a y administrador. | Actores | Por validar |
| S-03 | Los estados de la solicitud son los propuestos en `03` y **no son oficiales**. | Proceso | Por validar |
| S-04 | Los documentos se cargan en PDF de hasta 5 MB. | Técnico | Supuesto del equipo, no normativo |
| S-05 | No se dispone de API, credenciales ni documentación del SIAAF; la integración se simula. | Integración | Coincide con `02.md` § 5.3 |
| S-06 | El sistema funcionará con Docker Compose y PostgreSQL. | Técnico | Propuesto en `04` |
| S-07 | Los datos de prueba serán ficticios; no se usarán datos reales de estudiantes. | Datos | Decisión del equipo |
| S-08 | La notificación al estudiante se resuelve mostrando estado y observaciones, sin correo. | Alcance | Por validar |

### 2.2 Restricciones

| ID | Restricción | Origen |
|---|---|---|
| R-01 | El sistema **no sustituye** la declaratoria de aptitud ni las decisiones del Secretario/a-Abogado/a o del Decano/a; solo las registra. | RRA-UNL Art. 235; `02.md` § 5.2 |
| R-02 | Plazo: el backend se desarrolla en 5 semanas (Unidad 1); el sistema completo se cierra en la Semana 16. | Sílabo |
| R-03 | El desarrollo sigue Spec-Driven Development con GitHub Spec Kit (constitution, specify, clarify, plan, tasks). | Sílabo; Banco de proyectos |
| R-04 | Protección de datos personales y mínimo privilegio en acceso a expedientes. | Sílabo (seguridad, protección de datos); `04` riesgo 4 |
| R-05 | El uso de herramientas de IA debe ser crítico, verificable y **declarado**. | Sílabo, § 3.2 y 3.4 |
| R-06 | La integración real con el SIAAF queda fuera mientras no exista acceso autorizado. | Banco de proyectos; `02.md` |

---

## 3. Preguntas de aclaración

Se separan en dos grupos: las que **ya tienen respuesta en fuentes documentales** y las que **requieren confirmación del docente o de la institución**.

### 3.1 Respondidas con fuentes documentales (a verificar)

| N.º | Pregunta | Respuesta documental | Fuente |
|---|---|---|---|
| P-01 | ¿Qué requisitos y documentos acompañan la solicitud de aptitud legal? | Récord académico legalizado; certificado de prácticas y vinculación; certificado B1; certificado del director del trabajo; certificado de Tesorería de no adeudar. | RRA-UNL Art. 235 |
| P-02 | ¿Qué actores intervienen en esa etapa? | Aspirante, Secretario/a-Abogado/a (verifica e informa) y Decano/a de Facultad (declara la aptitud). | RRA-UNL Art. 235 |
| P-03 | ¿Cuál es el plazo para declarar la aptitud? | Ocho días laborables desde la presentación de la solicitud. | RRA-UNL Art. 235 |
| P-04 | ¿Qué ocurre si hay una obligación pendiente? | No se emite el informe de aptitud legal, se notifica al interesado y se informa al Decano/a y al Director/a de carrera. | RRA-UNL Art. 235 |
| P-05 | ¿Qué sigue a la aptitud legal? | El aspirante solicita al Director/a de carrera la designación de tribunal y la fecha de sustentación. | RRA-UNL Art. 236 |
| P-06 | ¿Qué información debe conservarse para trazabilidad? | Mínimo propuesto: quién, cuándo y qué cambió en cada transición, documentos adjuntos y observaciones. El reglamento no fija un formato. | Propuesta del equipo basada en Sílabo (trazabilidad) |
| P-07 | ¿Qué normas de presentación aplican al informe final de titulación? | Guía de la Dirección de Investigación (2022): A4, márgenes 2,5 cm, interlineado 1,5, APA 7, secciones del Art. 229. Aplica al informe final del proyecto, no al sistema. | Guía UNL (2022); RRA-UNL Art. 229 |

### 3.2 Requieren confirmación del docente o de la institución

| N.º | Pregunta | A quién | Por qué importa |
|---|---|---|---|
| Q-01 | ¿Es la **aptitud legal** la etapa que se quiere digitalizar primero, o se prefiere otra? | Docente | Define todo el alcance del MVP. |
| Q-02 | ¿Qué estados oficiales usa la Facultad/Carrera y cuáles son sus transiciones permitidas? | Institución (Secretaría-Abogacía) | Los estados de `03` son una propuesta. |
| Q-03 | El Art. 235 indica que el aspirante presenta los requisitos «que le correspondan» y el Secretario/a-Abogado/a adjunta los demás de oficio. ¿**Cuáles carga cada actor**? | Institución | Cambia permisos y validaciones. |
| Q-04 | ¿El Secretario/a-Abogado/a y el Decano/a son los únicos actores que revisan/aprueban en esta etapa? ¿Hay delegaciones? | Institución | Define el RBAC. |
| Q-05 | ¿Los ocho días laborables excluyen feriados y periodos de vacaciones? ¿Desde qué momento se cuentan? | Institución | Afecta al cálculo de la fecha límite. |
| Q-06 | ¿Existe documentación técnica **autorizada** para integrarse con el SIAAF (API, credenciales, entorno de pruebas)? ¿Qué datos serían consultables? | DTI / docente | Determina si hay integración real o solo simulada. |
| Q-07 | ¿Qué formato, tamaño y validez deben cumplir los documentos adjuntos (copia escaneada, firma electrónica)? | Institución | Afecta a validaciones de carga. |
| Q-08 | ¿Se requiere notificación por correo en esta etapa o basta con consultar el estado? | Docente | Afecta al alcance y a la infraestructura. |
| Q-09 | ¿Cuánto tiempo y bajo qué reglas se conservan los expedientes y qué datos personales pueden almacenarse? | Institución / DTI | Restricción legal y de seguridad. |
| Q-10 | ¿El requirente exige un framework o plataforma específica (por ejemplo, restricción a JVM)? | Docente / DTI | `04` propone Django + DRF y plantea Spring Boot como alternativa si hubiera tal restricción. |
| Q-11 | ¿La aplicación móvil debe cubrir solo consulta de estado o también carga de documentos? | Docente | Define el alcance de la Unidad 3. |
| Q-12 | ¿Se acepta que el MVP use datos ficticios en lugar de datos reales? | Docente | Condiciona pruebas y demostración. |

---

## 4. Puntos de coherencia detectados entre los documentos del equipo

Durante la revisión de los archivos `01`–`04` se detectaron diferencias que el equipo debe resolver antes de la entrega. **Son observaciones de la revisión interna, no comentarios del docente.**

| N.º | Observación | Archivos | Decisión pendiente |
|---|---|---|---|
| C-01 | `02.md` propone los roles Estudiante, Revisor, Responsable autorizado y Administrador; `04` menciona Estudiantes, Tutores, Lectores y Administradores. | `02`, `04` | Unificar con los roles de `03` (Estudiante, Secretario/a-Abogado/a, Decano/a, Administrador). |
| C-02 | `04` justifica la decisión por un plazo de «5 meses»; el sílabo organiza el backend en 5 **semanas** dentro de un ciclo de 16 semanas. | `04`, sílabo | Corregir el argumento de tiempo en `04`. |
| C-03 | `04` presenta la elección de Django + DRF como «decisión», mientras el sílabo (Semana 3) prevé el ADR formal después de la evaluación de alternativas. | `04`, sílabo | Mantenerla como decisión **preliminar** y formalizarla en el ADR. |
| C-04 | Las referencias de `04` (Pressman 2019, Bass 2012, Fielding 2000, Ferraiolo 2001) son anteriores a los cinco años que sugiere la guía de la UNL «salvo libros clásicos». | `04`, Guía UNL | Confirmar con el docente si se aceptan como obras de referencia y complementar con fuentes recientes (por ejemplo, documentación oficial y OWASP). |
| C-05 | `04` plantea Celery/Redis como posible incorporación; el MVP no tiene procesamiento pesado. | `04`, `03` | Dejarlo como mejora futura, no como parte del MVP. |
| C-06 | `01` y `02` hablan del trámite de titulación en general; `03` lo acota a la aptitud legal. | `01`, `02`, `03` | Ajustar la redacción de `01` y `02` al alcance acordado. |

---

## 5. Síntesis de la retroalimentación y ajustes

> **Estado actual: pendiente de la revisión correspondiente.** No se ha registrado retroalimentación real del docente ni de otros equipos. La tabla está preparada para completarse con los comentarios efectivamente recibidos; no se deben anotar ajustes que aún no existan.

| ID | Fuente del comentario | Observación recibida | Sección afectada | Ajuste realizado | Estado |
|---|---|---|---|---|---|
| F-01 | Docente | *(pendiente de registrar)* | | | Pendiente |
| F-02 | Docente | *(pendiente de registrar)* | | | Pendiente |
| F-03 | Revisión entre compañeros | *(pendiente de registrar)* | | | Pendiente |
| F-04 | Revisión entre compañeros | *(pendiente de registrar)* | | | Pendiente |
| F-05 | Revisión del equipo | Diferencias entre documentos (ver sección 4, C-01 a C-06) | `01`, `02`, `03`, `04` | Por decidir en reunión del equipo | Pendiente |

**Instrucciones de uso**

1. Registrar el comentario con las palabras recibidas y su fecha.
2. Indicar el archivo y la sección modificada cuando se aplique el cambio.
3. Marcar `Aplicado` solo cuando el cambio exista en el repositorio; si se difiere, marcar `Pendiente` y anotar la razón.
4. Si hay desacuerdo en el equipo, registrar la decisión y quién la tomó.

### Registro de decisiones pendientes

| ID | Decisión | Depende de | Estado |
|---|---|---|---|
| D-01 | Confirmar el subproceso del MVP | Q-01 | Pendiente |
| D-02 | Fijar la lista oficial de estados | Q-02 | Pendiente |
| D-03 | Definir quién carga cada documento | Q-03 | Pendiente |
| D-04 | Definir alcance del cliente móvil | Q-11 | Pendiente |
| D-05 | Confirmar si el stack preliminar (Django + DRF) se mantiene | Q-10; ADR de Semana 3 | Pendiente |

---

## 6. Conclusiones iniciales

1. El trámite de titulación completo es demasiado amplio para el ciclo. Delimitarlo a la **solicitud de aptitud legal** permite apoyarse en el RRA-UNL (documentos, actores y plazo) sin inventar reglas.
2. El valor del sistema está en el **seguimiento, las observaciones y la trazabilidad**, no en almacenar formularios; por eso los criterios de éxito priorizan estados, historial y control de acceso.
3. La mayor incertidumbre sigue siendo la **integración con el SIAAF**: no hay evidencia de API ni acceso autorizado, de modo que el MVP la tratará como una posibilidad futura simulada.
4. Los estados y roles propuestos son **hipótesis de trabajo**; deben confirmarse con el docente o la institución antes de convertirse en requisitos (Q-01 a Q-04).
5. Antes de entregar es necesario **unificar la terminología y los plazos** entre los archivos `01` a `04` (sección 4).
6. Los criterios de éxito son verificables mediante pruebas automatizadas y revisión de trazabilidad, lo que encaja con la forma de evaluación de la Unidad 1 del sílabo.

---

## 7. Referencias

Universidad Nacional de Loja. (2021). *Reglamento de Régimen Académico de la Universidad Nacional de Loja* (SE-No.01-ROCS-No-04-27-01-2021). Loja, Ecuador.

Universidad Nacional de Loja, Dirección de Investigación. (2022). *Guía para la escritura y presentación del informe del trabajo de integración curricular o de titulación*. Loja, Ecuador.

Universidad Nacional de Loja, Carrera de Computación. (2026). *Sílabo de la asignatura Desarrollo Basado en Plataformas* (período septiembre 2026–marzo 2027). Loja, Ecuador.

Universidad Nacional de Loja, Carrera de Computación. (2026). *Banco de proyectos para análisis en ACD1: Desarrollo Basado en Plataformas*. Loja, Ecuador.

Equipo SGT-SIAAF. (2026). *Documentos de trabajo ACD1: 01-problema-contexto, 02 (actores y backend), 03-alcance-flujo-sdd, 04-decision-tecnica-riesgos*.

Las referencias técnicas de `04` (Pressman & Maxim, 2019; Fielding, 2000; Ferraiolo et al., 2001; Bass et al., 2012; ISO 31000:2018) se mantienen en ese archivo; ver observación C-04.

---

## 8. Matriz de contribución del equipo

Los nombres y los porcentajes los completa el equipo. Se parte de la distribución de archivos de la ACD1; no se asigna ningún porcentaje por defecto.

| Integrante | Nombre | Archivo / sección principal | Aportes específicos | Verificado por el equipo |
|---|---|---|---|---|
| 1 | *(completar)* | `01-problema-contexto.md` *(confirmar asignación)* | *(completar)* | Pendiente |
| 2 | *(completar)* | `02.md` — actores y papel del backend *(confirmar)* | *(completar)* | Pendiente |
| 3 | *(completar)* | `03-alcance-flujo-sdd.md` | Alcance del MVP, flujo y relación SDD | Pendiente |
| 4 | *(completar)* | `04-decision-tecnica-riesgos.md` *(confirmar)* | *(completar)* | Pendiente |
| 5 | *(completar)* | `05-validacion-retroalimentacion.md` | Criterios, supuestos, preguntas, retroalimentación y conclusiones | Pendiente |

**Uso de herramientas de IA:** el sílabo exige declarar su uso. El equipo debe registrar aquí qué herramienta se usó, para qué y quién verificó el contenido.

| Archivo | Herramienta | Uso | Verificado por |
|---|---|---|---|
| `03`, `05` | Claude (borrador inicial a partir de los documentos del equipo y del RRA-UNL) | Redacción de borrador, revisión de coherencia | *(completar)* |
