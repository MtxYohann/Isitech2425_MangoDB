# Cour MongoDB

## Qu'est-ce que le NoSQL

### Types de bases **NoSQL** :

**Document** : MongoDB, CouchDB

**Clé-valeur** : Redis, DynamoDB

**Colonne** : Cassandra, HBase

**Graphe** : Neo4j, OrientDB

### Caractéristique :

* Schéma flexible ou absent
* Conçu pour la scalabilité (sa réaction à une forme grandissante de données) horizontale
* Optimisé pour des modèles de données
* Compromis dans la cohérence

### Tableau de comparaison de concept SQL / MongoDB
![tableau de comparaison](/img/tableau_comparaison.jpg)

### Pourquoi MongoDB

#### Forces : 

* Schéma flexible adaptatif
* modèle de données intuitif (JSON)
* Performances élever en lecture/écriture (avec beaucoup de données)
* Scalabilité horizontale
* Requêtes riches et expressives
* Indexation avancée
* Distribution géographique
* Support de transaction multi-documents

#### Cas d'usage :
* App web et mobiles
* Gestion de contenus (CMS)
* E-commerce (catalogues produits)
* IoT et données en temps réel
* Big Data et analytique
* Gestion de métadonnées
* Stockage de données de configuration
* Caching et sessions

### Le format BJSON
*BJSON (Binary JSON)* est le format de stockage et d'échange de données utilisé par MangoDB

#### Caractéristique

* Extension binaire de JSON
* Encodage plus efficace en espace
* Support de types additionnels
* Optimisé pour la traversé rapide
* Conçu pour la sérialisation/désérialisation rapide

### tips

pour créer une collection il doit y avoir des données.


## Cours Mardi après-midi Indexation, Agrégation et requête Géospatial

### Indexation et Optimisation des performances

#### Sans Indexation

Performances linéaires O(n)

Problématique sur les grande collections

consomme beaucoup de ressource

#### Avec Indexation

Performances logarithmiques O(log n)

Améliore drastiquement les requêtes

Nécessaire pour mes app en prod

### Types d'index

* Index simple :
impacte sur l'ordre de trie en sortie

* Index composites : 
index sur plusieurs champ en même temps

* Index spécialisé
on verra plus tard

### Gestion des index

1. création d'index

2. les options :
Background, unique, sparse, partialFilterExpression, name

### Analyse des performances

Mode d'explain()

**Métrique à surveiller:**

nReturned

totalKeysExamined

totalDOcsExamined

ExecutionTimeMillis

Stage

* Index couvrants

Quand un index contient tous les champs nécessaires

* Index géospatiaux

