# Licence-Projet-Fin-Etudes-Sciences-des-Données-R

## L'objectif de l'étude
Ce projet, réalisé dans le cadre de ma licence en sciences des données, vise à effectuer une analyse exploratoire approfondie et à développer un modèle statistique explicatif et prédictif en utilisant le langage R. À chaque étape du processus, l'accent est mis sur la formulation de questions de recherche pertinentes pour optimiser les données et affiner le modèle. Les résultats de ces analyses et du modèle développé ont permis de répondre à ces questions spécifiques et de fournir des informations précieuses.


## Déscription des données
Les données proviennent du site Kaggle, une plateforme web qui organise des compétitions en science des données. L’objectif de cet ensemble de données est d’améliorer la recherche d’emploi, en particulier pour ceux qui ont été touchés par la pandémie. J'ai trouvé ce dataset particulièrement intéressant car il couvre l'ensemble des métiers de la data, ce qui permet d'analyser les métiers et secteurs les plus recherchés, un domaine dans lequel je me dirige. 

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

L'objectif ici était d'analyser et de se poser les questions nécessaires pour préparer les données afin qu'elles soient utilisables pour des analyses ultérieures et l'entraînement de modèles. Voici une description générale des principales étapes du prétraitement dans ce projet :

- **Transformation des variables textuelles** : Les variables textuelles ont été converties en catégories ou valeurs numériques adaptées à notre objectif d’analyse.
- **Création et suppression de variables** : De nouvelles variables ont été créées et les données inutiles supprimées pour améliorer la pertinence de l'ensemble de données.
- **Détecter et traiter les anomalies** : telles que les valeurs manquantes et les lignes dupliquées.
- **Analyse des valeurs manquantes** : Nous avons analysé la structure des valeurs manquantes et choisi une méthode appropriée pour les traiter (suppression ou imputation).
- **Gestion des variables qualitatives** : Les variables qualitatives ont été encodées (par exemple, via l'encodage de labels ou l'encodage one-hot) et le nombre de leurs modalités a 
                                           été géré pour assurer leur pertinence dans les analyses.

Pour plus de détails sur le prétraitement, consultez le notebook [Preprocessing](path_to_notebook).

#### 1.1 Choix de la Méthode approprié pour une imputation

L'une des premières questions soulevées concerne la gestion des valeurs manquantes. En anticipant la réalisation d'une régression linéaire simple dans notre analyse ultérieure, il est pertinent de se poser la question suivante :
- **une régression linéaire effectuée avec imputation des données manquantes produira-t-elle des résultats significativement différents ou améliorés par rapport à une régression linéaire réalisée après la suppression des données manquantes ?**
  
Cette comparaison nous permettra d'évaluer l'impact de l'imputation sur la qualité des résultats et de déterminer si elle constitue une étape bénéfique dans notre processus d'analyse.

Nous avons choisi d'utiliser l'algorithme MICE (Multiple Imputation by Chained Equations) pour gérer les données de type MAR (Missing at Random) et conserver un maximum d’informations tout en minimisant les biais. Cette méthode implémente une approche itérative de l'imputation des données manquantes en utilisant un modèle spécifique pour chaque variable avec des valeurs manquantes.

Pour plus de détails sur l'analyse et la justification de cette approche, veuillez consulter le notebook associé.


#### 1.2 Prétraitement Additionnel pour les Modalités Excessives et Rares

Afin de répondre à la complexité identifiée lors de l'analyse exploratoire, nous avons introduit des étapes de prétraitement supplémentaires pour gérer :

 - **Modalités Rares** : Catégories avec peu d'observations, pouvant entraîner des instabilités dans les résultats.
 - **Nombre Élevé de Modalités** : Complication des modèles prédictifs due à l'augmentation de la dimension de l'espace des caractéristiques.
 - **Différences de Fréquence** : Les disparités importantes de fréquence entre les modalités affectent la prédiction des catégories moins fréquentes.

