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


## Cours Mardi après-midi Indexation, Agrégation et requette Géospatial

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

# LES TP :

## TP 2 : Manipulation de données avec les opérations CRUD

### Exercice 1 : Création d’un jeu de données
**Création** de la collection ecommerce_produits via compass :

![création collection ecommerce_produits](/img/ecommerce.jpg)

**Insertion des 10 produits**

```bash
db.ecommerce_produits.insertMany([
    {
        nom: "Cube Anti-Stress",
        description: "Un cube pour réduire le stress et l'anxiété.",
        prix: 15.99,
        stock: {
            quantite: 300,
            seuilAlerte: 20,
            statut: "disponible",
            entrepots: [
                { id: "ENT-PARIS", quantite: 200 },
                { id: "ENT-LYON", quantite: 100 }
            ]
        },
        caracteristiques: {
            materiau: "Plastique",
            couleur: "noir",
            poids: 50,
            dimensions: {
                longueur: 50,
                largeur: 50,
                hauteur: 50
            }
        },
        categorie: "Jeux",
        "sous-categorie": "Anti-Stress",
        avis: [
            {
                utilisateur: "Alice Martin",
                commentaire: "Très efficace pour réduire le stress.",
                note: 4,
                date: new Date("2025-01-10")
            },
            {
                utilisateur: "Bob Brown",
                commentaire: "Bon produit mais un peu fragile.",
                note: 3,
                date: new Date("2025-01-15")
            }
        ],
        tags: ["stress", "cube", "noir", "relaxation"]
    },
    {
        nom: "Balle Anti-Stress",
        description: "Une balle souple pour évacuer le stress.",
        prix: 9.99,
        stock: {
            quantite: 500,
            seuilAlerte: 30,
            statut: "disponible",
            entrepots: [
                { id: "ENT-PARIS", quantite: 300 },
                { id: "ENT-LYON", quantite: 200 }
            ]
        },
        caracteristiques: {
            materiau: "Caoutchouc",
            couleur: "rouge",
            poids: 60,
            dimensions: {
                longueur: 70,
                largeur: 70,
                hauteur: 70
            }
        },
        categorie: "Jeux",
        "sous-categorie": "Anti-Stress",
        avis: [
            {
                utilisateur: "Charlie Davis",
                commentaire: "Très agréable à utiliser.",
                note: 5,
                date: new Date("2025-02-01")
            },
            {
                utilisateur: "Dana Evans",
                commentaire: "Bonne qualité, je recommande.",
                note: 4,
                date: new Date("2025-02-05")
            }
        ],
        tags: ["stress", "balle", "rouge", "relaxation"]
    },
    {
        nom: "Puzzle 3D",
        description: "Un puzzle en 3D pour les amateurs de défis.",
        prix: 25.99,
        stock: {
            quantite: 150,
            seuilAlerte: 10,
            statut: "disponible",
            entrepots: [
                { id: "ENT-PARIS", quantite: 100 },
                { id: "ENT-LYON", quantite: 50 }
            ]
        },
        caracteristiques: {
            materiau: "Bois",
            couleur: "naturel",
            poids: 200,
            dimensions: {
                longueur: 150,
                largeur: 150,
                hauteur: 150
            }
        },
        categorie: "Jeux",
        "sous-categorie": "Puzzle",
        avis: [
            {
                utilisateur: "Eve Foster",
                commentaire: "Très bon puzzle, bien conçu.",
                note: 5,
                date: new Date("2025-03-01")
            },
            {
                utilisateur: "Frank Green",
                commentaire: "Un peu difficile mais très satisfaisant.",
                note: 4,
                date: new Date("2025-03-05")
            }
        ],
        tags: ["puzzle", "3D", "bois", "défi"]
    },
    {
        nom: "Jeu de Cartes",
        description: "Un jeu de cartes classique pour toute la famille.",
        prix: 5.99,
        stock: {
            quantite: 1000,
            seuilAlerte: 50,
            statut: "disponible",
            entrepots: [
                { id: "ENT-PARIS", quantite: 600 },
                { id: "ENT-LYON", quantite: 400 }
            ]
        },
        caracteristiques: {
            materiau: "Papier",
            couleur: "multicolore",
            poids: 100,
            dimensions: {
                longueur: 90,
                largeur: 60,
                hauteur: 20
            }
        },
        categorie: "Jeux",
        "sous-categorie": "Cartes",
        avis: [
            {
                utilisateur: "Grace Harris",
                commentaire: "Très bon jeu de cartes, idéal pour les soirées.",
                note: 5,
                date: new Date("2025-01-20")
            },
            {
                utilisateur: "Henry Johnson",
                commentaire: "Les cartes sont de bonne qualité.",
                note: 4,
                date: new Date("2025-01-25")
            }
        ],
        tags: ["cartes", "jeu", "famille", "classique"]
    },
    {
        nom: "Jeu de Société",
        description: "Un jeu de société amusant pour toute la famille.",
        prix: 29.99,
        stock: {
            quantite: 200,
            seuilAlerte: 20,
            statut: "disponible",
            entrepots: [
                { id: "ENT-PARIS", quantite: 120 },
                { id: "ENT-LYON", quantite: 80 }
            ]
        },
        caracteristiques: {
            materiau: "Plastique",
            couleur: "multicolore",
            poids: 500,
            dimensions: {
                longueur: 300,
                largeur: 300,
                hauteur: 100
            }
        },
        categorie: "Jeux",
        "sous-categorie": "Société",
        avis: [
            {
                utilisateur: "Ivy King",
                commentaire: "Très amusant, on ne s'en lasse pas.",
                note: 5,
                date: new Date("2025-02-10")
            },
            {
                utilisateur: "Jack Lee",
                commentaire: "Bon jeu mais les règles sont un peu compliquées.",
                note: 3,
                date: new Date("2025-02-15")
            }
        ],
        tags: ["société", "jeu", "famille", "amusement"]
    },
    {
        nom: "Jeu de Construction",
        description: "Un jeu de construction pour développer la créativité.",
        prix: 19.99,
        stock: {
            quantite: 250,
            seuilAlerte: 15,
            statut: "disponible",
            entrepots: [
                { id: "ENT-PARIS", quantite: 150 },
                { id: "ENT-LYON", quantite: 100 }
            ]
        },
        caracteristiques: {
            materiau: "Plastique",
            couleur: "multicolore",
            poids: 300,
            dimensions: {
                longueur: 200,
                largeur: 200,
                hauteur: 100
            }
        },
        categorie: "Jeux",
        "sous-categorie": "Construction",
        avis: [
            {
                utilisateur: "Karen Miller",
                commentaire: "Très bon jeu pour les enfants.",
                note: 5,
                date: new Date("2025-03-01")
            },
            {
                utilisateur: "Leo Nelson",
                commentaire: "Les pièces sont solides et bien conçues.",
                note: 4,
                date: new Date("2025-03-05")
            }
        ],
        tags: ["construction", "jeu", "créativité", "enfants"]
    },
    {
        nom: "Jeu de Mémoire",
        description: "Un jeu pour améliorer la mémoire des enfants.",
        prix: 12.99,
        stock: {
            quantite: 400,
            seuilAlerte: 25,
            statut: "disponible",
            entrepots: [
                { id: "ENT-PARIS", quantite: 250 },
                { id: "ENT-LYON", quantite: 150 }
            ]
        },
        caracteristiques: {
            materiau: "Carton",
            couleur: "multicolore",
            poids: 150,
            dimensions: {
                longueur: 150,
                largeur: 150,
                hauteur: 50
            }
        },
        categorie: "Jeux",
        "sous-categorie": "Mémoire",
        avis: [
            {
                utilisateur: "Mia Owens",
                commentaire: "Très bon jeu pour les enfants.",
                note: 5,
                date: new Date("2025-01-30")
            },
            {
                utilisateur: "Noah Parker",
                commentaire: "Les enfants adorent ce jeu.",
                note: 4,
                date: new Date("2025-02-05")
            }
        ],
        tags: ["mémoire", "jeu", "enfants", "éducation"]
    },
    {
        nom: "Jeu de Stratégie",
        description: "Un jeu de stratégie pour les amateurs de défis.",
        prix: 34.99,
        stock: {
            quantite: 100,
            seuilAlerte: 10,
            statut: "disponible",
            entrepots: [
                { id: "ENT-PARIS", quantite: 60 },
                { id: "ENT-LYON", quantite: 40 }
            ]
        },
        caracteristiques: {
            materiau: "Plastique",
            couleur: "multicolore",
            poids: 600,
            dimensions: {
                longueur: 350,
                largeur: 350,
                hauteur: 150
            }
        },
        categorie: "Jeux",
        "sous-categorie": "Stratégie",
        avis: [
            {
                utilisateur: "Olivia Quinn",
                commentaire: "Très bon jeu de stratégie, bien conçu.",
                note: 5,
                date: new Date("2025-02-20")
            },
            {
                utilisateur: "Paul Roberts",
                commentaire: "Un peu compliqué mais très intéressant.",
                note: 4,
                date: new Date("2025-02-25")
            }
        ],
        tags: ["stratégie", "jeu", "défi", "intéressant"]
    },
    {
        nom: "Jeu de Réflexion",
        description: "Un jeu pour stimuler la réflexion et la logique.",
        prix: 22.99,
        stock: {
            quantite: 180,
            seuilAlerte: 15,
            statut: "disponible",
            entrepots: [
                { id: "ENT-PARIS", quantite: 120 },
                { id: "ENT-LYON", quantite: 60 }
            ]
        },
        caracteristiques: {
            materiau: "Bois",
            couleur: "naturel",
            poids: 250,
            dimensions: {
                longueur: 200,
                largeur: 200,
                hauteur: 100
            }
        },
        categorie: "Jeux",
        "sous-categorie": "Réflexion",
        avis: [
            {
                utilisateur: "Quinn Scott",
                commentaire: "Très bon jeu pour stimuler la réflexion.",
                note: 5,
                date: new Date("2025-03-10")
            },
            {
                utilisateur: "Rachel Taylor",
                commentaire: "Les enfants adorent ce jeu.",
                note: 4,
                date: new Date("2025-03-15")
            }
        ],
        tags: ["réflexion", "jeu", "logique", "enfants"]
    },
    {
        nom: "HandSnipper",
        description: "sa tourne",
        prix: "13.99",
        stock: {
            quantite: 200,
            seuilAlerte: 10,
            statut: "disponible",
            entrepots: [
                { id: "ENT-PARIS", quantite: 150 },
                { id: "ENT-LYON", quantite: 50 }
            ]
        },
        caracteristiques: {
            materiau: "Métal",
            couleur: "bleu",
            poids: 40,
            dimensions: {
                longueur: 730,
                largeur: 730,
                hauteur: 70
            }
        },
        categorie: "Jeux",
        "sous-categorie": "Satifaisant",
        avis: [
            {
                utilisateur: "John Doe",
                commentaire: "Très bon produit, mes enfants l'adorent !",
                note: 5,
                date: new Date("2025-02-15")
            },
            {
                utilisateur: "Jane Smith",
                commentaire: "Pas mal, mais un peu cher pour ce que c'est.",
                note: 3,
                date: new Date("2025-02-20")
            }
        ],
        tags: ["enfant", "satisfaissant", "bleu", "tourne", "spin"]
    }
])
```
### Exercice 2 : Requêtes de lecture

