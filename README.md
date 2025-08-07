# Système de Newsletter Automatisée (GitHub & Mailchimp)

![Statut du Workflow](https://github.com/vfarcy/nl_LinkedIN/actions/workflows/newsletter.yml/badge.svg)


Ce dépôt héberge un système d'automatisation complet pour la création et l'envoi de newsletters. Le principe est de séparer entièrement la rédaction du contenu de la complexité technique de l'envoi. L'utilisateur se concentre uniquement sur l'écriture d'un fichier Markdown, et une GitHub Action gère la mise en page, les tests et la distribution via l'API Mailchimp.

## ✨ Fonctionnalités

- **Rédaction Simplifiée** : Écriture des newsletters en format Markdown simple et intuitif.
- **Templates Professionnels** : Le contenu est automatiquement injecté dans un template HTML personnalisé sur Mailchimp, assurant une cohérence de marque.
- **Automatisation via `git push`** : Le simple fait de pousser un nouveau fichier de newsletter sur le dépôt déclenche l'ensemble du processus.
- **Cycle de Validation Sécurisé** : Un e-mail de test est automatiquement envoyé à une adresse prédéfinie. Le workflow se met en pause et attend une approbation manuelle dans l'interface de GitHub avant de continuer.
- **Gestion des Erreurs Intelligente** :
  - **Auto-Nettoyage** : Si un envoi est manuellement **rejeté**, la campagne brouillon est automatiquement supprimée de Mailchimp pour éviter le désordre.
  - **Préservation des Brouillons** : Si un envoi **échoue** pour une raison technique (ex: erreur de configuration Mailchimp), le brouillon est conservé sur Mailchimp pour permettre un diagnostic.
- **Configuration Flexible** : Les informations clés (clés API, ID, nom et e-mail de l'expéditeur) sont gérées de manière sécurisée via les Secrets de GitHub.

## 🚀 Le Flux d'Automatisation

Le processus suit un cycle de vie précis pour garantir sécurité et qualité :

```
1. [Vous] Écrivez la newsletter (fichier .md)
       │
       └─> 2. [Vous] `git push` vers le dépôt GitHub
             │
             └─> 3. [GitHub Action] Détecte le nouveau fichier et se déclenche
                   │
                   ├─> 4. [Script] Crée une campagne brouillon sur Mailchimp
                   │
                   └─> 5. [Script] Vous envoie un e-mail de test
                         │
                         └─> 6. [GitHub Action] Se met en PAUSE, attendant votre décision...
                               /               \
[Vous] REJETEZ ❌ <---------- 7. Vous validez le test ----------> [Vous] APPROUVEZ ✅
       │                                                            │
       ├─> 8a. Le job est "Annulé".                                 ├─> 8b. La campagne e-mail est envoyée via Mailchimp.
       │                                                            │
       └─> 9a. Le script de nettoyage supprime le brouillon.         └─> 9b. [GitHub Action] La construction du site démarre.
             │                                                        │
             └─> FIN (Annulé)                                         └─> 10. Le site statique est construit et déployé.
                                                                          │
                                                                          └─> FIN (Succès)
```

## 🛠️ Installation et Configuration

Une configuration initiale est requise pour lier GitHub et Mailchimp.

### Prérequis
* Un compte [GitHub](https://github.com/).
* Un compte [Mailchimp](https://mailchimp.com/).
* [Git](https://git-scm.com/) installé sur votre ordinateur.

### Étapes de Configuration

#### 1. Configuration de Mailchimp
- **Audience** : Préparez votre liste de contacts et notez son **ID d'Audience** (Plan Mailchimp gratuit jusque 500 abonnés à la newsletter).
- **Clé API** : Générez une clé API et notez la **Clé API** elle-même ainsi que le **Préfixe Serveur** (ex: `us19`).
- **Template d'e-mail** : Utiliser le modèle `emailtemplate.html` ou bien créez un template HTML.
    - **Crucial** : Définissez une zone éditable avec l'attribut `mc:edit="main_content"`.
    - **Crucial** : Assurez-vous que le pied de page (footer) contient les balises obligatoires `*|UNSUB|*` et `*|LIST:ADDRESS_LINE|*`.
    - Notez l'**ID numérique du Template**.
- **Domaine d'envoi** : Assurez-vous d'avoir un domaine d'expéditeur vérifié dans les paramètres de votre compte.

#### 2. Configuration du Dépôt GitHub
1.  **Clonez ce dépôt** sur votre machine.
2.  **Créez l'Environnement** :
    - Dans `Settings > Environments > New environment`, créez un environnement nommé `Production`.
    - Activez la règle `Required reviewers` et ajoutez-vous comme réviseur.
3.  **Configurez les 7 Secrets** :
    - Dans `Settings > Secrets and variables > Actions`, créez les 7 secrets suivants avec les valeurs récupérées précédemment :
      1.  `MAILCHIMP_API_KEY`
      2.  `MAILCHIMP_AUDIENCE_ID`
      3.  `MAILCHIMP_SERVER_PREFIX`
      4.  `MAILCHIMP_TEMPLATE_ID`
      5.  `MAILCHIMP_TEST_EMAIL` (votre e-mail pour recevoir les tests)
      6.  `MAILCHIMP_FROM_NAME` (le nom de l'expéditeur, ex: "La Newsletter XYZ")
      7.  `MAILCHIMP_REPLY_TO_EMAIL` (l'e-mail d'expédition, qui doit être vérifié sur Mailchimp)

## ✍️ Utilisation Quotidienne

1.  **Rédiger** : Créez un nouveau fichier `.md` dans le dossier `/newsletters`. Attention au nom du fichier, obligatoirement de la forme `AAAA-MM-JJ-LeTitre`. La première ligne de ce fichier, (`# Titre`) deviendra le sujet de l'e-mail.
2.  **Déclencher** : Ouvrez votre terminal et envoyez vos modifications.
    ```bash
    git add .
    git commit -m "Publication de la newsletter sur les nouveautés d'août"
    git push
    ```
    **Cas Spécifique : Mettre à jour le site SANS envoyer d'e-mail**
    
    Si vous ne modifiez que des aspects du site (CSS, page "À Propos", etc.) et que vous ne voulez **pas** lancer le processus d'envoi d'e-mail, ajoutez le mot-clé `[skip-email]` à la fin de votre message de commit.
    ```bash
    git commit -m "Fix: Correction d'une typo sur la page A Propos [skip-email]"
    git push
    ```
   Le workflow détectera ce mot-clé et sautera toutes les étapes liées à Mailchimp sans solliciter l'API Mailchimp.
    
3.  **Valider** (si applicable):
    - Allez dans l'onglet **Actions** de votre dépôt GitHub.
    - Vous recevrez l'e-mail de test. Vérifiez-le attentivement.
    - Le workflow sera en attente de votre décision.
    - **Si tout est parfait** : Cliquez sur `Review deployments` et `Approve and deploy`. La campagne est envoyée.
    - **S'il y a une erreur** : Cliquez sur `Reject`. Le workflow s'arrêtera et la campagne brouillon sera automatiquement supprimée de Mailchimp. Vous pouvez alors corriger votre fichier `.md` et relancer le processus avec un nouveau `push`.

## 💻 Technologies Utilisées

* **Automatisation** : [GitHub Actions](https://github.com/features/actions)
* **Scripting** : [Python 3](https://www.python.org/)
* **API** : [API Marketing de Mailchimp v3](https://mailchimp.com/developer/marketing/api/)
* **Contenu** : [Markdown](https://www.markdownguide.org/)
