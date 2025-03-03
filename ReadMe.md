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


## LES TP :

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

```db.ecommerce_produits.find({"stock.qunatite": {$gt: 0 }})```

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
        { $set: { tags: "nouveau"}}
)
```