Commande pour récupérer dans la collection ecommerce_produits les objets qui ont "Jeux" en catégorie: 

```db.ecommerce_produits.find({categorie: {$eq: "Jeux"}})```

Trouver les produits dont le prix est entre 20€ et 30€

```db.ecommerce_produits.find({prix: {$gt: 20, $lt: 30 }})```

lister les porduits en stock (stock >0)

```db.ecommerce_produits.find({"stock.quantite": {$gt: 0 }})```

Lister les porduits avec au moins 3 avis

```
db.ecommerce_produits.find({ $expr: {$gte: [{ $size: "$avis" }, 2] }})
```

### Exercice 3 : Mises à jour

Augmenter le prix de **5%** des produit d'une même catégorie

```
db.ecommerce_produits.updateMany({categorie: {$eq: "Jeux"}},{$mul: {prix: 1.05}})
```

Ajouter un champ "promotion" à certain produits produits (si il a le tag "famille")

```
db.ecommerce_produits.updateMany(
        { tags: {$eq: "famille"} },
        { $set: { promotion: {actif: true, pourcentage: 25,dateDebut: new Date("2025-03-03"), dateFin: new Date("2025-04-03") }} }
)
```

Ajouter un nouveau tag à tous les produits d'une catégorie

```
db.ecommerce_produits.updateMany(
        { categorie: {$eq: "Jeux"} },
        { $addToSet: { tags: "nouveau"}}
)
```