Ces étapes incluent le regroupement de modalités rares, la fusion de catégories similaires, et l'utilisation de méthodes de réduction de dimension pour les variables qualitatives.
Du coup nous avons utilisé deux approches pour résoudre ces problèmes :
  - **Agrégation Manuelle** : Regroupement manuel des modalités similaires basé sur leur rareté et similarité pour simplifier l'analyse.
  - **Classification Ascendante Hiérarchique (CAH) avec Matrice de Dissimilarité de l'Indice de Dice** : Cette méthode capture efficacement les similarités entre modalités et s'adapte bien aux structures des données qualitatives. Nous avons déterminé le nombre de clusters  à l'aide de la Méthode Silhouette, de la Méthode Elbow, et de la Méthode Gap Statistic.

**Mesures de Validation Interne** :

Une fois que nous avons ddéterminer le nombre optimal de clusters lors du clustering. Des mesures de validation interne sont appliquées pour évaluer la qualité des clusters formés. Ces mesures de validation interne sont complémentaires et permettent de confirmer la pertinence du nombre de clusters choisi ainsi que la qualité du clustering.

- Indice de Dunn : Évalue la qualité des clusters en termes de séparation et de compacité.
- Entropie : Vérifie la pureté des clusters.
- Moyenne au Sein des Clusters : Indique la compacité à l’intérieur des clusters.

Ces mesures assurent que les clusters formés sont significatifs


**Étapes de Gestion des Variables Qualitatives** :

- Examen Initial : Utilisation de la fonction generate_frequency_table pour identifier les problèmes potentiels.
- Nettoyage Initial : Utilisation de la fonction nettoyer_variable pour supprimer les niveaux indésirables et les valeurs manquantes.
- Choix de l’Approche de Gestion : Décision entre agrégation manuelle ou automatique selon la complexité de la variable qualitative et les problèmes identifiés.





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



## Modélisation

Nous cherchons à repondre à cette question centrale : **existe-t-il une influence statistique entre l’évaluation de l’entreprise et le salaire des employés ?**

Nous avons opté pour un modèle de régression linéaire en raison de sa simplicité et de son interprétabilité

### Développement du Modèle
L'étape de la modélisation se déroule en deux phases. Dans un premier temps, nous cherchons à déterminer quelle méthode de régression donne les meilleurs résultats : l'imputation des données manquantes ou la suppression de celles-ci. Ensuite en fonction de la méthode qui produit les meilleurs résultats, nous vérifions la fiabilité du modèle de régression en examinant les hypothèses liées aux résidus.

- **Choix du modèle :** Nous avons opté pour un modèle de régression linéaire en raison de sa simplicité et de son interprétabilité, particulièrement adapté pour analyser la relation entre le salaire des employés et l'évaluation de l'entreprise.

### Première Étape : Comparaison des Méthodes de Régression simple avec imputation vs Régression simple avec suppression des données manquantes

#### 1.1 Sélection des Modèles d'Imputation avec la Fonction MICE
Pour résoudre les problèmes de données manquantes, nous avons utilisé la fonction MICE en explorant différentes méthodes d'imputation adaptées à la nature quantitative des variables.
Nous avons initialiser trois argument important de cette fonction. Pour identifier le meilleur ensemble de données pour une méthode et choisir la meilleure méthode, nous 
utiliserons la métrique R² (coefficient de détermination) du modèle linéaire. Enfin pour la qualité de l'imputation nous avons examiné la convergence du processus. Cette évaluation implique l'analyse du comportement de la distribution de notre variable explicative imputée et l'observation des interactions au fil des itérations.


### 1.2 : Comparaison du Modèles avec Données Complètes Imputées et Données après Suppression des Valeurs Manquantes

#### Critères de Comparaison
Nous avons utilisé la fonction `glance` du package `broom` pour évaluer les modèles en nous basant sur :

- **Coefficient de Détermination (R²) :** Indique la capacité du modèle à expliquer les variations observées.
- **Erreur Standard Résiduelle (RSE ou sigma) :** Mesure la précision du modèle.
- **Critère d'Information d'Akaike (AIC) et Critère d'Information Bayésien (BIC) :** Évaluent l'équilibre entre ajustement et simplicité du modèle.


### Étape 2 : Validation du Modèle Linéaire - Hypothèses des Résidus

Après avoir choisi la méthode d'imputation optimale et comparé les modèles avec données complètes imputées et avec suppression des valeurs manquantes, nous validons notre modèle de régression linéaire basé sur les données complètes imputées. Cette validation va au-delà des simples métriques de performance et se concentre sur les hypothèses fondamentales du modèle pour garantir la fiabilité et la pertinence des résultats.

