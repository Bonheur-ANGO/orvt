ORVT

# **Introduction**

## **1 - Système d'information géographique**
Un système d'information géographique est un système d'information, composé de matériels, d'outils informatiques, de logiciels et de personnel qualifié, spécialement conçu pour recueillir, stocker, analyser, traiter, gérer et diffuser les données géographiques. On appelle donnée géographique une donnée contenant une référence à un lieu ou une position des entités à la surfaces de la terre. Elles sont utilisées dans plusieurs domaines notamment de la cadre de la recherche scientifique, dans le domaine des transport, de l'agriculture etc...


### **1.1 Acquisition des données géographiques**
Les données géographiques sont obtenues de plusieurs façons. Les deux manières les plus utilisées sont l'imagerie aérienne et l'imagerie satellitaire.
- **Imagerie aérienne** : Une photographie aérienne désigne une photographie prise depuis les airs. L'IGN avec sa flotte aérienne (4 avions), photographie l'ensemble du territoire avec des caméras numériques tous les 3 ans. Les avions se placent à une certaine altitude et s'y tiennent en prennant des images numériques verticales couvrant 1.5 km de longeur par 1.5 km de largeur au sol. Ces opérations se font itératicement afin de couvrir l'ensemble du térritoire national.
- **Imagerie satellitaire** : Une image satellite désigne une prise de vue transmise par un satellite en orbite. Grâce à un personnel compétent en géométrie et en photogrammétrie, l'IGN a réussi à développer des chaînes de production oppérationnel de cartographie à parties d'images satellites.


## **2 - Données géographiques : vecteur et raster**
Il existe deux manières de représenter les données géographiques de manière numérique à savoir: le mode vecteur et le mode raster.

### 2.1 Mode vecteur
#### 2.1.1 Généralités
Les données géographiques en mode vecteur permettent de modéliser le monde réel à travers des objets. Un objet est caractérisé par deux éléments : les informations qui lui sont associées ou données attributaires et sa forme ou sa géométrie. La géométrie est constituée d’un ou plusieurs points interconnectés. Un point est une position dans l’espace. Il existe trois principaux types de géométrie : point, ligne et polygone. Si un objet dispose d’un seul point, alors sa forme est un point. Si un objet dispose de plusieurs points qui ne forment pas de forme géométrique fermée, alors sa forme est une ligne. Si un objet dispose de plusieurs points interconnectés formant une forme géométrique close, alors sa forme est un polygone.

![Les différentes formes de vecteurs](schema_vecteur_formes.png)

Les objets peuvent être par exemple des maisons, des routes ou arbres. Ils sont stockés 
dans des bases de données avec leurs coordonnées spatiales et leurs données attributaires ou métadonnées. 

![table attributaire](table_attributaire.png)


### 2.2 Mode raster
#### 2.2.1 Généralités
Les données géographiques en mode raster sont des images /*(plans scannés, photographie aériennes, images satellitaires, modèles numériques de terrain)*/ constitués de plusieurs pixels organisés sous forme de grilles en lignes et colonnes. Un pixel est l'unité de base de la définition d'une image numérique matricielle. A chaque pixel est associé une ou plusieurs valeurs numériques décrivant les caractéristiques de l'espace telles que la température, l'altitude ou la végétation. 

![raster](raster_pix.jpg)


### 1.3 Le principe de couches
Si l’on souhaite représenter différents types d’objets on utilise le principe de superposition 
de couches qui consiste à disposer différentes couches d’objets les unes sur les autres afin de constituer une carte. Chaque couche regroupes les données appartenant à une même thématique ou classe d'objets (immeubles, routes etc...)

![superposition des couches](superposition_des_couches.jpg)


## **3 - Web Mapping**
Une carte géographique est une représentation graphique d'un espace géographique. Avec l'évolution des technologies et d'internet, le besoin d'affichage de carte géographique sur tous types d'écrans devient de plus en plus demandé par les utilisateurs et cela est possible grâce au Web mapping. Le web mapping ou cartographie web est la forme de cartographie qui fait usage d’internet afin de concevoir, traiter, produire et publier des cartes géographiques. Ces communications sont possibles grâce à un ensemble de règles appelées protocole. L’Open Géospatial Consortium (OGC) est une organisation internationale qui implémente des standards pour les services et le contenu géospatial, le traitement de données géographiques et les formats d’échange.
Parmi les spécifications, les plus couramment utilisés à l'IGN sont :
- **Web Feature Service (WFS)** : Permet au moyen d’une URL formatée, d’interroger des 
serveurs cartographiques afin de manipuler des objets géographiques vectoriels. Les opérations de manipulations permettent de : créer de nouveaux objets, effacer, récupérer, rechercher ou mettre à jour des objets. Le protocole WFS permet d'effectuer 5 principales requêtes afin d'obtenir des informations :
    - GetCapabilities : permet de connaître les capacités du serveur (quelles opérations sont supportées et quels objets sont fournis).
    - DescribeFeatureType : permet de retourner la structure de chaque entité susceptible d’être fournie par le serveur.
    - GetFeature : permet de livrer des objets (géométrie et/ou attributs) en GML (Geography Markup Language).
    - LockFeature : permet de bloquer des objets lors d'une transaction.
    - Transaction : permet de modifier l'objet (création, mise à jour, effacer).

    On retrouve plusieurs paramètres selon les requêtes :
    - paramètres communs :
        - VERSION : la version du service utilisée (1.0.0, 2.0.0, …)
        - REQUEST : la requête adressée au serveur (GetCapabilities, GetFeature ou DescribreFeatureType)
        - SERVICE : le type de service (ici "WFS")
    - paramètres requête DescribeFeatureType :
        - TYPENAME : le nom du type d’élément (classe d’objet) dont on veut connaitre la structure
    - paramètres requête GetFeature :
        - TYPENAME : le nom du type d’élément (classe d’objet) dont on veut récupérer des éléments (objets)
        - OUTPUTFORMAT : le format de réponse à la requête
        - COUNT : le nombre maximum d’éléments retournés (MAXFEATURES si version 1.0.0, COUNT si version 2.0.0)
        - FILTER : le filtre personnalisé qui va permettre d’effectuer des sélection sur les éléments à récupérer.

    La structure d'une URL WFS est la suivante : http://domaine.com:port/sigserver/wfs/?service=wfs&version=version&request=request


-  **Web Map Service (WMS)** : Permet de mettre à disposition d’utilisateurs distants des images géoréférencées, via une simple requête HTTP, à partir de données sources raster (image) ou vecteur.
- **Web Map Tile Service (WMTS)** : Permet d'obtenir des cartes géo-référencées tuilées à partir d'un serveur. Ce service est comparable au Web Map Service (WMS) mais tandis que le WMS permet de faire des requêtes nécessitant une certaine puissance de calcul côté serveur à chaque requête, le WMTS met l'accent sur la performance et ne permet de requêter que des images pré-calculées (tuiles)par le serveur.
- **Tile Map Service (TMS)** : Le service TMS est comme le service WFS. Il Transmet des données géographiques vectorielles mais sous formes de tuiles vecteurs. Les tuiles sont des paquets de données géographiques prédécoupées en forme de dalles par le serveur, prêtes à être transférées lorsqu’une requête est émise. Ces tuiles sont produites par le serveur en fonction de l’échelle de visualisation. On appelle cela le principe de la pyramide.