Mettre à jour le stock après une vente

```
db.ecommerce_produits.updateOne(
    {nom: "HandSnipper"},
    { $inc: { "stock.quantite": -1 } }
)
```

### Exercice 4 : Requêtes complexes

Trouver les produits disponibles avec tag1 et tag2

```
db.ecommerce_produits.find({
    "stock.statut": "disponible",
    tags: { $all: ["tag1", "tag2"] }
})
```

Lister les produits premium avec un stock faible
```
db.ecommerce_produits.find({
    tags: "premium",
    "stock.quantite": {$lt: 5}
})
```

Rechercher les produits ayant reçu au moins un avis 5 étoiles
```
db.ecommerce_produits.find({
    avis: { $elemMatch: { note: 5 } }
})
```

Trouver les produits d'une catégorie, trié par prix décroissant, limités aux 5 premier

```
db.ecommerce_produits.find({ categorie: "Jeux" })
    .sort({ prix: -1 })
    .limit(5)
```

## TP 3 SIMPLIFIÉ MONGODB - JOUR 1 : GESTION D'UNE BIBLIOTHÈQUE

### Partie 1 Configuration et création de la base de données

#### 1.1 Configuration de l'environnement

![configuration environnement](/img/biblio.jpg)

#### 1.2 Insertion de documents (Create)

