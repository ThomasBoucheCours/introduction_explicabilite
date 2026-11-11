# TP 
Quatres sessions de TP : 
- 1 : Prise en main des données et du modèle, manipulation de la Webapp Shapash, compréhensions des graphiques d'explicabilités, premières analyses
- 2 : Poursuite des analyses à partir des données du Master, Améliorations du modèles : Feature engineering, Performance du modèle, Format de la restitution,...
- 3 : Poursuite des améliorations à partir de fonctionnalités plus avancées : Eurybia, Rapport Shapash, Explainability Quality Metrics 
- 4 : Fourniture du rapport Shapash et présentations

## Session 1

- Remplir le [sondage](https://framaforms.org/barometres-representations-sociales-du-changement-climatique-1700638177
)
- cloner le repo `git clone mon_repo`
- Créer un environnement `python -m venv .venv`
- Activer ce nouvel environnmement : `source .venv/bin/activate`
- Installer les librairies `pip install -r ./chemin/requirements.dev.txt`
- 2 notebooks : une [classification](TP/session1/TP1_classification.ipynb) et une [regression](TP/session1/TP1_regression.ipynb)

  Objectifs :
  - Accéder à la web shapash à partir du notebook classification
  - Prendre en main la web app
  - Comprendre le feature engineering et les données
  - Même chose sur la régression

## Session 2
- Travailler sur les notebooks présents dans le dossier [session2](TP/session2): En fin de notebook l'application permet de d'afficher les prédictions du modèle sur les sondages

### Objectif

À l’aide de Shapash, vous devez comprendre le fonctionnement du modèle, puis proposer des pistes pour l’améliorer.

Pour chaque question, il est conseillé de noter :
- une réponse rédigée (2 à 5 phrases)
- un graphique : capture d’écran du graphique ou code permettant de l’obtenir
- une interprétation : ce que cela signifie concrètement, pas seulement ce que montre le graphique

#### 1 - Comprendre le modèle
##### Fonctionnement global
Quelles variables pèsent le plus, et comment ?

Quelles sont les variables qui influencent le plus la prédiction ? Quelles variables expliquent qu’il soit prédit plutôt faible ou plutôt élevé ?
Pour les variables principales, comment la valeur de la variable modifie-t-elle la prédiction ?
Certaines variables ont-elles un effet contre-intuitif (opposé à ce qu'attendrait le métier) ? Proposez une hypothèse pour l’expliquer (biais, corrélation avec une autre variable, problème de données…).

##### Les erreurs du modèle
Où et pourquoi le modèle se trompe-t-il ?

Identifiez les individus pour lesquels l’écart entre prédiction et valeur réelle est le plus important. Quelles variables expliquent ces gros écarts ?
Ces erreurs ont-elles un point commun (profil, période, valeur d’une variable) ?

##### Les différences entre groupes
Le modèle se comporte-t-il de la même façon partout ?

Vagues d’enquête : le modèle utilise-t-il les mêmes variables, de la même manière, d’une vague à l’autre ? Quelles différences constatez-vous ?
Sous-populations : choisissez au moins deux sous-populations pertinentes (par exemple par secteur, taille, région…). Les variables importantes et la qualité des prédictions sont-elles différentes ?

#### 2 - Proposer des améliorations
Cette partie s’appuie sur votre compréhension du fonctionnement du modèle. 

##### Limites des données
Quelles limites du jeu de données observez-vous (déséquilibre, valeurs manquantes, sous-représentation d’un groupe, variables peu fiables, peu d’observations dans certaines vagues…) ?
Quel impact ces limites ont-elles sur le modèle ?

##### Transformations et création de variables
Diagnostic : certaines transformations existantes posent-elles problème (encodage, regroupement de modalités, traitement des valeurs manquantes, création de variables) ? Qu’est-ce qui le montre dans les explications ?
Propositions : quelles nouvelles transformations ou nouvelles variables pourraient améliorer le modèle ? Pourquoi ?

##### Nettoyage des données
Faut-il corriger certaines valeurs ou écarter certaines réponses (anomalies, incohérences, réponses suspectes) ? 

##### Synthèse
Classez vos propositions par priorité (impact attendu / effort nécessaire).
Si possible, testez au moins une proposition et mesurez son effet.


## Evaluation
### Présentation
- Durée : x minutes
- Choisir un cas d'usage du modèle pour illustrer l'usage de l'explicabilité :
    - A quoi sert le modèle ? 
    - Qui présente ? et à quel public ? 

### Rapport Shapash
- Livrer un rapport Shapash associé à l'explicabilité du modèle

### Critères d'évaluations
- Les méthodes d'explicabilités sont comprises
- La bonne compréhension et explication du fonctionnement du modèle
- La bonne appropriation des données 
- Les améliorations ou pistes d'améliorations apportées au modèle
- L'utilité de l'explicabilité dans l'usage
