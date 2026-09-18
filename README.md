# Proyecto Big Data G5 — Uso de Bicicletas (Londres)

Módulo: Ingeniería y Analítica de Grandes Cantidades de Datos.

Comparación de Pandas, DuckDB y SQLite sobre el histórico de viajes del sistema
público de bicicletas de Londres (hasta 38.2 millones de filas), en tres
computadoras distintas, con una fase de optimización (CSV → Parquet con
proyección y tipos reducidos) y una propuesta de pipeline ETL en AWS.

## Estructura

- `codigo/` — notebook con el harness de medición y las tres implementaciones
  (Pandas, DuckDB, SQLite) para las tres preguntas analíticas.
- `resultados/` — salidas crudas (tiempos, RAM, tablas y figuras) por equipo:
  `pc_jesus/`, `pc_martha/`, `pc_nahin/`.
- `reporte/proyecto-latex/` — fuente LaTeX del reporte final (`main.tex`) y el
  PDF compilado.

## Reporte

El reporte final, con metodología, resultados, análisis de optimización,
diseño del pipeline ETL y análisis de costos, está en
[`reporte/proyecto-latex/main.pdf`](reporte/proyecto-latex/main.pdf).

## Grupo 5

Cevallos Vinces Nahin Jussephe · Sanchez Guzman Annabella Noelia ·
Martha Maritza Duran Navarrete · Suarez Aspiazu Jesus David ·
Martin Pimentel Isabella Ivonne · Zurita Guerrero Angelo Saul ·
Munizaga Torres Juan Andres
