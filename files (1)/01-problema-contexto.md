# 01. Problema, contexto y necesidad

**Proyecto:** SGT-SIAAF — Sistema de Gestión del Trámite de Titulación integrado al SIAAF
**Asignatura:** Desarrollo Basado en Plataformas (5.º ciclo, Computación, UNL) — ACD1
**Responsable de este archivo:** Integrante 1 — Mark Gonzales *(confirmar asignación)*
**Estado:** Borrador revisado. Lo marcado como *por confirmar* proviene de lo explicado en clase y aún no está validado con el docente o la institución.

---

## 1. Nombre provisional del proyecto

**SGT-SIAAF — Sistema de Gestión del Trámite de Titulación integrado al SIAAF**

---

## 2. Descripción del problema

### 2.1 Situación actual (por confirmar)

Según lo entendido en clase, el trámite de titulación en la Universidad Nacional de Loja (UNL):

- **demora mucho**, y la demora es la falla principal;
- **pasa por varios departamentos** para que los documentos sean revisados y **legalizados**;
- no está claro si cada paso se hace de forma manual (papel, ventanilla, correo) o con apoyo de algún sistema.

> **Por confirmar:** qué departamentos intervienen exactamente, en qué orden, cuánto tarda cada paso y si el proceso es manual. Hasta tenerlo, no se afirma ningún medio específico (ventanilla, correo, hojas de cálculo). Esas preguntas están en `05-validacion-retroalimentacion.md` (Q-13).

### 2.2 Consecuencias del problema

Estas consecuencias se **derivan** de la situación anterior y deben verificarse:

- el estudiante no sabe en qué etapa está su trámite ni qué falta;
- los documentos pasan de un departamento a otro sin un registro único;
- no queda constancia clara de quién revisó, observó o aprobó cada paso;
- las observaciones y correcciones se comunican de manera informal;
- no se puede medir cuánto tiempo permanece el trámite en cada etapa, por lo que es difícil detectar dónde está el retraso.

---

## 3. Necesidad identificada

La necesidad (independiente de cualquier tecnología) es:

> **Reducir la incertidumbre y el tiempo perdido en el trámite de titulación**, haciendo visibles las etapas, los responsables, las observaciones y los tiempos, y dejando registro de cada acción.

Esta necesidad se concreta en el MVP como el **subproceso de solicitud de aptitud legal** (RRA-UNL, Art. 235), definido en `03-alcance-flujo-sdd.md`. La relación futura con sistemas institucionales como el SIAAF se considera una posibilidad, no un requisito del MVP.

| Necesidad | Posible respuesta (hipótesis) |
|---|---|
| El estudiante no sabe en qué etapa está su trámite | Consulta del estado actual de la solicitud |
| Los documentos se pierden o se duplican | Repositorio centralizado de documentos por solicitud |
| Las observaciones no quedan registradas | Historial de observaciones y correcciones |
| No se sabe quién debe revisar o aprobar | Flujo de estados con responsables por rol |
| No se sabe dónde se acumula la demora | Historial con fecha de cada cambio de estado, del cual se pueden calcular tiempos por etapa |
| La información no se reutiliza entre sistemas | Futura integración con el SIAAF (simulada en el MVP) |

---

## 4. Fuentes

- Explicación en clase sobre el proceso actual de titulación (por confirmar con el docente).
- Universidad Nacional de Loja. (2021). *Reglamento de Régimen Académico de la Universidad Nacional de Loja* (Art. 235).
- Universidad Nacional de Loja. (2026). *Banco de proyectos para ACD1* (Proyecto 6).
