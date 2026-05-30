# CANVAS — Projet ML / Data Science

> Fichier de référence pour structurer et documenter tout nouveau projet.
> Copier ce fichier dans chaque nouveau projet et remplir chaque section.

---

## 1. Vue d'ensemble

| Champ            | Valeur                                      |
|------------------|---------------------------------------------|
| Champ            | Valeur                                      |
|------------------|---------------------------------------------|
| Nom du projet    | Housing Price Prediction                    |
| Objectif         | Prédire le prix de vente d'une maison       |
| Type de problème | Régression supervisée                       |
| Dataset          | Kaggle — House Prices (Ames, Iowa)          |
| Taille dataset   | 1460 lignes × 81 colonnes                   |
| Variable cible   | `SalePrice`                                 |
| Lien Kaggle      | house-prices-advanced-regression-techniques |
| Auteur           | Alexis Fokam                                |
| Date de début    | 2026-05                                     |

---

## 2. Structure du projet

```
project-name/
├── data/
│   ├── train.csv
│   ├── test.csv
│   └── data_description.txt
├── notebooks/
│   └── main.ipynb
├── model.pkl
├── requirements.txt
├── analyse.txt          ← observations EDA (texte libre)          ← ce fichier
└── README.md
```

---

## 3. Dataset — Analyse initiale

### 3.1 Dimensions & types

| Info               | Valeur |
|--------------------|--------|
| Nb lignes          | 1460   |
| Nb colonnes        | 81     |
| Colonnes numériques| 38     |
| Colonnes catégo.   | 43     |

### 3.2 Variable cible

| Stat       | Valeur     |
|------------|------------|
| Min        | 34 900 $   |
| Médiane    | 163 000 $  |
| Moyenne    | 180 921 $  |
| Max        | 755 000 $  |
| Distribution | Asymétrique droite (moyenne > médiane) |
| Transformation nécessaire | `log(SalePrice)` avant entraînement |

### 3.3 Valeurs manquantes

| Colonne       | % NaN | Type    | Action                          |
|---------------|-------|---------|----------------------------------|
| Alley         | —     | Logique | Imputer "None" (pas de ruelle)  |
| PoolQC        | —     | Logique | Imputer "None" (pas de piscine) |
| Fence         | —     | Logique | Imputer "None" (pas de clôture) |
| MiscFeature   | —     | Logique | Imputer "None"                  |
| LotFrontage   | 17.7% | Réel    | Médiane par quartier            |
| MasVnrArea    | 0.5%  | Réel    | Imputer 0                       |
| GarageYrBlt   | —     | Logique | Lié à l'absence de garage       |

### 3.4 Colonnes mal typées

| Colonne      | Type actuel | Type correct | Action              |
|--------------|-------------|--------------|---------------------|
| MSSubClass   | int64       | str          | Convertir avant encodage |

### 3.5 Outliers

| Colonne    | Valeur max | Problème               | Décision |
|------------|-----------|------------------------|----------|
| LotArea    | 215 245   | 20× la moyenne (10 516)| Surveiller / capper |
| GrLivArea  | —         | Maisons très grandes   | Surveiller |
| SalePrice  | 755 000 $ | Quelques maisons > 700k| Surveiller |

### 3.6 Colonnes à faible variance (à supprimer)

| Colonne     | Raison                                  |
|-------------|----------------------------------------|
| Street      | Quasi toutes "Pave" — peu d'info       |
| Utilities   | Quasi toutes "AllPub" — à supprimer    |
| PoolArea    | 75% à 0 (peu de piscines)              |
| 3SsnPorch   | Majorité à 0                           |
| ScreenPorch | Majorité à 0                           |
| Id          | Identifiant séquentiel, sans valeur prédictive |

---

## 4. Feature Engineering

| Feature créée | Formule / Logique                          | Justification                          |
|---------------|--------------------------------------------|----------------------------------------|
| TotalSF       | `GrLivArea + TotalBsmtSF`                  | Surface totale = feature très prédictive |
| TotalBath     | Somme des salles de bain                   | Confort global de la maison            |
| HouseAge      | `YrSold - YearBuilt`                       | L'âge à la vente influence le prix     |

---

## 5. Pipeline de prétraitement

