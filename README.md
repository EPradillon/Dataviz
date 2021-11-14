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


5. On génère un graphique à nuage de points prenez les valeurs des lipides et protéines.
```python
D.plot(x="lipides",y="proteines",kind="scatter")
```
`<AxesSubplot:xlabel='lipides', ylabel='proteines'>`

![nuage de points](https://github.com/EPradillon/Dataviz/blob/main/lipid%26prot-nuage-de-points.png)

6. Affiche des graphiques à nuage de points avec toutes les paires possibles du tableau de
valeurs.
```python
#nuages par paire
import seaborn as sns
sns.pairplot(D)
```
`<seaborn.axisgrid.PairGrid at 0x13f151adc10>`

![nuage de points](https://github.com/EPradillon/Dataviz/blob/main/baresmoyenne.png)

7. Renvoi la moyenne des valeurs du tableau.

```python
#moyennes par variabLe
D.mean(axis=0)
```

| sodium    | 210.086207 |
|-----------|------------|
| calcium   | 185.734483 |
| lipides   | 24.158621  |
| proteines | 20.168966  |
`dtype: float64`

8. Renvoi l'écart type par variable, permettant de mesurer la dispersion de celles-ci.
```python
#écarts-type par variabLe
D.std(axis=0)
```

| sodium    | 108.678923 |
|-----------|------------|
| calcium   | 72.528882  |
| lipides   | 8.129642   |
| proteines | 6.959788   |

8. On standardise ( centrée-réduite ) les variable en deux étapes :
Par centrage en soutrayant la moyenne aux valeurs du datase, on aura alors l'écart
positif ou négatif avec cette moyenne
Par Réduction en divisant les variables par l'écart type
Une fois la variable standardisée, la moyenne devrait etre de zéro et l'écart type de 1

```python
#standardisation
Z = (D - D.mean(axis=0))/D.std(axis=0)
print(Z)
```

|     | sodium    | calcium   | lipides   | proteines |
|-----|-----------|-----------|-----------|-----------|
| 0   | 1.319610  | -1.559854 | 0.263404  | 0.119405  |
| 1   | 0.256846  | 0.331806  | 0.115796  | 0.349297  |
| 2   | -0.902532 | 1.015671  | 1.124450  | 0.924027  |
| 3   | 1.158585  | 0.349730  | 0.583221  | 0.004459  |
| 4   | 0.956154  | 0.415910  | -0.573041 | 0.464243  |
| 5   | 0.422472  | 1.079094  | 0.570920  | 0.406770  |
| 6   | -0.166419 | -1.358555 | 0.460215  | -0.096119 |
| 7   | 0.606500  | -0.728461 | 0.152698  | -0.340379 |
| ... | ...       | ...       | ...       | ...       |

9. On vérifie la standardisation des variables par leur moyenne. La valeur de la moyenne est
bien très proche de zéro pour chaque variables.
```python
#Vérification de La standardisation - moyennes
Z.mean(axis=0)
```

| sodium    | 1.071939e-16 |
|-----------|--------------|
| calcium   | 4.689735e-16 |
| lipides   | 3.368953e-16 |
| proteines | 9.188053e-17 |
`dtype: float64`

10. On vérifie l'écart type des variables. La valeur moyenne est bien de 1, la standardisation est
donc bien executée.

```python
#Vérification - écarts-type
Z.std(axis=0)
```

| sodium    | 1.0 |
|-----------|-----|
| calcium   | 1.0 |
| lipides   | 1.0 |
| proteines | 1.0 |
`dtype: float64`

11. Partitionnement des données en cluster, la fonction a généré 2 clusters Kmeans.
```python
#k-means avec 2 groupes
from sklearn import cluster
res = cluster.KMeans(n_clusters=2)
res.fit(Z)
```
`KMeans(n_clusters=2)`

12. Affiche les labels pour chaque set de variables
13. `res.labels_`
14. `array([0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 1, 1, 0, 0, 0, 1, 0,
0, 0, 0, 0, 0, 0, 1])`

13. Récupère le nom d'éléments unique du cluster, soit les 2 clusters
14. ```python
#num. de cLuster affectés aux groupes res.labels
#effectifs par groupe
import numpy
numpy.unique(res.labels_,return_counts=True)
```
`(array([0, 1]), array([24,
5], dtype=int64))`

14. On ajoute au dataset les labels correspondant au cluster et génère le graphique de nuage à
points avec toutes les paires possibles en séparant les

```python
#ajouter La variabLe au data-frame initial
B = D.copy()
B['labels'] = res.labels_
#Configuration des points dans Le pLan #des variabLes prises par paire
sns.pairplot(B,hue="labels")
```

`<seaborn.axisgrid.PairGrid at 0x13f151ad1c0>`

![](https://github.com/EPradillon/Dataviz/blob/main/curves.png)
15. Groupe les données dans le dataset en fonction du label lié aux clusters.
```python
#moyennes par groupe
gb = D.groupby(res.labels_)
#effectifs par cLasse$
gb.size()
```

| 0 | 24 |
|---|----|
| 1 | 5  |

16. Récupère la moyenne des variables en fonction des clusters.
```python
#moyennes par cLasse
gb.mean()
```
|   | sodium     | calcium    | lipides | proteines |
|---|------------|------------|---------|-----------|
| 0 | 239.729167 | 199.104167 | 27.375  | 22.708333 |
| 1 | 67.800000  | 121.560000 | 8.720   | 7.980000  |

17. Créé des clusters en forcant leur nombre à 3.
```python
#CLustering en 3 cLasses
resBis = cluster.KMeans(n_clusters=3)
resBis.fit(Z)
```
`KMeans(n_clusters=3)`
18. On modifie le dataset avec les nouveaux clusters et on génère le graphique de nuage a
points pour toutes les paires possibles en fonction des clusters. On définit les couleurs en
fonction du label.
```python
#graphique
B = D.copy()
B['labels'] = resBis.labels_
sns.pairplot(B,hue="labels", palette={0:'blue', 1:'green', 2:'red'})
```

`<seaborn.axisgrid.PairGrid at 0x13f179d7070>`
![](https://github.com/EPradillon/Dataviz/blob/main/multicurvecolor.png)
