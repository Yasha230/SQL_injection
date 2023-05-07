# Exemple

Prennons l'exmple d'une application de vente, qui présente des produits appartenant à différentes catégories. Lorsqu'un utilisateur clique sur la catégorie 'gifts', le navigateur requiert l'URL suivant:

```{admonition} URL
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

```{admonition} URL
released = 1
```
indique que l'article est publié et donc visible aux clients. 

Si au contraire,

```{admonition} URL
released = 0
```
l'article n'est pas visible et l'on ne peut pas interagir avec.

L'application ne présente aucune défense contre des attaques SQL, ce qui signifie qu'il est vulnérable à des attaques telles que celle-ci

```{admonition} URL
https://insecure-website.com/products?catergory='Gifts'--
```
Cette petite modification de l'URL entraîne aussi une modification de la requête SQL.

```{code-block} SQL
SELECT * FROM products
WHERE category = ‘Gifts’--'
AND released = 1
```
Le double tiret "--" indique que le reste de la requête n'est qu'un commentaire et n'est donc pas pris en compte. Cela supprime en effet la restriction qui bloquait au départ l'accès aux éléments non publiés.


# Démonstration artificielle

