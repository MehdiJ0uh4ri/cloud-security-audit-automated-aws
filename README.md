# Scripts d'automatisation des audits de sécurité AWS

Une collection d'automatisation de la sécurité AWS comportant des outils d'audit, des remédiations automatisées, une validation de la conformité (SOC 2, GDPR), et de l'optimisation des coûts pour les environnements.

#### Fonctionnalités

**Outils de Scannage de Sécurité**

* **Intégration Prowler** : Évaluations de sécurité automatisées quotidiennes avec Prowler.
* **Intégration ScoutSuite** : Scans de sécurité quotidiens avec ScoutSuite.
* **AWS Trusted Advisor** : Rapport automatisé des résultats des contrôles AWS Trusted Advisor.

**Fonctions de Remédiation Automatisée**

* **Chiffrement des volumes EBS** : Identification et chiffrement automatiques des volumes EBS non chiffrés.
* **Conformité des volumes EBS** : Surveillance et notification des volumes EBS trop volumineux.
* **Remédiation des groupes de sécurité** : Suppression automatique des règles SSH trop permissives (0.0.0.0/0 sur le port 22).
* **Renforcement des politiques IAM** : Identification et suppression des politiques IAM trop permissives.
* **Renforcement de la sécurité RDS** : Activation du chiffrement, des sauvegardes et des configurations de sécurité pour RDS.
* **Nettoyage des ressources inutilisées** : Identification et suppression des ressources AWS inutilisées pour réduire les coûts.

**Fonctions d'Évaluation et de Conformité**

* **Vérification de la conformité SOC 2** : Validation de l'environnement AWS contre les contrôles de sécurité SOC 2.
* **Scanner de protection des données GDPR** : Recherche des violations de conformité GDPR et des informations personnelles non chiffrées (PII).

#### Prérequis

* Compte AWS
* Actions GitHub
* Python 3.9+
* AWS CLI

#### Structure du dépôt

```
.
├── .github/workflows/
│   ├── prowler.yml        # Workflow GitHub Actions pour Prowler
│   └── scoutsuite.yml     # Workflow GitHub Actions pour ScoutSuite
├── AWS_Trusted_Advisor/
│   └── Lambda_function.py # Fonction de rapport Trusted Advisor
├── Lambda_fn/
│   ├── EBS_Volume_compliance_notification.py  # Surveillance des volumes EBS
│   ├── EBS_Volume_Encryption.py               # Remédiation de chiffrement EBS
│   ├── SG-Remediation.py                      # Nettoyage des groupes de sécurité
│   ├── IAM_Policy_Hardening.py                # Remédiation des politiques IAM
│   ├── RDS_Security_Hardening.py              # Améliorations de la sécurité RDS
│   ├── Unused_Resource_Cleanup.py             # Automatisation du nettoyage des ressources
│   ├── SOC2_Compliance_Checker.py             # Validation de conformité SOC 2
│   └── GDPR_Data_Protection_Scanner.py        # Scan de conformité GDPR
└── Prowler_Codedeploy/
    └── buildspec.yml      # Spécification AWS CodeBuild pour Prowler
```

#### Instructions d'installation

1. **Configuration des Actions GitHub**
2. **Configuration AWS**
3. **Déploiement des fonctions Lambda**

#### Planning des scans de sécurité

| Service         | Planification                       |
| --------------- | ----------------------------------- |
| Prowler         | Quotidiennement à 2h00 UTC          |
| ScoutSuite      | Quotidiennement à 3h00 UTC          |
| Trusted Advisor | Configurable via déclencheur Lambda |

#### Documentation des fonctions Lambda

##### Fonctions de remédiation de sécurité

1. **Chiffrement des volumes EBS** (EBS\_Volume\_Encryption.py)
2. **Remédiation des groupes de sécurité** (SG-Remediation.py)
3. **Renforcement des politiques IAM** (IAM\_Policy\_Hardening.py)
4. **Renforcement de la sécurité RDS** (RDS\_Security\_Hardening.py)
5. **Nettoyage des ressources inutilisées** (Unused\_Resource\_Cleanup.py)

##### Fonctions d'évaluation et de conformité

6. **Vérification de la conformité SOC 2** (SOC2\_Compliance\_Checker.py)
7. **Scanner de protection des données GDPR** (GDPR\_Data\_Protection\_Scanner.py)
8. **Notification de conformité des volumes EBS** (EBS\_Volume\_compliance\_notification.py)
9. **Intégration avec AWS Trusted Advisor** (Lambda\_function.py)

#### Guide de déploiement et d'utilisation