Insertion livres :

```bash
db.livres.insertMany([
    {
        titre: "1984",
        auteur: "George Orwell",
        annee_publication: 1949,
        editeur: "Secker & Warburg",
        genre: ["Dystopie", "Science-fiction"],
        nombre_pages: 328,
        langue: "Anglais",
        disponible: true,
        stock: 5,
        note_moyenne: 4.7,
        description: "Un roman dystopique qui explore les dangers du totalitarisme et de la surveillance de masse.",
        prix: 9.99,
        isbn: "9780451524935",
        date_ajout: new Date("2023-02-10")
    },
    {
        titre: "Pride and Prejudice",
        auteur: "Jane Austen",
        annee_publication: 1813,
        editeur: "T. Egerton",
        genre: ["Roman", "Romance"],
        nombre_pages: 279,
        langue: "Anglais",
        disponible: true,
        stock: 7,
        note_moyenne: 4.6,
        description: "Un classique de la littérature anglaise qui explore les thèmes de l'amour et des préjugés sociaux.",
        prix: 8.50,
        isbn: "9780141439518",
        date_ajout: new Date("2023-03-05")
    },
    {
        titre: "To Kill a Mockingbird",
        auteur: "Harper Lee",
        annee_publication: 1960,
        editeur: "J.B. Lippincott & Co.",
        genre: ["Roman", "Drame"],
        nombre_pages: 281,
        langue: "Anglais",
        disponible: true,
        stock: 4,
        note_moyenne: 4.9,
        description: "Un roman poignant sur les injustices raciales dans le sud des États-Unis, vu à travers les yeux d'une jeune fille.",
        prix: 10.99,
        isbn: "9780061120084",
        date_ajout: new Date("2023-04-12")
    },
    {
        titre: "Moby-Dick",
        auteur: "Herman Melville",
        annee_publication: 1851,
        editeur: "Harper & Brothers",
        genre: ["Roman", "Aventure"],
        nombre_pages: 635,
        langue: "Anglais",
        disponible: true,
        stock: 2,
        note_moyenne: 4.3,
        description: "L'histoire épique de la quête obsessionnelle du capitaine Achab pour capturer la baleine blanche Moby Dick.",
        prix: 12.50,
        isbn: "9780142437247",
        date_ajout: new Date("2023-05-20")
    },
    {
        titre: "Le Petit Prince",
        auteur: "Antoine de Saint-Exupéry",
        annee_publication: 1943,
        editeur: "Gallimard",
        genre: ["Conte", "Philosophie"],
        nombre_pages: 96,
        langue: "Français",
        disponible: true,
        stock: 5,
        note_moyenne: 48,
        description: "Un pilote d'avion, qui s'est écrasé dans le désert du Sahara, rencontre un jeune prince venu d'une autre planète...",
        prix: 7.50,
        isbn: "9782070612758",
        date_ajout: new Date("2023-01-15")
    }
])
```

Insertion utilisateurs :

