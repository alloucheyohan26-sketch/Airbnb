# 🏠 Airbnb Price Prediction

## 📌 Présentation du projet

Ce projet consiste à développer un modèle de **Machine Learning capable de prédire le prix d'une location Airbnb** à partir des caractéristiques du logement, de l'hôte, de sa localisation et des équipements proposés.

La variable cible du projet est `log_price`, correspondant au **logarithme naturel du prix de location par nuit en dollars américains**.

L'objectif principal est de transformer des données brutes, notamment des variables textuelles, catégorielles et temporelles, en variables exploitables par des algorithmes de Machine Learning, puis de comparer plusieurs modèles afin de sélectionner le plus performant.

---

## 🎯 Objectifs

- Explorer et comprendre les données Airbnb
- Identifier et traiter les valeurs manquantes
- Transformer les variables textuelles et catégorielles
- Créer de nouvelles variables pertinentes (**Feature Engineering**)
- Comparer plusieurs modèles de régression
- Évaluer les modèles avec une validation croisée 5-fold
- Sélectionner le meilleur modèle
- Générer les prédictions finales sur le jeu de test

---

## 📊 Données

Le projet utilise deux jeux de données :

- `airbnb_train.csv.gz` : **22 234 logements**, avec la variable cible `log_price`
- `airbnb_test.csv.gz` : **51 877 logements**, utilisés pour générer les prédictions finales

Les données contiennent notamment des informations concernant :

- le type de logement
- le type de chambre
- le nombre de personnes pouvant être accueillies
- le nombre de salles de bain, chambres et lits
- les équipements disponibles
- la localisation
- les caractéristiques de l'hôte
- les avis
- les dates de création et de commentaires
- la description du logement
- la possibilité de réserver instantanément

---

## 🔎 Analyse exploratoire des données

Une analyse exploratoire (**EDA — Exploratory Data Analysis**) a été réalisée afin de comprendre :

- la distribution de `log_price`
- les valeurs manquantes
- les types de variables
- les relations entre les caractéristiques et le prix
- la répartition des types de logements
- la fréquence des équipements
- les corrélations entre variables

Le jeu d'entraînement contient **28 colonnes**, avec différents types de données : numériques, catégorielles, booléennes et textuelles.

---

## 🛠️ Feature Engineering

Une partie importante du projet consiste à transformer les données brutes en variables numériques exploitables par les modèles.

### Équipements

La colonne `amenities`, initialement sous forme de texte, est analysée afin d'extraire les équipements du logement.

25 équipements sont transformés en variables binaires, auxquels s'ajoute le nombre total d'équipements disponibles.

Exemples :

- Wi-Fi
- Cuisine
- Climatisation
- Chauffage
- Piscine
- Salle de sport
- Parking
- Lave-linge
- Ascenseur
- etc.

### Dates

Les variables temporelles suivantes sont transformées en nombre de jours par rapport au 01/01/2017 :

- `host_since`
- `first_review`
- `last_review`

Une variable `review_recency` est également créée afin de représenter la durée entre le premier et le dernier avis.

### Variables catégorielles

Les variables catégorielles sont transformées grâce au **Frequency Encoding** afin d'éviter une explosion du nombre de dimensions, notamment pour les codes postaux.

Les variables concernées comprennent notamment :

- `property_type`
- `room_type`
- `bed_type`
- `cancellation_policy`
- `city`
- `neighbourhood`
- `zipcode`

### Variables textuelles

La description et le nom du logement sont transformés en variables numériques représentant notamment :

- la longueur de la description
- la longueur du nom
- le nombre de mots dans la description

Au total, le processus de Feature Engineering produit **53 variables** exploitables par les modèles. :chatgpt-content-reference{index="1"}

---

## 🤖 Modélisation

Plusieurs modèles de régression ont été comparés avec une **validation croisée 5-fold** :

- Baseline — prédiction de la moyenne
- Ridge Regression
- Decision Tree
- Extra Trees
- Random Forest
- Gradient Boosting

### Résultats

| Modèle | RMSE moyen |
|---|---:|
| Baseline | 0.7188 |
| Ridge | 0.4884 |
| Decision Tree | 0.4637 |
| Extra Trees | 0.4502 |
| Random Forest | 0.4185 |
| **Gradient Boosting** | **0.3944** |

Le **Gradient Boosting** obtient la meilleure performance avec un RMSE de **0.3944 ± 0.0093** lors de la validation croisée. :chatgpt-content-reference{index="2"}

---

## 🏆 Modèle final

Le modèle sélectionné est un **Gradient Boosting Regressor**.

Les principaux hyperparamètres utilisés sont :

```text
n_estimators = 300
learning_rate = 0.05
max_depth = 5
subsample = 0.8
min_samples_leaf = 5
random_state = 42
