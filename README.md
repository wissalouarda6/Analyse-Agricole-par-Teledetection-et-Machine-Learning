# Projet d'Analyse Agricole : Recommandation de Cultures et Analyse des Indices Spectraux
## Original file is located at
    https://colab.research.google.com/drive/12ZyNXmD0mL_yLEu1KP-Mya-NWY0yKHcm
## Vue 'ensemble du 

Ce projet d'analyse agricole combine l'apprentissage automatique, la télédétection et l'analyse spatiale pour fournir des recommandations intelligentes de cultures basées sur les conditions du sol, les données climatiques et l'imagerie satellite. Le projet intègre trois composantes analytiques majeures pour fournir des informations exploitables pour la prise de décision agricole.

## Objectifs principaux

- Classification et recommandation de cultures par apprentissage automatique
- Analyse spectrale temporelle sur plusieurs années (250+ images de 2020-2023)
- Intégration de données spatiales (terrain et satellite)
- Clustering saisonnier pour identifier les périodes optimales de plantation
- Modélisation par régression pour prédire les variables environnementales

---

## Structure du projet

Le projet est divisé en trois parties principales :

### Partie 1 : Système de recommandation de cultures
**Fichiers requis :** `agriculture_data_required.csv`

**Objectifs :**
- Analyse exploratoire des conditions agricoles (N, P, K, température, humidité, pH, précipitations)
- Clustering K-Means pour regrouper les cultures similaires
- Modèle de régression logistique pour la prédiction de cultures
- Évaluation avec matrice de confusion et rapport de classification

**Résultats clés :**
```
Nombre optimal de clusters : 3
Précision du modèle : Évaluée via matrice de confusion
Variables analysées : 7 variables environnementales
Cultures : 22 types différents
```

**Graphiques générés :**
- Distribution des variables agricoles (histogrammes)
- Méthode du coude (Elbow Method) pour K-Means
- Matrice de confusion du modèle de classification

### Partie 2 : Analyse historique des indices spectraux
**Fichiers requis :** `historique_spectral_indices.csv`, `polygon3.geojson`

**Attention :** Le code d'extraction des images satellite prend environ 1 heure. Utiliser le fichier CSV fourni pour éviter la ré-exécution.

**Objectifs :**
- Extraction d'images Sentinel-2 via Google Earth Engine
- Calcul de 10 indices spectraux (NDVI, NDWI, MNDWI, NDRE, PRI, CIRE, GNDVI, SIPI, LST, NMDI)
- Analyse temporelle des indices (par mois et par année)
- Analyse en Composantes Principales (ACP)
- Clustering hiérarchique et K-Means pour identifier les saisons agricoles

**Indices calculés :**
```
NDVI  : Normalized Difference Vegetation Index
NDWI  : Normalized Difference Water Index
MNDWI : Modified Normalized Difference Water Index
NDRE  : Normalized Difference Red Edge
PRI   : Photochemical Reflectance Index
CIRE  : Chlorophyll Index Red Edge
GNDVI : Green Normalized Difference Vegetation Index
SIPI  : Structure Insensitive Pigment Index
LST   : Land Surface Temperature
NMDI  : Normalized Multi-band Drought Index
```

**Résultats du clustering :**
```
Cluster 0 : Mois [1, 2, 3, 12]  - Forte activité végétative
            Mois représentatif : 2
            
Cluster 1 : Mois [4, 5, 6, 9, 10, 11] - Transition/Valeurs moyennes
            Mois représentatif : 5
            
Cluster 2 : Mois [7, 8] - Stress hydrique potentiel
            Mois représentatif : 7
```

**Graphiques générés :**
- Boxplots et violin plots des indices par mois
- Évolution temporelle des moyennes mensuelles
- Heatmaps des valeurs d'indices
- Visualisation avec deux axes Y (LST vs autres indices)
- Cercle de corrélation (ACP)
- Carte factorielle avec clusters
- Dendrogramme hiérarchique
- Indice de silhouette pour sélection du nombre de clusters

### Partie 3 : Intégration données terrain et spectrales
**Fichiers requis :** 
- `terrain_N045_or.csv`
- `polygon3.geojson`
- `terrain&spectral_data_NOgeol_for_regre.csv`

**Objectifs :**
- Fusion des données de terrain avec les indices spectraux
- Agrégation spatiale sur grille de 10m x 10m
- Analyse de corrélation entre variables environnementales et indices
- Régression linéaire multiple pour prédire les variables du sol
- Génération de recommandations de cultures par cluster saisonnier

**Régressions effectuées :**
```
Variables dépendantes : N, P, K, temperature, humidity, pH, rainfall
Variables indépendantes : 10 indices spectraux

Exemple (Azote - N) :
N = 0.0746*NDVI - 0.0105*NDWI - 0.0018*MNDWI + ... + 45.1388
R² = 0.8357, MSE = 0.0001, MAE = 0.0079
```

**Graphiques générés :**
- Heatmap de corrélation (variables environnementales vs indices)
- Extraction et visualisation des valeurs de pixels

---

## Configuration et installation

### Prérequis

