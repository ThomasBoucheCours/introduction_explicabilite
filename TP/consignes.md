# TP 
Quatres sessions de TP : 
- 1 : Prise en main des données et du modèle, manipulation de la Webapp Shapash, compréhensions des graphiques d'explicabilités, premières analyses
- 2 : Améliorations du modèles
- 3 : Poursuite des améliorations à partir de feature plus avancés : Eurybia, Rapport Shapash, Explainability Quality Metrics 
- 4 : Fourniture du rapport Shapash et présentations

## Session 1

- Remplir le [sondage](https://framaforms.org/barometres-representations-sociales-du-changement-climatique-1700638177
)
- (Recommandé) : Créer un environnement avec conda prompt `conda create -n shapash_env` ou un venv `python -m venv .venv`
- Activer ce nouvel environnmement : pour conda `source activate mon_env` ou pour un venv `source .venv/bin/activate`
- Installer les librairies `shapash` et `catboost` (`pip install shapash`). 
- Télécharger les données : [ici](data/BDD OpinionWay pour l'ADEME - Barometre changement climatique - Compilation 2023.csv)
- 2 choix de notebook : une [classification](TP/Travaux_pratiques_barometre_changement_climatique_classification.ipynb) ou une [regression](TP/Travaux_pratiques_barometre_changement_climatique_regression.ipynb)

## Evaluation
### Présentation
- Durée : x minutes
- Choisir un cas d'usage du modèle pour illustrer l'usage de l'explicabilité

### Rapport Shapash
- Livrer un rapport Shapash associé à l'explicabilité du modèle

### Critères d'évaluations
- Les méthodes d'explicabilités sont comprises
- La bonne compréhension et explication du fonctionnement du modèle
- Les améliorations ou pistes d'améliorations apportées au modèle
- L'utilité de l'explicabilité dans l'usage
