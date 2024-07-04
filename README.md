# Licence-Projet-Fin-Etudes-Sciences-des-Données-R

## L'objectif de l'étude
Ce projet, réalisé dans le cadre de ma licence en sciences des données, vise à effectuer une analyse exploratoire approfondie et à développer un modèle statistique explicatif et prédictif en utilisant le langage R. À chaque étape du processus, l'accent est mis sur la formulation de questions de recherche pertinentes pour optimiser les données et affiner le modèle. Les résultats de ces analyses et du modèle développé ont permis de répondre à ces questions spécifiques et de fournir des informations précieuses.

Types d'exemples de questions de recherche qui ont émergé des analyses univariées et bivariées effectuées durant l'étude mais nous reviendrons plus en detaille pour chaque parties:
- **Question de recherche :** Quelles sont les 6 emplois les plus demandées ? (Analyse univariée)
- **Question de recherche :** Quelles sont les 6 secteurs d’activités les plus rechercher ? (Analyse univariée)
- **Question de recherche :** Quel est le type de poste le plus fréquent dans chaque région ? (Analyse bivariée)
- **Question de recherche :** Le type de poste recherché est-il indépendant de la région ? (Analyse bivariée)

Types d'exemples des questions de recherche suivantes ont émergé du développement et de l'évaluation du modèle statistique :
- **Question de recherche :** : existe-t-il une influence statistique entre l’évaluation de l’entreprise et le salaire des employés ? (Modélisation)
- **Question de recherche :** Quelle est la précision prédictive du modèle ? (Modélisation)

  
## Déscription des données
Les données proviennent du site Kaggle, une plateforme web qui organise des compétitions en science des données. L’objectif de cet ensemble de données est d’améliorer la recherche d’emploi, en particulier pour ceux qui ont été touchés par la pandémie. 

### Structure des Données
Ce dataframe contient 3909 observations et 17 variables :
- **Observations** : Chaque ligne représente une offre d’emploi spécifique.
- **Variables** : Chaque colonne représente une caractéristique associée à chaque offre d’emploi. Les variables incluent des informations telles que l’estimation du salaire, l’emplacement, l’évaluation de l’entreprise, la description de l’emploi, et d’autres détails pertinents.

https://www.kaggle.com/datasets/andrewmvd/data-scientist-jobs/data



# Méthodologie

Les étapes de la méthodologie comprennent :

1. **Prétraitement des Données** : Charger, comprendre et nettoyer les données pour assurer leur qualité.
2. **Analyse Exploratoire des Données (EDA)** : Utiliser des graphiques, des statistiques descriptives et des tests statistiques pour explorer les données, identifier les tendances et les relations entre les variables, et répondre aux questions de recherche.
3. **Modélisation Statistique** : Développer des modèles prédictifs, tels que des modèles linéaires ou d’autres algorithmes pertinents, pour répondre aux questions de recherche et comparer ces modèles pour évaluer leur performance.
4. **Interprétation et Recommandations** : Interpréter les résultats de manière approfondie et formuler des recommandations basées sur l'analyse.


## Détails des Étapes

### 1. Prétraitement des Données

Le prétraitement de ces données à consister :
- **Transformation des variables textuelles** : Les variables textuelles ont été converties en catégories ou valeurs numériques adaptées à notre objectif d’analyse.
- **Création et suppression de variables** : De nouvelles variables ont été créées et les données inutiles supprimées pour améliorer la pertinence de l'ensemble de données.
- **Détecter et traiter les anomalies** : telles que les valeurs manquantes et les lignes dupliquées.
- **Analyse des valeurs manquantes** : Nous avons analysé la structure des valeurs manquantes et choisi une méthode appropriée pour les traiter (suppression ou imputation).

Pour plus de détails sur le prétraitement, consultez le notebook [Preprocessing](path_to_notebook).

#### 1.1 Choix de la Méthode approprié 

L'une des premières questions soulevées concerne la gestion des valeurs manquantes. En anticipant la réalisation d'une régression linéaire simple dans notre analyse ultérieure, il est pertinent de se poser la question suivante : **une régression linéaire effectuée avec imputation des données manquantes produira-t-elle des résultats significativement différents ou améliorés par rapport à une régression linéaire réalisée après la suppression des données manquantes ?** Cette comparaison nous permettra d'évaluer l'impact de l'imputation sur la qualité des résultats et de déterminer si elle constitue une étape bénéfique dans notre processus d'analyse.

