# Adopción, feedback y cuantificación de la arquitectura agentic

Guía estructurada para impulsar el uso de la arquitectura en la empresa, obtener feedback útil más allá de la experiencia propia y cuantificar su impacto de forma práctica.

---

## 1. Cómo impulsar la adopción

Objetivo: que más personas usen la arquitectura de forma real, no solo asistan a cursos.

### 1.1 Piloto acotado

- Elegir **un equipo o 2–3 personas** que se comprometan a usarla en un flujo real (un sprint, un epic).
- Condición: feedback breve al final (qué costó, qué ayudó).
- Genera casos concretos y evidencia para el resto de la organización.

### 1.2 Reducir fricción al primer uso

- Definir un **primer paso muy claro**: por ejemplo, “para tu próximo ticket: 1) abre un chat con @planning, 2) pega el ticket, 3) cuando tengas el plan, usa /blueprint”.
- Ofrecer un **quickstart** o **ejemplo ya rellenado** (plan, brief, issue) que se pueda copiar.
- Priorizar “siguiente paso concreto” sobre más teoría.

### 1.3 Mostrar resultados con datos

- Aunque sea de forma ligera: “en el piloto cerramos N tickets, M PRs, con Z iteraciones de review”.
- O: “antes tardaba N días en tener un plan listo, ahora 1 sesión”.
- Ayuda a vender la adopción y a que dirección vea mejora, no solo “otro curso”.

### 1.4 Integrar en rituales existentes

- En **refinement** o **planning**: que el DoR incluya “plan en .plan.md” o “brief con /brief”.
- En **retro**: una pregunta tipo “¿usaste la arquitectura (modos/comandos) esta iteración?”.
- Así la arquitectura forma parte del flujo oficial, no queda como algo opcional y aislado.

### 1.5 Punto de contacto claro

- Definir dónde resolver dudas o bloqueos: una persona (ej. tú) o un canal concreto.
- Objetivo: que sea obvio a quién preguntar y que las respuestas lleguen rápido.
- Si la gente no sabe por dónde empezar o se atasca sin respuesta, la adopción se frena.

---

## 2. Cómo obtener feedback (más allá de tu experiencia)

Objetivo: que la mejora de la arquitectura se base en señal real de quienes la usan o la prueban.

### 2.1 Feedback estructurado después del piloto (o de cada curso práctico)

- Formato corto: **3–5 preguntas**, por ejemplo:
  - ¿Qué te costó más?
  - ¿Qué quitarías o simplificarías?
  - ¿Qué echaste en falta?
  - Del 1 al 5, ¿lo usarías en tu próximo ticket?
- Herramienta: formulario breve o doc compartido; no hace falta nada complejo.
- Importante: **cerrar el ciclo** — comunicar “con tu feedback cambiamos X” en el siguiente curso o en un canal interno.

### 2.2 Office hours o “clínica de arquitectura”

- **Una hora fija** a la semana o quincenal donde quien use (o pruebe) la arquitectura pueda llevar un caso real.
- Ejemplos: “no sé cómo encajar este ticket”, “el plan me quedó muy largo”, “no entiendo cuándo usar @dev vs @planning”.
- Tú resuelves en vivo y **anotas** patrones: qué se repite, qué confunde, qué falta en documentación o en modos.
- Es feedback muy valioso sin depender de que la gente escriba informes.

### 2.3 Observar uso real

- Cuando haya adopción (piloto u otros): acordar **revisar juntos 1–2 planes y 1–2 flujos** (plan → dev → PR).
- No hace falta automatización: ver cómo lo usan y dónde se atascan ya da input para mejorar contratos, modos y comandos.

### 2.4 Changelog “qué cambió por tu feedback”

- Un doc o sección (ej. en la propuesta) tipo: “v3.1: simplificamos X porque en el piloto vimos que…”.
- La gente ve que su feedback tiene impacto; tú mantienes un historial de por qué la arquitectura evoluciona.
- Refuerza adopción y obliga a traducir feedback en cambios concretos.

---

## 3. Cuantificación de la arquitectura (seis pilares)

Marco para medir si la arquitectura aporta valor. Las métricas se pueden recoger de forma manual o ligera (p. ej. una vez por sprint o al cierre del piloto). Escala común: **1–10** por pilar (guía orientativa).

