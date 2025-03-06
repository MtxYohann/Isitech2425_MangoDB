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
