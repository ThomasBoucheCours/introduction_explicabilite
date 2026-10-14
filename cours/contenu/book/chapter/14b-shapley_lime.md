# Modèles explicables, valeurs de Shapley et LIME

Avant d'introduire des méthodes d'explicabilité avancées, on peut rappeler qu'il existe des modèles **par essence explicables**.

## 1) Modèles intrinsèquement explicables

### Régression linéaire

Dans une régression linéaire, la prédiction est une somme pondérée :

$$
\hat{y} = \beta_0 + \sum_{j=1}^{p} \beta_j x_j
$$

Interprétation :
- $\beta_j$ représente la variation attendue de $\hat{y}$ quand $x_j$ augmente d'une unité (à variables égales par ailleurs).
- Le signe de $\beta_j$ indique le sens de l'effet (positif ou négatif).

Limites :
- L'interprétation dépend de la colinéarité entre variables.
- Effets non linéaires et interactions peu capturés sans transformations.

### Arbre de décision

Un arbre de décision suit des règles du type :
- "si température > seuil, alors..."
- "sinon, aller à une autre règle"

Pourquoi c'est interprétable :
- Chaque prédiction suit un **chemin de décisions** lisible.
- Les seuils sont explicites et justifiables métier.

Limites :
- Un arbre profond devient difficile à lire.
- Une forêt (Random Forest) ou du boosting perd cette lisibilité locale directe.

## 2) LIME (Local Interpretable Model-agnostic Explanations)

Idée principale :
- On explique **localement** une prédiction en remplaçant le modèle complexe par un modèle interprétable simple (souvent linéaire) autour de l'observation étudiée.

Étapes :
1. Générer des points synthétiques proches de l'observation.
2. Prédire ces points avec le modèle "boîte noire".
3. Donner plus de poids aux points les plus proches.
4. Ajuster un modèle simple local.
5. Lire les coefficients du modèle local comme explication.

| Avantages ✅ | Limites / inconvénients ⚠️ |
|---|---|
| Très intuitif localement; agnostique au modèle. | Sensible au voisinage simulé et aux hyperparamètres; stabilité parfois limitée; fidélité locale, pas globale. |

#### Année et référence

2016 : Ribeiro, Singh, Guestrin, "Why Should I Trust You?: Explaining the Predictions of Any Classifier".

## 3) Idée des valeurs de Shapley

Petit préambule historique :
- Les valeurs de Shapley ne viennent pas du machine learning à l'origine.
- Elles ont été proposées en 1953 par **Lloyd Shapley**, en théorie des jeux coopératifs, pour répartir équitablement un gain entre plusieurs "joueurs".
- Plus tard, cette idée a été adaptée à l'explicabilité : les "joueurs" deviennent les variables du modèle.

Les valeurs de Shapley viennent donc de la théorie des jeux coopératifs.

Analogie :
- Chaque variable est un "joueur".
- La prédiction est le "gain" à répartir.
- La contribution de chaque variable est sa part moyenne de gain sur toutes les coalitions possibles.

Formellement, pour une variable $i$:

$$
\phi_i(f, x) = \sum_{S \subseteq N \setminus \{i\}} \frac{|S|!(|N|-|S|-1)!}{|N|!}
\big[f_{x}(S \cup \{i\}) - f_{x}(S)\big]
$$

Où :
- $N$ est l'ensemble des variables,
- $S$ une coalition de variables,
- $f_x(S)$ la prédiction du modèle quand seules les variables de $S$ sont connues (les autres étant marginalisées).

Propriété clé :
- **Additivité locale** : prédiction = baseline + somme des contributions.

### Comment évaluer $f_x(S)$ en pratique ?

Dans la formule, $f_x(S)$ signifie :
- on garde les variables de $S$ à leurs valeurs de l'observation $x$,
- on "retire" les autres variables,
- puis on calcule la prédiction moyenne du modèle.

