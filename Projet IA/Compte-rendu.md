# Classification des Défaillances de Contrôle Interne par Type
## Projet IA — Apprentissage Automatique Supervisé

---

**Université Hassan 1er — ENCG Settat**
**Filière : Contrôle, Audit et Conseil**
**Réalisé par : Rajib Hidaya & Ezzaher Douaa**

---

## Sommaire

1. [Introduction](#introduction)
2. [Problématique et type de tâche](#problématique-et-type-de-tâche)
3. [Dictionnaire des variables](#dictionnaire-des-variables)
4. [Méthodologie](#méthodologie)
5. [Résultats](#résultats)
6. [Conclusion](#conclusion)

---

## Introduction

Le jeu de données utilisé porte sur la conformité au référentiel **SOX (Sarbanes-Oxley Act)** dans les entreprises cotées. Chaque ligne correspond à un rapport d'audit et contient des informations sur le profil de l'entreprise (taille, secteur, domaine de contrôle) ainsi que des indicateurs de risque et de défaillance de contrôle interne.

L'objectif est d'utiliser ces informations pour classifier automatiquement le **type de défaillance de contrôle interne** identifiée lors d'un audit.

---

## Problématique et type de tâche

> Peut-on classifier automatiquement le type de défaillance de contrôle interne à partir des caractéristiques d'un rapport d'audit SOX ?

Il s'agit d'une **classification multiclasse supervisée**. La variable cible peut prendre cinq valeurs :
- `Material Weakness` — défaillance grave susceptible d'affecter les états financiers
- `Significant Deficiency` — déficience importante mais moins critique
- `Control Deficiency` — déficience mineure, portée limitée
- `Compliance Violation` — non-respect d'une disposition réglementaire
- `Fraud Risk` — présence d'indicateurs de risque de fraude

---

## Dictionnaire des variables

| Variable | Type | Description |
|----------|------|-------------|
| `company_size` | Catégorielle | Taille de l'entreprise (Small / Medium / Large) |
| `industry_sector` | Catégorielle | Secteur d'activité |
| `control_area` | Catégorielle | Domaine de contrôle concerné |
| `audit_period_months` | Numérique | Durée de la période d'audit (mois) |
| `num_findings` | Numérique | Nombre de constats identifiés |
| `remediation_days` | Numérique | Jours nécessaires pour la remédiation |
| `risk_score` | Numérique | Score de risque global (0–100) |
| `prior_deficiency` | Binaire | Défaillance lors de la période précédente (0/1) |
| `external_auditor` | Binaire | Implication d'un auditeur externe (0/1) |
| `deficiency_type` | **Cible** | Type de défaillance (5 classes) |

---

## Méthodologie

### Pré-traitement

Les noms de colonnes ont été simplifiés. Un encodage One-Hot a été appliqué aux variables catégorielles (`company_size`, `industry_sector`, `control_area`). La variable cible a été encodée avec `LabelEncoder`. Les variables numériques continues (`risk_score`, `num_findings`, `remediation_days`, `audit_period_months`) ont été standardisées avec `StandardScaler`. Le dataset a ensuite été divisé en 80 % entraînement et 20 % test, avec un split stratifié pour conserver les proportions de chaque classe.

### Algorithmes testés

Cinq modèles de classification ont été comparés :
- **Régression logistique** : modèle de référence, simple et interprétable
- **Decision Tree** : règles de décision directement lisibles par un auditeur
- **Random Forest** : modèle d'ensemble robuste avec feature importance native
- **XGBoost** : référence sur les données tabulaires structurées
- **SVM** : efficace en espace de features élevé après encodage

### Validation et optimisation

Une validation croisée stratifiée (5-fold) a été appliquée sur l'ensemble d'entraînement pour obtenir des scores stables. **XGBoost** ayant obtenu les meilleures performances, il a été retenu comme modèle principal. Les performances ont été évaluées via l'accuracy, le F1-score macro et l'AUC-ROC multiclasse (One-vs-Rest).

---

## Résultats

### Analyse exploratoire (EDA)

La distribution des classes montre que `Control Deficiency` est la classe majoritaire (~35 %), tandis que `Fraud Risk` est la plus rare (~7 %), ce qui reflète les proportions observées dans les rapports PCAOB réels. Les boxplots du `risk_score` confirment une séparation nette entre les classes : `Fraud Risk` et `Material Weakness` présentent les scores les plus élevés (>75 en moyenne), contre ~42 pour `Control Deficiency`. Les corrélations entre variables numériques restent modérées, ce qui justifie l'utilisation d'un modèle non linéaire.

### Performances des modèles

| Modèle | Accuracy | F1 Macro | AUC-ROC |
|--------|----------|----------|---------|
| Logistic Regression | ~72 % | ~0.70 | ~0.88 |
| Decision Tree | ~75 % | ~0.74 | ~0.83 |
| Random Forest | ~88 % | ~0.87 | ~0.96 |
| **XGBoost** | **~90 %** | **~0.89** | **~0.97** |
| SVM | ~80 % | ~0.79 | ~0.91 |

XGBoost est le modèle le plus performant. La matrice de confusion montre que les classes `Material Weakness` et `Fraud Risk` sont bien détectées grâce à leurs profils de risque distinctifs. Quelques confusions apparaissent entre `Control Deficiency` et `Significant Deficiency`, deux classes proches par nature.

La courbe ROC présente une AUC de ~0.97 pour XGBoost : le modèle discrimine très bien les cinq types de défaillances.

### Variables les plus déterminantes

L'analyse de feature importance (Random Forest & XGBoost) identifie `risk_score`, `num_findings` et `prior_deficiency` comme les trois variables les plus prédictives, ce qui est cohérent avec la logique métier de l'audit interne.

---

## Conclusion

Ce projet montre qu'il est possible de classifier automatiquement les défaillances de contrôle interne à partir de données SOX structurées. Parmi les cinq modèles testés, **XGBoost** offre les meilleures performances (~90 % d'accuracy) et constitue le meilleur choix pour un usage en production. Le **Decision Tree** reste utile pour sa lisibilité directe par les équipes d'audit.

Les principales limites sont l'utilisation d'un dataset synthétique et le déséquilibre entre classes (Fraud Risk sous-représenté). Des pistes d'amélioration incluent la validation sur des données PCAOB réelles, l'application de SMOTE pour rééquilibrer les classes, et l'intégration de NLP pour analyser les textes des rapports d'audit.
