# Analyse Agricole par Télédétection et Machine Learning

Projet d'analyse de données satellitaires (Sentinel-2) et de données terrain pour la recommandation de cultures agricoles basée sur des indices spectraux et des conditions environnementales.

## 📋 Description

Ce projet combine trois approches analytiques :

1. **Analyse des données agricoles** - Classification et clustering des cultures
2. **Analyse temporelle des indices spectraux** - Étude de l'évolution saisonnière via Sentinel-2
3. **Fusion et régression** - Prédiction des conditions optimales pour les cultures

## 🚀 Fonctionnalités

- Extraction d'images Sentinel-2 via Google Earth Engine
- Calcul d'indices spectraux (NDVI, NDWI, MNDWI, NDRE, etc.)
- Analyse en Composantes Principales (ACP)
- Clustering hiérarchique et K-Means
- Régression linéaire multiple
- Recommandation de cultures par saison

## 📦 Installation

### Prérequis

- Python 3.8+
- Compte Google Earth Engine (authentification requise)

### Installation des dépendances

```bash
pip install -r requirements.txt
```

## 🗂️ Données requises

Placez ces fichiers dans le dossier `data/` :

- `agriculture_data_required.csv` - Données de cultures avec conditions environnementales
- `terrain_N045_or.csv` - Mesures terrain (N, P, K, température, humidité, pH)
- `polygon3.geojson` - Zone d'étude géographique

## 🎯 Utilisation

### Partie 1 : Analyse des données agricoles

Analyse descriptive, clustering et modèle prédictif.

```python
# Exécuter la section "Partie 1" du script
# Durée : ~2-5 minutes
```

### Partie 2 : Analyse temporelle (⚠️ Longue durée)

Extraction de 250 images Sentinel-2 (2020-2023).

```python
# ⚠️ ATTENTION : Cette section prend ~1 heure
# Utilisez plutôt le fichier pré-généré : historique_spectral_indices.csv
```

### Partie 3 : Fusion et régression

Fusion des données spectrales et terrain.

```python
# Exécuter la section "Partie 3" du script
# Durée : ~5-10 minutes
```

## 📊 Résultats

Le modèle génère :

- Clusters saisonniers de mois agricoles
- Équations de régression pour prédire N, P, K, température, humidité, pH
- Recommandations de cultures par cluster/saison

### Exemple de sortie

| Cluster | Mois          | Mois central | Culture suggérée | N     | P     | K     | Temp  | Humidité | pH   |
| ------- | ------------- | ------------ | ---------------- | ----- | ----- | ----- | ----- | -------- | ---- |
| 0       | 1,2,3,12      | 2            | rice             | 50.78 | 47.99 | 33.14 | 23.31 | 54.98    | 6.90 |
| 1       | 4,5,6,9,10,11 | 5            | maize            | 50.53 | 47.69 | 33.08 | 29.57 | 54.87    | 6.99 |
| 2       | 7,8           | 7            | cotton           | 48.77 | 47.05 | 33.71 | 34.08 | 52.16    | 6.86 |

## 🛠️ Technologies utilisées

- **Google Earth Engine** - Données Sentinel-2
- **scikit-learn** - Machine Learning
- **pandas, numpy** - Manipulation de données
- **matplotlib, seaborn** - Visualisation
- **fanalysis** - ACP

## ⚙️ Configuration Earth Engine

Avant la première exécution :

```python
import ee
ee.Authenticate()
ee.Initialize(project='votre-projet-id')
```

## 📝 Notes importantes

- ⚠️ La Partie 2 nécessite ~1h d'exécution (250 requêtes API)
- Les fichiers CSV générés sont volumineux et non versionnés
- Résolution spatiale : 10m (Sentinel-2)
- Période d'analyse : Juillet 2020 - Décembre 2023

## 🤝 Contribution

Les contributions sont les bienvenues ! Ouvrez une issue ou un pull request.

## 👤 Auteur

ouhassine wissal ouarda

## 🔗 Liens utiles

- [Documentation Google Earth Engine](https://developers.google.com/earth-engine)
- [Sentinel-2 User Handbook](https://sentinels.copernicus.eu/web/sentinel/user-guides/sentinel-2-msi)
