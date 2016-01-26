# Programmer avec PHP

L'objectif de ce chapitre est de savoir utiliser les structures de bases de la programmation avec le langage PHP.

## Variables

### Définition d'une variable

Une variable joue en PHP le même rôle que dans tout autre langage : stocker une information. Une variable PHP est définie par un nom qui commence obligatoirement par le symbole `$`.

```
<?php $message = "Bonjour Monde !";
echo $message; ?>
```

On remarque au passage que la variable `$message` n'a pas un type explicite comme `string` ou `int`. PHP n'impose pas au programmeur de définir les types des variables. On parle de **typage dynamique**.

Après sa définition, une variable PHP est utilisable à n'importe quel endroit de la page, même dans un autre bloc de code.

```
<?php $message = "Bonjour Monde !"; ?>
<h1>Un titre</h1>
<h2>Un sous-titre</h2>
<p><?php echo $message; ?></p>
```

### Chaînes de caractères

Les chaînes de caractères PHP sont délimitées par des guillemets doubles `"..."` ou par des guillemets simples `'...'`. Il est possible de concaténer (assembler) plusieurs chaînes de caractères au moyen du symbole `.`.

```php
<?php echo "Bonjour" . " " . "Monde !"; ?>
<?php echo 'Bonjour' . ' ' . 'Monde !'; ?>
```

Chaque ligne ci-dessus affiche `Bonjour Monde !`.

La différence entre guillemets simples et doubles apparaît lorsqu'on inclut une variable dans une chaîne de caractères. 

```php
<?php $age = 39; ?>
<p><?php echo "Vous avez $age ans"; ?></p>
<p><?php echo 'Vous avez $age ans'; ?></p>
```

Le premier `echo` affiche : `Vous avez 39 ans`. Le second `echo` affiche : `Vous avez $age ans`.

Lorsqu'on utilise des guillemets doubles pour définir une chaîne de caractères, les variables sont inerprétées (remplacées par leur valeur). Ce n'est pas le cas avec des guillemets simples.

## Tests et conditions

### L'instruction `if`

Dans un programme, on souhaite fréquemment agir en fonction du résultat d'une condition. Les traitements à effectuer peuvent être différents selon que la condition soit vraie ou fausse. En PHP, une alternative s'exprime grâce à l'instruction `if` éventuellement associée à une instruction `else`,. Elle permet de conditionner des traitements en fonction de certains critères. On parle parfois de branchement logique. Pendant l'exécution, les instructions exécutées seront différentes selon la valeur de la condition. Un seul des deux blocs d'instructions sera pris en compte.

```php
<?php
if ($nb1 > $nb2) {
    // sera exécuté si $nb1 > $nb2
}
else {
    // sera exécuté si $nb1 <= $nb2
}
?>
```

Il est possible de placer une instruction `if` à l'intérieur d'un bloc d'une autre instruction `if`. C'est ce qu'on appelle **imbriquer des conditions**. Attention à toujours bien refléter l'imbrication des blocs en décalant les instructions associées dans le code source : c'est ce qu'on appelle **l'indentation**.

Si la seule opération réalisée dans un bloc `else` est un `if`, on peut également employer l'instruction `elseif`, plus concise.

```php
<?php
if ($nb1 > $nb2) {
    // sera exécuté si $nb1 > $nb2
}
elseif ($nb2 > $nb1) {
    // sera exécuté si $nb1 < $nb2
}
else {
    // sera exécuté si $nb1 = $nb2
}
?>
```

### La notion de condition

