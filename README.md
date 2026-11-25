# Plateforme de Sélection de Caractéristiques par Méthodes Exactes et Métaheuristiques

Projet de Recherche Opérationnelle (ENSA) : comparaison d'approches exactes et de métaheuristiques pour la sélection de caractéristiques (*feature selection*) en apprentissage automatique.

## Objectif

Pour un jeu de données de `n` variables, il existe `2^n` sous-ensembles possibles, ce qui rend l'énumération impossible dès que `n` dépasse une vingtaine. Ce projet compare deux approches :

- **Phase exacte** (petites instances, n ≤ 15) : PLNE 0-1 et Branch & Bound, pour obtenir l'optimum global `f*`.
- **Phase approchée** : heuristiques constructives et 4 métaheuristiques, étalonnées par rapport à `f*` :
  - Recherche Tabou (RT)
  - Recuit Simulé (RS)
  - Recherche à Voisinage Variable (RVV / VNS)
  - Algorithme Génétique (AG)

## Fonction objectif

```
min f(x) = α·(1 − ScoreCV(x)) + β·(Σxj / n) + γ·Redondance(x)
```

Valeurs par défaut : α = 0,80, β = 0,15, γ = 0,05.

## Structure du projet

```
fs-metaheuristics-platform/
├── data/            # Jeux de données
├── src/
│   ├── problem.py        # Données, validation croisée, f(x), cache
│   ├── exact_solver.py   # PLNE 0-1 et Branch & Bound
│   ├── constructive.py   # Heuristiques initiales
│   ├── meta/             # tabu.py, sa.py, vns.py, ga.py
│   └── benchmark.py      # Expérimentations multi-exécutions
├── app/dashboard.py      # Interface Streamlit
├── tests/                # Tests
├── report/               # Rapport et soutenance
└── docs/                 # Documentation
```

## Installation

```bash
git clone https://github.com/imranebiar-jpg/fs-metaheuristics-platform.git
cd fs-metaheuristics-platform
python -m venv .venv
.venv\Scripts\Activate.ps1        # Windows (PowerShell)
# source .venv/bin/activate       # Mac / Linux
python -m pip install -r requirements.txt
```

## Utilisation

```bash
python -m pytest                           # lancer les tests
python -m src.benchmark                    # campagne expérimentale
python -m streamlit run app/dashboard.py   # tableau de bord
```

## Interface commune des solveurs

Chaque algorithme expose `solve(problem, **params)` qui retourne un dictionnaire contenant :

| Clé | Description |
|---|---|
| `best_solution` | Vecteur binaire NumPy de taille `(n,)` |
| `best_fitness` | Meilleure valeur de f(x) |
| `selected_features` | Caractéristiques retenues |
| `history` | Meilleure valeur de f(x) à chaque itération |
| `evaluations` | Nombre d'appels à la fonction objectif |
| `exec_time` | Temps d'exécution (secondes) |

## Protocole expérimental

- Jeux de données : Wine (n = 13), Breast Cancer (n = 30), Ionosphere ou Sonar (n = 34 à 60).
- 10 exécutions indépendantes par métaheuristique, avec graines fixées (`seed`).
- Même budget d'évaluations et mêmes partitions de validation croisée pour tous les algorithmes.
- Mesures : qualité (fmin, moyenne, écart-type, Gap %), parcimonie, précision sur test (hold-out 20 %), temps de calcul et vitesse de convergence.

## Équipe

| Membre | Rôle |
|---|---|
| Membre 1 | Noyau ML (`problem.py`) |
| Membre 2 | Phase exacte (`exact_solver.py`) |
| Membre 3 | Heuristiques et Recherche Tabou |
| Membre 4 | Recuit Simulé |
| Membre 5 | Recherche à Voisinage Variable |
| Membre 6 | Algorithme Génétique |
| Membre 7 | Benchmark, interface et rapport |

## Encadrement

Projet réalisé dans le cadre du module de Recherche Opérationnelle, ENSA. Année universitaire 2026/2027.
