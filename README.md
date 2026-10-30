# skills_sddflow

Respaldo de mis skills de Claude Code.

## Contenido

```
skills/
├── README.md
└── sdd-flow/
    ├── SKILL.md                  # La skill: el flujo SDD completo
    └── Plantillas-SDD-Flow.md    # Plantillas de spec por tipo de trabajo
```

## sdd-flow

Aplica Spec-Driven Development. Se activa **solo** si el mensaje empieza con uno de estos prefijos:

| Prefijo | Para qué |
|---|---|
| `SDD-agregar:` | Funcionalidad nueva |
| `SDD-bug:` | Corrección de un bug |
| `SDD-refactor:` | Refactor sin cambiar comportamiento |
| `SDD-migracion:` | Cambio de esquema o migración de datos |
| `SDD-optimizacion:` | Mejora de rendimiento sin cambiar comportamiento |
| `SDD-tests:` | Cobertura de tests para código sin tests |
| `SDD-investigacion:` | Spike técnico, sin comprometerse a implementar |

Palabras sueltas como "agregar" o "arreglar" sin el prefijo no activan el flujo.

### Etapas

| Etapa | Qué hace |
|---|---|
| 0. Continuidad | Revisa proyectos anteriores relacionados |
| 1. Spec | Lee o crea la spec según el tipo (usa `Plantillas-SDD-Flow.md`) |
| 2. Arquitectura | Define la arquitectura simple antes del plan |
| 3. Plan | Propone el plan, sin implementar |
| 4. Aprobación | Espera el visto bueno |
| 5. Implementación | Código, con guardrail de alcance |
| 6. Tests | Tests ≥ criterios de aceptación, con casos de error |
| 7. Verificación | Corre los tests hasta que pasen |
| **8. Validación de tests (sioma-tests)** | **Obligatoria.** Audita los tests contra el estándar de Sioma y los verifica por mutación |
| 9. Revisión | `/ponytail-review` y `/code-review` |
| 10. Documentación | Docs, Swagger, entrega al front y descripción del PR |
| 11. Checkpoint | `/ponytail-debt`, `.gitignore` y `docs/CIERRE-PROYECTO-XX.md` |
| Paso final | Pruebas manuales (`docs/PRUEBAS-MANUALES-XXX.md`), no bloquea el cierre |

`SDD-investigacion` no tiene tests, así que la Etapa 8 no le aplica.

### Etapa 8: sioma-tests

Usa la skill `sioma-tests` del repo `Sioma-app/sioma-skills` (carpeta `sioma-tests/`). Revisa nombres, Arrange/Act/Assert, cobertura positivo/negativo/frontera/destructivo y antipatrones, y comprueba por mutación que los tests fallan cuando el código se rompe. Va antes de las revisiones para que `/code-review` y `/ponytail-review` vean los tests ya validados. No se sustituye por juicio manual: si la skill no está instalada, se lee desde el repo.

## Instalación

Copiar la carpeta de la skill a `~/.claude/skills/` y abrir una sesión nueva de Claude Code:

```bash
cp -r sdd-flow ~/.claude/skills/
```

La copia instalada y este respaldo deben mantenerse iguales; para comprobarlo:

```bash
cmp sdd-flow/SKILL.md ~/.claude/skills/sdd-flow/SKILL.md
```

## Historial de cambios

- **2026-10-27:** se agrega la Etapa 8 (validación de tests con `sioma-tests`). Las etapas de revisión, documentación y checkpoint pasan a ser la 9, 10 y 11.