```python
# Bibliothèques principales
pip install geojson
pip install fanalysis
pip install adjustText
pip install pydataset
pip install prince
pip install --upgrade scikit-learn
pip install earthengine-api
pip install pandas numpy matplotlib seaborn
```

### Configuration Google Earth Engine

```python
import ee
ee.Authenticate()
ee.Initialize(project='votre-projet-id')
```

---

## Fichiers de données

### Fichiers d'entrée requis

| Fichier | Description | Partie |
|---------|-------------|--------|
| `agriculture_data_required.csv` | Données agricoles de base | 1 |
| `historique_spectral_indices.csv` | Indices spectraux historiques | 2 |
| `polygon3.geojson` | Région d'intérêt (ROI) | 2, 3 |
| `terrain_N045_or.csv` | Mesures terrain | 3 |
| `terrain&spectral_data_NOgeol_for_regre.csv` | Données fusionnées | 3 |

### Fichiers de sortie générés

| Fichier | Description |
|---------|-------------|
| `description_globale.csv` | Statistiques globales des indices |
| `description_par_annee.csv` | Statistiques annuelles |
| `description_par_mois.csv` | Statistiques mensuelles |
| `pixel_values.csv` | Valeurs extraites par pixel |
| `correlation_results.csv` | Matrice de corrélation |

---

## Méthodologie

### Partie 1 : Machine Learning pour recommandation

```
1. Chargement et exploration des données
2. Analyse statistique descriptive
3. Clustering K-Means (méthode du coude)
4. Division train/test (80/20)
5. Entraînement régression logistique
6. Évaluation et prédiction
```

### Partie 2 : Analyse temporelle des indices

```
1. Extraction images Sentinel-2 (Google Earth Engine)
2. Calcul des 10 indices spectraux
3. Agrégation temporelle (jour, mois, année)
4. Visualisations multiples (boxplots, violin plots, heatmaps)
5. ACP pour réduction dimensionnelle
6. Clustering pour identification des saisons
```

### Partie 3 : Fusion et régression

```
1. Agrégation spatiale sur grille 10m
2. Fusion données terrain/spectral
3. Analyse de corrélation
4. Régression linéaire multiple
5. Prédiction des variables environnementales
6. Génération recommandations finales
```

---

## Résultats finaux

### Recommandations par cluster saisonnier

```
+--------+------------------+---------------+------------------+
| Cluster| Mois             | Mois central  | Culture suggérée |
+--------+------------------+---------------+------------------+
| 0      | 1, 2, 3, 12      | 2             | [Prédiction ML]  |
| 1      | 4, 5, 6, 9,10,11 | 5             | [Prédiction ML]  |
| 2      | 7, 8             | 7             | [Prédiction ML]  |
+--------+------------------+---------------+------------------+
```

### Interprétation des clusters

**Cluster 0 (Hiver-Printemps):**
- Température moyenne : 23-24°C
- Humidité élevée : 51-56%
- NDVI élevé : Forte activité végétative
- Conditions optimales pour cultures de saison fraîche

**Cluster 1 (Printemps-Automne):**
- Température modérée : 25-30°C
- Transition saisonnière
- Valeurs d'indices intermédiaires
- Période polyvalente

**Cluster 2 (Été):**
- Température élevée : 31-34°C
- NMDI élevé : Stress hydrique potentiel
- Nécessite irrigation
- Cultures résistantes à la chaleur

---

## Visualisations clés

Le projet génère plus de 20 types de graphiques différents :

**Partie 1:**
- Distributions des 7 variables agricoles
- Graphique du coude (Elbow)
- Matrice de confusion

**Partie 2:**
- 10 x 4 graphiques par indice (boxplot, violin, tendance, heatmap)
- Graphique combiné deux axes Y
- Cercle de corrélation ACP
- Carte factorielle avec variables et observations
- Dendrogramme hiérarchique
- Courbe silhouette

**Partie 3:**
- Heatmap corrélation globale (7x10)
- Visualisations spatiales

---

## Notes d'exécution

### Temps d'exécution estimé

```
Partie 1 : ~5 minutes
Partie 2 : ~1 heure (extraction GEE) ou ~10 minutes (avec CSV)
Partie 3 : ~15 minutes
```

### Sections à ne pas exécuter

Les sections marquées `!ne executez pas` dans le code correspondent à l'extraction des images satellite. Utiliser les fichiers CSV fournis à la place.

---

## Interprétations scientifiques

### Corrélations observées

**NDVI, NDRE, GNDVI :**
- Fortement corrélés entre eux (indices de végétation)
- Corrélation positive avec N, P
- Indicateurs de biomasse et activité photosynthétique

**NDWI, MNDWI :**
- Corrélés avec l'humidité
- Anti-corrélés avec la température
- Indicateurs de stress hydrique

**LST (température de surface) :**
- Corrélation avec température ambiante
- Influence sur évapotranspiration
- Pic en mars et juin

### Liens environnementaux

```
Température optimale → NDVI élevé → Croissance active
Humidité suffisante → NDWI élevé → Pas de stress hydrique
N, P, K équilibrés → Indices végétation élevés → Santé des cultures
```

---
