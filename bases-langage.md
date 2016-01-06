# Les bases du langage

## Fichiers source PHP

Le code PHP est écrit dans des fichiers source portant l'extension `.php`. Le plus souvent, un fichier source PHP contient un mélange de balises HTML et de code PHP. Au moment où un client demande ce fichier à un serveur Web, le code PHP est exécuté par le serveur pour produire une page Web dynamique.

![](images/intro-dev-web/web_php_htmlcss.png)

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

## Commentaires

A l'intérieur d'un bloc de code PHP, on peut ajouter des commentaires avec les symboles communs à de nombreux langages de programmation : `//` pour un commentaire sur une seule ligne et `/* ... */` pour un commentaire sur plusieurs lignes. 

A l'extérieur d'un bloc PHP, on utilise la syntaxe HTML `<!-- ... -->` pour ajouter des commentaires.

```php
<h1>Affichons du texte avec PHP...</h1>  <!-- un commentaire HTML -->
...
<?php echo "<h3>Celui-là est entièrement généré avec PHP</h3>"; 
// un commentaire PHP?>
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

## Chaînes de caractères

Les chaînes de caractères PHP sont délimitées par des guillemets doubles `"..."` ou par des guillemets simples `'...'`. La différence entre les deux syntaxes apparaît lorsque la chaîne contient des variables (voir plus loin).

Il est possible de concaténer (assembler) plusieurs chaînes de caractères au moyen du symbole `.`.

```php
<?php echo "Bonjour" . " " . "Monde !"; ?>
<?php echo 'Bonjour' . ' ' . 'Monde !'; ?>
```

Chaque ligne ci-dessus affiche le texte `Bonjour Monde !`.

## Variables

Une variable joue en PHP le même rôle que dans tout autre langage : stocker une information. Une variable PHP est définie par un nom qui commence obligatoirement par le symbole `$`.

```php
<?php $message = "Bonjour Monde !";
echo $message; ?>
```

On remarque au passage que la variable `$message` n'a pas de type explicite comme `string` ou `int`. PHP n'impose pas au programmeur de définir les types des variables. On parle de **typage dynamique**.

Après sa définition, une variable PHP est utilisable à n'importe quel endroit de la page, même dans un autre bloc de code.

```php
<?php $message = "Bonjour Monde !"; ?>
<h1>Un titre</h1>
<h2>Un sous-titre</h2>
<p><?php echo $message; ?></p>
```

La différence entre guillemets simples et doubles apparaît lorsqu'on inclut une variable dans une chaîne de caractères. 

```php
<?php $age = "39"; ?>
<p><?php echo "Vous avez $age ans"; ?></p>
<p><?php echo 'Vous avez $age ans'; ?></p>
```

Le premier `echo` affiche : `Vous avez 39 ans`.
Le second `echo` affiche : `Vous avez $age ans`.

Lorsqu'on utilise des guillemets doubles pour définir une chaîne de caractères, les variables sont interprétées (remplacées par leur valeur). Ce n'est pas le cas avec des guillemets simples.
