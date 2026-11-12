# Analyse financière des sociétés cotées à la BRVM (1998-2025)

Projet d'analyse des états financiers de 50 sociétés cotées à la Bourse Régionale des Valeurs Mobilières (BRVM) : contrôle de la qualité des données, calcul de ratios et comparaison de la rentabilité par période, par type de société et par secteur.

## Source des données et droits

- Les données sont issues de la base des états financiers des sociétés de la BRVM (bilan, compte de résultat, flux de trésorerie, depuis 1998) publiée par Mr Oumar Dicko, à partir des publications de la BRVM : https://lnkd.in/p/d2AjnHPd.
- Les données ne sont pas redistribuées dans ce dépôt : pour les obtenir, voir les liens ci-dessus.

## Les données

- 50 sociétés (15 financières, 35 autres), exercices 1998 à 2025.
- Trois tableaux : bilan (1 032 lignes), compte de résultat (1 035 lignes), flux de trésorerie (939 lignes).
- Les postes sont des postes communs, moins détaillés que dans les rapports publiés (précision donnée par l'auteur de la base).

## Méthode

1. **Contrôle de qualité** : couverture par société et par année, doublons, cohérence entre les trois tableaux, équilibre du bilan, détection des sauts anormaux, journal de 12 anomalies (`outputs/journal_anomalies.csv`).
2. **Séparation des financières et des autres sociétés**, car leurs comptes ne se lisent pas de la même façon.
3. **Classification sectorielle** saisie d'après la liste de la BRVM (`data/secteurs.csv`) ; 3 sociétés disparues ou fusionnées ont été classées d'après leur activité.
4. **Ratios** : marges, ROE, ROA, levier, liquidité, croissance, ratios bancaires.
5. **ROE fiable** : calculé seulement si les capitaux propres dépassent 1 % du total d'actif. L'effet de ce filtre est mesuré (voir Limites).

## Formules des ratios

Tous les montants sont des valeurs de fin d'exercice. Un ratio n'est calculé que si son dénominateur est strictement positif.

| Ratio | Formule |
|---|---|
| **ROE** (rentabilité des capitaux propres) | `Résultat net ÷ Capitaux propres (intérêts + part minoritaire)` |
| **ROE fiable** | `ROE`, calculé seulement si `Capitaux propres > 1 % du Total actif` |
| **ROA** (rentabilité de l'actif) | `Résultat net ÷ Total actif` |
| **Levier** | `Total actif ÷ Capitaux propres` (calculé seulement si `Capitaux propres > 1 % du Total actif`) |
| **Marge nette** | `Résultat net ÷ Chiffre d'affaires` |
| **Marge d'exploitation** | `Résultat d'exploitation ÷ Chiffre d'affaires` |
| **Endettement** | `(Dette financière long terme + Dette financière court terme) ÷ Capitaux propres` |
| **Liquidité générale** | `Total actif courant ÷ Total passif courant` |
| **Croissance du chiffre d'affaires** | `CA(t) ÷ CA(t-1) - 1`, calculée seulement entre deux années consécutives |
| **Flux de trésorerie libre** | `Net cash des opérations + Cash d'acquisition d'immobilisations` |
| **Conversion en trésorerie** | `Net cash des opérations ÷ Résultat net` (si le résultat net est positif) |
| **Coefficient d'exploitation** (banques) | `1 - Résultat brut d'exploitation ÷ Produit net bancaire` |
| **Coût du risque / PNB** (banques) | `valeur absolue du Coût du risque ÷ Produit net bancaire` |
| **Crédits / dépôts** (banques) | `Créances sur la clientèle ÷ Dépôts de la clientèle` |

## Résultats

### Qualité des données

- L'équilibre actif = passif + capitaux propres est respecté sur toutes les lignes comparables (1 020), sans écart supérieur à 0,1 %. Ce résultat confirme le contrôle annoncé par l'auteur de la base.
- 24 années manquantes à l'intérieur des séries du bilan, et 93 lignes société-année sans flux de trésorerie (surtout pour des sociétés financières).
- 12 anomalies documentées, à vérifier dans les rapports annuels.

### Rentabilité dans le temps

![ROE médian par année](outputs/roe_median_par_annee.png)

- **Sociétés non financières** : ROE médian de 10,0 % (1998-2005), 18,1 % (2006-2015) puis 20,4 % (2016-2025). En supposant le pire ou le meilleur cas pour les 6 à 10 % de lignes exclues du ROE fiable, la médiane reste comprise entre 8,5 et 11,5 %, 16,3 et 20,2 %, puis 19,2 et 21,1 %.
- **Sociétés présentes au moins 20 ans (24 sociétés)** : ROA médian de 3,6 %, 6,8 % puis 5,8 %, et marge nette de 3,4 %, 5,4 % puis 4,8 %. Entre les deux premières périodes, ROE, ROA et marge progressent ensemble.
- **Banques et financières** : ROE médian de 9,9 %, 15,8 % puis 20,8 %. Le saut de 2006 et le sommet de 2016 se retrouvent aussi sur les 6 banques présentes au moins 20 ans. Le niveau depuis 2017 dépend de l'échantillon.

![Échantillon stable](outputs/roe_median_echantillon_stable.png)

### Rentabilité par secteur

![ROA par société et par secteur](outputs/roa_par_societe_et_secteur.png)

| Secteur | Sociétés | ROA médian des sociétés | Plus bas / plus haut | Levier médian |
|---|---|---|---|---|
| Télécommunications | 3 | 8,3 % | 7,5 % / 15,1 % | 2,9 |
| Consommation de base | 10 | 5,7 % | -4,1 % / 18,3 % | 2,2 |
| Consommation discrétionnaire | 8 | 5,3 % | 0,4 % / 8,2 % | 2,6 |
| Industriels | 7 | 5,0 % | -1,8 % / 6,6 % | 2,6 |
| Énergie | 3 | 4,8 % | 3,6 % / 7,4 % | 3,7 |
| Services financiers | 15 | 1,5 % | 0,5 % / 2,6 % | 11,3 |
| Services publics | 2 | 1,3 % | 1,2 % / 1,4 % | 16,2 |

- Les écarts entre sociétés d'un même secteur sont souvent plus grands que les écarts entre secteurs (consommation de base : de -4,1 % à 18,3 %).
- Les services financiers et les services publics ont un ROA faible mais un levier élevé (actif de 11 à 16 fois les capitaux propres) : leur ROE élevé reflète des capitaux propres faibles, pas une forte rentabilité de l'activité.
- Médianes calculées sur les sociétés avec au moins 5 années de ratios (48 sur 50).

## Limites

- **Biais de survie** : l'échantillon des sociétés présentes 20 ans ou plus favorise les survivantes.
- **Peu de banques** par année (entre 4 et 15) : leurs médianes sont fragiles.
- **Petits secteurs** : services publics (2 sociétés), télécoms (3) et énergie (3) sont des études de cas, pas des tendances sectorielles.
- **Filtre du ROE** : il écarte surtout des années de pertes (64,5 % des lignes écartées ont un résultat net négatif, contre 11,8 % des lignes gardées). L'effet sur les médianes d'ensemble est de 1 à 2 points, mais le ROE de certaines sociétés (UNILEVER CI, SETAO CI) n'est pas interprétable ; le ROA, calculé sur toutes les années, est préféré pour comparer les sociétés.
- **Composition changeante** : des sociétés entrent et sortent du fichier ; NEI CI et CEDA CI (jusqu'en 2010) deviennent NEI CEDA CI (2011) ; les données de CROWN CI, MOVIS CI et UNILEVER CI s'arrêtent avant 2024 pour une raison non vérifiée.
- **Sociétés majoritairement ivoiriennes** : les résultats ne représentent pas toute la zone UEMOA.
- **Classification sectorielle** saisie à la main.
- Les rapprochements avec des événements économiques sont des hypothèses, non testées.
- Plusieurs anomalies de données restent à vérifier dans les rapports annuels.

## Structure du dépôt

- `notebooks/` : analyses
- `outputs/` : graphiques et journal des anomalies
- `data/secteurs.csv` : classification sectorielle ; les données financières ne sont pas publiées
- `requirements.txt` : bibliothèques utilisées

## Reproduire

Installer les bibliothèques avec `pip install -r requirements.txt`, obtenir la base auprès de son auteur, la placer dans `data/raw/`, puis exécuter le notebook `notebooks/01_exploration.ipynb`.

Auteur : Jean Théophile Ilboudo www.linkedin.com/in/wend-nongré-jean-théophile-ilboudo-a65357222