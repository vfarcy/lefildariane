---
layout: post
title: "J'ai débogué ma plante verte (c'était un problème de cache)"
---

# J'ai débogué ma plante verte (c'était un problème de cache)

Ma plante verte (un Ficus Benjamina v1.3, nom de code "Robert") ne répondait plus correctement. Les feuilles jaunissaient. Le déploiement de nouvelles branches était en échec. Clairement, il y avait un bug dans la production.

Mon premier réflexe a été de consulter la documentation en ligne (un forum de jardinage). La réponse classique : "Avez-vous essayé de l'arroser et de ne pas l'arroser en même temps ?" Une approche quantique du problème, peu utile.

J'ai donc décidé d'appliquer une méthode de diagnostic systémique.

1.  **Analyse des logs :** Les feuilles du bas tombaient. C'est un `Error 404: Nutriments not found`.
2.  **Vérification des dépendances :** Le pot semblait à la bonne taille. La terre n'était pas un `legacy system` obsolète.
3.  **Le "ping" :** J'ai tapoté le pot. Pas de réponse. Le système était clairement `unresponsive`.

Puis j'ai eu une révélation en déplaçant Robert. Le problème n'était pas l'input (l'eau) ou l'output (les feuilles), mais l'environnement d'exécution. Il était placé juste à côté d'une fenêtre où le soleil tapait fort à midi.

Le problème, ce n'était pas un bug. C'était un **problème de cache**.

Le cache du sol s'asséchait trop vite à cause d'une surcharge de requêtes solaires. La solution n'était pas d'ajouter plus de ressources (plus d'eau), mais de déplacer le serveur.

Je l'ai mis dans un coin avec une lumière indirecte. J'ai aussi fait un `reboot` en le rempotant. Les performances sont revenues à la normale.

La zététique nous apprend à questionner nos hypothèses. Mon erreur a été de traiter la plante comme un système isolé, alors qu'elle faisait partie d'un système plus large incluant une étoile située à 150 millions de kilomètres. Un oubli de débutant.

