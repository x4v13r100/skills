# Plantillas de Spec por Tipo de Trabajo

Regla de oro: **nunca se salta la spec**, sin importar el tipo de trabajo.
Solo cambia cuál plantilla usar y qué tan extensa es.

Regla de título (obligatoria, sin excepción): el título (`#`) de todo
archivo de spec SIEMPRE es un comando `SDD-<tipo>: <pedido>`, nunca un
nombre inventado — sin importar si el usuario escribió el prefijo
literal o no.
- Si el usuario ya escribió el prefijo (`SDD-agregar:`, `SDD-bug:`, etc.),
  el título es ese mensaje tal cual, sin editar.
- Si el usuario pidió la spec de otra forma (sin el prefijo, en varios
  mensajes, o pidiendo "haz una spec de..."), quien arma la spec
  **construye** el título eligiendo el prefijo correcto según el tipo de
  trabajo (tabla de abajo) y resumiendo el pedido real del usuario en una
  sola línea — nunca se deja el placeholder ni se usa un nombre genérico
  como título.
- El título sirve como comando exacto para re-disparar/retomar esa spec
  después, así que debe quedar completo y accionable, no un fragmento.

Ej. con prefijo literal: `# SDD-agregar: quiero agregar un endpoint que
permita marcar varias tareas como completadas a la vez`.
Ej. construido (el usuario no usó el prefijo): `# SDD-agregar: la
asignación de finca ya no se hace con las fincas que tenga el usuario,
sino que ahora habrá una nueva propiedad en el body que será finca_ids,
un array de donde se toman los finca_id`.

Cómo identificar cuál usar, por el prefijo que escribes en el chat:

| Prefijo | Cuándo usarlo |
|---|---|
| `SDD-agregar:` | Funcionalidad nueva |
| `SDD-bug:` | Corregir un bug existente |
| `SDD-refactor:` | Reorganizar código sin cambiar comportamiento |
| `SDD-migracion:` | Cambio de esquema o migración de datos |
| `SDD-optimizacion:` | Mejorar rendimiento sin cambiar comportamiento |
| `SDD-tests:` | Agregar cobertura a código que no tiene tests |
| `SDD-investigacion:` | Spike/investigación antes de decidir un enfoque |

---

## 1. Feature nueva

Guarda como `docs/specs/SPEC-XXX.md`

```markdown
# [Comando completo: "SDD-agregar: <texto tal cual lo escribió el usuario>"]

## Problema
[¿Qué necesidad real resuelve esto? 1-3 líneas, sin tecnología todavía.]

## Objetivo
[Una frase concreta y verificable.]

## Alcance
- [Qué SÍ incluye esta versión]

## Fuera de alcance
- [Qué NO incluye]

## Requisitos funcionales
1. [Cada requisito numerado]

## Inputs / Outputs
- Entrada: [formato exacto]
- Salida: [formato exacto]

## Manejo de errores
- [Caso de error] → [respuesta esperada]

## Seguridad
- [Qué validar, qué NO exponer]

## Acceptance Criteria
- [ ] [Cada criterio debe poder convertirse directamente en un test]
```

---

## 2. Bugfix

Guarda como `docs/specs/SPEC-BUGFIX-XXX.md`

```markdown
# [Comando completo: "SDD-bug: <texto tal cual lo escribió el usuario>"]

## Comportamiento actual (el bug)
[Qué está pasando que no debería. Input exacto → output incorrecto observado.]

## Comportamiento esperado
[Qué debería pasar con ese mismo input.]

## Causa raíz (si ya se conoce)
[Qué archivo/función está fallando y por qué. Si no se sabe aún, dejar
"por investigar" — investigarlo es parte del plan.]

## Alcance del fix
- [Qué se va a tocar]

## Fuera de alcance
- [Qué NO se va a tocar aunque se vea relacionado — evita que el fix
  se convierta en refactor improvisado]

## Test de regresión
- [ ] Existe un test que reproduce el bug ANTES del fix (debe fallar)
- [ ] Ese mismo test pasa DESPUÉS del fix
- [ ] Los tests existentes que ya pasaban siguen pasando

## Acceptance Criteria
- [ ] [Input específico] → [output correcto esperado]
- [ ] No se modificó código fuera del alcance declarado
```

---

## 3. Refactor

Guarda como `docs/specs/SPEC-REFACTOR-XXX.md`

