# Resultados de tiempos — nahin-laptop

- Equipo: **Intel64 Family 6 Model 170 Stepping 4, GenuineIntel**, 39.4 GB RAM
- Núcleos: 16 físicos / 22 lógicos
- Sistema: Windows 11 (AMD64)
- Python 3.14.0 · pandas 3.0.1 · duckdb 1.5.5 · sqlite 3.50.4
- Límite por ejecución: 10 minutos (carga + query)
- Versiones medidas: m, l, xl

## 1. Tiempos con el código BASE (sin optimizar)

Tiempo total = carga del CSV + ejecución de la query, en segundos.

| version   |   ('duckdb', 'Q1') |   ('duckdb', 'Q2') |   ('duckdb', 'Q3') |   ('pandas', 'Q1') |   ('pandas', 'Q2') |   ('pandas', 'Q3') |   ('sqlite', 'Q1') |   ('sqlite', 'Q2') |   ('sqlite', 'Q3') |
|:----------|-------------------:|-------------------:|-------------------:|-------------------:|-------------------:|-------------------:|-------------------:|-------------------:|-------------------:|
| m         |               4.11 |               4.25 |               6.93 |              47.98 |              45.91 |              49.23 |             132.79 |             164.69 |             147.51 |
| l         |               6.12 |               8.08 |              11.68 |              98.61 |              92.69 |              96.78 |             229.34 |             305.96 |             241.65 |
| xl        |              36.69 |              37.91 |              47.72 |             158.75 |             165    |             177.63 |             420.54 |             678.71 |             nan    |

## 2. Tiempos con el código OPTIMIZADO

Mismos ejes; ahora las herramientas leen el artefacto optimizado en vez del CSV.

| version   |   ('duckdb-opt', 'Q1') |   ('duckdb-opt', 'Q2') |   ('duckdb-opt', 'Q3') |   ('pandas-opt', 'Q1') |   ('pandas-opt', 'Q2') |   ('pandas-opt', 'Q3') |   ('sqlite-opt', 'Q1') |   ('sqlite-opt', 'Q2') |   ('sqlite-opt', 'Q3') |
|:----------|-----------------------:|-----------------------:|-----------------------:|-----------------------:|-----------------------:|-----------------------:|-----------------------:|-----------------------:|-----------------------:|
| m         |                   0.57 |                   0.44 |                   2.77 |                   2.03 |                   0.45 |                   1.47 |                  43.12 |                  99.86 |                  78.76 |
| l         |                   0.88 |                   0.93 |                   4.67 |                   2.05 |                   1.06 |                   2.9  |                  95.36 |                 217.72 |                 170.7  |
| xl        |                   1.8  |                   1.94 |                  10.85 |                   3.51 |                   2.52 |                   8.37 |                 237.77 |                 539.54 |                 380.52 |

## 3. Antes vs. después, lado a lado (segundos)

|               |   pandas antes |   pandas después |   pandas × |   duckdb antes |   duckdb después |   duckdb × |   sqlite antes |   sqlite después |   sqlite × |
|:--------------|---------------:|-----------------:|-----------:|---------------:|-----------------:|-----------:|---------------:|-----------------:|-----------:|
| ('m', 'ETL')  |         nan    |           nan    |      nan   |         nan    |           nan    |      nan   |         nan    |           nan    |      nan   |
| ('m', 'Q1')   |          47.98 |             2.03 |       23.7 |           4.11 |             0.57 |        7.3 |         132.79 |            43.12 |        3.1 |
| ('m', 'Q2')   |          45.91 |             0.45 |      102.9 |           4.25 |             0.44 |        9.7 |         164.69 |            99.86 |        1.6 |
| ('m', 'Q3')   |          49.23 |             1.47 |       33.5 |           6.93 |             2.77 |        2.5 |         147.51 |            78.76 |        1.9 |
| ('l', 'ETL')  |         nan    |           nan    |      nan   |         nan    |           nan    |      nan   |         nan    |           nan    |      nan   |
| ('l', 'Q1')   |          98.61 |             2.05 |       48.2 |           6.12 |             0.88 |        7   |         229.34 |            95.36 |        2.4 |
| ('l', 'Q2')   |          92.69 |             1.06 |       87.3 |           8.08 |             0.93 |        8.7 |         305.96 |           217.72 |        1.4 |
| ('l', 'Q3')   |          96.78 |             2.9  |       33.4 |          11.68 |             4.67 |        2.5 |         241.65 |           170.7  |        1.4 |
| ('xl', 'ETL') |         nan    |           nan    |      nan   |         nan    |           nan    |      nan   |         nan    |           nan    |      nan   |
| ('xl', 'Q1')  |         158.75 |             3.51 |       45.2 |          36.69 |             1.8  |       20.3 |         420.54 |           237.77 |        1.8 |
| ('xl', 'Q2')  |         165    |             2.52 |       65.4 |          37.91 |             1.94 |       19.5 |         678.71 |           539.54 |        1.3 |
| ('xl', 'Q3')  |         177.63 |             8.37 |       21.2 |          47.72 |            10.85 |        4.4 |         nan    |           380.52 |      nan   |

## 4. Resumen por versión y amortización del ETL

| version   | herramienta   |   antes_s |   despues_s |   mejora_x |   ahorro_s |   etl_s |   corridas_para_amortizar |
|:----------|:--------------|----------:|------------:|-----------:|-----------:|--------:|--------------------------:|
| m         | pandas        |    143.12 |        3.94 |       36.3 |     139.18 |    8.43 |                      0.06 |
| m         | duckdb        |     15.29 |        3.77 |        4.1 |      11.52 |    8.43 |                      0.73 |
| m         | sqlite        |    444.99 |      221.74 |        2   |     223.25 |    8.43 |                      0.04 |
| l         | pandas        |    288.08 |        6.01 |       47.9 |     282.07 |    5.45 |                      0.02 |
| l         | duckdb        |     25.88 |        6.47 |        4   |      19.41 |    5.45 |                      0.28 |
| l         | sqlite        |    776.95 |      483.77 |        1.6 |     293.18 |    5.45 |                      0.02 |
| xl        | pandas        |    501.37 |       14.4  |       34.8 |     486.96 |   14.83 |                      0.03 |
| xl        | duckdb        |    122.31 |       14.59 |        8.4 |     107.72 |   14.83 |                      0.14 |
| xl        | sqlite        |   1099.25 |     1157.83 |        0.9 |     -58.58 |   14.83 |                    inf    |

## 5. Ejecuciones que no completaron

| etapa   | version   | herramienta   | pregunta   | estado   | error                                            |
|:--------|:----------|:--------------|:-----------|:---------|:-------------------------------------------------|
| base    | xl        | sqlite        | Q2         | EXCEDE   | nan                                              |
| base    | xl        | sqlite        | Q3         | omitido  | la ejecución anterior superó el límite de 10 min |
