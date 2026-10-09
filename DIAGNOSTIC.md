# Diagnostic

Une section par test en échec : renseignez ses quatre champs.

## testAddingALineToAnUnknownOrderIsNotFound

**Symptôme** : On recoit un 500 au lieu d'un 404 si on essaye d'ajouter des lignes sur un order non accessible.

**Cause** : L'opération ne fait pas remonter l'object réel sur lequel faire l'évaluation d'appartenance donc le serveur lève la 500 en premier.

**Règle du module en jeu** : Un Provider doit servir l'object courant pour vérifier cette règle sécurité sur une opération.

**Correctif** : Rajouter le OrderProvider qui fournit cet object directement sur l'opération concernée

## testAddingALineToAPaidOrderIsAConflict

**Symptôme** : Ajouter une ligne dans un order déjà payé renvoie une 201 signe de réussite de l'opération au lieu d'une 409 qui indiquerait que cette opération crée un conflit

**Cause** : Le service d'Order ne fait pas de vérifications sur le statut de l'order avant de faire un addLine()

**Règle du module en jeu** : Toujours faire les vérifications sur les opérations d'ajout dans un cas de lien fort car une ligne qui se crée dans un order déjà payé va à l'encontre du contrat métier.

**Correctif** : Rajouter la vérification du status du panier dans addLine() de OrderService et lever l'exception appropriée.

## testAddingALineToMyOrder

**Symptôme** :

**Cause** :

**Règle du module en jeu** :

**Correctif** :

## testAddingALineWithAZeroQuantityIsUnprocessable

**Symptôme** : On peut rajouter une ligne avec une quantité de 0 au lieu de recevoir une 422 qui indique que l'entré n'est pas valide

**Cause** : La DTO d'entrée pour les lignes fais valider une assertion, ou une validation fausse selon les règles métiers en acceptant des quantités positives ou nulles.

**Règle du module en jeu** : Toujours bien faire valider en surface le format des objets reçus en entrée pour se conformer au contrat métier

**Correctif** : Corriger la contrainte sur la quantité en mettant juste le "#[Assert\Positive]"

## testListingKitchenTicketsReturnsMine

**Symptôme** : Alice consulte à la demande des bons qui pourtant ne lui appartiennent pas, elle n'en a pas et devrait pas en recevoir

**Cause** : La requête de récupération de bons est mal formée et laisse remonter tous les bons sans filtres

**Règle du module en jeu** : Un utilisateur ne doit avoir accès qu'au données qui le concerne et cette règle commence par la bonne constitution de requêtes

**Correctif** : Rajouter le filtre qui vérifie que les bons à remonter appartiennent bien à l'user courant dans le repository des Kitchens Tickets

## testListingKitchenTicketsWithoutTokenIsUnauthorized

**Symptôme** : L'on peut lister les bons/tickets diponibles alors qu'on est pas connecté (200) au lieu d'une 401 qui déclare qu'on est pas authorisé car pas authentifier

**Cause** : L'opération de récupérations de la liste de tickets n'est pas du tout sécurisée

**Règle du module en jeu** : Les opérations qui nécessitent une authentification doivent être sécurisées dans la déclaration de l'opération pour que la règle d'authentification soit la priorité

**Correctif** : Rajouter le security: "is_granted('ROLE_USER')" sur l'opération GET des tickets dans le domaine concerné

## testOpeningAnOrderIgnoresAnAbandonedOne

**Symptôme** : Le test attends de recevoir un tableau vide soit création d'un order vide sur le POST, alors que l'API elle renvoie quand même les informations d'un order alors qu'il est abandonné.
**Cause** : La requête part avec un biais depuis le repository, puisque le filtre sur la colonne deletedAt n'est pas appliqué, et les données suprimées(soft-delete) remontent.

**Règle du module en jeu** : Une base de donnée doit faire du soft-delete dans l'idéal, donc toutes les requêtes qui font remonter de la donnée doivent filtrer la colonne pour éviter de faire monter des informations "supprimées"

**Correctif** : Rajouter le filtre (deletedAt is null )directement sur la requête findActiveFor dans le OrderRepository. Dans une persepective d'évolution créer un SqlFilter Général pour filtrer automatiquement les requêtes.

## testPayingMyOrderMarksItPaid

**Symptôme** :

**Cause** :

**Règle du module en jeu** :

**Correctif** :

## testRefreshingTwiceWithTheSameTokenIsUnauthorized

**Symptôme** :

**Cause** :

**Règle du module en jeu** :

**Correctif** :

## testRemovingALineFromSomeoneElsesOrderIsForbidden

**Symptôme** : Essayer de supprimer la ligne d'un autre user renvoie une 204 au lieu d'une 403 qui indique clairement qu'il n'est pas autorisé à faire cette action

**Cause** : les règles de sécurtité sur cette opérations ne sont pas strictes et ne permettent pas de lever la bonne exception

**Règle du module en jeu** : Les règles de sécurité se déclarent sur l'opération et doivent êtres le plus stricte et le plus explicite possible

**Correctif** : Remplacer le or par and au niveau de security de l'operation DELETE concerneé pour valider strictement les deux règles : avoir un rôle user et être le propriétaire de la ressource à supprimer