#### 2.1 Analyse des Résidus préliminaire
Nous débutons cette phase en examinant les résidus de la régression. Cette étape cruciale nous permet d'évaluer la pertinence du modèle et de détecter d'éventuelles anomalies.

- **Le constat du Nuage de Points des Résidus cest fait sur  :**

  - Répartition Aléatoire : Nous vérifions l'absence de structure particulière dans le nuage de points des résidus, ce qui suggère que le modèle capture adéquatement la relation entre 
                            la variable dépendante et la variable indépendante.
  - Symétrie par rapport à l'Axe des Abscisses : Nous évaluons la symétrie des résidus pour confirmer leur distribution normale, une hypothèse fondamentale pour l'application de tests 
                                                 statistiques et d'intervalles de confiance.
    
- **Vérification des Points Aberrants, Leviers et Influents :**

  - Points Aberrants : Identification via les résidus et la loi de Student. Un résidu significativement élevé indique une observation potentiellement aberrante.
  - Points Leviers :   Analyse de la matrice de projection avec des seuils spécifiques. Un point est considéré comme levier si la valeur dépasse certains seuils.
  - Points Influents : Utilisation de la distance de Cook pour évaluer l'influence des observations sur l'estimation des paramètres. Une observation est influente si sa distance de Cook 
                       est élevée.

#### 2.2 Hypothèses Fondamentales du Modèle
Pour vérifier ces hypothèses, nous utiliserons des graphiques et des statistiques spécifiques.
Les résultats de la régression linéaire soient fiables, certaines hypothèses sur les erreurs doivent être respectées :

- **Linéarité** : La relation entre la variable indépendante (X) et la variable dépendante (Y) doit être linéaire.
   - **Graphiques** :
      - *Residuals vs Fitted* : Permet une inspection visuelle de l'absence de structure systématique dans la distribution des résidus par rapport aux valeurs ajustées.

  - **Test** :
     - *Test de rainbow* : Quantifie la non-linéarité dans le modèle. Un rejet significatif de l'hypothèse nulle suggère une non-linéarité résiduelle, indiquant une possible violation 
                           de l'adéquation linéaire.

- **Normalité** : Les erreurs doivent suivre une distribution normale.Lors de l’analyse univariée, nous avons observé des queues dans les distributions des variables, suggérant des 
                  déviations de la normalité, notamment aux extrémités de la distribution.
    + **Graphique** :
       - *QQ Plot (Quantile-Quantile Plot)* : Compare les quantiles des résidus avec ceux d’une distribution normale. Des déviations mineures peuvent apparaître significatives en raison 
                                              de la taille importante de notre échantillon.
    - **Test** :
        - *Test d’Anderson-Darling* : Ce test est plus performant que d'autres tests de normalité car il accorde une attention particulière aux déviations dans les queues de 
                                      distribution. Contrairement à des tests comme le test de Shapiro-Wilk, qui est plus sensible aux déviations dans la partie centrale de la 
                                      distribution, le test d'Anderson-Darling est plus rigoureux pour détecter les écarts dans les extrémités. Par exemple, si les résidus montrent des 
                                      queues épaisses (écarts importants aux extrémités), le test d'Anderson-Darling peut détecter ces déviations plus efficacement, fournissant ainsi 
                                      une évaluation plus précise de la normalité dans des situations où les autres tests pourraient passer à côté des déviations critiques.


- **Homoscédasticité** : La variance des erreurs doit être constante à tous les niveaux de la variable indépendante.
    - **Graphique**
        - *Residuals vs Fitted Values* : Ce graphique montre les résidus en fonction des valeurs ajustées. Chaque point représente une observation, et sa position verticale 
                                         indique le résidu pour la valeur ajustée correspondante.
        - *Graphique “Scale-Location* : Ce graphique représente la racine carrée des valeurs absolues des résidus en fonction des valeurs ajustées (prédictions du modèle).
   
    - **Test** :
       - *Test de Breusch-Pagan* : Évaluer si la variance des résidus est constante à travers les valeurs ajustées. Si la p-valeur associée à la statistique de test est supérieure au 
                                  seuil de 0,05, nous ne rejetons pas l’hypothèse nulle qui est la variance des résidus est constante (homoscédasticité) et concluons à 
                                  l’homoscédasticité.


  