Nous avons choisi d'utiliser l'algorithme MICE (Multiple Imputation by Chained Equations) pour gérer les données de type MAR (Missing at Random) et conserver un maximum d’informations tout en minimisant les biais. Cette méthode implémente une approche itérative de l'imputation des données manquantes en utilisant un modèle spécifique pour chaque variable avec des valeurs manquantes.

Pour plus de détails sur l'analyse et la justification de cette approche, veuillez consulter le notebook associé.


### 2. Analyse Exploratoire des Données (EDA)
Une fois le prétraitement initial achevé, nous entamons l'analyse exploratoire des données (EDA). Cette phase nous permet de visualiser la distribution des variables, d'identifier des tendances significatives, d'explorer les relations entre différentes variables, et de repérer d'éventuelles anomalies. Cela guidera le choix de méthodes d’analyse statistique appropriées, jetant ainsi les bases d’une analyse robuste pour répondre efficacement à nos questions de recherche et formuler des recommandations pertinentes.

#### 2.1 Analyse Univariée pour variables qualitatves: 
- Visualisation des distributions : Nous utilisons un graphique camemberts (pie charts) pour représenter la distribution des modalités de chaque variable qualitative.
- Identification des modalités fréquentes : Nous déterminons les modalités les plus fréquentes pour chaque variable e afin de comprendre quelles catégories dominent dans les données.

Cette démarche nous permet de mieux comprendre la distribution des modalités au sein de chaque variable qualitative et de répondre à des questions spécifiques comme :
- Quelles sont les 6 emplois les plus demandées ?
- Quelles sont les 6 secteurs d’activités les plus rechercher ?

Pour une analyse plus détaillée et des visualisations supplémentaires, veuillez consulter le notebook associé.


#### 2.2 Analyse Bivariée pour variables qualitatives :

L'idée ici est d'évaluer la dépendance entre deux variables qualitatives pour identifier des relations significatives, détecter les Redondances et améliorer nos futurs modèles.

- **Visualisation :** Utilisation de "Diagramme en Tuyaux d’Orgue" pour représenter la répartition des catégories, facilitant la compréhension des associations et des différences entre les catégories.

- **Tests d'Indépendance :** Nous utilisons principalement le test du khi-deux pour évaluer l'indépendance entre deux variables. Si les conditions ne sont pas remplies (effectifs théoriques nuls ou < 5), nous optons pour le test exact de Fisher.

* **Hypothèses :**
    + **H0 (Hypothèse Nulle) :** Les deux variables sont indépendantes (p-valeur ≥ 0,05).
    + **H1 (Hypothèse Alternative) :** Les deux variables ne sont pas indépendantes (p-valeur < 0,05).

- **Seuil de Signification :** 5 % pour les p-valeurs.
- **Mesure de l'Association :** En cas de lien significatif, nous utilisons le V de Cramer (0 à 1) pour mesurer l'intensité de l'association.

Cette démarche nous permet de mieux comprendre les relations entre les variables qualitatives et de répondre à des questions spécifiques comme :

* Question 1.1: concernant les variables “types de Postes”, “Régions”
    + Quel est le type de poste le plus fréquent dans chaque région ?
    + Existe-t-il des régions où un certain type de poste prédomine par rapport aux autres ?
    +  Le type de poste recherché est-il indépendant de la région ?


Pour des détails supplémentaires et des visualisations, veuillez consulter le notebook associé.


#### 2.3 Analyse Univariée pour Variables Quantitatives

L'analyse univariée a été effectuée pour comprendre la distribution et les caractéristiques de chaque variable quantitative individuellement.

*  **Visualisation des distributions :**
   + **Histogramme :** Cet outil graphique permet de visualiser la distribution des données en les regroupant par intervalles. Il fournit des indications sur la forme de la 
                       distribution, la présence de modes et d’asymétries.
   +  **Densité :** Utilisée en conjonction avec l’histogramme, la densité offre une représentation visuelle de la forme globale de la distribution et des variations de densité le long 
                     de l’axe des valeurs.