```bash
db.utilisateurs.insertMany([
	{
	  nom: "Mathieux",
	  prenom: "Yohann",
	  email: "yohann.mathieux@gmail.com",
	  age: 20,
	  adresse: {
		rue: "40 route de soucieu",
		ville: "Saint Laurent-d'agny",
		code_postal: "69440"
	  },
	  date_inscription: new Date("2025-03-03"),
	  livres_empruntes: [
		{
		  livre_id: ObjectId("67c5d089b0e44d2765d0a17b"),
		  titre: "Le Petit Prince",
		  date_emprunt: new Date("2025-03-03"),
		  date_retour_prevue: new Date("2025-03-17")
		}
	  ],
	  tags: ["Aventure", "Conte"]
	},
	{
	  nom: "Nova",
	  prenom: "Arthur",
	  email: "arhur.nova@gmail.com",
	  age: 20,
	  adresse: {
		rue: "8 impasse des sources",
		ville: "Saint martin en haut",
		code_postal: "69850"
	  },
	  date_inscription: new Date("2025-01-23"),
	  livres_empruntes: [
		{
		  livre_id: ObjectId("67c5d089b0e44d2765d0a177"),
		  titre: "1984",
		  date_emprunt: new Date("2025-03-02"),
		  date_retour_prevue: new Date("2025-03-16")
		}
	  ],
	  tags: ["Histoire", "Science-fiction"]
	},
	{
	  nom: "Girardet",
	  prenom: "Maxime",
	  email: "maxime.girardet@gmail.com",
	  age: 20,
	  adresse: {
		rue: "25 chemin du pré poulet",
		ville: "Montagny",
		code_postal: "69700"
	  },
	  date_inscription: new Date("2025-02-03"),
	  livres_empruntes: [
		{
		  livre_id: ObjectId("67c5d089b0e44d2765d0a178"),
		  titre: "Pride and Prejudice",
		  date_emprunt: new Date("2025-03-03"),
		  date_retour_prevue: new Date("2025-03-17")
		}
	  ],
	  tags: ["Romance", "Conte"]
	}
])
```


### Partie 2 : Requêtes de lecture (Read)

1. ``db.livres.find({disponible: {$eq: true }})``

2. ``db.livres.find({annee_publication: {$gt: 2000}})``

3. ``db.livres.find({auteur: {$eq: "Antoine de Saint-Exupéry" }})``

4. ``db.livres.find({note_moyenne: {$gt: 4 }})``

5. ``db.utilisateurs.find({"adresse.ville": {$eq: "Saint Laurent-d'agny" }})``

6. ``db.livres.find({genre: "Romance"})``

7. ``db.livres.find({prix: {$lt: 15}, note_moyenne: {$gt: 4 }})``

8. ``db.utilisateurs.find({
    livres_empruntes: {
        $elemMatch: { titre: "Le Petit Prince" }
    }
})``

### Partie 3 : Mise à jour de documents (Update)

1. ``db.livres.updateOne({titre: "1984"},  {$set: {titre: "1985"}})``

2. ``db.livres.updateMany({},{$set: {stock: 5}})``

3. ``db.livres.updateOne({titre: "1985"}, {$set: {disponible: false}})``

4.
```db.utilisateurs.updateOne(
        {nom: "Nova"},
        {$push: { 
            livres_empruntes: {
                livre_id: ObjectId("67c5d089b0e44d2765d0a17a"),
                titre: "Moby-Dick",
                date_emprunt: new Date("2025-03-04"),
                date_retour_prevue: new Date("2025-03-18")
            }
        }
    })
```
 

5. 
```db.utilisateurs.updateOne(
        {nom: "Nova"},
        { 
            $set: { 
                adresse: {
                    rue: "10 impasse des sources",
                    ville: "Saint martin en haut",
                    code_postal: "69850"
                }
            }
        }
        )
```

6. ``db.utilisateurs.updateOne({nom: "Mathieux"}, {$push: {tags: "Histoire"}})``

7. ``db.livres.updateOne({titre: "1985"}, {$set: {note_moyenne: 4.8}})``

### Partie 4 : Suppression de documents (Delete)

1. ``db.livres.deleteOne({titre: "1985"})``

2. ``db.livres.deleteMany({auteur: "George Orwell"})``

3. ``db.utilisateurs.deleteOne({email: "yohann.mathieux@gmail.com"})``

### Partie 5 : Requêtes avancées et projection

1. ``db.livres.find().sort({note_moyenne: -1})``

2. ``db.livres.find().sort({annee_publication: 1}).limit(3)``

3. ``db.livres.countDocuments({auteur: "George Orwell"})``

4. ``db.livres.find(
    {},
    { _id: 0, titre: 1, auteur: 1, note_moyenne: 1 }
)``

5. ``db.utilisateurs.find({ $expr: {$gte: [{ $size: "$livres_empruntes" }, 1] }})``

6. ``db.livres.find({
    titre: { $regex: "Prince" }
})``

7. ``db.livres.find({prix: {$gt: 10, $lt: 20 }})``

### Partie 6 : Modélisation de données (Mini-exercice)

#### 6.1 Modèle embarqué vs référence

1. 

