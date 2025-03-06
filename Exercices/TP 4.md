
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

#### Exercice 1.3 Index spécialisés

**Création d'un index spécialisé sur les champs titre et description**

```bash
db.livres.createIndex(
    { titre: "text", description: "text" },
    { 
        background: true,
        name: "idx_texte_titre_description"
    }
)
```

Test avec la commande:

`db.livres.explain("executionStats").find({ $text: { $search: "Moby" } })`

Analyse des performances

| Stage       | nReturned | Execution Time (ms) | Total Keys Examined | Total Docs Examined |
|-------------|-----------|---------------------|----------------------|---------------------|
| TEXT_MATCH / FETCH / IXSCAN  | 19992     | 34                  | 19992                | 19992               |


**Création de la nouvelle collection sessions_utilisateurs et insert des documents :**

```bash
const utilisateurs = [
    ObjectId("67c6b6824fcb73f58f466188"),
    ObjectId("67c6b6824fcb73f58f466189"),
    ObjectId("67c6b6824fcb73f58f46618a"),
];

function getRandomElement(arr) {
    return arr[Math.floor(Math.random() * arr.length)];
}

function getRandomDate(start, end) {
    return new Date(start.getTime() + Math.random() * (end.getTime() - start.getTime()));
}

const sessions = [];
for (let i = 0; i < 200; i++) {
    const utilisateur_id = getRandomElement(utilisateurs);
    const date_derniere_activite = getRandomDate(new Date(2025, 0, 1), new Date(2025, 11, 31));
    const donnees_session = {
        ip: `192.168.1.${Math.floor(Math.random() * 255)}`,
        navigateur: getRandomElement(["Chrome", "Firefox", "Safari", "Edge"]),
        os: getRandomElement(["Windows", "macOS", "Linux", "Android", "iOS"]),
        duree: Math.floor(Math.random() * 3600) 
    };

    sessions.push({
        utilisateur_id: utilisateur_id,
        date_derniere_activite: date_derniere_activite,
        donnees_session: donnees_session
    });
}

db.sessions_utilisateurs.insertMany(sessions);
```

**Création d'un index TTL pour supprimer automatiquement les sessions après 30min d'inactivité.**

````
db.sessions_utilisateurs.createIndex(
    { date_derniere_activite: 1 },
    { expireAfterSeconds: 1800 }
)
````

#### Exercice 1.4 Optimisation avancées

**Création de l'index couvrants**

```bash
db.livres.createIndex(
    { auteur: 1, annee_publication: 1, titre: 1, note_moyenne: 1 },
    { 
        background: true,
        name: "idx_couvrant_auteur_annee_titre_note"
    }
)
```

On vas vérifier que l'index couvrants est bien utilisé :

```bash
db.livres.find(
    { auteur: "George Orwell", annee_publication: { $gte: 1900 } },
    { _id: 0, titre: 1, note_moyenne: 1 }
).explain("executionStats")
```

Statistique :

| Stage       | nReturned | Execution Time (ms) | Total Keys Examined | Total Docs Examined |
|-------------|-----------|---------------------|----------------------|---------------------|
| PROJECTION_COVERED / IXSCAN / PROJECTION_SIMPLE / FETCH / IXSCAN | 11162     | 14                  | 11162                | 0               |

On voit que l'index couvrants à bien été utilisé grâce au Stage utilisé et le nombre de documents examiné est à 0.


**Création de l'index unique pour le champ ISBN**

```bash
db.livres.createIndex(
  { isbn: 1 },
  { 
    background: true,
    unique: true,
    sparse: false,
    name: "idx_isbn"
  }
)
```
![liste index](/img/isbn.jpg)

On voit bien que l'index est actif je vais donc essayer d'insérer un livre avec un isbn déjà éxistant :

```bash
db.livres.insertOne(
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
        isbn: "978-5247575858",
        date_ajout: new Date("2023-03-05")
    }
)
```

![erreur isbn](/img/erreur_isbn.jpg)

#### Livrable TP 1

1. j'ai déjà répondu tout au long du TP

2. 

Quelles améliorations de performance avez-vous observées après l'ajout d'index ?

Après l'ajout des index simple, composite et couvrants on peut observé des améliorations dans la rapidité d'éxécution des requêtes alors que nous n'avons pas créer des index très complexe(sur 1 ou 2 champs).

Quels types d'index ont été les plus efficaces pour vos requêtes ?

On peut observé que plus les index était complexes plus il y avait un gain de temps.
Si je prend l'exemple de mon index `idx_genre_langue_note_moyenne` on voit que la requête prend 2 fois moins de temps que sans index.

Avez-vous identifié des compromis entre performance de lecture et d'écriture ?

L'utisation d'index permet effectivement de gagner des performance sur la lecture, mais lors de l'écriture ils peuvent créer des latence car ils sont mise à jour à chaque écriture (ajout/modif/suppression).

Comment choisiriez-vous les index pour une application de bibliothèque en production ?
**Répondre à la fin**

### TP2 Requêtes Géospatial

#### Exercice 2.1 Enrichissement des données


**Mise à jour de la collection utilisateurs**

![mise à jour collection utilisateur](/img/collection_utilisateurs.jpg)

**Création de la collection bibliothèques**

![Création collection bibliothèque](/img/collection_biblio.jpg)


**Création des index géospatiaux**

```bash
db.utilisateurs.createIndex(
    { "adresse.localisation": "2dsphere" },
    { name: "idx_localisation_utilisateurs" }
)
```

```bash
db.bibliotheques.createIndex(
    { "adresse.localisation": "2dsphere" },
    { name: "idx_localisation_bibliotheques" }
)

```

```bash
db.bibliotheques.createIndex(
    { "zone de service": "2dsphere" },
    { name: "idx_zone_service_bibliotheques" }
)
```

