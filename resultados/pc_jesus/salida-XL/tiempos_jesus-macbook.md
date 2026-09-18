# Resultados de tiempos — jesus-macbook

- Equipo: **arm**, 8.0 GB RAM
- Núcleos: 8 físicos / 8 lógicos
- Sistema: Darwin 27.0.0 (arm64)
- Python 3.14.3 · pandas 3.0.5 · duckdb 1.5.5 · sqlite 3.50.4
- Límite por ejecución: 10 minutos (carga + query)
- Versiones medidas: xl

## 1. Tiempos con el código BASE (sin optimizar)

Tiempo total = carga del CSV + ejecución de la query, en segundos.

| version   |   ('duckdb', 'Q1') |   ('duckdb', 'Q2') |   ('duckdb', 'Q3') |   ('pandas', 'Q1') |   ('pandas', 'Q2') |   ('pandas', 'Q3') |   ('sqlite', 'Q1') |   ('sqlite', 'Q2') |   ('sqlite', 'Q3') |
|:----------|-------------------:|-------------------:|-------------------:|-------------------:|-------------------:|-------------------:|-------------------:|-------------------:|-------------------:|
| xl        |                9.3 |               8.42 |               9.59 |              59.45 |              57.13 |              78.79 |             128.42 |             191.47 |             152.75 |

## 2. Tiempos con el código OPTIMIZADO

Mismos ejes; ahora las herramientas leen el artefacto optimizado en vez del CSV.

| version   |   ('duckdb-opt', 'Q1') |   ('duckdb-opt', 'Q2') |   ('duckdb-opt', 'Q3') |   ('pandas-opt', 'Q1') |   ('pandas-opt', 'Q2') |   ('pandas-opt', 'Q3') |   ('sqlite-opt', 'Q1') |   ('sqlite-opt', 'Q2') |   ('sqlite-opt', 'Q3') |
|:----------|-----------------------:|-----------------------:|-----------------------:|-----------------------:|-----------------------:|-----------------------:|-----------------------:|-----------------------:|-----------------------:|
| xl        |                   0.32 |                   0.37 |                   2.01 |                    1.5 |                   0.52 |                   1.42 |                  66.47 |                 132.28 |                  92.77 |

## 3. Antes vs. después, lado a lado (segundos)

|               |   pandas antes |   pandas después |   pandas × |   duckdb antes |   duckdb después |   duckdb × |   sqlite antes |   sqlite después |   sqlite × |
|:--------------|---------------:|-----------------:|-----------:|---------------:|-----------------:|-----------:|---------------:|-----------------:|-----------:|
| ('xl', 'ETL') |         nan    |           nan    |      nan   |         nan    |           nan    |      nan   |         nan    |           nan    |      nan   |
| ('xl', 'Q1')  |          59.45 |             1.5  |       39.7 |           9.3  |             0.32 |       28.6 |         128.42 |            66.47 |        1.9 |
| ('xl', 'Q2')  |          57.13 |             0.52 |      109.7 |           8.42 |             0.37 |       22.5 |         191.47 |           132.28 |        1.4 |
| ('xl', 'Q3')  |          78.79 |             1.42 |       55.4 |           9.59 |             2.01 |        4.8 |         152.75 |            92.77 |        1.6 |

## 4. Resumen por versión y amortización del ETL

| version   | herramienta   |   antes_s |   despues_s |   mejora_x |   ahorro_s |   etl_s |   corridas_para_amortizar |
|:----------|:--------------|----------:|------------:|-----------:|-----------:|--------:|--------------------------:|
| xl        | pandas        |    195.37 |        3.44 |       56.8 |     191.93 |     6.3 |                      0.03 |
| xl        | duckdb        |     27.31 |        2.71 |       10.1 |      24.6  |     6.3 |                      0.26 |
| xl        | sqlite        |    472.63 |      291.52 |        1.6 |     181.12 |     6.3 |                      0.03 |