```markdown
# [Comando completo: "SDD-refactor: <texto tal cual lo escribió el usuario>"]

## Motivo
[Por qué hace falta, no "porque sí". Ej: código duplicado, función que
mezcla responsabilidades, archivo demasiado largo, etc.]

## Comportamiento actual (debe permanecer IDÉNTICO)
[Inputs/outputs de referencia que no deben cambiar.]

## Qué se va a cambiar
- [Estructura interna, nombres, organización de archivos, etc.]

## Qué NO se va a cambiar
- [El comportamiento externo/API pública, salvo que se declare
  explícitamente lo contrario]

## Riesgo
- [Qué tan cubierto está esto por tests existentes ANTES de tocar nada.
  Si no hay cobertura, el primer paso del plan es escribir tests de
  caracterización antes de refactorizar.]

## Acceptance Criteria
- [ ] Todos los tests que pasaban ANTES siguen pasando DESPUÉS, sin modificarlos
- [ ] El comportamiento observable es idéntico antes y después
- [ ] Ponytail Review confirma reducción de complejidad/duplicación
```

---

## 4. Migración de datos/esquema

Guarda como `docs/specs/SPEC-MIGRACION-XXX.md`

```markdown
# [Comando completo: "SDD-migracion: <texto tal cual lo escribió el usuario>"]

## Motivo
[Por qué hace falta el cambio de esquema/datos.]

## Estado actual
[Estructura/datos tal como están ahora — describe con precisión.]

## Estado deseado
[Estructura/datos después de la migración.]

## Estrategia de migración
[Pasos concretos: qué se transforma, en qué orden.]

## Plan de rollback
[Cómo revertir si algo sale mal a mitad de camino — obligatorio, no opcional.]

## Riesgo de pérdida de datos
[Qué podría perderse o corromperse, y cómo se mitiga (backup previo,
ejecución en transacción, etc.)]

## Validación
- [ ] Conteo de registros antes == después (o la diferencia esperada, documentada)
- [ ] Muestra de registros verificada manualmente
- [ ] Se ejecutó primero en un entorno de prueba/copia, no en producción directo

## Fuera de alcance
- [Qué no se toca en esta migración]

## Acceptance Criteria
- [ ] Los datos migrados cumplen el estado deseado descrito arriba
- [ ] No se perdió ningún registro que no debía perderse
- [ ] El rollback fue probado y funciona
```

---

## 5. Optimización de rendimiento

Guarda como `docs/specs/SPEC-OPTIMIZACION-XXX.md`

```markdown
# [Comando completo: "SDD-optimizacion: <texto tal cual lo escribió el usuario>"]

## Problema de rendimiento
[Medición actual concreta: tiempo, cantidad de queries, uso de memoria,
etc. No "es lento" — un número.]

## Objetivo medible
[Meta concreta: "de 8s a menos de 1s", "de 200 queries a menos de 10".]

## Alcance
- [Qué parte del sistema se toca]

## Fuera de alcance
- [Qué no se toca, aunque también sea lento]

## Método de medición
[Cómo se va a medir el "antes" y el "después" de forma reproducible.]

## Riesgo de regresión funcional
[La optimización no debe cambiar el resultado, solo la velocidad —
cómo se va a confirmar eso.]

## Acceptance Criteria
- [ ] La métrica objetivo se cumple (número concreto)
- [ ] Los tests funcionales existentes siguen pasando sin modificar
- [ ] El resultado (no solo la velocidad) es idéntico antes y después
```

---

## 6. Agregar tests a código legacy

Guarda como `docs/specs/SPEC-TESTS-XXX.md`

```markdown
# [Comando completo: "SDD-tests: <texto tal cual lo escribió el usuario>"]

## Código sin cobertura
[Qué módulo o función no tiene tests actualmente.]

## Motivo
[Por qué es riesgoso no tener tests aquí — ej: "se toca seguido y no
hay forma de saber si algo se rompió".]

## Comportamiento actual a documentar
[Qué hace hoy el código, tal cual es (aunque tenga bugs conocidos —
esos se documentan como "comportamiento actual", no se corrigen aquí).]

## Alcance
- [Qué funciones/casos se van a cubrir]

## Fuera de alcance
- [Cambios al código de producción — NO se tocan en este trabajo.
  Si se encuentra un bug, se anota para un SDD-bug aparte.]

## Acceptance Criteria
- [ ] Cada función listada en el alcance tiene al menos un test
- [ ] Los tests documentan el comportamiento actual (no el ideal)
- [ ] Los tests pasan contra el código sin modificar
- [ ] Se anotaron los bugs encontrados (si los hay) como hallazgos,
      sin corregirlos en este trabajo
```

---

## 7. Investigación / Spike técnico

Guarda como `docs/specs/SPEC-INVESTIGACION-XXX.md`