### 3.1 Throughput útil (entrega de valor)

**Qué mide:** cuánto trabajo “real” sale por unidad de tiempo.

**Qué recoger:**
- PRs mergeados por semana que cumplen DoD.
- Tickets cerrados por semana (opcional: ponderar por tamaño — story points o T-shirt).
- % de PRs revertidos o que generan hotfix (para no inflar con entregas de baja calidad).

**Escala 1–10 (guía):**
- 1–3: entregas esporádicas, sin ritmo.
- 4–6: ritmo estable pero con atasques.
- 7–10: cadencia consistente y predecible.

---

### 3.2 Lead time end-to-end (idea → producción)

**Qué mide:** eficiencia del flujo completo, no solo “codificar rápido”.

**Qué recoger:**
- Tiempo desde “ticket listo” hasta “deploy” (mediana o P80).
- Opcional: desglose por etapas (plan → implementación → review → QA → release).

**Escala 1–10 (guía):**
- 1–3: plazos de semanas con mucha varianza.
- 4–6: plazos de días/semana con varianza moderada.
- 7–10: plazos en días con P80 bajo y estable.

---

### 3.3 Calidad y retrabajo (defect escape rate)

**Qué mide:** si la arquitectura y el agente reducen bugs o solo los mueven de sitio.

**Qué recoger:**
- Bugs en producción por PR o por release.
- % de PRs que requieren “follow-up fix” en los 7 días siguientes.
- Reverts o hotfixes por mes.

**Escala 1–10 (guía):**
- 1–3: muchos escapes y correcciones reactivas.
- 4–6: calidad aceptable con picos.
- 7–10: baja tasa de escapes, correcciones poco frecuentes.

---

### 3.4 Coherencia de decisiones (consistencia / drift)

**Qué mide:** si la arquitectura mantiene estándares o se va desdibujando con el tiempo.

**Qué recoger:**
- % de PRs que cumplen checklist arquitectónico (linters, convenciones, ADRs).
- Cantidad de comentarios de “style/architecture” por PR (objetivo: que baje).
- Violaciones detectadas por tooling (lint, análisis estático).

**Escala 1–10 (guía):**
- 1–3: cada PR se siente distinto.
- 4–6: hay estándares pero se rompen a menudo.
- 7–10: alta consistencia y pocos recordatorios manuales.

---

### 3.5 Autonomía real (carga human-in-the-loop)

**Qué mide:** cuánto depende del humano frente a cuánto se “auto-sirve” el flujo.

**Qué recoger:**
- Tiempo humano por PR (planificación + correcciones + guía).
- Número de iteraciones de ida y vuelta con el agente por tarea.
- % de tareas donde el flujo “plan → PR listo” se completa con pocas intervenciones (definir N, ej. ≤ 3).

**Escala 1–10 (guía):**
- 1–3: el humano dicta todo.
- 4–6: el agente avanza pero necesita mucha guía.
- 7–10: el humano solo valida y decide.

---

### 3.6 Coste y eficiencia (cost efficiency)

**Qué mide:** si la ganancia de velocidad compensa el coste (tiempo, herramientas, tokens si aplica).

**Qué recoger:**
- Coste por ticket o por PR (si se trackea).
- Tiempo total de ejecución del agente por tarea (si está disponible).
- Coste por “merge exitoso” (incluyendo intentos fallidos o reintentos), si aplica.

**Escala 1–10 (guía):**
- 1–3: caro y poco predecible.
- 4–6: coste razonable con desperdicio.
- 7–10: coste estable y desperdicio bajo.

---

## Uso recomendado del marco de cuantificación

- **Piloto:** al cierre, rellenar los seis pilares con los datos que se tengan (aunque sea aproximado). Sirve para comunicar resultados y para decidir qué mejorar.
- **Seguimiento:** cada sprint o cada 2–4 semanas, revisar al menos 2–3 pilares (p. ej. throughput, calidad, autonomía) para detectar tendencias sin sobrecarga.
- **Retro y mejora:** usar los pilares como guía en la retro (“¿subimos o bajamos en coherencia? ¿en autonomía?”) y conectar con el changelog de feedback (sección 2.4).
