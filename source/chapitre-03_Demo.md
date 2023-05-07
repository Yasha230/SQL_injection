# Exemples

Prennons l'exmple d'une application de vente, qui présente des produits appartenant à différentes catégories. Lorsqu'un utilisateur clique sur la catégorie 'gifts', le navigateur requiert l'URL suivant:

```{code-block}
https://insecure-website.com/products?catergory=Gifts
```

Cela implique que l'application doit effectuer une requête SQL pour extraire la liste des produits concernés de la base de donnée.

```{code-block} SQL
SELECT * FROM products
WHERE category = ‘Gifts’
AND released = 1
```

Cette requête demande à la base de donnée de retourner **tous** les details, de la table **products** dans la catégorie **gifts**.

La dernière ligne de la requête est une restriction qui fait réference au statut de publication des artciles; où  

```{code-block}
released = 1
```
indique que l'article est publié et donc visible aux clients. 

Si au contraire,

```{code-block}
released = 0
```
l'article n'est pas visible et l'on ne peut pas interagir avec.

L'application ne présente aucune défense contre des attaques SQL, ce qui signifie qu'il est vulnérable à des attaques telles que celle-ci

```{code-block}
https://insecure-website.com/products?catergory='Gifts'--
```
Cette petite modification de l'URL entraîne aussi une modification de la requête SQL.

```{code-block} SQL
SELECT * FROM products
WHERE category = ‘Gifts’--'
AND released = 1
```
Le double tiret "- -" indique que le reste de la requête n'est qu'un commentaire et n'est donc pas pris en compte. Cela supprime en effet la restriction qui bloquait au départ l'accès aux éléments non publiés.


Si un hacker veut aller plus loin, il peut avoir accès à plus que des produits inédits ; il peut faire en sorte que l'application affiche tous les produits de n'importe quelle catégorie, même les catégories qu'il ne connaît pas. Pour ce faire, l'attaquant utilise l'URL suivante: 

```{code-block}
https://insecure-website.com/products?catergory='Gifts'+OR+1=1--
```
Il s'agit d'une méthode de type UNION dans laquelle l'attaquant ajoute une ligne de code à la requête pour accéder aux informations souhaitées de la base de données, résultant dans la requête suivante:

```{code-block} SQL
SELECT * FROM products
WHERE category = ‘Gifts’
OR 1=1--'AND released = 1
```
La requête modifiée retournera tous les articles de la catégorie "Gifts" ou 1 est égal à 1, ce qui est toujours vrai. La requête retournera donc tous les articles.

# Démonstration artificielle

Pour un exemple plus visuel et pratique, nous pouvons utiliser le site suivant. 
```{code-block}
https://www.hacksplaining.com/exercises/sql-injection#/start 
```
Ce site fournit une fausse base de données bancaire sur laquelle l'utilisateur peut s'entraîner à injecter du code dans les requêtes, par le biais de la page de connexion.