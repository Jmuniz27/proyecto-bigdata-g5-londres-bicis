# Resultados de tiempos — jesus-macbook

- Equipo: **arm**, 8.0 GB RAM
- Núcleos: 8 físicos / 8 lógicos
- Sistema: Darwin 27.0.0 (arm64)
- Python 3.14.3 · pandas 3.0.5 · duckdb 1.5.5 · sqlite 3.50.4
- Límite por ejecución: 10 minutos (carga + query)
- Versiones medidas: l

## 1. Tiempos con el código BASE (sin optimizar)

Tiempo total = carga del CSV + ejecución de la query, en segundos.

| version   |   ('duckdb', 'Q1') |   ('duckdb', 'Q2') |   ('duckdb', 'Q3') |   ('pandas', 'Q1') |   ('pandas', 'Q2') |   ('pandas', 'Q3') |   ('sqlite', 'Q1') |   ('sqlite', 'Q2') |   ('sqlite', 'Q3') |
|:----------|-------------------:|-------------------:|-------------------:|-------------------:|-------------------:|-------------------:|-------------------:|-------------------:|-------------------:|
| l         |               3.37 |               2.83 |               5.34 |              25.85 |                 25 |              28.29 |              64.11 |              94.28 |              74.45 |

## 2. Tiempos con el código OPTIMIZADO

Mismos ejes; ahora las herramientas leen el artefacto optimizado en vez del CSV.

| version   |   ('duckdb-opt', 'Q1') |   ('duckdb-opt', 'Q2') |   ('duckdb-opt', 'Q3') |   ('pandas-opt', 'Q1') |   ('pandas-opt', 'Q2') |   ('pandas-opt', 'Q3') |   ('sqlite-opt', 'Q1') |   ('sqlite-opt', 'Q2') |   ('sqlite-opt', 'Q3') |
|:----------|-----------------------:|-----------------------:|-----------------------:|-----------------------:|-----------------------:|-----------------------:|-----------------------:|-----------------------:|-----------------------:|
| l         |                   0.19 |                   0.14 |                    0.7 |                   2.33 |                   0.23 |                    0.6 |                  28.07 |                  58.06 |                  43.32 |

## 3. Antes vs. después, lado a lado (segundos)

|              |   pandas antes |   pandas después |   pandas × |   duckdb antes |   duckdb después |   duckdb × |   sqlite antes |   sqlite después |   sqlite × |
|:-------------|---------------:|-----------------:|-----------:|---------------:|-----------------:|-----------:|---------------:|-----------------:|-----------:|
| ('l', 'ETL') |         nan    |           nan    |      nan   |         nan    |           nan    |      nan   |         nan    |           nan    |      nan   |
| ('l', 'Q1')  |          25.85 |             2.33 |       11.1 |           3.37 |             0.19 |       18.1 |          64.11 |            28.07 |        2.3 |
| ('l', 'Q2')  |          25    |             0.23 |      107.5 |           2.83 |             0.14 |       20.9 |          94.28 |            58.06 |        1.6 |
| ('l', 'Q3')  |          28.29 |             0.6  |       47.5 |           5.34 |             0.7  |        7.6 |          74.45 |            43.32 |        1.7 |

## 4. Resumen por versión y amortización del ETL

| version   | herramienta   |   antes_s |   despues_s |   mejora_x |   ahorro_s |   etl_s |   corridas_para_amortizar |
|:----------|:--------------|----------:|------------:|-----------:|-----------:|--------:|--------------------------:|
| l         | pandas        |     79.14 |        3.16 |       25.1 |      75.99 |    2.59 |                      0.03 |
| l         | duckdb        |     11.54 |        1.02 |       11.3 |      10.52 |    2.59 |                      0.25 |
| l         | sqlite        |    232.84 |      129.45 |        1.8 |     103.39 |    2.59 |                      0.03 |

