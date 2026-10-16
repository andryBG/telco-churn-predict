# Telco Churn : méthode PREDICT

## P : Problème
**Contexte :** TelcoPlus (7 000 clients) perd des clients chaque mois (~600 € par
client perdu). L'équipe rétention peut appeler 200 clients/mois (15 € l'appel,
remise moyenne 60 €), aujourd'hui choisis au hasard.

**Décision :** choisir chaque mois, en début de mois, les 200 clients à appeler.

**Problème ML :** classification binaire. Cible : Churn (1 = résilie, 0 = reste).
On prédit une probabilité de départ pour classer les clients.

**Coût des erreurs :** faux négatif (~600 €) beaucoup plus coûteux que faux
positif (~75 €), mais capacité limitée à 200 appels.

**Métrique principale :** Precision@200 (et lift@200). Secondaire : PR-AUC.
L'accuracy est écartée (classes déséquilibrées).

**Baselines :** (1) appel au hasard, lift = 1 ; (2) règle simple
(contrat mensuel + faible ancienneté).

**Limite connue :** prédire le risque de départ ne garantit pas que la remise
convaincra le client à rester.

## R : Données
Source : Telco Customer Churn (IBM), 7 043 clients, 21 colonnes. Copie brute dans `data/telco_churn_raw.csv`.

**Décisions sur les colonnes (provisoires, à confirmer en D) :**
- Cible : `Churn`.
- Retirées : `customerID` (identifiant), `TotalCharges` (redondante avec tenure x MonthlyCharges, risque de fuite), `gender` (aucun gain, risque éthique).
- Gardées sous surveillance : `SeniorCitizen`, `Partner`, `Dependents` (vérification d'équité prévue en étape C).
- Toutes les autres colonnes sont connues en début de mois : utilisables.

**Questions pour TelcoPlus :**
1. Sur quelle période ces données ont-elles été collectées, et la situation commerciale est-elle comparable à aujourd'hui ?
2. Comment `Churn` est-il défini (événement exact et fenêtre de temps) ?
