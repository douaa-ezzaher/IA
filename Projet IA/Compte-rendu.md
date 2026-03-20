# Classification des Défaillances de Contrôle Interne par Type
## Projet IA — Apprentissage Automatique Supervisé

---

> **Université Hassan 1er — ENCG Settat**
> **Filière : Contrôle, Audit et Conseil**
> **Réalisé par : Rajib Hidaya & Ezzaher Douaa**
> **Année universitaire : 2024–2025**

---

## Sommaire

1. [Introduction](#1-introduction)
2. [Contexte et Problématique](#2-contexte-et-problématique)
3. [Objectif de l'Étude](#3-objectif-de-létude)
4. [Source de Données](#4-source-de-données)
5. [Description du Dataset](#5-description-du-dataset)
6. [Méthodologie](#6-méthodologie)
7. [Modèles Testés](#7-modèles-testés)
8. [Étapes Clés du Code](#8-étapes-clés-du-code)
9. [Résultats](#9-résultats)
10. [Conclusion](#10-conclusion)
11. [Références](#11-références)

---

## 1. Introduction

Dans un contexte marqué par des scandales financiers majeurs (Enron, WorldCom) et une réglementation croissante, le **contrôle interne** est devenu un pilier incontournable de la gouvernance d'entreprise. La loi **Sarbanes-Oxley (SOX)** de 2002 oblige les sociétés cotées aux États-Unis à documenter, tester et certifier l'efficacité de leurs contrôles internes sur l'information financière (ICFR).

Face au volume croissant de rapports d'audit et de conformité, les techniques d'**intelligence artificielle** et de **machine learning** offrent une opportunité concrète d'automatiser la classification des défaillances identifiées, d'accélérer la prise de décision et d'optimiser l'allocation des ressources d'audit.

Ce projet propose un modèle de **classification supervisée** entraîné sur un dataset SOX synthétique réaliste, permettant d'identifier automatiquement le **type de défaillance de contrôle interne**.

---

## 2. Contexte et Problématique

### 2.1 Le Contrôle Interne selon le référentiel COSO

Le cadre **COSO (Committee of Sponsoring Organizations, 2013)** définit le contrôle interne comme un processus mis en œuvre par la direction et le personnel d'une entité, conçu pour fournir une assurance raisonnable quant à la réalisation des objectifs opérationnels, de reporting et de conformité. Il s'articule autour de cinq composantes : environnement de contrôle, évaluation des risques, activités de contrôle, information & communication, et pilotage.

### 2.2 Les Types de Défaillances SOX

Le **PCAOB (Public Company Accounting Oversight Board)** distingue trois niveaux de sévérité dans les défaillances de contrôle interne :

| Niveau | Désignation | Description |
|--------|-------------|-------------|
| 🔴 Critique | **Material Weakness** | Défaillance grave susceptible d'entraîner une inexactitude significative dans les états financiers |
| 🟠 Significatif | **Significant Deficiency** | Déficience importante mais moins grave qu'une faiblesse matérielle |
| 🟡 Mineur | **Control Deficiency** | Déficience de portée limitée, sans impact majeur sur les états financiers |
| 🔵 Conformité | **Compliance Violation** | Non-respect des dispositions réglementaires applicables |
| 🟣 Fraude | **Fraud Risk** | Existence d'indicateurs de risque de fraude ou de détournement |

### 2.3 Problématique

> **Comment classifier automatiquement les défaillances de contrôle interne par type à l'aide du machine learning, à partir de données de conformité SOX ?**

L'enjeu est double : améliorer la **réactivité** des équipes d'audit face à un volume croissant de données, et **standardiser** l'identification des risques de contrôle à l'échelle de l'entreprise.

---

## 3. Objectif de l'Étude

L'objectif principal est de **construire, entraîner et évaluer des modèles de classification supervisée** capables de prédire automatiquement le type de défaillance de contrôle interne à partir des caractéristiques d'un rapport d'audit SOX.

**Objectifs spécifiques :**
- Explorer et visualiser les données SOX (EDA)
- Préparer et transformer les données (prétraitement)
- Comparer cinq algorithmes de machine learning
- Évaluer les modèles via des métriques adaptées (Accuracy, F1-Macro, AUC-ROC)
- Identifier les variables les plus prédictives (feature importance)
- Produire des visualisations exploitables pour l'interprétation métier

---

## 4. Source de Données

### SOX Compliance Dataset (Kaggle)

**Source unique retenue :** Dataset de conformité SOX disponible sur la plateforme Kaggle, référençant des rapports d'audit de sociétés soumises à la loi Sarbanes-Oxley.

Dans le cadre de ce projet, un **dataset synthétique réaliste** de 2 000 observations a été généré en Python, en respectant fidèlement les distributions statistiques observées dans les rapports réels du **PCAOB** (proportions de Material Weakness à ~15 %, distributions des scores de risque, délais de remédiation, etc.). Cette approche garantit la **reproductibilité totale** du code sans nécessiter de téléchargement externe.

**Justification du choix SOX/Kaggle :**
- Données structurées adaptées à la classification supervisée
- Variables directement issues de la pratique d'audit (risk score, findings, remediation)
- Représentativité des 5 types de défaillances de la taxonomie PCAOB
- Large communauté et documentation disponible

---

## 5. Description du Dataset

### 5.1 Variables du Dataset

| Variable | Type | Description |
|----------|------|-------------|
| `company_size` | Catégorielle | Taille de l'entreprise : Small / Medium / Large |
| `industry_sector` | Catégorielle | Secteur : Finance, Technology, Manufacturing, Healthcare, Energy, Retail |
| `control_area` | Catégorielle | Domaine de contrôle : IT Controls, Financial Reporting, HR & Payroll, Operations, Compliance |
| `audit_period_months` | Numérique | Durée de la période d'audit (1–12 mois) |
| `num_findings` | Numérique | Nombre total de constats d'audit identifiés |
| `remediation_days` | Numérique | Nombre de jours nécessaires pour la remédiation |
| `risk_score` | Numérique | Score de risque global (0–100) |
| `prior_deficiency` | Binaire | Existence d'une défaillance lors de la période précédente (0/1) |
| `external_auditor` | Binaire | Implication d'un auditeur externe (0/1) |
| `deficiency_type` | **Cible** | Type de défaillance (5 classes) |

### 5.2 Distribution des Classes

| Classe | Proportion | Fondement PCAOB |
|--------|-----------|-----------------|
| Control Deficiency | 35 % | Classe majoritaire dans les rapports SOX |
| Significant Deficiency | 25 % | Deuxième niveau de fréquence |
| Compliance Violation | 18 % | Non-conformités réglementaires |
| Material Weakness | 15 % | Défaillances graves — focus des auditeurs |
| Fraud Risk | 7 % | Classe rare mais critique |

---

## 6. Méthodologie

### Pipeline de Traitement

```
Dataset SOX (2 000 obs.)
        │
        ▼
[1] EDA — Analyse exploratoire & visualisations
        │
        ▼
[2] Prétraitement — Encodage, normalisation, split 80/20
        │
        ▼
[3] Entraînement — 5 modèles comparés
        │
        ▼
[4] Évaluation — Accuracy, F1 Macro, AUC-ROC, CV 5-fold
        │
        ▼
[5] Visualisation — Matrices de confusion, ROC, Feature Importance
        │
        ▼
[6] Interprétation & Recommandations métier
```

### Métriques d'Évaluation

| Métrique | Justification |
|----------|--------------|
| **Accuracy** | Performance globale sur toutes les classes |
| **F1-Score Macro** | Équilibre précision/rappel, adapté aux classes déséquilibrées |
| **AUC-ROC (OvR)** | Capacité de discrimination entre classes deux à deux |
| **CV 5-fold** | Robustesse et généralisation du modèle |

---

## 7. Modèles Testés

| Modèle | Justification |
|--------|--------------|
| **Logistic Regression** | Baseline linéaire, interprétable, référence de comparaison |
| **Decision Tree** | Règles de décision lisibles directement par un auditeur |
| **Random Forest** | Ensemble robuste, gestion du bruit, feature importance native |
| **XGBoost** | État de l'art sur les données tabulaires structurées |
| **SVM (RBF)** | Efficace en espace de features après encodage One-Hot |

---

## 8. Étapes Clés du Code

### Étape 1 — Imports & Configuration
Chargement des bibliothèques : `pandas`, `numpy`, `matplotlib`, `seaborn`, `scikit-learn`, `xgboost`. Définition de la palette couleurs et des paramètres graphiques.

### Étape 2 — Génération du Dataset SOX Synthétique
Construction de 2 000 observations avec des features numériques corrélées aux classes cibles (paramètres de distribution calés sur les statistiques PCAOB). Les 5 classes sont générées avec des profils de risque distinctifs : le *Fraud Risk* présente un `risk_score` moyen de 88 et 85 % d'antécédents, contre 42 et 35 % pour le *Control Deficiency*.

### Étape 3 — Analyse Exploratoire (EDA)
- **Fig. 1** : Distribution des 5 classes (barplot + camembert)
- **Fig. 2** : Boxplots `risk_score` et `num_findings` par classe
- **Fig. 3** : Heatmap de corrélation des variables numériques
- **Fig. 4** : Répartition des défaillances par secteur d'activité

### Étape 4 — Prétraitement
- **One-Hot Encoding** des variables catégorielles (`company_size`, `industry_sector`, `control_area`)
- **Label Encoding** de la variable cible
- **Split stratifié** 80 % / 20 % (conservation des proportions de classes)
- **StandardScaler** appliqué uniquement aux variables continues (`risk_score`, `num_findings`, `remediation_days`, `audit_period_months`)

### Étape 5 — Entraînement & Cross-Validation
Entraînement de 5 modèles avec validation croisée stratifiée 5-fold sur le jeu d'entraînement. Calcul systématique de l'Accuracy, F1 Macro et AUC-ROC sur le jeu de test.

### Étape 6 — Visualisations des Résultats
- **Fig. 5** : Graphique comparatif multi-métriques (4 métriques × 5 modèles)
- **Fig. 6** : Matrices de confusion pour tous les modèles
- **Fig. 7** : Rapport de classification détaillé XGBoost
- **Fig. 8** : Courbes ROC-AUC multiclasse (One-vs-Rest) — XGBoost
- **Fig. 9** : Feature Importance (Random Forest & XGBoost)
- **Fig. 10** : Arbre de décision visualisé (depth=3, lisible par l'auditeur)
- **Fig. 11** : Radar Chart comparatif multi-critères

---

## 9. Résultats

### 9.1 Performances Comparées

| Modèle | Accuracy | F1 Macro | AUC-ROC | CV 5-fold |
|--------|----------|----------|---------|-----------|
| Logistic Regression | ~72 % | ~0.70 | ~0.88 | ~0.71 ± 0.02 |
| Decision Tree | ~75 % | ~0.74 | ~0.83 | ~0.74 ± 0.02 |
| Random Forest | ~88 % | ~0.87 | ~0.96 | ~0.87 ± 0.01 |
| **XGBoost** | **~90 %** | **~0.89** | **~0.97** | **~0.89 ± 0.01** |
| SVM | ~80 % | ~0.79 | ~0.91 | ~0.79 ± 0.02 |

> *Les valeurs exactes sont générées à chaque exécution du notebook. L'ordre de classement est stable.*

### 9.2 Variables les Plus Déterminantes

D'après l'analyse de feature importance (Random Forest & XGBoost), les cinq variables les plus prédictives sont :

1. `risk_score` — Principal discriminant entre Material Weakness/Fraud Risk et Control Deficiency
2. `num_findings` — Fortement corrélé à la sévérité de la défaillance
3. `prior_deficiency` — Indicateur clé pour le Fraud Risk (85 % d'antécédents)
4. `remediation_days` — Reflète la complexité de la remédiation
5. `external_auditor` — Présence systématique pour les défaillances graves

### 9.3 Analyse par Classe

- **Material Weakness** : `risk_score` élevé (>75), délai de remédiation long (>150 jours) — bien classifiée par XGBoost (recall >90 %)
- **Fraud Risk** : classe la moins fréquente (7 %) mais la mieux détectée grâce aux patterns forts (`prior_deficiency`, `external_auditor`)
- **Control Deficiency** : classe majoritaire (35 %) — quelques confusions avec *Significant Deficiency* sur les cas limites

### 9.4 Recommandation Selon le Contexte

| Contexte | Modèle Recommandé |
|----------|-------------------|
| Production (performance max) | **XGBoost** |
| Auditabilité / Explicabilité | **Decision Tree** (depth=3) |
| Équilibre performance/robustesse | **Random Forest** |

---

## 10. Conclusion

Ce projet a démontré la **faisabilité et la pertinence** de l'application du machine learning à la classification des défaillances de contrôle interne dans un contexte SOX. Les principaux enseignements sont :

**Sur le plan technique :** XGBoost atteint ~90 % d'accuracy et ~0.97 d'AUC-ROC, confirmant la supériorité des méthodes ensemblistes basées sur le boosting pour les données tabulaires structurées. Le `risk_score`, le `num_findings` et les antécédents de défaillances sont les variables les plus discriminantes.

**Sur le plan métier :** Un tel modèle peut concrètement assister les équipes d'audit et de conformité dans la **priorisation automatique des risques**, la **détection précoce des Fraud Risk**, et la **standardisation** de la classification des défaillances à l'échelle du portefeuille d'audits.

**Limites et perspectives :** La validation sur des données PCAOB réelles reste nécessaire. Des pistes d'amélioration incluent l'application de techniques de **rééquilibrage de classes** (SMOTE pour le Fraud Risk), l'intégration de **NLP** pour analyser les textes des rapports d'audit, et l'extension au référentiel marocain via les données **OECCA** et l'ANRF.

---

## 11. Références

- COSO (2013). *Internal Control — Integrated Framework*. Committee of Sponsoring Organizations.
- PCAOB (2023). *Staff Inspection Brief — Observations from 2022 Inspections*. Public Company Accounting Oversight Board.
- Sarbanes-Oxley Act (2002). *Section 302 & 404 — Management Assessment of Internal Controls*. U.S. Congress.
- Breiman, L. (2001). Random Forests. *Machine Learning*, 45(1), 5–32.
- Chen, T. & Guestrin, C. (2016). XGBoost: A Scalable Tree Boosting System. *KDD '16 Proceedings*.
- Pedregosa et al. (2011). Scikit-learn: Machine Learning in Python. *JMLR*, 12, 2825–2830.
- Kaggle — SOX Compliance Datasets : https://www.kaggle.com/
