# Croiser deux fichiers : recouvrement d'un fichier COUNTER

En partant d'une liste de Lignes de bon de commande de livres électroniques, nous voudrions mettre en regard le coût de chaque titre avec les données d'usage tirées d'un rapport COUNTER fourni par l'éditeur.  

## 1. Import des fichiers 

### 1.1 Créez un premier projet
Importez le fichier [Cas2_LBC_Ebooks_Springer.xlsx](Cas2_LBC_Ebooks_Springer.xlsx) contenant les lignes du bon de commande. Nommez le projet *LBC*

### 1.2 Créez un second projet 
Importez le fichier [Cas2_LBC_Ebooks_Springer.xlsx](Cas2_LBC_Ebooks_Springer.xlsx) contenant les données COUNTER. Nommez le projet *COUNTER*.

> 💡 **ASTUCE :** Pour ouvrir un deuxième projet à partir d'un projet en cour d'édition cliquez sur le bouton ``Ouvrir...`` en haut à droite de l'espace de travail.

## 2. Préparer la clef de recouvrement

Observez les deux projets pour déterminer quelle colonne pourra servir de clef de recouvrement.

Les deux projets ont une colonne ISBN (*Numéro normalisé* dans le projet *LBC* et *Origin ISBN* dans le projet *COUNTER*). Malheureusement, l'ISBN du projet *LBC* est un ISBN 10 alors que l'ISBN du projet COUNTER est un ISBN 13. Nous allons donc transformer l'ISBN 10 en ISBN 13 

### 2.1 Transformer un ISBN 10 en ISSBN 13 

Dans le projet *LBC*, à partir de la colonne *Numéro normalisé*, choisir ``Editer la colonne>Ajouter une colonne en fonction de cette colonne``.

Dans la fenêtre de saisie de l'expression ajoutez la formule suivante :
```
with('978'+value.replace("-", "")[0,9],v,v+((10-(sum(forRange(0,12,1,i,toNumber(v[i])*(1+(i%2*2)) )) %10)) %10).toString()[0] )
```

>⚠ Cette formule ne fonctionnera qu'avec des isbns 13 préfixés par 978.

>💡 Cette formule est tiré de la page [Recipes du dépôt git d'Open Refine](https://github.com/OpenRefine/OpenRefine/wiki/Recipes) 

2.1.3 Nommez la colonne *ISBN13*. 

### 2.2 Retirer les tirets de l'ISBN

Dans le projet *COUNTER*, à partir de la colonne *Origin ISBN*, choisir ``Editer les cellules>Transformer...``.

Dans la fenêtre de saisie de l'expression ajoutez la formule suivante :
```
value.replace("-","")
```
Renommez la colonne en *ISBN13*

>📣 **RAPPEL :** Pour renommer une colonne cliquez sur ``Editer la colonne>Renommer cette colonne``

## 3. Croiser les deux fichiers


À partir du projet *LBC* et de la colonne *ISBN13*, choisir ``Editer la colonne>Ajouter une colonne en fonction de cette colonne``.

Dans la fenêtre de saisie de l'expression ajoutez la formule suivante :
```
cell.cross("COUNTER","ISBN13").cells['Unique Title Requests 2020'].value[0]
```

Répétez l'opération pour les années 2021,2022 ,2023