#### Exercice 2.2 Requêtes géospatiales avancées

1. Trouver les 5 utilisateurs les plus proche du point dans une limite de 5km.

```bash
db.utilisateurs.find({
  "adresse.localisation": {
    $near: {
      $geometry: {
        type: "Point",
        coordinates: [45.676021, 4.761109] 
      },
      $maxDistance: 5000
    }
  }
}).limit(5)
```

2. Trouvez les bibliothèques les plus proches d'un utilisateur spécifique.

utilisation d'une agrégation avec : 

$match pour trouver l'utilisateur

$lookup pour faire la jointure avec la collection bibliotheques, cotient notre jointure et une variable pour les coordonnées de l'utilisateur.
Ensuite notre pipeline ou l'on vas utiliser $geoNear pour trouver les bibliotheques les plus proches de notre utilisateurs, le champ key est obligatoir car j'ai plusieur index géospatial de type 2dsphere.

j'impose ensuite une limite de 3 pour limiter le nombre de bibliotheques en sortie.

et pour finir le $project pour choisir les données en sortie.

```bash
db.utilisateurs.aggregate([
    {
        $match: {
            nom: "Mathieux"
        }
    },
    {
        $lookup: {
            from: "bibliotheques",
            let: {userLocation: "$adresse.localisation"},
            pipeline: [
                {
                    $geoNear: {
                        near: "$$userLocation",
                        distanceField: "distance",
                        spherical: true,
                        maxDistance: 10000,
                        key: "adresse.localisation"
                    }
                },
                {$limit: 3}
            ],
            as: "bibliotheques_proches"
        }

    },
    {
        $project: {
            _id: 0,
            nom: 1,
            bibliotheques_proches: 1
        }
    }
])
```

3.  Utilisez l'opérateur $geoNear dans un pipeline d'agrégation pour obtenir les bibliothèques triées par distance et calculer précisément cette distance (en km).


```bash
db.bibliotheques.aggregate([
  {
    $geoNear: {
      near: { type: "Point", coordinates: [4.685405, 45.641307] }, // Pas de point précis dans la consigne on peut mettre celui que l'on veut
      distanceField: "distance",
      spherical: true, 
      distanceMultiplier: 0.001 
      key: "adresse.localisation" 
    }
  },
  {
    $project: {
      _id: 0,
      nom: 1,
      adresse: 1,
      distance: 1 
    }
  },
  {
    $sort: { distance: 1 } 
  }
])
```

#### Exercice 2.3 Requêtes géospatial avancées

1. Utilisez $geoWithin pour trouver tous les utilisateurs à l'intérieur d'une zone définie par un polygone

Utilisation de keplet pour avoir es coordonnées du polygon beaucoup plus facilement
(il faut que l'utilisateur sois dans le polygon sinon pas de résultat)


```bash
db.utilisateurs.find({
  "adresse.localisation": {
    $geoWithin: {
        $geometry: {
            type: "Polygon",
            coordinates:[
                [
                    [4.64865625499346,45.596342557268066],[4.863261865698382,45.69821440153976],[4.683616201253452,45.76465773710486],[4.588082090681525,45.67838690792586],[4.64865625499346,45.596342557268066]
                ]
            ]
        }
    }
  }
})
```

2.  Trouvez tous les utilisateurs qui se trouvent dans la zone de service d'une bibliothèque spécifique.

On vient en premier récupérer la zone de service que l'on souhaite, et ensuite on peut utiliser le $geoWithin


```bash
const zoneDeService = db.bibliotheques.findOne(
  { nom: "Eclats de lire" }, 
  { "zone de service": 1, _id: 0 }
);

db.utilisateurs.find({
  "adresse.localisation": {
    $geoWithin: {
      $geometry: zoneDeService["zone de service"]
    }
  }
})
```

3.  Créez une collection rues avec au moins une rue représentée comme un LineString GeoJSON, puis utilisez $geoIntersects pour trouver les bibliothèques dont la zone de service intersecte cette rue.

**Création de la collection rues**

![collection rues](/img/collection_rues.jpg)


```bash
const rue = db.rues.findOne({ nom: "Route de soucieu" }, { localisation: 1, _id: 0 });

db.bibliotheques.find({
  "zone de service": {
    $geoIntersects: {
      $geometry: rue.localisation
    }
  }
})
```

#### Exercice 2.4 Cas d'utilisation métier

1. Créez une collection livraisons pour suivre les livraisons de livres.

![Collection livraisons](/img/collection_livraison.jpg)

Création des index géospatiales :

```
db.livraisons.createIndex({ "point_depart": "2dsphere" });
db.livraisons.createIndex({ "point_arrivee": "2dsphere" });
db.livraisons.createIndex({ "position_actuelle": "2dsphere" });
db.livraisons.createIndex({ "itineraire_planifie": "2dsphere" });
```

2. Implémentez une fonction pour mettre à jour la position d'une livraison.

```bash
function mettreAJourPositionLivraison(livraisonId, nouvellesCoordonnees) {
  db.livraisons.updateOne(
    { _id: ObjectId(livraisonId) }, 
    {
      $set: {
        "position_actuelle": {
          type: "Point",
          coordinates: nouvellesCoordonnees 
        }
      }
    }
  );
}

mettreAJourPositionLivraison("67c99b4c410ebd38f474933e", [4.715678, 45.692345]);
```

3. Créez une requête pour trouver toutes les livraisons en cours dans un rayon de 1km autour d'un point donné.

```bash
const pointDonne = [4.700763, 45.679650];
const rayonEnKm = 10; 

db.livraisons.find({
  "position_actuelle": {
    $geoWithin: {
      $centerSphere: [
        pointDonne, 
        rayonEnKm / 6378.1
      ]
    }
  }
})
```