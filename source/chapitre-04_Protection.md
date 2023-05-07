# Méthodes de protection

Il existe plusieurs méthodes pour se protéger contre les attaques SQL. La première consiste à utiliser des requêtes paramétrées ou des procédures stockées pour accéder à la base de données. Les requêtes paramétrées permettent de préparer une instruction SQL avec des paramètres de substitution définis à l'avance. Cela réduit considérablement le risque d'injection SQL, car les entrées de l'utilisateur sont vérifiées et validées avant d'être incluses dans la requête.

Un exemple de requête paramétrée serait :

```{code-block} SQL
SELECT * FROM users WHERE username = ? AND password = ?
```
Dans cet exemple, les variables « username » et « password » sont des **paramètres** de substitution qui sont remplacés par les valeurs saisies par l'utilisateur. Les paramètres sont vérifiés et validés avant d'être inclus dans la requête, réduisant ainsi le risque d'injection SQL.


Une autre méthode de protection consiste à **échapper** les caractères spéciaux dans les entrées utilisateur. Cela signifie que les caractères spéciaux tels que les guillemets simples, les guillemets doubles et les caractères de retour à la ligne sont transformés en leur équivalent littéral, plutôt que d'être interprétés comme faisant partie de la requête SQL.

Un exemple d'échappement de caractères spéciaux serait :

```{code-block} SQL
INSERT INTO users (username, password) VALUES ('John O\'Connor', 'password123')
```
Dans cet exemple, le guillemet simple dans le nom d'utilisateur est échappé en utilisant une barre oblique inversée () pour qu'il soit interprété littéralement plutôt que comme une fin de chaîne de caractères.

