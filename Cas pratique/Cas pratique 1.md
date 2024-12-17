# Fil conducteur
## Objectif : 
Nettoyer et harmoniser un fichier décrivant des lettres numérisés dans un fonds d’archives pour permettre la création de métadonnées dans une bibliothèque numérique.
## Principales transformations :
 - supprimer les entêtes, numéros de lots et espaces entre les 3 lots
 - harmoniser les cotes (passer en majuscules)
 -  harmoniser les noms des auteurs et destinataires
 -  harmoniser les sujets
 -  supprimer le sujet “archéologie”
 -  harmoniser les dates et la gestion des dates manquantes
 -  harmoniser les langues et la gestion des langue manquantes
 -  harmoniser les nombre de pages et la gestion des nombr de  -  pages manquants
 -  ajouter le titre dans une colonne, en le construisant sur le modèle “[Lettre de … à … du jj mmois aaaa] page …”
 -  dupliquer les lignes pour chaque lettre comportant plusieurs images
 -  identifier le fichier correspondant à chaque page
 -  renommer des colonnes
 -  déplacer des colonnes
# 4 Gestion des projets et import de données
## 4.1 Découverte de l’interface
Passer l'interface à Français
## 4.2 Liste des projets
Modifier la description et taguer les projets
## 4.3 Les différents types d’imports de données
### Import de fichiers
Possibilités d'importer plusieurs fichiers ayant la même structure. Ils sont mis bout à bout + ajout colonne nom de fichier.
Archives
### Import depuis un classeur Google doc
### Import depuis une base de données (MySQL/MariaDB, PostgreSQL, SQLite)
### Data packages : 
Format d’échange de données issu du monde de l’open data, associe données brutes et fichier de métadonnées en JSON
## 4.4 Importer le fichier d’exercice :
### Ecran d'import
### Les différents formats
> ❔ Est-ce que l'on fait une démo d'un fichier json. Voici un lien vers un retour json d'un appel de l'API IDREF qui présente la livre de tous les bouqions écrits apr Douglas Adams.
> https://www.idref.fr/services/biblio/026677636.json
PC-Axis = format d'échange de données stat.
Le typage des données est conservé via les fichiers Excel ou LibreOffice.
Perte du typage en CSV
### Les principaux paramètres d'import
  - Encodage
  - Séparateurs pour les fichiers csv
  - Détection automatiques des nombres et des dates (⚠ A utiliser avec prudence)
  - Exclusion des premières lignes du fichier 
  - Définition des en-têtes de colonnes
>🛠 Importer le fichier Excel
# 5 Exploration et nettoyage de base
## 5.1 Découverte de l’espace de travail
## 5.2 Les types de données
La grille de données peut contenir 4 types de valeurs principales :
 -  Texte (en noir, aligné à gauche)
 - Nombre (en vert, aligné à droite)
 - Date (en vert, aligné à droite)
 - Booléen (en vert, aligné à droite)
Ainsi que deux valeurs spéciales, qui ne peuvent pas être saisies manuellement :
 -  “null” (absence de toute information, normalement invisibles, mais peuvent être affichées en gris après activation du menu Toutes > Aperçu > Afficher/masquer les valeurs nulles dans les cellules)
 -  “erreurs” (en rouge), créées par certaines formules si une option est activée dans l’éditeur de formules)