2. 
````bash
db.emprunts.insertMany([
	{
	  utilisateur_id: ObjectId("67c6b6824fcb73f58f466188"),
	  livre_id: ObjectId("67c5d089b0e44d2765d0a17b"),
	  date_emprunt: new Date("2025-03-03"),
	  date_retour_prevue: new Date("2025-03-17"),
	  date_retour_effective: null,
	  statut: "en cours" // en cours, retourné, en retard
	},
	{
	  utilisateur_id: ObjectId("67c6b6824fcb73f58f466189"),
	  livre_id: ObjectId("67c5d089b0e44d2765d0a177"),
	  date_emprunt: new Date("2025-03-02"),
	  date_retour_prevue: new Date("2025-03-16"),
	  date_retour_effective: null,
	  statut: "en cours" // en cours, retourné, en retard
	},
	{
	  utilisateur_id: ObjectId("67c6b6824fcb73f58f46618a"),
	  livre_id: ObjectId("67c5d089b0e44d2765d0a178"),
	  date_emprunt: new Date("2025-03-03"),
	  date_retour_prevue: new Date("2025-03-17"),
	  date_retour_effective: null,
	  statut: "en cours" // en cours, retourné, en retard
	}
])
````

3. 
Si les emprunts sont directement fait dans le document utilisateur alors cela veut dire que c'est un pattern "un-à-plusieurs" (Embarqué), ce modèle représente des avantages :

Un seul accès pour les données : Toutes les informations sur l'utilisateur et ses emprunts sont stockées dans un seul document, ce qui permet de récupérer toutes les données en une seule requête.

Atomicité des mises à jour, ce qui garantit la cohérence des données.

Pas de jointure nécessaire, se qui améliore les performances de lecture

Mais il a des limites :

Taille max de document (16Mo, imposé par MongoDB).

dépassement possible, se qui vas engendrer des problèmes de performances et de gestion.

Difficulté de gestion des relations complexes.


Alors que actuellement on vas utiliser un pattern "Plusieurs-à-plusieurs" (Référence), car on a une collection dédié à l'emprunts de livres se qui présente des avantages :

Flexibilité et évolutivité, les données étant stockés dans une collection séparée permet de gérer un plus grand nombre d'emprunts sans taille max.

les données sont maintenues à jour à un seul endroit ce qui facilite la mise à jour et la gestion des données.

La gestion efficace des relations complexes.

Il y a aussi des inconvénients :

Les requêtes sont plus complexe, si on veut les informations d'un utilisateur il vas falloir utiliser des jointures.

#### 6.2 Réflexion sur la modélisation

2. Quelle approche privilégieriez-vous pour une application réelle et pourquoi ?

Dans une application réel un modèle référencé semble être plus adapté pour plusieurs raison :
La flexibilité et l'évolutivité des données, les emprunts étant stockés dans leur propre collection, on vas pouvoir les gérer sans se soucier de la taille maximal des documents.
La facilité à ajouter/supprimer/modifier les emprunts sans directement toucher aux utilisateurs.

Gestion efficace des relations complexes :
Les relations entre les utilisateurs, les emprunts et les livres peuvent être gérées de manière plus flexible et évolutive.
Les données sont maintenues à jour à un seul endroit, ce qui facilite la mise à jour et la gestion des données.

Performance et scalabilité :
Les collections séparées pour les utilisateurs et les emprunts permettent une meilleure distribution des données et une scalabilité horizontale plus facile.
Les requêtes peuvent être optimisées avec des index appropriés sur les collections référencées.

3. Comment modéliseriez-vous les cas où un même livre peut exister en plusieurs exemplaires ?

j'aurais séparée les informations stock contenue dans la collection livres dans une nouvelle collection stock

par exemple pour le petit prince :

```
db.livres.insertOne([
    {
        _id: ObjectId("67c5d089b0e44d2765d0a17b"),
        titre: "Le Petit Prince",
        auteur: "Antoine de Saint-Exupéry",
        annee_publication: 1943,
        editeur: "Gallimard",
        genre: ["Conte", "Philosophie"],
        nombre_pages: 96,
        langue: "Français",
        description: "Un pilote d'avion, qui s'est écrasé dans le désert du Sahara, rencontre un jeune prince venu d'une autre planète...",
        prix: 7.50,
        isbn: "9782070612758",
        date_ajout: new Date("2023-01-15")
    },  
])
```
```
db.stock.insertMany([
    {
        _id: ObjectId("67c6b6824fcb73f58f466188"),
        livre_id: ObjectId("67c5d089b0e44d2765d0a17b"),
        etat: "neuf",
        disponible: true,
        localisation: "Bibliothèque"
    },
    {
        _id: ObjectId("67c6b6824fcb73f58f466189"),
        livre_id: ObjectId("67c5d089b0e44d2765d0a17b"),
        etat: "bon",
        disponible: true,
        localisation: "Stock"
    }
])
```

