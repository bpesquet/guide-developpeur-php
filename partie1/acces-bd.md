# Accéder à une base de données

## Introduction

Vous connaissez à présent les bases du langage PHP et vous savez transmettre des informations d'une page à une autre. Cependant, il nous reste à étudier le moyen de stocker des données de manière persistante. Ainsi, nous pourrons mémoriser les informations saisies par les utilisateurs, ou bien construire des pages dynamiques à partir des données externes.

**Définition** : une donnée est dite **persistante** lorsqu'elle survit à l'arrêt du logiciel ou de la machine qui la manipule. Le contraire de "persistante" est "volatile".

Actuellement, la technique la plus utilisée pour rendre des données persistantes consiste à les sauvegarder dans un logiciel dédié appelé SGBDR ou Système de Gestion de Bases de Données Relationelles. Parmi les SGBDR les plus connus, citons MySQL, PostgreSQL ou encore ORACLE.

Nous allons donc étudier comment interagir avec un SGBDR depuis une page PHP. Pour cela, nous allons utiliser une extension récente du langage PHP appelée PDO (*Php Data Objects*). Par rapport aux autres solutions existantes, PDO possède le double avantage d'être orientée objet et d'être indépendante du SGBDR utilisé.

Le schéma ci-dessous décrit l'architecture que nous allons mettre en oeuvre. Il s'agit d'un exemple d'**architecture trois tiers** (client, serveur Web, SGBD). Le rôle de PDO va être de faire le lien entre les pages PHP du serveur et les données stockées dans le SGBDR.

![](images/acces-bd/archi-pdo.png)

## Connexion à la base de données

La connexion à une base de données depuis un fichier source PHP est réalisée de manière orientée objet, en instanciant un objet de la classe `PDO` par le biais de son constructeur.  

```php
$bdd = new PDO("mysql:host=localhost;dbname=mabase;charset=utf8", 
    "mabase_util", "mabase_mdp", array(PDO::ATTR_ERRMODE => PDO::ERRMODE_EXCEPTION));
```

Le constructeur utilisé ici comporte quatre paramètres :

* Le premier paramètre (`mysql:...`) définit le nom de la source de données ou DSN, *Data Source Name*. Ce nom contient le nom et le type du SGBD (ici MySQL sur la machine locale, `localhost`), le nom de la base de données (ici `mabase`) et le jeu de caractères utilisé (ici `utf8`). 
* Le deuxième paramètre (`mabase_util`) est le login utilisé pour se connecter à la BD. Il doit auparavant avoir été créé au niveau du SGBDR.
* Le troisième paramètre (`mabase_mdp`) est le mot de passe associé au login.
* Le quatrième paramètre (`array(PDO::...)`) est relatif à la gestion des erreurs.

**Note** : il est déconseillé d'utiliser le login `root` ayant tous les droits pour se connecter à une base de données depuis du code PHP. Il vaut mieux créer dans MySQL un utilisateur dédié n'ayant des droits que sur cette base.

On peut traiter immédiatement les erreurs (base de données introuvable, mauvais login ou mot de passe, etc) en intégrant la connexion dans un bloc `try/catch`. Il s'agit d'un mécanisme de gestion des erreurs utilisant les **exceptions**. Sans rentrer dans des détails inutiles, son fonctionnement est le suivant :

1. Les instructions situées dans le bloc `try` sont exécutées.
2. Si l'une des instructions du bloc `try` provoque une erreur, elle est interceptée par le bloc `catch`, dont les instructions sont alors exécutées.
3. Si aucune instruction du bloc `try` n'échoue, le contenu du bloc `catch` est ignoré et l'exécution se poursuit.

~~~php
try {
    $bdd = new PDO("mysql:host=localhost;dbname=mabase;charset=utf8", "mabase_util", 
        "mabase_mdp", array(PDO::ATTR_ERRMODE => PDO::ERRMODE_EXCEPTION));   
}
catch (Exception $e) {
    die('Erreur fatale : ' . $e->getMessage());
}
// ...
~~~

Si un problème se produit pendant la connexion, l'exécution PHP est interrompue (fonction `die`) et un message d'erreur est affiché. Sinon, le reste de la page est exécuté normalement.

La classe PDO dispose d'une [documentation en ligne](http://www.php.net/manual/fr/class.pdo.php).

## Exécution de requêtes SQL

