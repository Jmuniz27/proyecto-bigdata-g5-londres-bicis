# Resultados de tiempos — jesus-macbook

- Equipo: **arm**, 8.0 GB RAM
- Núcleos: 8 físicos / 8 lógicos
- Sistema: Darwin 27.0.0 (arm64)
- Python 3.14.3 · pandas 3.0.5 · duckdb 1.5.5 · sqlite 3.50.4
- Límite por ejecución: 10 minutos (carga + query)
- Versiones medidas: m

## 1. Tiempos con el código BASE (sin optimizar)

Tiempo total = carga del CSV + ejecución de la query, en segundos.

| version   |   ('duckdb', 'Q1') |   ('duckdb', 'Q2') |   ('duckdb', 'Q3') |   ('pandas', 'Q1') |   ('pandas', 'Q2') |   ('pandas', 'Q3') |   ('sqlite', 'Q1') |   ('sqlite', 'Q2') |   ('sqlite', 'Q3') |
|:----------|-------------------:|-------------------:|-------------------:|-------------------:|-------------------:|-------------------:|-------------------:|-------------------:|-------------------:|
| m         |               1.67 |                1.4 |               1.96 |              11.66 |              10.93 |              11.96 |              31.81 |               41.3 |              35.09 |

## 2. Tiempos con el código OPTIMIZADO

Mismos ejes; ahora las herramientas leen el artefacto optimizado en vez del CSV.

| version   |   ('duckdb-opt', 'Q1') |   ('duckdb-opt', 'Q2') |   ('duckdb-opt', 'Q3') |   ('pandas-opt', 'Q1') |   ('pandas-opt', 'Q2') |   ('pandas-opt', 'Q3') |   ('sqlite-opt', 'Q1') |   ('sqlite-opt', 'Q2') |   ('sqlite-opt', 'Q3') |
|:----------|-----------------------:|-----------------------:|-----------------------:|-----------------------:|-----------------------:|-----------------------:|-----------------------:|-----------------------:|-----------------------:|
| m         |                   0.07 |                   0.08 |                    0.5 |                   0.34 |                   0.09 |                   0.29 |                  13.85 |                  27.09 |                   20.8 |

## 3. Antes vs. después, lado a lado (segundos)

|             |   pandas antes |   pandas después |   pandas × |   duckdb antes |   duckdb después |   duckdb × |   sqlite antes |   sqlite después |   sqlite × |
|:------------|---------------:|-----------------:|-----------:|---------------:|-----------------:|-----------:|---------------:|-----------------:|-----------:|
| ('m', 'Q1') |          11.66 |             0.34 |       34   |           1.67 |             0.07 |       24.1 |          31.81 |            13.85 |        2.3 |
| ('m', 'Q2') |          10.93 |             0.09 |      120.3 |           1.4  |             0.08 |       18.6 |          41.3  |            27.09 |        1.5 |
| ('m', 'Q3') |          11.96 |             0.29 |       41.2 |           1.96 |             0.5  |        3.9 |          35.09 |            20.8  |        1.7 |

## 4. Resumen por versión y amortización del ETL

| version   | herramienta   |   antes_s |   despues_s |   mejora_x |   ahorro_s |   etl_s |   corridas_para_amortizar |
|:----------|:--------------|----------:|------------:|-----------:|-----------:|--------:|--------------------------:|
| m         | pandas        |     34.54 |        0.72 |       47.7 |      33.82 |     nan |                       nan |
| m         | duckdb        |      5.03 |        0.64 |        7.8 |       4.39 |     nan |                       nan |
| m         | sqlite        |    108.2  |       61.74 |        1.8 |      46.45 |     nan |                       nan |

