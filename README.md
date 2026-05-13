# VertiGen ⚡ — Production Intelligente d'Hydrogène Vert

<div align="center">

![ProcessInnovate 2026](https://img.shields.io/badge/ProcessInnovate_2026-🥈_2ème_Prix-gold?style=for-the-badge)
![EMI](https://img.shields.io/badge/Compétition-EMI_Rabat-darkgreen?style=for-the-badge)
![ENSAK](https://img.shields.io/badge/École-ENSAK-blue?style=for-the-badge)
![License](https://img.shields.io/badge/License-MIT-teal?style=for-the-badge)

**Système de production d'hydrogène vert piloté par intelligence artificielle**  
*MPC + LSTM · Mix solaire-éolien · Tarfaya, Maroc*

</div>

---

## 🏆 Contexte

**Compétition :** ProcessInnovate 2026 — Green Hydrogen Day  
**Organisateur :** Club EMI Process · École Mohammadia d'Ingénieurs, Rabat  
**Thème :** *L'Hydrogène et la Transition Énergétique dans le cadre des Énergies Renouvelables et Durables*  
**Date :** 13 Mai 2026  
**Résultat :** 🥈 **2ème Prix** sur jury de professionnels du secteur énergie

---

## 👥 Auteurs

**Moulaye Abdoule Hady HAIDARA** & coéquipiers  
École Nationale des Sciences Appliquées de Kénitra (ENSAK)

---

## 🎯 Problématique

Tarfaya (29°N 12°W) dispose d'un potentiel exceptionnel en énergies renouvelables — irradiance moyenne de **6.2 kWh/m²/j** et vent moyen à 80m de **9.8 m/s**. Cependant, la variabilité de ces sources rend la production d'hydrogène vert inefficace avec un contrôle classique on/off :

- Taux d'utilisation de l'électrolyseur PEM limité à **45%**
- Cycles on/off excessifs → dégradation prématurée de la membrane
- LCOH élevé → non compétitif vs benchmark européen

**VertiGen répond à ce défi avec une architecture IA à 3 couches.**

---

## 🔧 Architecture du système

```
Sources renouvelables
  ├── Solaire PV     → 300 kW installés  (70.6% du mix · 7 717 MWh/an)
  └── Éolien         → 2 × 100 kW        (29.4% du mix · 3 214 MWh/an)
          │
          ▼
  ┌─────────────────────────────────┐
  │     Couche IA — 3 niveaux       │
  │  ① LSTM  · Prévision 48h ±15%  │
  │  ② MPC   · Dispatch temps réel  │
  │  ③ Optim · Convexe → min LCOH   │
  └─────────────────────────────────┘
          │
          ▼
  Électrolyseur PEM  500 kW · η = 70%
    Modulation : 20% → 100% (pas de coupure brutale)
          │
          ▼
  Stockage hydrures métalliques  2 t H₂
          │
          ▼
  Production annuelle : 200 t H₂/an
```

### Logique de dispatch MPC

| Zone | Puissance | Action |
|------|-----------|--------|
| **Veille** | P < 100 kW | Standby · buffer activé si prévision LSTM favorable |
| **Nominale** | 100–500 kW | Modulation proportionnelle 20→100% · anticipation +6h |
| **Surplus** | P > 500 kW | Saturation électrolyseur + stockage buffer (50% surplus) |

Recalcul toutes les **15 minutes** · Minimisation coût actualisé H₂ (fonction objectif convexe).

---

## 📊 Résultats clés

| Indicateur | Contrôle Classique | **VertiGen MPC** | Gain |
|------------|-------------------|------------------|------|
| H₂ produit / an | ~125 t | **200 t** | +60% |
| LCOH | 3.15 $/kg | **2.87 $/kg** | −9% |
| Benchmark EU 2026 | 3.5–6 $/kg | **2.87 $/kg** | −18% min |
| Taux utilisation PEM | 45% | **72%** | **+60%** |
| Cycles on/off / an | 224 | **66** | **−71%** |
| Durée vie membrane | 60 000 h | **> 80 000 h** | **+33%** |
| CO₂ évité | — | **2 000 t/an** | ≡ 870 voitures |

---

## 📈 Analyse économique (horizon 20 ans)

| Scénario | Prix H₂ | VAN |
|----------|---------|-----|
| Pessimiste | 3 $/kg | Négatif |
| **Médian** | **4 $/kg** | **Positif · ROI ~8 ans** |
| Optimiste | 5 $/kg | Très positif |

**CAPEX :** 2.5 M€ · **OPEX :** ~150 k€/an  
**Cible 2030 :** 2 $/kg à grande échelle (50 MW · −35% CAPEX unitaire)

---

## 🛣️ Roadmap industrielle

```
2027 ──────────── 2028 ──────────── 2030+
  │                 │                 │
Pilote ONEE      Régional          National
Tarfaya          Maroc Sud         Export EU
500 kW           10 MW             50–100 MW
200 t H₂/an      4 000 t H₂/an    40 000 t/an
```

---

## 🗂️ Structure du dépôt

```
ProcessInnovate-Challenge/
├── README.md
├── LICENSE
├── Images/                        # Figures et visuels du projet
├── Presentation.pptx              # Présentation pitch ProcessInnovate 2026
├── Rapport.pdf                    # Rapport technique complet (22 pages)
├── tarfaya_data.csv               # NASA POWER Dataset 2024 · 8 784h · Tarfaya
└── VertiGen_Dashboard.html        # Dashboard interactif (Chart.js · standalone)
```

---

## 🚀 Lancer le dashboard

Aucune installation requise — le dashboard est 100% standalone (HTML + Chart.js CDN).

```bash
# Cloner le repo
git clone https://github.com/bilily1/ProcessInnovate-Challenge.git
cd ProcessInnovate-Challenge

# Ouvrir dans le navigateur
open VertiGen_Dashboard.html          # macOS
xdg-open VertiGen_Dashboard.html      # Linux
start VertiGen_Dashboard.html         # Windows
# ou simplement double-cliquer sur le fichier
```

Le dashboard contient **5 onglets interactifs** :
- **Vue d'ensemble** — KPIs clés + mix énergétique + régimes
- **Production H₂** — profils mensuels, saisonniers, heatmap 24h×12mois
- **Contrôle MPC** — comparaison MPC vs classique + semaine type
- **Économie** — LCOH, VAN 3 scénarios, cash-flow cumulé
- **Roadmap** — économies d'échelle + feuille de route industrielle

---

## 📡 Données

**Fichier :** `data/tarfaya_data.csv`  
**Source :** NASA POWER Dataset 2024  
**Site :** Tarfaya, Maroc · 29°N 12°W  
**Résolution :** Horaire · **8 784 heures**  
**Variables :** Irradiance GHI, vitesse vent 80m, température, production PV simulée, production éolienne simulée, puissance disponible totale, régime de fonctionnement

---

## 🌱 Impact environnemental

- **2 000 t CO₂ évitées / an** ≡ 870 voitures thermiques retirées
- **−85%** d'émissions vs hydrogène gris (vaporeformage gaz naturel)
- **5 200 MWh/an** d'énergie fossile économisée ≡ 1 450 ménages
- **85%** de l'eau d'électrolyse recyclée (circuit fermé · 1 800 m³/an)

---

## 🔬 Stack technique

| Composant | Technologie |
|-----------|-------------|
| Modèle prédictif IA | Random Forest · 100 arbres · Python (scikit-learn) |
| Prévision météo | LSTM (Python) · horizon 48h · ±15% |
| Contrôle optimal | MPC (Model Predictive Control) · pas 15 min |
| Optimisation LCOH | Programmation convexe |
| Données climatiques | NASA POWER API · 8 784h · 2024 |
| Traitement données | Python · VertiGen_Predictor.py |
| Dashboard | HTML · JavaScript · Chart.js 4.4 |
| Typographie | Syne · JetBrains Mono |

---

## 📄 Licence

MIT License — Copyright © 2026 Moulaye Abdoule Hady HAIDARA & coéquipiers

---

<div align="center">

*« L'hydrogène vert n'est pas l'énergie du futur. C'est l'énergie que nous construisons aujourd'hui. »*

**VertiGen · ProcessInnovate 2026 · ENSAK**

</div>