- **Indépendance** : Les erreurs doivent être indépendantes entre elles.
    - **Graphique** 
        - Résidus vs Ordre d’Observation* : Trace les résidus en fonction de l’ordre d’observation. L'absence de structure systématique ou de motifs dans le graphique suggère 
                                                que les résidus sont indépendants. La présence de motifs pourrait indiquer une dépendance temporelle ou sérielle.
        - *Autocorrélogramme des Résidus* : Affiche l'autocorrélation des résidus à différents retards (lags). Des barres situées près de zéro indiquent une indépendance des résidus. La 
                                            présence de barres significatives à certains retards suggère une corrélation sérielle, indiquant une dépendance entre les résidus.
        - *Corrélogramme Partiel (PACF Plot)* : Affiche la corrélation partielle entre les résidus à différents retards, en contrôlant les effets des retards intermédiaires.
                                              Un PACF plot montrant des valeurs proches de zéro pour tous les retards suggère que les résidus sont indépendants. Des pics significatifs à 
                                              certains retards peuvent indiquer des corrélations sérielles non capturées par les autres outils.
    - **Test**
        -  *Test de Durbin-Watson* : Évaluer la présence de corrélation sérielle entre les résidus.
              - *Hypothèse Nulle (H0)* : Les résidus sont indépendants (absence de corrélation sérielle).
              - *Hypothèse Alternative (H1)* : Les résidus sont corrélés (présence de corrélation sérielle).
   Une statistique proche de 2 indique une absence de corrélation sérielle et donc une indépendance des résidus. Des valeurs significativement inférieures à 2 suggèrent une corrélation 
   positive entre les résidus, tandis que des valeurs significativement supérieures à 2 indiquent une corrélation négative. Si les p-valeurs associées aux tests statistiques montrent 
   des résultats inférieurs à 0,05, cela pourrait indiquer la présence de corrélation sérielle, nécessitant des ajustements ou des modèles alternatifs.





Dans notre cas, si la p-valeur est inférieure à un seuil de 0,05, nous rejetterons l’hypothèse nulle selon laquelle les données suivent une distribution normale. Pour vérifier ces hypothèses, nous utiliserons des graphiques comme le QQ plot et des statistiques spécifiques telles que le test d’Anderson-Darling, afin d’évaluer la normalité des résidus et détecter les éventuelles déviations de la distribution normale.


Nous avons vérifié les résidus pour s'assurer que les hypothèses de la régression linéaire sont respectées à la fois sur l'ensemble d'entraînement et sur l'ensemble de test :

#### 3.1 Ensemble d'Entraînement
- **Normalité des résidus :** Test de Shapiro-Wilk, QQ plot.
- **Homoscédasticité :** Plots de résidus.
- **Indépendance des résidus :** Vérification visuelle et tests statistiques.
- **Absence d'autocorrélation :** Test de Durbin-Watson.

#### 3.2 Ensemble de Test
- **Normalité des résidus :** Test de Shapiro-Wilk, QQ plot.
- **Homoscédasticité :** Plots de résidus.
- **Indépendance des résidus :** Vérification visuelle et tests statistiques.
- **Absence d'autocorrélation :** Test de Durbin-Watson.

### Conclusion
Les résultats obtenus après la vérification des résidus sur les ensembles d'entraînement et de test confirment la robustesse et la fiabilité du modèle de régression linéaire. Les hypothèses de la régression étant respectées, nous pouvons interpréter les résultats avec confiance.

Pour des détails supplémentaires et des visualisations, veuillez consulter le notebook associé.
  
- **Préparation des Données :**
  - **Imputation des données manquantes :** Utilisation de l'algorithme MICE pour imputer les valeurs manquantes.
  - **Suppression des données manquantes :** Élimination des entrées contenant des valeurs manquantes.
  
- **Entraînement du modèle :** Le modèle a été entraîné en utilisant les deux jeux de données préparés. Les hyperparamètres du modèle, le cas échéant, ont été optimisés pour chaque méthode.