*  **Statistiques Descriptives :**
      + **Tendance Globale :** En utilisant des mesures de tendance centrale telles que la moyenne, la médiane et le mode, nous obtenons des indications sur la position centrale des 
                           données, mettant en lumière la valeur centrale ou la plus fréquente.
      + **Dispersion :** À travers des mesures de dispersion comme l’écart-type, l’étendue interquartile et le coefficient de variation, nous quantifions la variabilité et la dispersion 
                     des données, permettant de mieux comprendre la distribution.

Cette approche combinée, avec l’utilisation d’outils graphiques et de statistiques descriptives, nous offre une analyse approfondie des variables quantitatives. Elle facilite la détection de tendances, de modes et de variations significatives dans nos données, essentielles pour formuler des hypothèses et orienter nos analyses futures.

Cette démarche nous permet de mieux comprendre la distribution des valeurs au sein de chaque variable quantitative et de répondre à des questions spécifiques comme :
- Quelle est la distribution des salaires agrégés dans notre échantillon ?
- Quels sont les niveaux moyens et les variations de la taille des entreprises ?

Pour des détails supplémentaires et des visualisations, veuillez consulter le notebook associé.

Pour une analyse plus détaillée et des visualisations supplémentaires, veuillez consulter le notebook associé.


#### 2.4 Analyse Bivariée pour Variables Quantitatives

Dans cette section, nous explorons les relations entre deux variables quantitatives pour identifier des patterns et des associations significatives :

1. **Graphiques de Dispersion :**
   - **Diagramme de dispersion :** Ce graphique permet de visualiser la relation entre deux variables quantitatives en plaçant chaque paire de valeurs sur un graphique cartésien. Cela nous permet d'observer la direction, la forme et la force de la relation entre les variables.

2. **Mesures de Corrélation :**
   - **Coefficient de corrélation de Pearson :** Mesure la force et la direction de la relation linéaire entre deux variables continues. Une valeur proche de 1 indique une corrélation positive forte, -1 une corrélation négative forte, et 0 indique une absence de corrélation linéaire.
   - **Coefficient de corrélation de Spearman :** Évalue la relation monotone entre deux variables continues. Il est utilisé lorsque la relation entre les variables ne suit pas une distribution normale.

3. **Tests d'Hypothèses :**
   - **Test de corrélation :** Nous utilisons des tests statistiques pour déterminer si la corrélation observée entre les deux variables est significative. Un seuil de signification de 5 % est souvent utilisé pour évaluer la p-valeur associée.

Cette approche nous permet d'identifier et de quantifier les relations linéaires et non linéaires entre les variables quantitatives, fournissant ainsi des insights cruciaux pour nos analyses et modèles futurs.

Pour plus de détails et des visualisations spécifiques, veuillez vous référer au notebook associé.


### Analyse Bivariée
L'analyse bivariée a été réalisée pour examiner les relations entre paires de variables. Voici un résumé des principales relations observées :
- Relation entre Variable 1 et Variable 2 : [Résumé des observations]
- Relation entre Variable 2 et Variable 3 : [Résumé des observations]
- ...

## Modélisation
### Développement du Modèle
Pour répondre aux questions de recherche identifiées, un modèle statistique a été développé. Voici les étapes suivies :
- Choix du modèle : [Description]
- Entraînement du modèle : [Description]
- Validation du modèle : [Description]

### Résultats du Modèle
Les résultats du modèle sont les suivants :
- [Résultat 1]
- [Résultat 2]
- [Résultat 3]

## Interprétation des Résultats
L'interprétation des résultats du modèle permet de répondre aux questions de recherche initiales :
1. **Question de recherche 1 :** [Interprétation des résultats en lien avec la question]
2. **Question de recherche 2 :** [Interprétation des résultats en lien avec la question]
3. **Question de recherche 3 :** [Interprétation des résultats en lien avec la question]
4. **Question de recherche 4 :** [Interprétation des résultats en lien avec la question]
5. **Question de recherche 5 :** [Interprétation des résultats en lien avec la question]

## Conclusion
Cette étude a permis de découvrir [résumé des découvertes principales]. Les résultats obtenus ouvrent la voie à [suggestions pour des recherches futures].

## Annexes
### Code Source
Le code source de l'analyse et de la modélisation est disponible dans le répertoire `src/`.

### Documentation
Des détails supplémentaires sur les données et les méthodes utilisées peuvent être trouvés dans le répertoire `docs/`.




