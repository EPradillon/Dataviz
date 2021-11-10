# Datamining

## 1. Congiguration de l'environnement python
```python
# ------- IMPORT LIBRARY ------- #
import os
import pandas
# directory :
    os.chdir("/home/etienne/epsi/dataviz")
```

## 2. Configuration des données

```python
# Lecture du fichier dans le dossier
D = pandas.read_table("fromage.txt",decimal=".")
# Pré-visualisation des données
D.info()
```

> Tableau retravaillé sur https://www.tablesgenerator.com/markdown_tables

| Column |    Non    |    Count    |  Dtype  |
|-------:|:---------:|:-----------:|:-------:|
|      0 |   sodium  | 29 non-null | float64 |
|      1 |  calcium  | 29 non-null | float64 |
|      2 |  lipides  | 29 non-null | float64 |
|      3 | proteines | 29 non-null | float64 |

```python
# Affichage des valeurs "de tête"
D.head()
```
|   | sodium | calcium | lipides | proteines |
|--:|:------:|:-------:|:-------:|:---------:|
| 0 |  353.5 |   72.6  |   26.3  |    21.0   |
| 1 |  238.0 |  209.8  |   25.1  |    22.6   |
| 2 |  112.0 |  259.4  |   33.3  |    26.6   |
| 3 |  336.0 |  211.1  |   28.9  |    20.2   |
| 4 |  314.0 |  215.9  |   19.5  |    23.4   |

[nuage de points](https://github.com/EPradillon/Dataviz/blob/main/lipid%26prot-nuage-de-points.png)