- [ ] Supprimer colonnes inutiles (Id, low-variance…)
- [ ] Imputer valeurs manquantes (catégorielles → "None", numériques → médiane/0)
- [ ] Corriger types (ex: MSSubClass → str)
- [ ] Appliquer log sur la cible si distribution asymétrique
- [ ] Encoder variables catégorielles (OrdinalEncoder / OneHotEncoder)
- [ ] Scaler les features numériques (StandardScaler / MinMaxScaler)

---

## 6. Modèles entraînés

| Modèle                              | Paramètres   | R² CV (5-fold)   | R² test | RMSE       | Notes           |
|-------------------------------------|-------------|------------------|---------|------------|-----------------|
| Linear Regression + StandardScaler  | défaut      | **0.9048 ± 0.007** | **0.8881** | ~21 811 $ | Meilleur modèle |
| Random Forest                       | défaut      | 0.8751 ± 0.008   | 0.8618  | ~25 043 $  |                 |

**Modèle retenu :** Linear Regression + StandardScaler  
**Raison du choix :** Meilleur R² CV et test, RMSE plus faible, plus interprétable

---

## 7. Features les plus importantes

| Rang | Feature     | Importance / Coef            |
|------|-------------|------------------------------|
| 1    | TotalSF     | Feature engineerée — surface totale |
| 2    | OverallQual | Qualité globale des matériaux |
| 3    | GrLivArea   | Surface habitable            |
| 4    | TotalBath   | Feature engineerée — salles de bain |
| 5    | HouseAge    | Feature engineerée — âge à la vente |
| 6    | GarageCars  | Capacité du garage           |

---

## 8. Observations & Décisions clés

> Copier ici les points importants de `analyse.txt`, ou les noter directement.

- **Obs 1 — SalePrice asymétrique** : distribution skewed à droite (moyenne > médiane). Appliquer `log(SalePrice)` avant entraînement.
- **Obs 2 — NaN logiques vs réels** : `Alley`, `PoolQC`, `Fence`, `MiscFeature` → NaN = catégorie "None", pas une donnée manquante.
- **Obs 3 — MSSubClass mal typé** : valeurs numériques (20, 30, 60…) mais c'est un code catégoriel → convertir en `str` avant encodage.
- **Obs 4 — Outliers** : `LotArea` max = 215 245 (20× la moyenne), quelques maisons > 700k dans `SalePrice`.
- **Obs 5 — Faible variance** : `Street`, `Utilities` → quasi constantes, à supprimer.
- **Obs 6 — Id inutile** : identifiant séquentiel sans valeur prédictive, à supprimer immédiatement.

---

## 9. Stratégie Git

```
main                        ← code final propre
├── dev                     ← développement général
├── feature/data-exploration
├── feature/data-cleaning
├── feature/feature-engineering
├── feature/model-training
├── feature/model-evaluation
└── feature/visualizations
```

---

## 10. Stack technique

```
pandas          # manipulation données
numpy           # calcul numérique
scikit-learn    # modèles ML + preprocessing
matplotlib      # visualisations
seaborn         # visualisations statistiques
joblib          # sérialisation modèle
jupyter         # notebook
```

---

## 11. Checklist avant publication (Upwork / GitHub)

- [ ] README.md complet (objectif, résultats, how to run)
- [ ] requirements.txt à jour
- [ ] Notebook propre (outputs inclus, cellules dans l'ordre)
- [ ] model.pkl sauvegardé
- [ ] .gitignore configuré (venv, .env, data brutes si trop lourdes)
- [ ] Résultats du meilleur modèle visibles dans le README

---

## Résumé projet (à remplir en dernier)

> 3 phrases max. Copier dans le profil Upwork ou la description GitHub.

Prédiction du prix de vente de maisons sur le dataset Kaggle "House Prices" (Ames, Iowa — 1460 maisons, 81 features). Pipeline complet : nettoyage, feature engineering (TotalSF, TotalBath, HouseAge), et régression avec StandardScaler. Meilleur modèle : Linear Regression — R² = 0.90, RMSE ≈ 21 811 $.

---

*Auteur : Alexis Fokam — Data Scientist & ML Engineer*  
*Upwork : https://www.upwork.com/freelancers/~01b522970676cfcfb6*
