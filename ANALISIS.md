# Análisis de la práctica GitHub Actions - StockGuard

## Preguntas de análisis

**1. Ejecución exitosa y fallida**
Las ejecuciones #1 (commit b3ee87e) y #2 (commit 64117bc) fallaron en el step
"Ejecutar pruebas" por un error de import. La ejecución #6 (commit 3446c3b)
fue exitosa tras agregar PYTHONPATH a ese mismo step.

**2. Por qué local no reemplaza CI**
Localmente pytest resolvía el import de `src` por el contexto del directorio
de trabajo. En el runner limpio de GitHub Actions, sin PYTHONPATH explícito,
`src` no era encontrado.

**3. Riesgo de mergear sin esperar checks**
Se podría integrar código roto a main sin detectarlo hasta que otro miembro
del equipo lo descubra manualmente.

**4. Línea esencial del error**
`ModuleNotFoundError: No module named 'src'`
Esto permitió descartar errores de lógica y apuntar a un problema de
configuración del entorno de ejecución.

**5. Diferencia entre uses y run**
`uses` ejecuta una acción reutilizable y versionada; `run` ejecuta comandos
de shell directamente en el runner.

**6. ${{ github.actor }} en run-name**
Muestra quién disparó la ejecución, dando trazabilidad en un proyecto de equipo.

**7. CI pasa pero Docker falla local**
No significa que Actions no sirva: solo verifica lo que se configuró
(sintaxis y pruebas unitarias), no la integración completa del sistema.

**8. Tres checks propuestos para RapidGo**
- Validar sintaxis con compileall
- Pruebas unitarias de la regla de negocio propia
- Validación de archivos de configuración

**9. Cuál implementar primero**
Las pruebas de la regla de negocio, por ser el corazón funcional del sistema.

**10. Prueba demasiado costosa para cada push**
Levantar todo el stack Kafka + Spark + Airflow; se validaría manualmente o
en una ejecución programada, no en cada commit.

## Evidencia de ejecuciones
- Run #1 (b3ee87e): FALLÓ - https://github.com/JsantiagoM21/github-actions-stockguard/actions/runs/40911848743
- Run #2 (64117bc): FALLÓ - https://github.com/JsantiagoM21/github-actions-stockguard/actions/runs/40911875891
- Run #6 (3446c3b): PASÓ - https://github.com/JsantiagoM21/github-actions-stockguard/actions/runs/41086942253
- Run #7 (PR #3): PASÓ - https://github.com/JsantiagoM21/github-actions-stockguard/actions/runs/41088783726
- Run #9 (manual, workflow_dispatch): PASÓ - https://github.com/JsantiagoM21/github-actions-stockguard/actions/runs/41090181537
