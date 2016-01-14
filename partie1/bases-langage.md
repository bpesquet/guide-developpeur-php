# Les bases du langage

L'objectif de ce chapitre est de découvrir les bases du langage PHP.

## Anatomie d'un fichier source PHP

Le code PHP est écrit dans des fichiers source portant l'extension `.php`. Le plus souvent, un fichier source PHP contient un mélange de balises HTML et de code PHP. Au moment où un client demande ce fichier à un serveur Web, le code PHP est exécuté par le serveur pour produire dynamiquement une page Web.

![](images/bases-langage/web_php_htmlcss.png)

**Attention** : un fichier contenant du code PHP mais portant l'extension `.html` sera renvoyé directement par le serveur sans exécution du code PHP qu'il contient.

## Définition d'un bloc de code PHP

Dans un fichier source PHP, on définit une portion de code PHP gràce aux balises `<?php` et `?>`. Il est possible de définir plusieurs blocs de code dans un même fichier source PHP. A l'intérieur d'un bloc de code, on peut utiliser les fonctionnalités du langage. Chaque instruction doit se terminer par le symbole `;`.

```php
<!doctype html>
<html>
    <head>
        <meta charset="utf-8" />
        <title>Ma première page PHP</title>
    </head>
    <body>
        <h1>Affichons du texte avec PHP...</h1>
        <h3>Ce titre est écrit directement en HTML</h3>
        <h3>Celui-ci contient une partie <?php echo "générée avec PHP"; ?></h3>
        <?php echo "<h3>Celui-là est entièrement généré avec PHP</h3>"; ?>
    </body>
</html>
```

## Affichage de texte

L'affichage de texte s'effectue grâce à la commande `echo`.

```php
<?php echo "Bonjour Monde !"; ?>
```

On peut inclure des balises HTML dans le texte affiché par `echo`, ou bien inclure l'appel à `echo` dans des balises HTML. Ainsi, les deux appels suivants produisent exactement le même résultat.

```php
<?php echo "<p>Bonjour Monde !</p>"; ?>
<p><?php echo "Bonjour Monde !"; ?></p>
```

**Conseil** : sauf cas particulier, on utilisera plutôt la seconde technique, qui préserve la structure HTML de la page. 

TODO Ajout <?= ... ?>

## Commentaires

A l'intérieur d'un bloc de code PHP, on peut ajouter des commentaires avec les symboles communs à de nombreux langages de programmation : `//` pour un commentaire sur une seule ligne et `/* ... */` pour un commentaire sur plusieurs lignes. 

A l'extérieur d'un bloc PHP, on utilise la syntaxe HTML `<!-- ... -->` pour ajouter des commentaires.

```php
<h1>Affichons du texte avec PHP...</h1>  <!-- un commentaire HTML -->
...
<?php echo "<h3>Celui-là est entièrement généré avec PHP</h3>"; 
// un commentaire PHP ?>
```

## Inclure des portions de page

Un fichier PHP peut inclure le contenu d'un autre fichier grâce à l'instruction `include`.

```php
<?php include("monfichier.php"); ?>
```

Au moment de l'exécution, cette instruction sera remplacée par le contenu du fichier inclus. 

Cette technique permet de centraliser le code des éléments communs à plusieurs fichiers PHP, comme par exemple des menus ou des pieds de page, pour éviter la duplication de code.

```php
<!doctype html>
<html>
    <head>
        <meta charset="utf-8" />
        <title>Une page PHP modulaire</title>
    </head>
    <body>
        <?php include("header.php"); ?>
        <?php include("menu.php"); ?>
        
        <!-- ... (contenu spécifique) -->
        
        <?php include("footer.php"); ?>
    </body>
</html>
```

TODO Ajout require et *_once