**Déploiement rapide**

Clonez le dépôt :

```bash
git clone https://github.com/ToluGIT/AWS-Security-Audits-Automated.git
cd AWS-Security-Audits-Automated
```

Configurez les buckets S3 pour les rapports :

```bash
# Créer des buckets pour différents types de rapports
aws s3 mb s3://your-security-reports-bucket
aws s3 mb s3://your-prowler-reports-bucket
aws s3 mb s3://your-compliance-reports-bucket
```

Déployez les fonctions Lambda :

```bash
# Emballez chaque fonction avec ses dépendances
cd Lambda_fn
zip -r iam_policy_hardening.zip IAM_Policy_Hardening.py
aws lambda create-function --function-name IAM-Policy-Hardening \
  --runtime python3.9 --role arn:aws:iam::123456789012:role/lambda-execution-role \
  --handler IAM_Policy_Hardening.lambda_handler --zip-file fileb://iam_policy_hardening.zip
```

#### Planification d'exécution recommandée

| Fonction                               | Fréquence     | Raison                                 | Meilleur Moment           |
| -------------------------------------- | ------------- | -------------------------------------- | ------------------------- |
| Vérification de la conformité SOC 2    | Mensuel       | Cycle de rapport de conformité         | 1er du mois, 9h UTC       |
| Scanner de protection des données GDPR | Trimestriel   | Préparation à l'audit réglementaire    | 1er du trimestre, 10h UTC |
| Renforcement des politiques IAM        | Hebdomadaire  | Prévention de la dérive des politiques | Dimanche, 2h UTC          |
| Renforcement de la sécurité RDS        | Mensuel       | Maintenance de la sécurité des bases   | 2ème dimanche, 3h UTC     |
| Nettoyage des ressources inutilisées   | Hebdomadaire  | Optimisation des coûts                 | Vendredi, 23h UTC         |
| Chiffrement des volumes EBS            | À la demande  | Pour les nouvelles instances           | Déclenchement manuel      |
| Remédiation des groupes de sécurité    | En temps réel | Réponse immédiate aux menaces          | Déclencheur CloudTrail    |
| Conformité des volumes EBS             | Quotidien     | Surveillance proactive                 | 6h UTC                    |
| Intégration Trusted Advisor            | Quotidien     | Amélioration continue                  | 4h UTC                    |

#### Modèles d'architecture

**Modèle 1 : Réponse immédiate à la sécurité**

CloudTrail → Événements CloudWatch → Lambda → Remédiation immédiate

* **Utilisation pour** : Remédiation des groupes de sécurité
* **Avantages** : Atténuation immédiate des menaces, fenêtre d'exposition minimale
* **Configuration** : Activer les événements de données CloudTrail, créer des règles EventBridge

**Modèle 2 : Évaluation de la conformité programmée**

Événements CloudWatch → Lambda → Rapports S3 → Notifications SNS

* **Utilisation pour** : Vérifications SOC 2, conformité GDPR
* **Avantages** : Surveillance régulière de la conformité, génération d'audits
* **Configuration** : Créer des règles EventBridge programmées, configurer des sujets SNS

**Modèle 3 : Nettoyage optimisé des coûts**

Événements CloudWatch → Lambda → Création de sauvegarde → Suppression des ressources → Rapports de coûts

* **Utilisation pour** : Nettoyage des ressources inutilisées
* **Avantages** : Suppression sûre des ressources, suivi des coûts
* **Configuration** : Planification pendant les périodes de faible utilisation, activer la facturation détaillée

#### Surveillance et alertes

**Métriques CloudWatch à suivre**

* **Taux de succès de l'exécution** : AWS/Lambda/Errors
* **Temps de traitement** : AWS/Lambda/Duration
* **Économies de coûts** : Métier personnalisé à partir des fonctions de nettoyage
* **Score de conformité** : Métier personnalisé à partir des fonctions d'évaluation
* **Conclusions de sécurité** : Métier personnalisé à partir des fonctions de remédiation

**Alarmes recommandées**

```bash
# Alarme de défaillance de fonction Lambda
aws cloudwatch put-metric-alarm \
  --alarm-name "Security-Lambda-Failures" \
  --alarm-description "Alerte sur les défaillances des fonctions de sécurité" \
  --metric-name Errors \
  --namespace AWS/Lambda \
  --statistic Sum \
  --period 300 \
  --threshold 1 \
  --comparison-operator GreaterThanOrEqualToThreshold \
  --evaluation-periods 1
```

#### Configuration de sécurité

\*\*Autorisations IAM nécessaires par fonction