L'instruction `if` est associée à une condition. C'est une expression (une combinaison de variables, de valeurs et d'opérateurs) dont l'évaluation donne la valeur vraie (`true`) ou la valeur faux (`false`). On parle d'expression booléenne.

Toute expression renvoyant une valeur booléenne peut être utilisée comme condition avec un `if`. C'est le cas des expressions utilisant des opérateurs de comparaison, dont voici la liste.

| Opérateur | Signification|
|-----------|--------------|
| == | Egal à|
| != | Différent de|
| < | Inférieur strictement|
| <= | Inférieur ou égal|
| > | Supérieur strictement|
| >= | Supérieur ou égal|

**Note** : la plupart des langages de programmation utilisent le symbole `=` pour symboliser l'affectation, et le symbole `==` pour l'égalité. Attention aux confusions avec le sens mathématique de l'opérateur `=`.

### Les opérateurs logiques

On peut définir des conditions plus complexes ("La valeur de X est entre 100 et 200") grâce aux opérateurs logiques. Ceux du langage PHP sont les suivants : `&&` (Et), `||` (Ou), `!` (Non).

### L'instruction `switch`

L'instruction `switch` déclenche l'exécution d'un bloc d'instructions parmi plusieurs possibles. Seul le bloc correspondant à la valeur testée sera pris en compte.

```php
<?php
switch ($nombre) {
    case 1:
        ...
        break;     // break force la sortie du switch
    case 2:
        ...
        break;
    case 3:
        ...
        break;
    default:
        ...
        break;
}
?>
```

Il n'y a pas de limite au nombre de cas possibles. Le mot-clé `default`, à placer en fin de switch, est optionnel. Il sert souvent à gérer les cas d'erreurs.

Les instructions `break;` dans les blocs `case` sont indispensables pour éviter de passer d'un bloc à un autre.

## Tableaux

### Tableaux simples

Un tableau permet de regrouper plusieurs données. On obtient la taille (nombre d'éléments) d'un tableau grâce à l'instruction PHP `count`.

On accède aux éléments d'un tableau en utilisant leur indice, un entier qui commence à 0 et non à 1. 

```php
<?php
$langages = array('C#', 'PHP', 'HTML');
$nbLangages = count($langages);
echo "Je connais $nbLangages langages de programmation";
?>
<ul>
     <li><?php echo $langages[0]; ?></li>
     <li><?php echo $langages[1]; ?></li>
     <li><?php echo $langages[2]; ?></li>
</ul>
```

L'instruction PHP `print_r` permet d'afficher l'ensemble du contenu du tableau. Elle est utile pour déboguer un script.

```php
<p>Affichage direct du tableau :
<?php print_r($langages); ?></p>
```

### Tableaux associatifs

Un tableau associatif, parfois appelé dictionnaire, joue le même rôle qu'un tableau, mais on accède à ses éléments grâce à une chaîne de caractères (appelée clé) et non un entier. 

```php
<?php
$client = array(
    "Nom" => "Annie ZETTE",
    "Ville" => "Lyon",
    "Courriel" => "annie@zette.fr"
);
$nbChamps = count($client);
echo "Le tableau client contient $nbChamps champs";
?>
<ul>
    <li><?php echo $client["Nom"]; ?></li>
    <li><?php echo $client["Ville"]; ?></li>
    <li><?php echo $client["Courriel"]; ?></li>
</ul>
```

## Boucles

### Définition

Une structure répétitive, également appelée structure itérative ou encore boucle, permet de répéter plusieurs fois l'exécution d'une ou plusieurs instructions. Le nombre de répétitions peut : 

* être connu à l'avance.
* dépendre de l'évaluation d'une condition. 

A chaque répétition, les instructions contenues dans la boucle sont exécutées. C'est ce qu'on appelle un tour de boucle ou encore une itération.

### La boucle `while`

La boucle `while` permet de répéter des instructions tant qu'une condition est vérifiée.

```php
<?php
$i = 0;
while ($i < 10) {
    ...
    $i++;
}
?>
```

Avant chaque tour de boucle, la condition associée au `while` est évaluée :
Si elle est vraie, les instructions du bloc `while` sont exécutées. Ensuite, la ligne du while est à nouveau exécutée et la condition vérifiée.
Si elle est fausse, les instructions du bloc ne sont pas exécutées et le programme continue juste après le bloc `while`.

**Attention** : il faut absolument que la condition de la boucle `while` puisse devenir fausse. Dans le cas contraire, on obtient une boucle infinie qui ne s'arrête jamais.

### La boucle `for`

La boucle `for` permet de répéter un bloc d'instructions un nombre défini de fois.

```php
<?php
for ($i = 0; $i < 10; $i++) {
    ...
}
?>
```

Voici son fonctionnement :

* L'initialisation se produit une seule fois, au début de l'exécution.
* La condition est évaluée avant chaque tour de boucle. Si elle est vraie, un nouveau tour de boucle est effectué. Sinon, la boucle est terminée.
* L'étape est réalisée après chaque tour de boucle. 

La variable utilisée dans l'initialisation, la condition et l'étape est appelée le compteur de la boucle. Par convention, elle est souvent nommée `i`.

### La boucle `foreach`

La boucle `foreach` est principalement utilisée pour parcourir un tableau (associatif ou non).

```php
<?php
$langages = array('C#', 'PHP', 'HTML');
foreach($langages as $langage) {
    echo "$langage<br>";
}
?>
```

**Attention** : ne pas confondre la variable qui représente le tableau (ici `$langages`, écrite au pluriel) et la variable qui représente l'élément courant dans la boucle (ici `$langage`).

Le parcours d'un tableau associatif avec une boucle `foreach` permet d'obtenir la liste des clés et des valeurs associées.

```php
<?php
$client = array(
    "Nom" => "Annie ZETTE",
    "Ville" => "Lyon",
    "Courriel" => "annie@zette.fr"
);
?>
<ul>
<?php foreach($client as $cle => $valeur) { ?>
    <li><?php echo $cle . ' : ' . $valeur ?></li>
<?php } ?>
</ul>
```

## Fonctions

### Utilisation de fonctions prédéfinies

Les fonctions permettent de centraliser une portion de code utilisée régulièrement. 

Le langage PHP dispose d'une large biblothèque de fonctions prédéfinies. En voici quelques exemples :

* `count` permet de renvoyer la taille d'un tableau.
* `date` renvoie une date/heure qu'on peut ensuite formater pour affichage.
* `isset` vérifie si une variable existe ou non.
* `strlen` renvoie le nombre de caractères d'une chaîne.

```php
<?php
$langages = array('C#', 'PHP', 'HTML');
$nbLangages = count($langages);
if (isset($a) == false) {
    ...
}
$date = date("d-m-Y H:i:s");
$msg = "Hello there";
$longueurMsg = strlen($msg);
?>
```

### Ecriture de fonctions

Il est possible et souvent utile de définir ses propres fonctions PHP. Cela permet de décomposer un problème à résoudre en sous-parties plus simples, parfois réutilisables.

Lorsque qu'on a besoin d'utiliser une fonction, on effectue un appel à celle-ci. Cet appel provoque un "branchement" vers la fonction, qui s'exécute. Une fois l'exécution de la fonction terminée, le contrôle revient au niveau de l'appelant et l'exécution se poursuit. 

Les paramètres utilisés dans la définition du sous-programme sont appelés paramètres formels. Au moment de l'exécution, leur valeur devient celle des variables utilisés pour l'appel, appelées paramètres effectifs ou encore arguments. 

```php
<?php
// $a et $b sont les paramètres de la fonction
function multiplier($a, $b) { 
    return $a * $b;
}

$nb1 = 3;
$nb2 = 7.5;
// $nb1 et $nb2 sont les arguments de l'appel de la fonction
$resultat = multiplier($nb1, $nb2);
// La variable $resultat vaut maintenant 22.5
?>
```

TODO Ajouter portée variables "globales" dans fonctions

Le plus souvent, on externalise la définition des fonctions dans un fichier dédié qui est inclus dans la page utilisant la fonction.

```php
<?php include('fonctions.php'); ?>
```
