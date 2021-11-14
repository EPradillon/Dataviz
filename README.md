# Datamining
Objectif : créer un procéssus d'analyse de clustering a partir d'un jeu de données.
On va essayer a partir des propriétés des fromages de créer des regroupements
## 1. Congiguration de l'environnement python
```python
# ------- IMPORT LIBRARY ------- #
import os
import pandas
# dModification du repertoire de travail
    os.chdir("/home/etienne/epsi/dataviz")
```

## 2. Configuration des données

```python
# Chargement du jeu de données :
D = pandas.read_table("fromage.txt",decimal=".")
# Affichage d'un résumé concis des données
D.info()
```

| Column |    Non    |    Count    |  Dtype  |
|-------:|:---------:|:-----------:|:-------:|
|      0 |   sodium  | 29 non-null | float64 |
|      1 |  calcium  | 29 non-null | float64 |
|      2 |  lipides  | 29 non-null | float64 |
|      3 | proteines | 29 non-null | float64 |

```python
# Affichage des premières valeurs (permet de rapidement vérifier les données présentes)
D.head()
```
|   | sodium | calcium | lipides | proteines |
|--:|:------:|:-------:|:-------:|:---------:|
| 0 |  353.5 |   72.6  |   26.3  |    21.0   |
| 1 |  238.0 |  209.8  |   25.1  |    22.6   |
| 2 |  112.0 |  259.4  |   33.3  |    26.6   |
| 3 |  336.0 |  211.1  |   28.9  |    20.2   |
| 4 |  314.0 |  215.9  |   19.5  |    23.4   |


Graphique nuage de point
```python
D.plot(x="lipides",y="proteines",kind="scatter")
```
`<AxesSubplot:xlabel='lipides', ylabel='proteines'>`

![nuage de points](https://github.com/EPradillon/Dataviz/blob/main/lipid%26prot-nuage-de-points.png)

```python
#nuages par paire
import seaborn as sns
sns.pairplot(D)
```
`<seaborn.axisgrid.PairGrid at 0x13f151adc10>`

![nuage de points](https://github.com/EPradillon/Dataviz/blob/main/baresmoyenne.png)

>L'avantage de ce graphique est de pouvoir créer des relations entre certaines propriétés. Par exemple il est facil de >remarquer que les calories d'un fromage sont intimement liées à la quantités de lipides. (Ce qui est logique)
>
Retourne la valeur moyenne du jeu de données pour un axe précis

```python
# Retourne la valeur moyenne du jeu de données pour un axe précis
D.mean(axis=0)
```

| sodium    | 210.086207 |
|-----------|------------|
| calcium   | 185.734483 |
| lipides   | 24.158621  |
| proteines | 20.168966  |
`dtype: float64`

Calcul de la déviation par variable (écarts-type)

```python
# Calcul de la déviation par variable (écarts-type)
D.std(axis=0)
```

| sodium    | 108.678923 |
|-----------|------------|
| calcium   | 72.528882  |
| lipides   | 8.129642   |
| proteines | 6.959788   |



Standardisation : l'objectif est de pouvoir comparer correctement les données entre elles.
On cherche à obtenir un format qui permetra de créer des correlations précise entre les variables.
(on fait un centrage puis une reduction)
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

Une fois standardisée les valeurs moyennes sont proches de 0
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

Pour vérifier que la standardisation est correctement configurée on peut executer a nouveau std pour vérifier l'écart type


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

Kmeans est utilisé pour créer des groupes (cluster) au seins du jeu de données.
C'est un algorithme non supervisé il n'y aura pas d'entrainement.
On peut décider du nombre de cluster a former
```python
#k-means avec 2 groupes
from sklearn import cluster
res = cluster.KMeans(n_clusters=2)
res.fit(Z)
```
`KMeans(n_clusters=2)`

 `res.labels_`
 `array([0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 1, 1, 0, 0, 0, 1, 0,
0, 0, 0, 0, 0, 0, 1])`
> dataframe
 Affichage des 2 clusters
 ```python
#num. de cLuster affectés aux groupes res.labels
#effectifs par groupe
import numpy
numpy.unique(res.labels_,return_counts=True)
```
`(array([0, 1]), array([24,
5], dtype=int64))`


```python
#ajouter La variabLe au data-frame initial
B = D.copy()
B['labels'] = res.labels_
#Configuration des points dans Le pLan #des variabLes prises par paire
sns.pairplot(B,hue="labels")
```

`<seaborn.axisgrid.PairGrid at 0x13f151ad1c0>`

![](https://github.com/EPradillon/Dataviz/blob/main/curves.png)
> Ce regroupement en 2 clusters permet de clairement identifier deux gammes de fromage très différentes.



```pythons
#moyennes par groupe
gb = D.groupby(res.labels_)
#effectifs par cLasse$
gb.size()
```
> L'opération implique de séparer les objets en groupes distinct et de combiner les éléments de données ressemblant entre eux.
| 0 | 24 |
|---|----|
| 1 | 5  |

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

```python
#graphique
B = D.copy()
B['labels'] = resBis.labels_
sns.pairplot(B,hue="labels", palette={0:'blue', 1:'green', 2:'red'})
```

`<seaborn.axisgrid.PairGrid at 0x13f179d7070>`
![](https://github.com/EPradillon/Dataviz/blob/main/multicurvecolor.png)
> Avec 3 regroupements on s'apperçoit qu'un groupe est toujours bien en marge mais cette fois ci il a été possible de séparer les autres éléments en 2 groupes. Un groupe étant largement plus riche que l'autre car toutes ses propriétés sont surélevées.
