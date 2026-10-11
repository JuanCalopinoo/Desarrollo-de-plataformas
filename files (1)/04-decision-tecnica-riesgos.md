# 04. Alternativas tecnológicas, decisión preliminar y riesgos

**Proyecto:** SGT-SIAAF — Sistema de Gestión del Trámite de Titulación integrado al SIAAF
**Responsable de este archivo:** Integrante 4 — Jimmy Jhon *(confirmar asignación)*
**Entorno previsto:** Docker / Docker Compose (backend, base de datos; clientes web y móvil en unidades posteriores)
**Estado:** Decisión **preliminar**; se formaliza en el ADR de la Semana 3 del sílabo.

---

## 1. Comparación de alternativas de backend

Se evaluaron *Django + DRF*, *Spring Boot* y *FastAPI* con criterios comunes, a partir de las necesidades del MVP (`03`): API REST para web y móvil, roles (RBAC), estados y transiciones, carga de documentos, pruebas, OpenAPI y Docker. La valoración es **cualitativa del equipo**, apoyada en la literatura citada, y debe contrastarse con la documentación oficial de cada framework en el ADR.

| Criterio | Django + DRF | Spring Boot | FastAPI | Referencia |
| :--- | :--- | :--- | :--- | :--- |
| **Desarrollo** | **Alta.** ORM, migraciones y panel de administración incluidos. | **Media/baja.** Más configuración y código base. | **Alta/media.** Conciso, pero hay que integrar ORM y administración. | Pressman y Maxim (2019) |
| **Validación** | **Excelente.** Serializadores con validación declarativa. | **Excelente.** Bean Validation. | **Excelente.** Pydantic. | Bass et al. (2012) |
| **Autenticación y roles** | **Alta.** Usuarios, grupos y permisos en el núcleo; JWT con librería de terceros. | **Alta/compleja.** Spring Security, curva alta. | **Media.** Depende de librerías para RBAC. | Ferraiolo et al. (2001) |
| **Exposición de API** | **Alta.** ViewSets y routers; OpenAPI mediante librería complementaria. | **Alta.** Controladores REST. | **Excelente.** OpenAPI generado de forma nativa. | Fielding (2000) |
| **Pruebas** | **Alta.** Cliente de pruebas de API y BD aislada. | **Excelente.** JUnit, Mockito. | **Alta.** `pytest` y cliente de pruebas. | Pressman y Maxim (2019) |
| **Documentación** | **Excelente.** Documentación oficial madura. | **Excelente.** Extensa. | **Excelente.** Interactiva por defecto. | Pressman y Maxim (2019) |
| **Mantenimiento** | **Alta.** Estructura modular en apps. | **Alta.** Arquitectura en capas para equipos grandes. | **Media/alta.** Hay que definir la arquitectura. | Bass et al. (2012) |
| **Interoperabilidad (SIAAF futuro)** | **Alta.** Cliente HTTP para servicios externos. | **Excelente.** Integración empresarial. | **Alta.** E/S asíncrona. | Bass et al. (2012) |

### Ventajas y limitaciones

**Django + DRF.** *Ventajas:* incluye autenticación, ORM y panel administrativo, lo que reduce el código de infraestructura (Pressman y Maxim, 2019). *Limitaciones:* su modo estándar es síncrono (WSGI); si se requiriera mayor concurrencia se puede desplegar con ASGI. Para un MVP académico de baja concurrencia no es una limitación relevante.

**Spring Boot.** *Ventajas:* buen rendimiento concurrente y tipado estático, apropiado para sistemas empresariales (Bass et al., 2012). *Limitaciones:* más configuración inicial y código base, lo que pesa en la Unidad 1 de **5 semanas** (sílabo).

**FastAPI.** *Ventajas:* OpenAPI generado de forma nativa y buen rendimiento asíncrono. *Limitaciones:* no incluye ORM ni panel administrativo; ensamblar librerías externas aumenta el trabajo de integración y mantenimiento.

---

## 2. Decisión técnica preliminar

### Alternativa preliminar
**Django + Django REST Framework (DRF)**, sujeta a confirmación en el ADR de la Semana 3.