Important : une même colonne peut contenir différents types de valeurs (contrairement à un dataset en R par exemple).
## 5.3 Les différents menus
## 5.4 Réordonner, supprimer, renommer des colonnes
>🛠 Déplacer la colonne “date” d’une position vers la gauche
> 1ere solution : Editer la colonne > Déplacer la colonne sur la gauche
> 2eme solution : Colonne Toutes > Editer les colonnes > Retirer/Supprimer les colonnes
## 5.5 Trier ses données
>🛠 Trier les lignes par cotes. 
>Appliquer le tri de façon permanente
## 5.6 Filtrer ses données
>🛠 Combien de lignes comprennent “carte postale” dans leur description ?
>🛠 Combien de lignes ne comprennent pas “carte postale” en description mais comprennent “1887” en “date” ?
> 🛠 Idem en élargissant à toute la décennie 1880 ?
### Expressiosn régulières
```
  ab -> a suivi de b
  a. -> a suivi de n'importe quel caractère
  \n -> saut de ligne
  \t -> tabulation
  \s -> espace, tabulation ou saut de ligne
  a[cfbde] -> a suivi de b ou c ou d ou e ou f (classe de caractères)
  a[b-f] -> idem
  a[^cfbde] -> n'importe quel caractère sauf b,c,d,e,f
  a[^b-f] -> n'importe quel caractère sauf b,c,d,e,f
  a(bc|ef) -> a suivi de bc ou ef (groupes)
  ? -> cherche 0 ou 1 fois le caractère, la classe ou le groupe qui précède
  + -> cherche 1 fois ou plus le caractère, la classe ou le groupe qui précède
  * -> cherche 0, 1 fois ou plus le caractère, la classe ou le groupe qui précède
  {3,6} -> cherche 3 à 6 fois ou plus le caractère, la classe ou le groupe qui précède
```
>🛠 Combien de lignes ne comprennent pas “carte postale” en description mais comprennent “1887” ou "1888" en “date” ?
`188[78]`
>🛠 Supprimer le filtre sur la description, et conserver celui sur les dates. Toutes les dates sont elles comprises entre 1800 et 1999?
`1[89][0-9]{2}`
>🛠 Supprimer le filtre
## 5.7 Remplacer ou modifier des valeurs
### 5.7.1 Modification manuelle d’une cellule unique
Important : les modifications isolées sont enregistrées dans l’historique et peuvent être “rejouées”, mais ne sont pas exportables.
### 5.7.2 Modification de toutes les cellules identiques d’une colonne
>🛠 Remplacer toutes les valeurs vides par “inconnu” dans la colonne date
### 5.7.3 Remplacer une chaîne ou un motif
menu Editer les ``cellules > Remplacer``
>🛠 Remplacer toutes les valeurs de “-” par “/” dans la colonne date
>🛠Remplacer en une opération “lot1”, “lot2” et “lot3” par une valeur vide "" dans la colonne cote
## 5.8 Annuler ou rejouer un traitement
>🛠 Annuler puis rejouer les dernières opération 
## 5.9 Appliquer des facettes sur les colonnes de données
>⚠Par défaut, seuls les 2000 premiers choix sont affichés. S’il y en a plus, OpenRefine propose d’augmenter cette limite, ou de n’afficher que les choix les plus fréquents (« facette par nombre de choix »). Le choix reste mémorisé lors des utilisations ultérieures. On peut relever le seuil à 100000 sans problème (si mémoire suffisante).
### 5.9.1 Premier type de facette : facettes «textuelles»
>🛠 Afficher les facettes textuelles correspondant au contenu de la colonne langue
>🛠 Repérer les anomalies apparentes.
#### Les options des facettes
Tri alphabétique ou par poid des facettes
Changer la facette Appliquer une fonction pour modifier les valeurs
inverser = toutes les valeurs sauf celle sélectionnée
groupe =  fonction de clusterisation
choix = récupérer liste des valeurs distinctes
>🛠Comment harmoniser la valeur en “fre” (en restant dans le volet facettes)?
>🛠 Comment sont classées les facettes? Peut-on en modifier l’ordre ?
>🛠 Ajouter une nouvelle facette sur la colonne description, classer les résultats par compte et sélectionner la valeur la plus fréquente. Quel est l’effet sur la facette par langue 
>🛠 Supprimer les deux facettes 
### 5.9.2 Autres types de facettes sur les colonnes
 - `Facette par mot` : Divise chaque mot (séparé d’un espace) d’une colonne et les présente en liste de valeurs distinctes [value.split(" ")].
 - ``Facette doublons``: valeurs booléennes indiquant si les valeurs sont uniques (false) ou non (true) [facetCount(value, ‘value’, ‘Symptome’) > 1].
 - ``Facettes logarithmiques``: modification logarithmique de valeurs numériques [value.log()].
 - ``Facette longueur de texte``: liste le nombre de caractères du contenu des cellules d’une colonne [value.length()].
 - Facettes permettant d’explorer les valeurs d’`erreur, nulles, vides`… [isEmptyString(value)]

>🛠 Utiliser une facette pour compter le nombre de lignes vides et non vides dans la colonne description 
>🛠 Utiliser une facette personnalisée pour vérifier si la colonne “cote” ne contient que des valeurs uniques.
>🛠 A partir de la colonne auteur, appliquez une facette personnalisée « par mots » (Les valeurs sont découpées en suite de caractères séparés par des espaces).Quel intérêt peut avoir l’opération?
## 5.10 Appliquer des facettes sur l’ensemble d’une ligne
## 5.10.1 Repérer les lignes complètement vides
>🛠 Existe-t-il des lignes vides ?
## 5.10.2 Les facettes par étoile et par marques
## 5.10.3 Repérer les colonnes comprenant des valeurs vides anormales
`Menu Toutes > Facette > Valeur non vide par colonne`
>🛠 Comparer le nombre de valeurs non vides pour chaque colonne au nombre de lignes totales. Quelles colonnes seraient à examiner ?
>🛠 Isoler les lignes à corriger et faire les corrections à la main 
## 5.11 Supprimer des lignes
>🛠Utiliser une facette personnalisée pour isoler les lignes dont la colonne cote ne fait pas 18 caractères.
>🛠Marquer les lignes avec une étoile
>🛠Supprimer les lignes étoilées
## 5.12 Regrouper des valeurs proches
A partir d’une colonne :`` Editer les cellules > Grouper et éditer``
A partir d’une facette : ``Sélectionner la facette > bouton “Grouper” en haut à droite.``
>🛠 Repérez les valeurs proches pour la colonne auteur puis grouper les résultats en utilisant les différentes méthodes proposées.
>Pour chaque doublon repéré, cliquer sur une forme pour choisir la forme retenue ou pour la modifier.
>Sélectionner les groupes à fusionner un par un ou bien tout sélectionner.
>1re méthode par défaut par empreinte
>2e méthode par plus proche voisin
>🛠 Le nettoyage est-il complet (vérifier en construisant une facette) ? Corriger à la main au besoin, en conservant la forme la plus précise.
>🛠 Vérifiez si les colonnes “sujet”, description“,”Fonds" et “destinataire” nécessitent le même traitement et corrigez les au besoin.
## 5.13 Changer la casse
>🛠 La colonne cote comprend un mélange anormal de majuscules et de minuscules. Corrigez la 
## 5.14 Supprimer les espaces superflus
>🛠 Supprimez les espaces superflus (espaces au début et à la fin de chaque cellule, espaces redoublés) dans la colonne “fichier et page 
## 5.15 Convertir dans un autre type de données
>🛠 Transformez en type “nombre” le contenu de la colonne nbpages
>corriger les données pour qu'elles soient transformables en nombre (retirer les chaines de caractère)
## 5.16 Vider complètement une colonne
## 5.17 Vider les valeurs répétées sur plusieurs lignes
“Vider” des valeurs répétées peut être nécessaire pour d’autres opérations :
    Supprimer des doublons
    Créer des « entrées » regroupant plusieurs lignes