En pratique, "retirer" une variable ne veut pas dire la mettre à zéro arbitrairement.
On utilise un **masquage** appuyé sur un jeu de données de référence (background) :

1. On choisit une observation de référence $z$ dans le background.
2. On construit une entrée mixte :
  - variables de $S$ prises dans $x$,
  - variables hors $S$ prises dans $z$.
3. On évalue le modèle sur cette entrée mixte.
4. On moyenne sur plusieurs observations de référence $z$.

Notation utile :

$$
f_x(S) \approx \mathbb{E}_{z \sim \text{background}}\left[f\big(x_S, z_{\bar{S}}\big)\right]
$$

Conséquence pédagogique importante :
- la qualité de l'explication dépend du choix du background,
- un background non représentatif peut biaiser les contributions.

## 4) Du calcul naïf aux méthodes efficaces

Le fil conducteur du cours est le suivant :
1. partir de la théorie (calcul exact),
2. comprendre pourquoi elle devient vite coûteuse,
3. introduire des algorithmes plus rapides selon le type de modèle.

### ExactExplainer : la version la plus proche de la théorie

Principe :
- On énumère (ou optimise fortement l'énumération de) toutes les coalitions utiles.
- On reste au plus près de la définition mathématique des valeurs de Shapley.

| Avantages ✅ | Limites / inconvénients ⚠️ |
|---|---|
| Référence théorique claire; interprétation fidèle à la formule. | Coût combinatoire très élevé; peu scalable quand le nombre de variables augmente. |

#### Année et référence

Pas de papier canonique dédié dans SHAP; implémentation explicite dans SHAP (documentation dédiée visible dès 2020).

### Méthodes agnostiques (boîte noire) : approcher le calcul exact

#### SamplingExplainer

Principe :
- Approximation par échantillonnage de coalitions/permutations.

Comment c'est appliqué :
1. On tire aléatoirement des coalitions (ou des ordres de variables).
2. Pour chaque tirage, on calcule une contribution marginale d'une variable.
3. On répète un grand nombre de fois.
4. On moyenne les contributions observées.

Intuition :
- plus le nombre d'échantillons est grand, plus l'approximation se rapproche de la valeur de Shapley théorique,
- mais le temps de calcul augmente.

| Avantages ✅ | Limites / inconvénients ⚠️ |
|---|---|
| Flexible (boîte noire); compromis précision/temps réglable. | Résultats bruités si peu d'échantillons; temps parfois important pour stabiliser. |

#### Année et référence

Pas de papier SHAP canonique dédié; méthode déjà présente dans SHAP au plus tard en 2019 (citée comme alternative dans les notes de version v0.33.0).

#### PermutationExplainer

Principe :
- Approxime les contributions en testant plusieurs ordres d'arrivée des variables.

Comment c'est appliqué :
1. On tire une permutation des variables, par exemple $(x_3, x_1, x_2, x_4)$.
2. On part d'une entrée "masquée" (baseline/background).
3. On ajoute les variables une par une selon l'ordre tiré.
4. À chaque ajout d'une variable, on mesure la variation de prédiction : c'est sa contribution marginale pour cette permutation.
5. On recommence sur de nombreuses permutations, puis on moyenne.

Intuition :
- la contribution finale d'une variable est sa contribution moyenne sur des contextes d'arrivée différents.

| Avantages ✅ | Limites / inconvénients ⚠️ |
|---|---|
| Très pédagogique; applicable à de nombreux modèles. | Sensible au nombre de permutations; sensible au choix du background. |

#### Année et référence

Implémentation SHAP introduite explicitement en 2019 (release v0.33.0 : "A new PermutationExplainer").

### Méthodes spécifiques (quand on connaît la structure du modèle)

#### TreeSHAP (modèles d'arbres)

Principe :
- Exploite la structure interne des arbres (Decision Tree, Random Forest, Gradient Boosting, CatBoost, XGBoost, LightGBM).
- Évite l'énumération naïve de toutes les coalitions.

Comment la structure des arbres aide à construire les coalitions :
1. Une coalition revient à "connaître" certaines variables et à "masquer" les autres.
2. Dans un arbre, connaître une variable signifie pouvoir suivre certains splits du chemin racine -> feuille.
3. Masquer une variable signifie que, au split correspondant, on ne suit pas une branche unique : on pondère les deux branches selon leur probabilité/couverture dans les données de référence.
4. TreeSHAP propage ces poids le long des chemins et agrège efficacement les contributions sans énumérer toutes les coalitions une par une.

Intuition :
- l'algorithme fait une programmation dynamique sur les chemins de l'arbre,
- ce qui remplace une complexité exponentielle par un calcul beaucoup plus rapide.

| Avantages ✅ | Limites / inconvénients ⚠️ |
|---|---|
| Très rapide sur les arbres; stable en pratique sur tabulaire. | Spécifique aux modèles d'arbres; plus technique à expliquer en détail. |

#### Année et référence

2018 (prépublication) / 2020 (publication) : Lundberg et al., "Consistent Individualized Feature Attribution for Tree Ensembles".

#### LinearExplainer (modèles linéaires)

Principe :
- Utilise la structure linéaire pour calculer les contributions.

| Avantages ✅ | Limites / inconvénients ⚠️ |
|---|---|
| Rapide; cohérent avec la lecture des coefficients. | Limité aux modèles linéaires. |

#### Année et référence

Pas de papier SHAP canonique dédié; nouvel explainer introduit en 2018 (release v0.21.0 de SHAP).

#### DeepExplainer (réseaux de neurones)

Principe :
- Approxime des valeurs de Shapley pour des modèles profonds.

Comment c'est appliqué :
1. On choisit un jeu de référence (background) représentant des entrées "neutres".
2. Pour une observation $x$, on la compare à chaque référence $z$ du background.
3. Le réseau propage l'effet de la différence $(x-z)$ des entrées vers la sortie (règles de type DeepLIFT/backprop adaptée).
4. On obtient des contributions par variable pour chaque paire $(x,z)$.
5. On moyenne ces contributions sur les différentes références $z$.

Intuition :
- on explique non pas "la prédiction brute", mais "le déplacement de la prédiction" entre une référence et l'observation,
- puis on moyenne ces déplacements pour stabiliser l'explication.

| Avantages ✅ | Limites / inconvénients ⚠️ |
|---|---|
| Adapté au deep learning; utile pour des modèles complexes. | Dépend fortement du background; plus difficile à diagnostiquer pédagogiquement. |

#### Année et référence

Pas de papier SHAP canonique dédié; implémentation pleinement fonctionnelle annoncée en 2018 (release v0.19.3). Cadre théorique SHAP : Lundberg & Lee, 2017.

## 5) LIME puis SHAP : place dans le cours

- LIME d'abord :
  - très utile pour l'intuition locale,
  - bonne porte d'entrée pour expliquer rapidement une prédiction.
- SHAP ensuite :
  - cadre théorique unifié,
  - continuité entre local et global,
  - utile pour comparer les individus de manière systématique.

En pratique :
- Commencer par LIME pour installer l'intuition locale.
- Puis, si l'objectif est d'ancrer la théorie : introduire **ExactExplainer** sur un petit exemple.
- Si le modèle est un arbre : privilégier **TreeSHAP**.
- Si le modèle est une boîte noire générale : utiliser **SamplingExplainer** ou **PermutationExplainer**.
- Garder LIME comme point de comparaison local tout au long du chapitre.

## 6) Message pédagogique à retenir

- Commencer par des modèles interprétables (régression linéaire, arbre simple) pour poser les bases.
- Introduire ensuite LIME pour construire l'intuition locale.
- Puis présenter SHAP en progression : exact (naïf) puis approximations puis méthodes spécifiques.
- Toujours rappeler qu'une explication n'est pas une causalité : elle décrit le comportement du modèle, pas le monde réel.
