# AGENTS (local, non versionne)

Ce fichier sert de memo local pour les agents/outils qui travaillent sur ce depot.
Il ne doit jamais etre commit/push.

## Objectif du repo
- Fournir un cours d'introduction a l'explicabilite des modeles de machine learning.
- Supporter des TP (classification, regression, multiclassification) autour du barometre changement climatique (ADEME/OpinionWay).
- Produire une documentation pedagogique publiee via Jupyter Book.

## Ce qu'on doit faire
- Preserver la finalite pedagogique: clarte, reproductibilite, interpretation des resultats.
- S'appuyer sur les notebooks de `TP/` pour les analyses et exercices.
- Maintenir la coherence du contenu du cours dans `cours/contenu/book/chapter/`.
- Garder les dependances minimales et explicites (`cours/requirements.txt`, `requirements.dev.txt`).
- Verifier que les outputs fournis (rapports explicabilite) restent aligns avec les notebooks et donnees.

## Ce qu'on doit eviter
- Ne pas casser la generation de doc Jupyter Book (`_build/` est un artefact, non source).
- Ne pas introduire de changements hors perimetre pedagogique sans justification.
- Ne pas versionner d'artefacts locaux/environnement (venv, checkpoints, fichiers temporaires).
- Ne pas modifier les donnees source brutes sans tracer clairement la raison.

## Principes TP (rappel)
- Session 1: prise en main donnees/modele + Shapash.
- Sessions suivantes: ameliorations du modele, features avancees, qualite d'explicabilite, rapport final.
- Livrables: presentation d'un cas d'usage + rapport Shapash.

## Regle Git locale
- Ce fichier est ignore par Git via `.gitignore` (`agents.md` et `AGENTS.md`).
- Ne jamais inclure ce fichier dans un commit ou une PR.