>🛠Si vous n’avez pas trié la colonne cote, triez la de façon permanente, puis videz les valeurs répétées dans cette colonne. Elles seront remplacées par des valeurs null.
## 5.18 Lignes et entrées
## 5.18.1 Principes
## 5.18.2 Création d’entrées
## 5.18.3 Prise en compte des entrées lors d’opérations de vidage/remplissage
>🛠Après avoir créé une entrée de plusieurs lignes, passez en mode ligne en mode ligne réalisez une opération de vidage des valeurs répétées dans la colonne auteur.
>Observez le résultat.
>Annulez l’opération, puis passez en mode “entrée” et réalisez la même opération.
>Observez le résultat.
## 5.19 Remplir les valeurs vides consécutives
>🛠En mode “lignes”, remplissez les valeurs vides consécutives de la colonne description.
>Annulez l’opération.
>Répétez l’opération en mode “entrées”.
>Comment expliquer la différence ?
>🛠En mode “lignes”, remplissez les valeurs vides consécutives de la colonne cote.
## 5.20 Supprimer des doublons
Opérations pour supprimer des doublons :
 - Se placer en mode lignes
 - (Si la colonne contient des valeurs vides, les remplacer par une valeur temporaire)
 - Trier de façon permanente la colonne servant de contrôle
 - Vider les valeurs répétées dans cette colonne
 - Appliquer une facette par valeur vide (blank)
 - Sélectionner la facette “true”
 - Supprimer toutes les lignes affichées
 - (Si la colonne contenait des valeurs vides, remplacer la valeur temporaire par une valeur vide)
 - Si le contrôle doit prendre en compte plusieurs colonnes : créer une colonne temporaire contenant la concaténation des colonnes concernées.
>🛠 Supprimez les lignes en double en utilisant les doublons dans la colonne cote comme critère 
# 6 Transformations avancées
## 6.1 Restructurer des données multivaluées
Besoin fréquent : passer de plusieurs valeurs par cellule à une seule, et réciproquement.
### 6.1.1 Axe vertical : créer et supprimer de nouvelles lignes
Eclater des cellules sur plusieurs lignes permet :
 - de compter les valeurs distinctes (facettes)
 - de les harmoniser
 - de les transformer ou combiner avec d’autres colonnes
 - de les aligner avec des référentiels extérieur

>🛠 Normalisez les sujets utilisés dans la colonne sujet : passez tout en minuscule avec majuscules accentuées, supprimez le mot archéologie, ordonnez les mots restant par ordre alphabétique
>🛠 Créer une ligne par fichier :
>- Remplacer les cellules vides par une valeur fictive dans toutes les colonnes : Repérer les cellule vide grâce aux facettes et éditer les cellules OU Toutes > Transformer : value.coalesce('').replace(/^$/,'####') 
>- Eclater la colonne fichier et page sur plusieurs lignes
>- Déplacer la colonne en 1re position
>- Remplir les cellules vides créées par l’opération dans les autres colonnes
>- Nettoyer les valeurs fictives : Utiliser remplacer ou Toutes > Transformer : value.replace('####','')
### 6.1.2 Axe horizontal : créer et supprimer de nouvelles colonnes
>🛠 Séparer les information “nom de fichier” et “page” de la colonne “fichier et page” dans deux colonnes
> - Choisir le séparateur “(”, décocher “deviner le type de cellule”
> - Renommer les colonnes obtenues: fichier et page
> - Supprimer les caractères non numériques dans la colonne Page
> - Supprimer le préfixe “file:” et “fichier:” dans la colonne Fichier 
# 7 Exporter les données et les traitements
## 7.1 Extraire et réappliquer des traitements
## 7.2 Exporter les données