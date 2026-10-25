# Especificaciones SDD (`specs/`)

Esta carpeta almacena las especificaciones **Spec-Driven Development (Kiro-style)** para cada feature con `"sdd": true` en [`feature_list.json`](../feature_list.json).

## Estructura por feature

```text
specs/<feature-name>/
├── requirements.md   # Requisitos verificables en notación EARS estricta (R1, R2, ...)
├── design.md         # Diseño técnico, archivos a modificar, contratos y alternativas descartadas
└── tasks.md          # Checklist secuencial de tareas ejecutables ([ ] T1..Tn) con trazabilidad a R<n>
```

Consulta [`docs/specs.md`](../docs/specs.md) para conocer el proceso completo y las puertas de aprobación humana.