Une fois la connexion établie, il est possible d'utiliser l'objet instancié pour réaliser des opérations d'accès à la base. Pour cela, il faut écrire et exécuter des requêtes en utilisant le langage SQL.

### Requêtes simples

La méthode `query` de la classe `PDO` permet d'exécuter une requête SQL simple (sans paramètres).

```php
// ...
$requete = "select * from employe";
$resultat = $bdd->query($requete);
```

Le symbole `->` est l'équivalent PHP du symbole `.` utilisé dans d'autres langages comme Java ou C#. L'instruction `$bdd->query` se lit : "J'appelle la méthode `query` sur mon objet `$bdd`".

Après l'appel à `query`, on peut parcourir le résultat ligne par ligne en appelant à la méthode `fetch` sur le résultat de la requête.

* Dans le cas d'une requête qui renvoie une seule ligne de résultat, un seul appel à `fetch` suffit. C'est notamment le cas lorsqu'on recherche un enregistrement à partir de sa clé primaire.

~~~php
// ...
$requete = "select * from employe where id=1;";
$resultat = $bdd->query($requete);

$ligne = $resultat->fetch();
// On accède à la valeur de macolonne avec $ligne['macolonne'];
~~~

* Si la requête renvoie plusieurs lignes de résultats, on peut itérer sur ces lignes avec une boucle `while` ou une boucle `foreach`.

```php
// ...
$requete = "select * from employe";
$resultat = $bdd->query($requete);

// Itération sur les résultats de la requête SQL
while ($ligne = $resultat->fetch()) {
    // On accède à la valeur de macolonne avec $ligne['macolonne'];
    // ...
}
?>
```

```php
// ...
$requete = "select * from employe";
$resultat = $bdd->query($requete);

// Récupération de tous les résultats de la requête dans un tableau
$donnees = $resultat->fetchAll();
// Itération sur le contenu du tableau
foreach ($donnees as $ligne) {
    // On accède à la valeur de macolonne avec $ligne['macolonne'];
    // ...
}
?>
```

Dans tous les cas, la variable `$ligne` s'utilise comme un tableau associatif. Elle rassemble les valeurs des différentes colonnes pour une ligne de résultat SQL.

### Requêtes avec paramètres

Lorsque la requête SQL à exécuter comporte des paramètres, il faut utiliser une technique différente.

```php
// ...

// Préparation de la requête : le ? correspond au paramètre attendu
$req = $bdd->prepare("select * from employe where servempl=?");

// On reçoit le code depuis un formulaire
$codeService = $_POST['service'];

// Exécution de la requête en lui passant le tableau des arguments
// (ici un seul élément : le code du service)
$req->execute(array($codeService));
```

Ce code source utilise ce qu'on appelle une **requête préparée**. Il s'agit d'une technique dans laquelle on définit d'abord le squelette de la requête (appel de la méthode `prepare` sur l'objet `$bdd`) en prévoyant ses différents paramètres, indiqués par des `?` dans le code SQL. Ensuite, on exécute la requête préparée (méthode `execute` sur l'objet `$req`). Lors de cet appel, on passe les paramètres nécessaires sous la forme d'un tableau.

**Avertissement** : le tableau des paramètres doit contenir autant d'élément qu'il y a de `?` dans la requête préparée. L'ordre doit également être respecté.

Outre le gain de temps lorsqu'une même requête est exécutée plusieurs fois avec des paramètres différents, l'utilisation d'une requête préparée évite de construire une requête SQL en y intégrant directement des données utilisateur. 

~~~php
// DANGEREUX : A EVITER ABSOLUMENT
$requete = "select * from employe where servempl=" . $_POST['service'];
~~~

La technique ci-dessus rend la base vulnérable aux attaques de type "injection SQL". L'injection SQL consiste à faire exécuter des requêtes SQL imprévues par le SGBDR, ce qui peut conduire à de graves problèmes de sécurité.

![](images/acces-bd/sql_injection.png)

Ce risque de sécurité n'existe pas lorsqu'on utilise des requêtes préparées, 

**Note** : ici, pas besoin de "nettoyer" la variable `$_POST['service']` reçue du formulaire comme nous l'avions fait dans le chapitre précédent. L'appel à `htmlspecialchars` désactive l'exécution de code JavaScript mais ne présente aucun intérêt dans le cas de données utilisées dans des requêtes SQL.
