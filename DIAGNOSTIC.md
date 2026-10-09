# Diagnostic

Une section par test en échec : renseignez ses quatre champs.

## testAddingALineToAnUnknownOrderIsNotFound

**Symptôme** :

**Cause** :

**Règle du module en jeu** :

**Correctif** :

## testAddingALineToAPaidOrderIsAConflict

**Symptôme** :

**Cause** :

**Règle du module en jeu** :

**Correctif** :

## testAddingALineToMyOrder

**Symptôme** :

**Cause** :

**Règle du module en jeu** :

**Correctif** :

## testAddingALineWithAZeroQuantityIsUnprocessable

**Symptôme** :

**Cause** :

**Règle du module en jeu** :

**Correctif** :

## testListingKitchenTicketsReturnsMine

**Symptôme** :

**Cause** :

**Règle du module en jeu** :

**Correctif** :

## testListingKitchenTicketsWithoutTokenIsUnauthorized

**Symptôme** :

**Cause** :

**Règle du module en jeu** :

**Correctif** :

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

**Symptôme** :

**Cause** :

**Règle du module en jeu** :

**Correctif** :
