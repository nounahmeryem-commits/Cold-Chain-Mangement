# Cold Chain Logistics — Temperature Monitoring & Alert Analysis

![Python](https://img.shields.io/badge/python-3.10-blue)
![pandas](https://img.shields.io/badge/pandas-2.x-blue)
![sklearn](https://img.shields.io/badge/scikit--learn-KMeans-orange)
![Status](https://img.shields.io/badge/status-EDA%20%2B%20Clustering-green)

Analyse exploratoire des données de capteurs IoT dans des conteneurs de transport frigorifique (cold chain).  
Objectif : identifier les facteurs de risque de rupture de chaîne du froid à travers des visualisations géospatiales et un clustering K-means.

---

## Problématique

Dans la logistique du froid, une rupture de température peut détruire des produits alimentaires, des médicaments ou des vaccins.  
Ce projet répond à 3 questions clés :
- Quels produits sont les plus exposés aux alertes de température ?
- Quelles zones géographiques présentent le plus de risques ?
- Peut-on regrouper automatiquement les zones à risque ?

---

## Dataset

**Source** : [Cold Chain Logistics Temperature Data — Kaggle](https://www.kaggle.com/)  
**Taille** : ~500+ enregistrements de capteurs  
**Variables clés** :

| Colonne | Description |
|---|---|
| `temperature_celsius` | Température relevée par le capteur |
| `humidity_percent` | Humidité dans le conteneur |
| `alert_flag` | Alerte déclenchée (True/False) |
| `is_within_safe_range` | Température dans la plage sécurisée |
| `product_type` | Type de produit transporté |
| `address_city` / `address_country` | Localisation du capteur |
| `location_latitude/longitude` | Coordonnées GPS |

> Les données brutes ne sont pas incluses dans ce repo. Télécharger sur Kaggle et placer dans `data/raw/`.

---

## Résultats clés

- **Taux d'alerte global : ~49.5%** — près d'un envoi sur deux déclenche une alerte
- Les produits **dairy, meat, pharmaceuticals, seafood** concentrent le plus d'alertes (11 chacun)
- La **température** est le facteur le plus corrélé aux dépassements de seuils (corrélation ~1 avec `safe_temperature_max`)
- L'**humidité** et la **géographie** ont un impact indirect faible
- Le clustering K-means identifie **5 zones géographiques** distinctes (Europe, Amériques, Asie, Océanie, Antarctique)
- Les stations **polaires (McMurdo, Amundsen-Scott) ont 100% d'alertes**

---

## Approche

```
1. Chargement et exploration (EDA)
   └── dt.info(), isnull().sum(), value_counts()

2. Nettoyage
   └── Conversion log_timestamp → datetime
   └── Imputation humidity_percent (mean)
   └── Encodage alert_flag (category → 0/1)

3. Analyse des alertes par produit et par ville
   └── groupby product_type + address_city

4. Visualisation géospatiale
   └── matplotlib scatter sur coordonnées GPS
   └── geopandas + fond de carte naturalearth

5. Calcul des KPIs
   └── risk_score par product_type (taux d'alerte moyen)

6. Clustering géographique
   └── KMeans(n_clusters=5) sur latitude/longitude
   └── Carte des zones à risque
```

---

## Installation

```bash
git clone https://github.com/ton-username/cold-chain-analysis
cd cold-chain-analysis
pip install -r requirements.txt
```

## Usage

```bash
# Lancer le notebook principal
jupyter notebook notebooks/cold_chain_analysis.ipynb
```

---

## Structure

```
cold-chain-analysis/
├── notebooks/
│   └── cold_chain_analysis.ipynb   # Notebook principal
├── data/
│   └── raw/                        # cold-chain-logistics-temperature-data.csv (non versionné)
├── reports/
│   └── figures/                    # Graphiques exportés
├── requirements.txt
├── .gitignore
└── README.md
```

---

## Pistes d'amélioration

- [ ] Modèle de classification supervisée (prédire `alert_flag`)
- [ ] Analyse du taux d'alerte pondéré (alertes / total envois par produit)
- [ ] Dashboard interactif (Plotly / Dash)
- [ ] Analyse temporelle des alertes (heure, saison)

---

## Licence

MIT License
