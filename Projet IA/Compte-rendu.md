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

# 1. Contexte et Problématique

## Contexte général

Le contrôle interne constitue un pilier fondamental de la gouvernance des entreprises.  
Il vise à garantir :

- la fiabilité de l'information financière  
- la conformité aux lois et réglementations  
- l'efficacité et l'efficience des opérations  

Dans un contexte international marqué par l'importance croissante de la transparence financière, plusieurs cadres réglementaires ont renforcé les exigences en matière de contrôle interne. Parmi les plus influents figurent :

- la **Section 404 de la loi Sarbanes-Oxley (SOX)** aux États-Unis  
- le **référentiel COSO 2013**, largement utilisé comme cadre de référence pour l’évaluation des dispositifs de contrôle interne  

Ces normes imposent aux entreprises d’identifier, documenter et évaluer les défaillances éventuelles de leurs dispositifs de contrôle interne.

Les auditeurs internes et externes doivent ainsi analyser de nombreux rapports, observations et recommandations afin de déterminer le niveau de gravité des déficiences constatées.

## Problématique

L'analyse manuelle de ces rapports présente plusieurs limites :

- volume important de données textuelles  
- risque d’interprétation subjective  
- temps d’analyse élevé  

Dans ce contexte, les techniques de **Machine Learning** et de **traitement automatique du langage (NLP)** peuvent offrir des solutions efficaces pour automatiser certaines tâches d’analyse.

> **Problématique du projet :**  
> Comment utiliser des techniques de classification automatique pour identifier et catégoriser les défaillances de contrôle interne à partir de descriptions textuelles issues de rapports d’audit et de conformité ?

---

# 2. Objectifs du Projet

## Objectif principal

Développer un modèle de **classification automatique** permettant d’identifier le type de défaillance de contrôle interne à partir d’informations textuelles.

## Objectifs spécifiques

- Identifier les caractéristiques linguistiques associées aux différents types de défaillances
- Construire un modèle de **classification supervisée**
- Automatiser la catégorisation des observations d’audit
- Faciliter l’analyse des rapports de contrôle interne
- Aider les auditeurs dans la priorisation des actions correctives

Ce projet vise également à démontrer l’apport de la **data science** dans les métiers de l’audit, du contrôle interne et de la gestion des risques.

---

# 3. Cadre Méthodologique

## Type de tâche

Le projet repose sur une approche de **classification supervisée multi-classes**.

Chaque observation correspondant à une défaillance de contrôle interne est associée à une catégorie spécifique indiquant son niveau de gravité.

Le modèle doit apprendre à prédire cette catégorie à partir des caractéristiques du texte décrivant la défaillance.

## Variable cible (Target Variable)

| Catégorie | Définition |
|-----------|------------|
| 🔴 **Déficience significative** | Faiblesse du contrôle interne pouvant empêcher la détection d’une anomalie importante dans les états financiers |
| ⚫ **Faiblesse matérielle** | Défaillance majeure du contrôle interne pouvant conduire à une anomalie significative non détectée |
| 🟡 **Point d’amélioration** | Observation mineure ou recommandation visant à améliorer les procédures existantes |

Ces catégories sont généralement utilisées dans les rapports d’audit et les évaluations du contrôle interne.

---

# 4. Sources de Données

Le projet repose sur plusieurs sources de données relatives à la conformité et au contrôle interne.

## SOX Compliance Datasets (Kaggle)

Ces jeux de données contiennent des informations liées aux déclarations de conformité des entreprises soumises à la loi Sarbanes-Oxley.

Ils comprennent notamment :

- descriptions des contrôles internes
- observations des auditeurs
- classification des déficiences

Ces données peuvent servir de base pour l'entraînement initial du modèle.

## Rapports du PCAOB

Le **Public Company Accounting Oversight Board (PCAOB)** publie régulièrement des rapports d’inspection concernant les cabinets d’audit.

Ces rapports contiennent :

- des exemples réels de défaillances de contrôle interne  
- des analyses détaillées des causes des anomalies  
- des recommandations pour améliorer les procédures d’audit  

Ces documents constituent une source précieuse pour l’analyse des défaillances.

## Guides de l'OECCA Maroc

Les guides publiés par l’**Ordre des Experts Comptables et Comptables Agréés (OECCA)** fournissent des références adaptées au contexte réglementaire marocain.

Ils permettent notamment :

- d’aligner l’analyse avec les normes d’audit locales  
- d’intégrer les spécificités du cadre juridique marocain  

---

# 5. Démarche Technique

La réalisation du projet suit plusieurs étapes principales.

## 1. Collecte des données

Les données sont collectées à partir de différentes sources :

- datasets publics
- rapports d’audit
- documents de conformité

## 2. Prétraitement des données

Les données textuelles doivent être nettoyées et préparées avant l'analyse.

Les principales étapes incluent :

- suppression des caractères inutiles  
- normalisation du texte  
- suppression des mots vides (stop words)  
- tokenisation  

## 3. Transformation des données

Le texte est ensuite converti en variables numériques utilisables par les modèles de machine learning.

Une technique couramment utilisée est :

- **TF-IDF (Term Frequency – Inverse Document Frequency)**

Cette méthode permet de mesurer l’importance des mots dans un document par rapport à l’ensemble du corpus.

## 4. Entraînement du modèle

Plusieurs algorithmes de classification peuvent être utilisés :

- Régression logistique  
- Random Forest  
- Support Vector Machine (SVM)  
- Naive Bayes  

Le modèle est entraîné à partir d’un ensemble de données annotées.

## 5. Évaluation du modèle

Les performances du modèle sont évaluées à l’aide de plusieurs métriques :

- **Accuracy**
- **Precision**
- **Recall**
- **F1-score**

Ces indicateurs permettent de mesurer la capacité du modèle à classifier correctement les défaillances.

---

# 6. Applications et Enjeux

L'utilisation de techniques de machine learning dans le domaine du contrôle interne présente plusieurs avantages.

### Amélioration de l'efficacité des audits

L’automatisation de certaines tâches d’analyse permet aux auditeurs de se concentrer sur les activités à plus forte valeur ajoutée.

### Réduction des risques

Une meilleure identification des défaillances de contrôle interne permet de réduire les risques d’erreurs financières et de fraude.

### Aide à la décision

Les modèles de classification peuvent servir d’outil d’aide à la décision pour les responsables du contrôle interne et de la gestion des risques.

---

# 7. Perspectives

Plusieurs perspectives peuvent être envisagées pour améliorer ce projet.

### Amélioration des modèles

L’utilisation de modèles avancés de traitement du langage naturel, tels que les modèles **transformers** ou **BERT**, pourrait améliorer les performances de classification.

### Intégration dans les systèmes d'audit

Le modèle pourrait être intégré dans des outils d’audit interne afin d’assister les professionnels dans l’analyse des rapports.

### Extension des données

L’ajout de nouvelles sources de données permettrait d’améliorer la robustesse et la précision du modèle.

---

# Conclusion

Ce projet illustre le potentiel des techniques de **Machine Learning** appliquées au domaine du contrôle interne et de la gouvernance.

En automatisant la classification des défaillances de contrôle interne, il devient possible d’améliorer l'efficacité des processus d'audit, de renforcer la gestion des risques et de soutenir la prise de décision au sein des organisations.