```
db.emprunts.insertMany([
    {
        utilisateur_id: ObjectId("67c6b6824fcb73f58f466188"),
        exemplaire_id: ObjectId("67c6b6824fcb73f58f466188"),
        date_emprunt: new Date("2025-03-03"),
        date_retour_prevue: new Date("2025-03-17"),
        date_retour_effective: null,
        statut: "en cours" // en cours, retourné, en retard
    },
    {
        utilisateur_id: ObjectId("67c6b6824fcb73f58f466189"),
        exemplaire_id: ObjectId("67c6b6824fcb73f58f466189"),
        date_emprunt: new Date("2025-03-02"),
        date_retour_prevue: new Date("2025-03-16"),
        date_retour_effective: null,
        statut: "en cours" 
    }
])
```


## TP MongoDB - Indexation, Géospatial et Agrégation

### TP1 : Indexation et Optimisation des performances

#### Exercice 1.1 Préparation et analyse des performances sans index

1. Pour ajouter les 1000 documents dans ma collection livre j'ai utilisé ce script sur mangosh :

1000 documents ne me semble pas assais pour voir les réel performances sans index je vais donc passer à 10 000 et refaire les commandes.

Je vais essayer avec 100 000 pour avoir plusieurs stats et bien se rendre compte

```bash
const auteurs = ["Herman Melville", "Jane Austen", "George Orwell", "Mark Twain", "J.K. Rowling"];
const titres = ["Moby-Dick", "Pride and Prejudice", "1984", "Adventures of Huckleberry Finn", "Harry Potter"];
const editeurs = ["Harper & Brothers", "T. Egerton", "Secker & Warburg", "Chatto & Windus", "Bloomsbury"];
const genres = [["Roman", "Aventure"], ["Roman", "Romance"], ["Dystopie", "Science-fiction"], ["Roman", "Aventure"], ["Fantaisie"]];
const langues = ["Anglais", "Français", "Espagnol", "Allemand", "Italien"];

function getRandomElement(arr) {
    return arr[Math.floor(Math.random() * arr.length)];
}

function getRandomInt(min, max) {
    return Math.floor(Math.random() * (max - min + 1)) + min;
}

const documents = [];
for (let i = 0; i < 90000; i++) {
    const auteur = getRandomElement(auteurs);
    const titre = getRandomElement(titres);
    const editeur = getRandomElement(editeurs);
    const genre = getRandomElement(genres);
    const langue = getRandomElement(langues);
    const annee_publication = getRandomInt(1800, 2025);
    const nombre_pages = getRandomInt(100, 1000);
    const stock = getRandomInt(1, 50);
    const note_moyenne = parseFloat((Math.random() * 5).toFixed(1));
    const prix = parseFloat((Math.random() * 50).toFixed(2));
    const date_ajout = new Date();

    documents.push({
        titre: titre,
        auteur: auteur,
        annee_publication: annee_publication,
        editeur: editeur,
        genre: genre,
        nombre_pages: nombre_pages,
        langue: langue,
        disponible: true,
        stock: stock,
        note_moyenne: note_moyenne,
        description: `Description for ${titre}`,
        prix: prix,
        isbn: `978-${getRandomInt(1000000000, 9999999999)}`,
        date_ajout: date_ajout
    });
}

db.livres.insertMany(documents);
```

2. Analyse des performances des requêtes sans index :

pour un titre exact :

`db.livres.explain("executionStats").find({ titre: "Moby-Dick" })` 

| Stage       | nReturned | Execution Time (ms) | Total Keys Examined | Total Docs Examined |
|-------------|-----------|---------------------|----------------------|---------------------|
| COLLSCAN    | 193       | 0                   | 0                    | 1005                |
| COLLSCAN    | 2019      | 5                   | 0                    | 10000               |
| COLLSCAN    | 19992     | 55                  | 0                    | 100000              |


pour un auteur :

`db.livres.explain("executionStats").find({ auteur: "George Orwell" })`

| Stage       | nReturned | Execution Time (ms) | Total Keys Examined | Total Docs Examined |
|-------------|-----------|---------------------|----------------------|---------------------|
| COLLSCAN    | 203       | 0                   | 0                    | 1005                |
| COLLSCAN    | 1986      | 5                   | 0                    | 10000               |
| COLLSCAN    | 19823     | 58                  | 0                    | 100000              |


Pour une plage de prix (10€ et 20€) et note minimale

`db.livres.explain("executionStats").find({
    $and: [
        { prix: { $gt: 10, $lt: 20 } },
        { note_moyenne: { $gte: 4 } }
    ]
})`

