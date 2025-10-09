# Bases de données

![bdd](https://cdn.pixabay.com/photo/2013/07/12/17/22/database-152091__180.png)

## 🟢 Introduction

### Qu'est-ce qu'une base de données ?

- Une courte [vidéo](https://www.youtube.com/watch?v=D5dE_XN8b6w) d'introduction.

### Le langage SQL

- Article [Wikipédia](https://fr.wikipedia.org/wiki/Structured_Query_Language)
- Cours et tutoriel : https://sql.sh/

### SQLite

- Article [Wikipédia](https://fr.wikipedia.org/wiki/SQLite)
- documentation officielle : https://sqlite.org/index.html

## 🟢 SQLite : mise en pratique

![sqlite](https://devopedia.org/images/article/97/5164.1547460358.svg)

### Installation de SQLite

Installation de sqlite3 sous *Linux* : `sudo apt install sqlite3`

### Utilisation

Voici un exemple de création d'une **base de données** contenant une petite **table** en utilisant `sqlite3` dans un terminal *Linux* (avec quelques exemples de requêtes).

Le **schéma** de l'unique **table** peut être représenté comme [ceci](https://drawdb.vercel.app/editor?shareId=8ef8046687c3f52366dfad7b4eb7262e).

```
franck@MINI-PC:~$ sqlite3 mabase.db
SQLite version 3.37.2 2022-01-06 13:25:41
Enter ".help" for usage hints.
sqlite> CREATE TABLE matable (
   ...> id INTEGER PRIMARY KEY AUTOINCREMENT,
   ...> nom TEXT,
   ...> age INTEGER);
sqlite> .table
matable
sqlite> INSERT INTO matable (nom, age) VALUES ('toto', 20);
sqlite> INSERT INTO matable (nom, age) VALUES ('titi', 18);
sqlite> SELECT * FROM matable;
1|toto|20
2|titi|18
sqlite> .mode box
sqlite> SELECT * FROM matable;
┌────┬──────┬─────┐
│ id │ nom  │ age │
├────┼──────┼─────┤
│ 1  │ toto │ 20  │
│ 2  │ titi │ 18  │
└────┴──────┴─────┘
sqlite> INSERT INTO matable (nom, age) VALUES ('tata', 46);
sqlite> SELECT * FROM matable;
┌────┬──────┬─────┐
│ id │ nom  │ age │
├────┼──────┼─────┤
│ 1  │ toto │ 20  │
│ 2  │ titi │ 18  │
│ 3  │ tata │ 46  │
└────┴──────┴─────┘
sqlite> SELECT nom, age FROM matable;
┌──────┬─────┐
│ nom  │ age │
├──────┼─────┤
│ toto │ 20  │
│ titi │ 18  │
│ tata │ 46  │
└──────┴─────┘
sqlite> SELECT nom FROM matable WHERE age > 30;
┌──────┐
│ nom  │
├──────┤
│ tata │
└──────┘
sqlite> UPDATE matable SET age=47 WHERE nom=='tata';
sqlite> SELECT * FROM matable;
┌────┬──────┬─────┐
│ id │ nom  │ age │
├────┼──────┼─────┤
│ 1  │ toto │ 20  │
│ 2  │ titi │ 18  │
│ 3  │ tata │ 47  │
└────┴──────┴─────┘
sqlite> VACUUM;
sqlite>.quit
franck@MINI-PC:~$
```

**Attention** : 

- Ne pas confondre les commandes **SQL** et les commandes **internes** à `sqlite3` (qui commencent par un *point* et ne sont pas terminées par un *point-virgule*). Voir [cette page](https://tutoriels.edu.lat/pub/sqlite/sqlite-commands/sqlite-commandes), et utilisez systématiquement la commande interne `.help`

## 🟢 À faire par vous même

### 1) Création d'une  base de données

**Schéma** de la table sous forme **textuelle** (la *clé primaire* est écrite en **gras**) :
> villes (**Id** INTEGER, Nom TEXT, CP TEXT, Latitude REAL, Longitude REAL, Altitude INTEGER)

Contenu de la table :

|**Id** |Nom     |CP   |Latitude|Longitude|Altitude|
|-------|--------|-----|--------|---------|--------|
|1      |Laon    |02000|49.56667|3.61667  |145     |
|2      |Soissons|02200|49.38167|3.32361  |50      |
|3      |Nancy   |54000|48.68439|6.18496  |212     |
|4      |Reims   |51100|49.25000|4.03333  |84      |
|5      |Paris   |75000|48.85341|2.34880  |42      |

Lancez `sqlite3 villes.db` pour créer la base de données

Établissez ensuite les requêtes SQL permettant de :

1. Créer la table *villes*
2. peupler cette table

Rassemblez toutes vos requêtes dans un seul fichier `creation.sql`.

- Visualisez votre base de données avec votre navigateur web sur [sqliteviewer](https://sqliteviewer.app/)
- Visualisez votre base de données dans *VScode* après avoir installé l'extension [sqlite3-editor](https://marketplace.visualstudio.com/items?itemName=yy0931.vscode-sqlite3-editor)

### Modification de la table

Établir les requêtes SQL pour :

1. Modifier l'altitude de *Reims* qui est de 86 m
2. Supprimer l'enregistrement de *Nancy*
3. Ajouter la ville de *Metz* :
	- Code postal : 57000
	- Latitude : 49.11911°
	- Longitude : 6.17269°
	- Altitude : 182 m
4. Ajouter un attribut *Région* (à remplir !)

- Vous rassemblerez vos commandes SQL dans un fichier nommé `modification.sql`. Chaque modification sera suivie d'un affichage de la table complète comme indiqué ci-dessous.
```sql
.mode box
--1
votre requête SQL
SELECT * FROM villes;
--2
votre requête SQL
SELECT * FROM villes;
--3
etc.
```

- Exportez cette nouvelle table au [format CSV](https://fr.wikipedia.org/wiki/Comma-separated_values) dans un fichier nommé `villes.csv` puis vérifiez son contenu avec `cat`.

### Requêtes simples

- Utilisez la table modifiée.
- Utilisez le mode **box** de SQlite3 et rassemblez toutes vos requêtes dans un seul fichier `requetes.sql`

Établir les requêtes SQL pour :

1. Afficher la table complète
2. Idem, par ordre décroissant d'altitude
3. Afficher les noms et les codes postaux des villes situées dans le département de l'*Aisne*
4. Afficher les Noms des villes qui sont en dessous de 100 m d'altitude
5. Afficher le nombre de villes qui sont à plus de 100 m d'altitude
6. Afficher les noms des villes dont le nom contient au moins une fois la lettre *s*
7. Afficher la ville la plus au nord
8. Afficher les deux villes les plus à l'ouest
9. Afficher la différence de latitude entre *Laon* et *Soissons*

### 2) Connecteur Python

![python](https://www.edureka.co/blog/wp-content/uploads/2019/06/Python-150x150.png)
Consultez ce [guide](https://www.ionos.fr/digitalguide/sites-internet/developpement-web/sqlite3-avec-python/) puis créez un programme *Python* permettant d'extraire les coordonnées géographiques des villes de *Laon* et *Soissons* de la base de données `villes.db`, puis d'afficher dans le terminal la distance **à vol d'oiseau** entre ces deux villes (en km).

Pour le calcul, vous utiliserez la [fonction](https://courspython.com/fonctions.html) suivante :

```python
from math import cos, asin, sqrt, pi

def distance(lat1, lon1, lat2, lon2):
    r = 6371.0  # km
    p = pi / 180  # rad/°
    return 2 * r * asin(sqrt(0.5 - cos((lat2-lat1)*p)/2 + cos(lat1*p) * cos(lat2*p) * (1-cos((lon2-lon1)*p))/2))
```

Il s'agit de la [formule de haversine](https://fr.wikipedia.org/wiki/Formule_de_haversine).

Les coordonnées des deux villes devront être récupérées dans un [dictionnaire](https://courspython.com/dictionnaire.html) nommé `coords`. L'exécution du programme devra fournir l'affichage suivant :

```
coords = {'Laon': (49.56667, 3.61667), 'Soissons': (49.38167, 3.32361)}
Distance à vol d'oiseau entre Laon et Soissons : 29.52 km
```

Consultez aussi la [documentation officielle](https://docs.python.org/fr/3/library/sqlite3.html)

### 3) Pour aller plus loin

#### Objectif

En vous faisant aider par une **IA**, établissez un script *Python* qui, à partir de la base de données `villes.db`, affiche dans la console un **tableau de distances** en code *markdown*.

#### Contraintes

L'affichage devra être **exactement** le suivant (table *markdown* compressée) :

```
| |Laon|Soissons|Reims|Paris|
|:---:|:---:|:---:|:---:|:---:|
|Soissons|30||||
|Reims|46|53|||
|Paris|122|92|130||
|Metz|192|209|156|281|
```
Ce qui donnera une fois copié-collé dans un document *markdown* :

| |Laon|Soissons|Reims|Paris|
|:---:|:---:|:---:|:---:|:---:|
|Soissons|30||||
|Reims|46|53|||
|Paris|122|92|130||
|Metz|192|209|156|281|

- Chaque case indique la distance (en km) entre la ville en ligne et la ville en colonne, arrondie au km.
- Affichez uniquement la moitié inférieure de la matrice (triangle inférieur strict, sans la diagonale ni répétition).
- Les cellules non concernées doivent rester vides.

#### Test unitaire supplémentaire

Aidez-vous de l'IA pour ajouter quelques villes dans la base de données, puis vérifiez que votre script donne le nouveau tableau attendu.

Par exemple :

| |Laon|Soissons|Reims|Paris|Metz|Brest|Nice|Toulouse|Angers|
|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
|Soissons|30|||||||||
|Reims|46|53||||||||
|Paris|122|92|130|||||||
|Metz|192|209|156|281||||||
|Brest|605|581|631|505|785|||||
|Nice|709|699|664|686|608|1044||||
|Toulouse|683|658|658|588|712|702|468|||
|Angers|385|356|392|265|530|309|739|458||
|Bordeaux|614|585|602|499|699|495|637|212|294|

#### Pour aller encore plus loin

Aidez-vous d'une IA pour ajouter au script un export HTML. L'affichage dans un navigateur web pourra ressembler à cela :

![tableau html](images/tableau-html.png)

---
*Fin de la page*