```markdown
# [Comando completo: "SDD-investigacion: <texto tal cual lo escribió el usuario>"]

## Pregunta
[Qué decisión hay que tomar, formulada como pregunta concreta.]

## Contexto
[Por qué hace falta decidir esto ahora.]

## Opciones a comparar
1. [Opción A]
2. [Opción B]

## Criterio de comparación
[Con qué se va a medir cada opción: rendimiento, simplicidad, costo,
tiempo de desarrollo, madurez de la herramienta, etc.]

## Fuera de alcance
- No se implementa la solución final en este trabajo, solo se investiga
  y se recomienda

## Timebox
[Tiempo máximo dedicado a esta investigación antes de decidir con lo
que se tenga.]

## Entregable
Documento en `docs/investigaciones/` con: hallazgos por opción,
comparación según el criterio, y recomendación final.

## Acceptance Criteria
- [ ] Cada opción fue evaluada contra el mismo criterio
- [ ] Hay una recomendación clara, no solo una lista de pros/contras
- [ ] La recomendación es accionable (el siguiente paso es claro)
```

---

## Diferencias clave entre los 7 tipos

| | ¿Comportamiento nuevo? | ¿Test ANTES de tocar código? | ¿Cambia API pública? | Ponytail |
|---|---|---|---|---|
| **Feature** | Sí | No (junto con el código) | Sí, es lo esperado | `/ponytail-review` |
| **Bugfix** | No (corrige incorrecto) | Sí, debe fallar primero | No, salvo que sea parte del fix | `/ponytail-review` + `/ponytail-debt` |
| **Refactor** | No (preserva el actual) | Sí, si no hay cobertura previa | No, salvo declarado | `/ponytail-audit` → `/ponytail-review` → `/ponytail-gain` |
| **Migración** | No (mismos datos, otra forma) | Tests de integridad de datos | N/A (es esquema/datos) | `/ponytail-audit` → `/ponytail-review` |
| **Optimización** | No (mismo resultado, más rápido) | Benchmark antes/después | No | `/ponytail-review` + `/ponytail-gain` |
| **Tests** | No (solo documenta el actual) | Es el propio entregable | No | `/ponytail-review` |
| **Investigación** | N/A (no hay código productivo) | No aplica | N/A | No aplica |

---

## Paso a paso: cómo usarlo con la skill instalada

### 1. Verifica que Claude Code la detecta

Abre una sesión nueva de `claude` en cualquier carpeta y pregunta:

```
¿Qué skills tienes disponibles?
```

Debe aparecer `sdd-flow` en la lista. Si no aparece, revisa que el
archivo esté exactamente en:
```
C:\Users\j4v13\.claude\skills\sdd-flow\SKILL.md
```

### 2. Ve a la carpeta de tu proyecto

```powershell
cd ruta\a\tu\proyecto
claude
```

### 3. Dispara el flujo con el prefijo correspondiente (obligatorio)

La skill NO se activa por palabras sueltas — necesita el prefijo
exacto al inicio del mensaje:

```
SDD-agregar: quiero agregar un endpoint que permita marcar varias
tareas como completadas a la vez.
```

```
SDD-bug: cuando envío una fecha en formato DD/MM/YYYY el sistema la
guarda mal. Debería aceptar solo YYYY-MM-DD y rechazar el resto.
```

```
SDD-refactor: la función procesar_pedido() tiene 150 líneas y mezcla
validación, cálculo de precio y guardado en base de datos. Quiero
simplificarla sin cambiar cómo funciona hacia afuera.
```

```
SDD-migracion: necesito convertir los INSERT de conceptos_tipo_novedads
a UPDATE, preservando los IDs existentes.
```

```
SDD-optimizacion: el endpoint GET /reportes tarda 8 segundos con
10,000 registros, necesito bajarlo a menos de 1 segundo.
```

```
SDD-tests: el módulo calcular_nomina.py no tiene ningún test y es
crítico, necesito cobertura antes de tocarlo.
```

```
SDD-investigacion: necesito decidir si usar LangGraph o hacer tool
calling manual para el agente SQL, antes de comprometerme a implementar.
```

Si escribes lo mismo sin el prefijo (ej: "arregla este error"), Claude
Code responde normal — no dispara spec, ni plan, ni las etapas.

### 4. Qué esperar después de tu mensaje

Si la skill está bien enganchada, Claude Code **no debería escribir
código de inmediato**. Debería:

1. Confirmar o crear la spec correspondiente al tipo
2. Definir la arquitectura o el enfoque (aunque sea una línea)
3. Proponerte un plan
4. **Esperar tu aprobación explícita** antes de tocar código

**Señal de alerta:** si empieza a programar directo sin mostrarte el
plan primero, la skill no se activó — revisa el paso 1.

### 5. Cómo aprobar y cerrar

```
Apruebo el plan, implementa.
```

Y al final:
```
Corre los tests, revisa con ponytail, genera la documentación y el
cierre.
```

Esto dispara las etapas de verificación, Ponytail (con el/los comando(s)
correcto(s) para ese tipo de trabajo), documentación y checkpoint, sin
que tengas que pedir cada una por separado.
