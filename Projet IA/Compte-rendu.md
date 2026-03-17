<div align="center">

# Université Hassan 1er  
## École Nationale de Commerce et de Gestion de Settat  

**Douaa EZZAHER**  
*Étudiante en 4ème année – Filière Contrôle, Audit et Conseil*

---

# Classification des Défaillances de Contrôle Interne par Type  
### Projet de Machine Learning - Thème : Contrôle Interne & Gouvernance

[![Type de tâche](https://img.shields.io/badge/Type-Classification-blue)]()
[![Contexte](https://img.shields.io/badge/Contexte-Audit%20%26%20Conformité-green)]()
[![Source](https://img.shields.io/badge/Data-SOX%20%7C%20PCAOB%20%7C%20OECCA-orange)]()

</div>

---

## 📋 Table des matières

1. [Contexte et Problématique](#1-contexte-et-problématique)
2. [Objectifs du Projet](#2-objectifs-du-projet)
3. [Cadre Méthodologique](#3-cadre-méthodologique)
4. [Sources de Données](#4-sources-de-données)
5. [Démarche Technique](#5-démarche-technique)
6. [Applications et Enjeux](#6-applications-et-enjeux)
7. [Perspectives](#7-perspectives)

---

## 1. Contexte et Problématique

### Contexte général
Dans un environnement réglementaire marqué par l'intensification des exigences de transparence (**Loi SOX** aux États-Unis, réformes **OECCA** au Maroc), la qualité du contrôle interne (CI) constitue un pilier fondamental de la gouvernance d'entreprise. 

L'identification précoce et la classification fine des défaillances de CI permettent aux auditeurs et aux directions financières d'optimiser leurs processus de remédiation et de réduire les risques d'erreurs significatives dans les états financiers.

### Problématique
> *Comment automatiser et fiabiliser la détection des différents niveaux de gravité des défaillances de contrôle interne à partir de données textuelles et structurelles, afin d'améliorer l'efficacité des processus d'audit et de conformité ?*

---

## 2. Objectifs du Projet

### Objectif principal
Développer un modèle de **classification automatique** des défaillances de CI selon leur niveau de criticité.

### Objectifs spécifiques
- [x] Analyser les patterns récurrents dans les déficiences identifiées par les régulateurs (PCAOB) et les guides locaux (OECCA)
- [x] Établir une cartographie des risques de contrôle par typologie
- [x] Proposer un outil décisionnel pour les auditeurs internes et externes

---

## 3. Cadre Méthodologique

### Type de tâche
**Classification supervisée multi-classes** visant à assigner à chaque observation (défaillance constatée) une étiquette précise parmi trois catégories distinctes de gravité.

### Variable Cible (Target Variable)

| Catégorie | Définition | Critères d'évaluation |
|-----------|------------|----------------------|
| **🔴 Déficience significative**<br>(*Significant Deficiency*) | Manquement dans le CI qui risque de ne pas prévenir ou détecter une anomalie significative, sans atteindre le niveau d'une faiblesse matérielle | • Impact modéré sur les états financiers<br>• Contournement possible par d'autres contrôles |
| **⚫ Faiblesse matérielle**<br>(*Material Weakness*) | Défaillance grave du CI créant une probabilité raisonnable qu'une anomalie significative ne soit pas détectée | • Impact important<br>• Absence de compensations<br>• Communication immédiate requise |
| **🟡 Point d'amélioration**<br>(*Improvement Area*) | Anomalie mineure ou écart par rapport aux meilleures pratiques | • Faible impact<br>• Recommandations préventives |

---

## 4. Sources de Données

Le modèle s'appuie sur un **corpus hétérogène** combinant données structurées et non structurées :

### 📊 SOX Compliance Datasets (Kaggle)
- **Nature :** Données issues des déclarations publiques d'entreprises cotées sous la *Section 404* de la Loi Sarbanes-Oxley
- **Contenu :** Descriptions textuelles des contrôles testés, résultats des évaluations, conclusions des auditeurs externes
- **Utilisation :** Entraînement initial du modèle NLP pour la reconnaissance des patterns linguistiques

### 📑 Rapports d'inspection du PCAOB
*(Public Company Accounting Oversight Board)*
- **Nature :** Rapports publics d'inspection des cabinets d'audit (*Part I findings*)
- **Apport :** Exemples validés par les régulateurs servant de *ground truth*
- **Spécificité :** Granularité élevée sur les causes racines des défaillances

### 📖 Guides de l'OECCA Maroc
*(Ordre des Experts Comptables et Comptables Agréés)*
- **Référence :** Guides d'application des normes d'audit adaptés au contexte marocain
- **Intérêt :** Alignement avec le contexte juridique local (Bourse de Casablanca, loi 69-21)
- **Données :** Cas pratiques issus des inspections professionnelles nationales

---

## 5. Démarche Technique

### Pipeline de traitement

```mermaid
graph LR
    A[Données brutes<br/>SOX/PCAOB/OECCA] --> B[Prétraitement<br/>NLP & Nettoyage]
    B --> C[Feature Engineering<br/>TF-IDF / Word2Vec]
    C --> D[Modélisation<br/>Random Forest / BERT]
    D --> E[Classification<br/>3 classes de gravité]
    E --> F[Évaluation<br/>F1-score & Recall]

Préparation des données
Nettoyage : Suppression des informations identifiantes, normalisation (lemmatisation)
Rééquilibrage : Utilisation de SMOTE pour la classe "faiblesse matérielle" (sous-représentée)
Feature Engineering :
Extraction de caractéristiques linguistiques (TF-IDF, Word2Vec)
Métriques structurelles (nombre de contrôles défaillants, ancienneté du système CI)
Modélisation envisagée
Algorithme	Usage	Justification
Random Forest	Baseline & Interprétabilité	Compréhensible pour les auditeurs métier
BERT	Analyse sémantique avancée	Capture du contexte dans les rapports textuels
SVM	Classification linéaire	Baseline robuste pour données textuelles
Métriques d'évaluation
Accuracy globale
F1-score macro (critique pour le déséquilibre des classes)
Recall spécifique pour la classe "Faiblesse matérielle" (minimiser les faux négatifs critiques)
6. Applications et Enjeux
Pour l'Audit Interne
Priorisation automatique des ressources sur les zones à haut risque
Réduction du temps d'analyse des rapports de contrôle
Pour la Conformité Réglementaire
Alignement avec les exigences OECCA Maroc concernant l'évaluation annuelle du CI
Préparation aux inspections de l'AMMC (Autorité Marocaine du Marché des Capitaux)
Pour la Gouvernance
Tableaux de bord prédictifs pour les Comités d'Audit
Détection précoce des signaux faibles avant escalation
7. Perspectives
Limites actuelles
Biais potentiel de sous-déclaration des faiblesses matérielles dans datasets publics
Complexité d'adaptation des catégories anglo-saxonnes au contexte marocain
Évolutions futures
 Intégration de données ESG et cybersécurité dans la classification
 Développement d'une interface web pour cabinets d'audit marocains
 Extension aux contrôles internes non financiers (RH, Supply Chain)
📚 Références
PCAOB. (2023). Staff Audit Practice Alert: Maintaining Audit Quality in the Current Environment
OECCA. (2022). Guide d'application des normes d'audit au Maroc
Sarbanes-Oxley Act, Section 404 - Internal Control over Financial Reporting
Kaggle Datasets : SOX Compliance and Internal Control Deficiencies