| Stage       | nReturned | Execution Time (ms) | Total Keys Examined | Total Docs Examined |
|-------------|-----------|---------------------|----------------------|---------------------|
| COLLSCAN    | 52        | 0                   | 0                    | 1005                |
| COLLSCAN    | 405       | 16                  | 0                    | 10000               |
| COLLSCAN    | 4198      | 85                  | 0                    | 100000              |


Pour une recherche filtrée par genre et langue avec tri par note décroissante

``db.livres.explain("executionStats").find({genre: "Roman",langue: "Espagnol"}).sort({note_moyenne: -1})``

| Stage       | nReturned | Execution Time (ms) | Total Keys Examined | Total Docs Examined |
|-------------|-----------|---------------------|----------------------|---------------------|
| SORT / COLLSCAN| 125    | 1                   | 0                    | 1005                |
| SORT / COLLSCAN| 2019   | 10                  | 0                    | 10000               |
| SORT / COLLSCAN| 19992  | 98                  | 0                    | 100000              |


#### Exercice 1.2 Création d'index simple et composites

**Création d'un index simple (idx_titre):** 

```bash
db.livres.createIndex(
    { titre: 1 },
    { 
        background: true,
        sparse: false,
        name: "idx_titre"
    }
)
```

éxécution de la commande `db.livres.explain("executionStats").find({ titre: "Moby-Dick" })` pour avoir les stats

| Stage       | nReturned | Execution Time (ms) | Total Keys Examined | Total Docs Examined |
|-------------|-----------|---------------------|----------------------|---------------------|
| FETCH / IXSCAN  | 19992     | 29                  | 19992                | 19992               |


On aperçoit une net amélioration des performances d'éxécution : Avant index 55ms, Après index 29ms. Alors oui c'est pas beaucoup 55ms mais c'est pour un jeu de donner de 100K documents et un index simple, on voit déjà la différence entre les deux
On peut aussi voir que les stage utilisé ne sont pas les mêmes, on avait un COLLSCAN sans index et avec l'utilisation de l'index simple on utilise le FETCH et IXSCAN.

**Création d'un index simple (idx_auteur):** 

```bash
db.livres.createIndex(
    { auteur: 1 },
    { 
        background: true,
        sparse: false,
        name: "idx_auteur"
    }
)
```

éxécution de la commande `db.livres.explain("executionStats").find({ auteur: "George Orwell" })` pour avoir les stats

| Stage       | nReturned | Execution Time (ms) | Total Keys Examined | Total Docs Examined |
|-------------|-----------|---------------------|----------------------|---------------------|
| FETCH / IXSCAN  | 19823     | 30                  | 19923                | 19823               |

Même condition, c'est juste le critaire de recherche qui change, utilisation d'un index simple donc on voit aussi une amélioration et un changement d'utilisation de stage.



**Création d'un index composite (idx_prix_note_moyenne):** 

```bash
db.livres.createIndex(
    { prix: 1, note_moyenne: 1 },
    { 
        background: true,
        sparse: false,
        name: "idx_prix_note_moyenne"
    }
)
```

éxécution de la commande `db.livres.explain("executionStats").find({$and: [{ prix: { $gt: 10, $lt: 20 } },{ note_moyenne: { $gte: 4 } }]})` pour avoir les stats

| Stage       | nReturned | Execution Time (ms) | Total Keys Examined | Total Docs Examined |
|-------------|-----------|---------------------|----------------------|---------------------|
| FETCH / IXSCAN  | 4198     | 16                  | 5198                | 4198               |

On utilise un index composite et la on peut observé une différence qui continue d'acroitre : Avant index 85 ms, Après 16ms.
L'utilisation de l'index composite avec 100K documents obtient les mêmes résultat que sans index mais avec 10K documents.


**Création d'un index composite (idx_genre_langue_note_moyenne):** 

```bash
db.livres.createIndex(
    { genre: 1, langue: 1, note_moyenne: -1 },
    { 
        background: true,
        sparse: false,
        name: "idx_genre_langue_note_moyenne"
    }
)
```

éxécution de la commande `db.livres.explain("executionStats").find({genre: "Roman",langue: "Espagnol"}).sort({note_moyenne: -1})` pour avoir les stats

| Stage       | nReturned | Execution Time (ms) | Total Keys Examined | Total Docs Examined |
|-------------|-----------|---------------------|----------------------|---------------------|
| FETCH / IXSCAN  | 11979     | 48                  | 11979                | 11979               |

On utilise un index composite, on peut observé une différence : Avant index 98ms, Après 48ms.

