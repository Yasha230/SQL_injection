# Les attaques d'injection SQL

Les injections SQL sont une technique de priatage informatique qui exploite les vulnérabilité des applications Web pour accéder et manipuler des données stockées dans une base de données. Les attaques par injection SQL peuvent prendre de nombreuses formes différentes, depuis les techniques de base comme l'injection basée sur les erreurs ou l'injection basée sur l'union, jusqu'aux méthodes plus avancées comme l'injection aveugle basée sur les booléens ou l'injection basée sur le temps. Chacune de ces méthodes ciblent des vulnérabilités différentes dans le langage de requête SQL, et elles requièrent toutes différents niveaux de connaissance et de complexité de la part de l'attaquant. En examinant les différentes techniques d'attaque utilisées dans les injections SQL, les chercheurs peuvent mieux comprendre la gravité et l'impact de ces attaques, et développer des défenses plus efficaces contre elles. Les exemples qui suiveront sont basés sur des modèles classique d’attaques. Ces attques peuvent être classées en cinq types différent selon la méthode utilisée, chacune avec son propre but.

## Types d'attaques

```{code-block} SQL

```

1. **Injection SQL Union**

Ce type d'injection SQL consiste à ajouter une requête SQL supplémentaire à une requête existante en utilisant l'opérateur "UNION". La requête supplémentaire est utilisée pour extraire des données d'autres tables de la base de données. Par exemple, supposons que la page de connexion d'un site web utilise la requête suivante pour authentifier les utilisateurs :

```{code-block} SQL
SELECT * FROM users WHERE username = '$username' AND password = '$password';
```

Un attaquant peut injecter une requête union pour récupérer des données d'une autre table de la base de données, telle que la table "products" :

```{code-block} SQL
' UNION SELECT product_name, product_description, price FROM products --
```

La requête résultante serait la suivante :

```{code-block} SQL
SELECT * FROM users WHERE username = '' UNION SELECT product_name, product_description, price FROM products --' AND password = '';
```

2. **Injection SQL error-based (par injection SQL in-band)**

Ce type d'injection SQL consiste à exploiter des erreurs SQL pour extraire des données de la base de données. Par exemple, un attaquant peut saisir une instruction SQL contenant une erreur intentionnelle, telle que :

```{code-block} SQL
' OR 1=1; DROP TABLE users; --
```

Si l'application ne valide pas correctement les données saisies par l'utilisateur, la requête risque d'échouer et de supprimer l'intégralité de la table "users". L'attaquant peut également utiliser le message d'erreur pour extraire des informations sensibles de la base de données.

3. **Injection SQL blind time-based**

Ce type d'injection SQL consiste à envoyer des requêtes à la base de données et à mesurer le temps de réponse. Le hacker peut alors déduire des informations sur la base de données en se basant sur le temps de réponse. Par exemple, un pirate peut injecter une requête retardée comme celle-ci :

```{code-block} SQL
' AND SLEEP(10) --
```

Si l'application met plus de temps que d'habitude à répondre, cela peut indiquer que la base de données est vulnérable à l'injection SQL.

4. **Injection SQL blind boolean-based**

Ce type d'injection SQL consiste à envoyer des requêtes à la base de données et à déduire des informations selon que la réponse est vraie ou fausse. Par exemple, un pirate peut injecter une requête qui vérifie si une condition particulière est vraie ou fausse :

```{code-block} SQL
' AND (SELECT COUNT(*) FROM users WHERE username = 'admin' AND SUBSTRING(password, 1, 1) = 'a') > 0 --
```

Si l'application répond par un message indiquant que la condition est vraie, l'attaquant peut en déduire que le mot de passe de l'administrateur commence par la lettre "a".

5. **Injection SQL out-of-band**

Ce type d'injection SQL implique l'utilisation d'un canal distinct pour communiquer avec la base de données, tel qu'un serveur de courrier électronique ou un serveur DNS. Par exemple, un pirate peut injecter une requête qui déclenche une recherche DNS :

```{code-block} SQL
' UNION SELECT username, password, email FROM users WHERE username = 'admin'; SELECT * FROM (SELECT pg_sleep(10))a --
```

Si l'application est vulnérable à l'injection SQL hors bande, elle effectuera une recherche DNS vers un domaine contrôlé par l'attaquant, ce qui permettra à ce dernier d'extraire des données de la base de données.


## Qui sont les plus vulnérables

Les attaques par injection SQL peuvent potentiellement toucher tout site web qui utilise une base de données pour stocker et récupérer des informations. Cependant, certaines organisations sont plus vulnérables que d'autres en raison de certaines caractéristiques. Par exemple, les sites web de petites et moyennes entreprises qui manquent de ressources pour effectuer des tests de sécurité réguliers peuvent être plus vulnérables aux attaques par injection SQL. Les sites web qui utilisent des applications web obsolètes ou qui ne sont plus prises en charge sont également plus à risque. Les organisations qui ne suivent pas les meilleures pratiques de développement web, telles que la validation de l'entrée utilisateur, peuvent également être plus vulnérables aux attaques par injection SQL. Enfin, les sites web qui stockent des informations sensibles telles que des données financières ou médicales peuvent être des cibles privilégiées pour les attaquants. Il est donc important que toutes les organisations, grandes ou petites, prennent des mesures pour protéger leur site web contre les attaques par injection SQL en mettant en place des mesures de sécurité adéquates et en effectuant régulièrement des tests de sécurité.

## Conséquences

    --> + examples (real past situations) 

