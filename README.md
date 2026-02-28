# 🏥 TP1 — Prétraitement des Données Médicales

> Pipeline complet de prétraitement de données en **Python (Google Colab)**, appliqué à un jeu de données médicales cardiovasculaires.

---

## 📋 Description

Ce dépôt contient le travail pratique n°1 du cours de **Prétraitement des Données** (Filière IA). L'objectif est d'implémenter en Python un pipeline de nettoyage et préparation de données, couvrant :

- Exploration statistique et visualisation
- Suppression des variables invariantes et des doublons
- Traitement des données éparses
- Imputation des valeurs manquantes (kNN, missForest, LOCF, MICE, Hybride)
- Détection et correction des valeurs aberrantes (outliers)

---

## 📁 Structure du Projet

```
TP-Pretraitement-Donnees-Medicales/
├── README.md
├── requirements.txt
├── data/
│   └── namefile.xlsx          # Jeu de données médicales (271 patients)
├── notebooks/
│   └── TP1_AI_complet.ipynb   # Notebook Google Colab complet
└── docs/
    └── Compte_Rendu_TP1_v2.pdf  # Compte rendu au format PDF
```

---

## 🗃️ Jeu de Données

Le fichier `data/namefile.xlsx` contient des données médicales relatives à **271 patients**, avec les variables suivantes :

| Variable | Type | Description |
|---|---|---|
| `age` | Numérique | Âge du patient (années) |
| `typedouleur` | Catégoriel | Type de douleur thoracique |
| `tauxmax` | Numérique | Taux cardiaque maximal atteint (bpm) |
| `sucre` | Catégoriel | Glycémie à jeun > 120 mg/dl |
| `angine` | Catégoriel | Angine induite par l'effort |
| `depression` | Numérique | Dépression ST induite par l'exercice |
| `nationaite` | Catégoriel | Variable invariante (supprimée) |
| `tension` | Numérique | Pression artérielle (80% de NaN → supprimée) |
| `fumeur` | Catégoriel | Statut tabagique (58% de NaN → supprimée) |

---

## 🔬 Étapes du Pipeline

### I. Exploration & Description
- `describe(include='all')` — équivalent de `summary()` en R
- `sns.pairplot()` — matrice de nuages de points croisés

### II. Données Invariantes
- Détection par `nunique(dropna=False) <= 1`
- Colonne supprimée : **`nationaite`** → (271, 10)

### III. Doublons
- `drop_duplicates()` — 1 doublon supprimé → (270, 10)

### IV. Données Éparses
- Seuil : > 40% de NaN
- Colonnes supprimées : **`tension`** (80%) et **`fumeur`** (58%) → (270, 8)

### V. Imputation des Valeurs Manquantes (19 NaN)

| Méthode | Bibliothèque | RMSE |
|---|---|---|
| **kNN (k=5)** | `sklearn.KNNImputer` | **19.78 🥇** |
| MICE | `sklearn.IterativeImputer + BayesianRidge` | ~20-21 |
| missForest | `sklearn.IterativeImputer + RandomForest` | 21.28 |
| LOCF | `pandas.ffill()` | 25.73 |
| **Hybride** | kNN (num.) + Mode (cat.) | — |

### VI. Outliers
- Détection par méthode IQR (boxplot)
- **1 outlier détecté** : patient 101, `tauxmax = 71 bpm`
- **Correction par kNN** : valeur estimée → **135.0 bpm**

---

## 🚀 Utilisation

### Sur Google Colab (recommandé)

1. Cloner le dépôt dans votre Google Drive
2. Ouvrir `notebooks/TP1_AI_complet.ipynb` dans Google Colab
3. Mettre à jour le chemin du fichier dans la cellule de chargement :
```python
file_path = '/content/drive/MyDrive/<votre_chemin>/data/namefile.xlsx'
```
4. Exécuter toutes les cellules (`Exécution > Tout exécuter`)

### En local

```bash
git clone https://github.com/ZaikOSS/TP-Pretraitement-Donnees-Medicales.git
cd TP-Pretraitement-Donnees-Medicales
pip install -r requirements.txt
jupyter notebook notebooks/TP1_AI_complet.ipynb
```

---

## 📦 Dépendances

Voir `requirements.txt`. Principales bibliothèques :

- `pandas` — manipulation des données
- `numpy` — calculs numériques
- `matplotlib` / `seaborn` — visualisation
- `scikit-learn` — imputation (KNNImputer, IterativeImputer)
- `openpyxl` — lecture des fichiers Excel

---

## 📄 Compte Rendu

Le compte rendu complet (PDF) est disponible dans `docs/Compte_Rendu_TP1_v2.pdf`. Il détaille chaque étape du pipeline avec les résultats, les graphiques et les discussions théoriques sur les outliers et les méthodes d'imputation.

---

## 👤 Auteur

**ZaikOSS** 
Année Universitaire : 2024–2025