- **Validation du modèle :** La validation du modèle a été réalisée en utilisant une approche de validation croisée pour assurer la robustesse des résultats. Les performances des modèles ont été évaluées et comparées pour déterminer l'impact de chaque méthode sur la performance du modèle.

#### Deuxième Étape : Vérification de la Fiabilité du Modèle
- **Évaluation du Modèle :** Une fois la meilleure méthode identifiée, nous procédons à une évaluation approfondie du modèle de régression linéaire.
  - **Métriques de Performance :** Analyse des performances des modèles en utilisant des métriques telles que le coefficient de détermination (R²), l'erreur quadratique moyenne (RMSE), et la moyenne absolue des erreurs (MAE). Cette analyse permet de comprendre la précision et la fiabilité des prédictions du modèle.
  
  - **Hypothèses des Résidus :** Vérification des hypothèses liées aux résidus pour s'assurer que le modèle est fiable :
    - **Normalité des Résidus :** Utilisation de tests statistiques (comme le test de Shapiro-Wilk) et de visualisations (comme le QQ plot).
    - **Homoscédasticité :** Vérification de l'égalité des variances des résidus à travers des plots de résidus.
    - **Absence d'Autocorrélation :** Utilisation de tests comme le test de Durbin-Watson.
    - **Absence de Multicolinéarité :** Vérification de la multicolinéarité entre les variables explicatives en utilisant le facteur d'inflation de la variance (VIF).

### Conclusion
L'approche méthodique suivie dans le développement et l'évaluation des modèles nous permet de déterminer si l'imputation des données manquantes améliore la performance du modèle de régression linéaire par rapport à la simple suppression des données manquantes. Les résultats obtenus guideront nos recommandations pour le traitement des valeurs manquantes dans des analyses futures.

### Recommandations
Sur la base des résultats de notre analyse, nous proposons les recommandations suivantes :
- **Traitement des Données Manquantes :** Adopter la méthode d'imputation identifiée comme la plus performante.
- **Amélioration du Modèle :** Suggestions pour améliorer le modèle basé sur l'évaluation des résidus et autres diagnostics.


### Développement du Modèle
Pour répondre aux questions de recherche identifiées, un modèle statistique a été développé. Voici les étapes suivies :

- **Choix du modèle :** Nous avons opté pour un modèle de régression linéaire en raison de sa simplicité et de son interprétabilité, particulièrement adapté pour analyser la relation entre le salaire des employés et l'évaluation de l'entreprise.
  
- **Entraînement du modèle :** Le modèle a été entraîné en utilisant les données prétraitées, avec une attention particulière portée aux valeurs manquantes. Deux approches ont été comparées : l'imputation des données manquantes via l'algorithme MICE et la suppression des données manquantes. Cette comparaison permet d'évaluer l'impact de chaque méthode sur la performance du modèle.

- **Validation du modèle :** La validation du modèle a été réalisée en utilisant une approche de validation croisée pour assurer la robustesse des résultats. Les performances des modèles ont été évaluées et comparées pour déterminer l'impact de l'imputation des données manquantes.

- **Évaluation du Modèle :**
  - Analyse des performances des modèles en utilisant des métriques telles que le coefficient de détermination (R²), l'erreur quadratique moyenne (RMSE), et la moyenne absolue des erreurs (MAE). Cette analyse permet de comprendre la précision et la fiabilité des prédictions du modèle.

### Conclusion
L'approche méthodique suivie dans le développement et l'évaluation des modèles nous permet de déterminer si l'imputation des données manquantes améliore la performance du modèle de régression linéaire par rapport à la simple suppression des données manquantes. Les résultats obtenus guideront nos recommandations pour le traitement des valeurs manquantes dans des analyses futures.


## Modélisation
### Développement du Modèle
Pour répondre aux questions de recherche identifiées, un modèle statistique a été développé. Voici les étapes suivies :
- Choix du modèle : [Description]
- Entraînement du modèle : [Description]
- Validation du modèle : [Description]

3. **Construction du Modèle :**
   - Réalisation de la régression linéaire avec les données imputées.
   - Comparaison avec le modèle construit après suppression des données manquantes.

4. **Évaluation du Modèle :**
   - Analyse des performances du modèles.
   - Comparaison des résultats pour déterminer l'impact de l'imputation sur la qualité des prédictions.

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




