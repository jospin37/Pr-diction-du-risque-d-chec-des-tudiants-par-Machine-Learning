# 🎓 Système intelligent de prédiction des performances académiques des étudiants

Ce projet met en œuvre un pipeline complet de Machine Learning pour prédire le **risque d'échec / d'abandon** des étudiants à partir de leurs données socio-démographiques et académiques (jeu de données UCI). Il s'appuie sur une comparaison de plusieurs algorithmes, une optimisation d'hyperparamètres, et se conclut par un système d'aide à la décision destiné aux responsables pédagogiques.

## 📌 Objectif

À partir de variables disponibles dès les premiers semestres (profil d'entrée, moyennes S1/S2, taux de réussite, contexte socio-économique), le modèle estime la probabilité qu'un étudiant abandonne ses études, afin de permettre une intervention précoce (tutorat, accompagnement social/financier, etc.).

## 🗂️ Jeu de données

Le jeu de données (`Dataset_UCI.xlsx`, non inclus dans ce dépôt pour des raisons de taille/licence — voir [`data/README.md`](data/README.md)) contient **4 424 étudiants** et **16 variables**, dont :

| Variable | Description |
|---|---|
| `id_etudiant` | Identifiant unique de l'étudiant |
| `sexe` | Sexe de l'étudiant |
| `age_entree` | Âge à l'entrée dans le cursus |
| `niveau_entree` | Niveau de qualification à l'entrée |
| `niveau_etude_mere` / `niveau_etude_pere` | Niveau d'études des parents |
| `statut_boursier` | Statut boursier |
| `endettement` | Situation d'endettement |
| `frais_a_jour` | Frais de scolarité à jour |
| `loin_domicile` | Éloignement du domicile familial |
| `moyenne_s1` / `moyenne_s2` | Moyennes académiques semestre 1 / 2 |
| `taux_reussite_s1_pct` / `taux_reussite_s2_pct` | Taux de réussite (%) semestre 1 / 2 |
| `resultat` | Résultat final (Abandon / Inscrit / Diplômé) |
| `risque_echec` | **Variable cible** (1 = Abandon, 0 = sinon) |

⚠️ `id_etudiant` et `resultat` sont exclus des variables explicatives (fuite de données / *data leakage*).

## 🔬 Méthodologie

1. **Analyse exploratoire (EDA)** : qualité des données, distribution de la cible, variables numériques/catégorielles, matrice de corrélation.
2. **Prétraitement** : encodage One-Hot des variables qualitatives, séparation train/test stratifiée (80/20), standardisation des variables quantitatives, analyse du déséquilibre des classes.
3. **Modélisation** — comparaison de 6 algorithmes via validation croisée stratifiée (5 plis) :
   - Régression Logistique (modèle de référence)
   - Arbre de décision
   - Random Forest
   - XGBoost
   - SVM (noyau RBF)
   - Réseau de neurones (ANN / MLP)
4. **Optimisation** du meilleur modèle par `GridSearchCV`.
5. **Interprétation** de l'importance des variables.
6. **Système d'aide à la décision** : score de risque, classement des étudiants par niveau de risque, rapport automatique par étudiant avec recommandation.

## 📊 Résultats

Le **Random Forest** optimisé a été retenu comme modèle final :

| Métrique | Valeur |
|---|---|
| F1-score (validation croisée) | 0.781 |
| Accuracy (jeu de test) | 0.88 |
| Precision / Recall (classe "à risque") | 0.81 / 0.82 |
| AUC (jeu de test) | 0.924 |

## 🚀 Installation & utilisation

```bash
# Cloner le dépôt
git clone https://github.com/<votre-utilisateur>/<nom-du-depot>.git
cd <nom-du-depot>

# Créer un environnement virtuel (recommandé)
python -m venv venv
source venv/bin/activate  # Windows : venv\Scripts\activate

# Installer les dépendances
pip install -r requirements.txt

# Lancer Jupyter
jupyter notebook Dataset_UCI.ipynb
```

> Placez le fichier `Dataset_UCI.xlsx` à la racine du dépôt avant d'exécuter le notebook (voir [`data/README.md`](data/README.md)).

## 📁 Structure du dépôt

```
.
├── Dataset_UCI.ipynb       # Notebook principal (analyse, modélisation, aide à la décision)
├── data/
│   └── README.md           # Instructions pour obtenir le jeu de données
├── outputs/                # Figures et exports générés par le notebook (gitignored)
├── requirements.txt         # Dépendances Python
├── .gitignore
├── LICENSE
└── README.md
```

## 🛠️ Stack technique

Python · pandas · NumPy · scikit-learn · XGBoost · Matplotlib · Seaborn · Jupyter

## 📄 Licence

Ce projet est distribué sous licence MIT — voir le fichier [LICENSE](LICENSE).

## ⚠️ Avertissement

Ce système est un outil d'aide à la décision à visée pédagogique et ne doit pas se substituer au jugement humain des équipes pédagogiques et sociales.
