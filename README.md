# Système de Newsletter Automatisée via GitHub et Mailchimp

![Statut du Workflow](https://github.com/VOTRE_NOM_UTILISATEUR/NOM_DE_VOTRE_DEPOT/actions/workflows/newsletter.yml/badge.svg)

Ce dépôt contient l'ensemble du système permettant d'automatiser la création et l'envoi d'une newsletter. L'objectif est de se concentrer uniquement sur l'écriture du contenu en Markdown, et de laisser la magie de GitHub Actions et de l'API Mailchimp s'occuper de tout le reste.

## ✨ Fonctionnalités Principales

* **Rédaction en Markdown** : Écrivez vos newsletters dans un format simple et lisible.
* **Templates Professionnels** : Le contenu est automatiquement injecté dans un template Mailchimp personnalisé.
* **Automatisation Complète** : Un simple `git push` déclenche tout le processus.
* **Cycle de Validation Sécurisé** : Un e-mail de test est envoyé pour validation avant tout envoi final. Le workflow se met en pause en attendant une approbation manuelle.
* **Auto-Nettoyage** : En cas de rejet de l'envoi de test, la campagne brouillon est automatiquement supprimée de Mailchimp pour garder un environnement propre.

## 🚀 Le Flux d'Automatisation

Le processus suit un cycle de vie précis pour garantir sécurité et qualité :

```
1. [Vous] Écrivez la newsletter (fichier .md)
       │
       └─> 2. [Vous] `git push` vers le dépôt GitHub
             │
             └─> 3. [GitHub Action] Se déclenche
                   │
                   ├─> 4. [Script] Crée une campagne brouillon sur Mailchimp
                   │
                   └─> 5. [Script] Vous envoie un e-mail de test
                         │
                         └─> 6. [GitHub Action] Se met en PAUSE, attendant votre décision
                               /               \
[Vous] REJETEZ ❌ <---------- 7. Vous validez l'e-mail de test ----------> [Vous] APPROUVEZ ✅
       │                                                                    │
       └─> 8a. [Script] Supprime le brouillon sur Mailchimp                  └─> 8b. [Script] Envoie la campagne aux abonnés
       │                                                                    │
       └─> FIN DU PROCESSUS                                                 └─> FIN DU PROCESSUS
```

## 🛠️ Installation et Configuration

Pour faire fonctionner ce système, une configuration initiale est nécessaire.

### Prérequis
* Un compte [GitHub](https://github.com/).
* Un compte [Mailchimp](https://mailchimp.com/).
* [Git](https://git-scm.com/) installé sur votre ordinateur.
* Un éditeur de code (ex: VS Code).

### Étapes de Configuration

#### 1. Côté Mailchimp
- **Créez une Audience** : C'est votre liste de contacts. Récupérez son **ID d'Audience**.
- **Créez une Clé API** : Dans les paramètres de votre compte, générez une clé API. Notez la **Clé API** et le **Préfixe Serveur** (ex: `us19`).
- **Créez un Template d'e-mail** : Concevez le design de votre newsletter.
    - **Important** : Dans une zone de texte, ajoutez l'attribut `mc:edit="main_content"` pour définir où le contenu sera injecté.
    - Récupérez l'**ID numérique du Template**.

#### 2. Côté GitHub
1.  **Clonez ce dépôt** sur votre machine locale.
2.  **Créez l'Environnement de Déploiement** :
    - Allez dans `Settings > Environments > New environment`.
    - Nommez-le `Production`.
    - Ajoutez une règle de protection (`protection rule`) : `Required reviewers` et sélectionnez-vous.
3.  **Configurez les Secrets** :
    - Allez dans `Settings > Secrets and variables > Actions`.
    - Créez les 5 secrets suivants avec les valeurs récupérées sur Mailchimp :
      - `MAILCHIMP_API_KEY`
      - `MAILCHIMP_AUDIENCE_ID`
      - `MAILCHIMP_SERVER_PREFIX`
      - `MAILCHIMP_TEMPLATE_ID`
      - `MAILCHIMP_TEST_EMAIL` (votre propre e-mail pour les tests).

Le système est maintenant configuré et prêt à l'emploi.

## ✍️ Comment Utiliser ce Système au Quotidien

1.  **Rédiger la Newsletter** :
    - Créez un nouveau fichier dans le dossier `/newsletters`. Exemple : `001-ma-premiere-newsletter.md`.
    - La première ligne doit être un titre de niveau 1 (`# Mon Sujet`) ; elle deviendra le sujet de votre e-mail.
    - Rédigez votre contenu en utilisant la syntaxe Markdown.

2.  **Envoyer et Déclencher l'Automatisation** :
    - Ouvrez un terminal, placez-vous dans le dossier du projet et exécutez les commandes suivantes :
    ```bash
    git add .
    git commit -m "Publication de la newsletter #1"
    git push
    ```

3.  **Valider l'Envoi** :
    - Consultez votre boîte mail pour l'e-mail de test.
    - Allez dans l'onglet **Actions** de votre dépôt GitHub.
    - Le workflow sera en attente de votre approbation.
    - **Si tout est correct** : Cliquez sur `Review deployments` et `Approve and deploy`.
    - **S'il y a une erreur** : Cliquez sur `Reject`. La campagne sera automatiquement supprimée de Mailchimp. Corrigez votre fichier `.md` et faites un nouveau `git push` pour relancer le cycle.

## 💻 Outils et Technologies

* [GitHub Actions](https://github.com/features/actions) pour l'automatisation.
* [Python 3](https://www.python.org/) pour le scripting.
* [API Mailchimp v3](https://mailchimp.com/developer/marketing/api/) pour la gestion des campagnes.

---
Ce projet est sous licence MIT.