### Justificación
1. **Tiempo disponible.** El backend se desarrolla en **5 semanas** (Unidad 1 del sílabo) dentro de un ciclo de 16. Reutilizar autenticación, ORM y administración reduce el esfuerzo (Pressman y Maxim, 2019).
2. **Soporte web y móvil.** Una API REST desacoplada (Fielding, 2000) permite que la web (Unidad 2) y el móvil (Unidad 3) consuman el mismo backend, con autenticación por tokens JWT.
3. **Control de acceso por roles.** El MVP necesita distinguir **Estudiante, Secretario/a-Abogado/a, Decano/a y Administrador** (`03`), lo que corresponde al modelo RBAC (Ferraiolo et al., 2001).
4. **Despliegue reproducible.** Docker Compose con PostgreSQL permite que otro integrante levante el entorno solo con el README (criterio CE-10 de `05`).
5. **Flujo de estados.** El ORM y las validaciones de Django facilitan implementar una máquina de estados con historial de cambios.

### Alternativas descartadas (por ahora)
- **Spring Boot:** mayor configuración inicial frente al plazo de 5 semanas; se reconsideraría si el requirente exige JVM (Q-10 en `05`).
- **FastAPI:** habría que construir o integrar manualmente administración y permisos, tiempo que el cronograma no permite.

### Supuestos técnicos
- Docker Compose con **PostgreSQL**.
- Comunicación cliente-servidor sobre **HTTPS** con tokens **JWT** (se aplicarán las buenas prácticas de OWASP; ver riesgo 4).
- Datos de prueba ficticios (S-07 en `05`).

### Condiciones de cambio
- Pasar a Spring Boot si la normativa de TI o el requirente exigen JVM.
- Las colas de tareas (Celery/Redis) **no forman parte del MVP**; solo se considerarían como mejora futura si apareciera procesamiento pesado de archivos.

---

## 3. Matriz preliminar de riesgos técnicos

Estructurada según ISO/IEC 31000 (International Organization for Standardization [ISO], 2018). Probabilidad e impacto son estimaciones del equipo.

| Riesgo técnico | Prob. | Impacto | Mitigación | Referencia |
| :--- | :---: | :---: | :--- | :--- |
| **1. Proceso actual y estados oficiales no documentados** | Media | Alto | Aclarar con el requirente (Q-02, Q-13); modelar los estados como máquina de estados configurable en un solo módulo para poder cambiarlos. | ISO (2018) |
| **2. Incertidumbre sobre la integración con el SIAAF** | Alta | Medio | Capa de abstracción con servicios simulados (*mock*) para no detener el MVP. | Bass et al. (2012) |
| **3. Permisos mal definidos entre roles** | Media | Alto | Matriz formal rol × endpoint y pruebas automáticas de permisos. | Ferraiolo et al. (2001) |
| **4. Exposición de datos personales y expedientes** | Media | Crítico | Autenticación con JWT sobre HTTPS, mínimo privilegio, archivos servidos solo por rutas autenticadas y controles OWASP (control de acceso, validación de archivos). | OWASP (s. f.) |
| **5. Cambios de requisitos durante el desarrollo** | Alta | Medio | Desarrollo incremental con congelamiento de alcance (*scope freeze*) tras la aclaración con el docente y cambios registrados en el repositorio. | Pressman y Maxim (2019) |
| **6. Ausencia de pruebas o de documentación** | Media | Alto | Pruebas con `pytest` y documentación OpenAPI; matriz de trazabilidad requisito → prueba. | Pressman y Maxim (2019) |

---

## 4. Resultado esperado

Django + DRF se propone como base preliminar por ajustarse al plazo, al control por roles y al despliegue con contenedores. La decisión se contrastará con documentación oficial y con las respuestas del requirente antes de formalizarse en el ADR.

---

## 5. Referencias

Bass, L., Clements, P., & Kazman, R. (2012). *Software architecture in practice* (3.ª ed.). Addison-Wesley Professional.

Ferraiolo, D. F., Sandhu, R., Gavrila, S., Kuhn, D. R., & Chandramouli, R. (2001). Proposed NIST standard for role-based access control. *ACM Transactions on Information and System Security, 4*(3), 224–274.

Fielding, R. T. (2000). *Architectural styles and the design of network-based software architectures* [Tesis doctoral, University of California, Irvine].

International Organization for Standardization. (2018). *Risk management — Guidelines* (ISO 31000:2018).

OWASP. (s. f.). *OWASP Top 10* y *OWASP API Security Top 10*. https://owasp.org *(consultar la versión vigente y completar año y URL exacta)*

Pressman, R. S., & Maxim, B. R. (2019). *Software engineering: A practitioner's approach* (9.ª ed.). McGraw-Hill Education.

> Nota: la guía de la UNL pide referencias de máximo cinco años, salvo libros clásicos. Las obras de Pressman, Bass y Fielding se consideran de referencia; en el ADR conviene añadir la documentación oficial de Django, DRF y PostgreSQL (consultar la versión vigente).
