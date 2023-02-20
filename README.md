# **ORVT : Outil de rasterisation pour les vecteurs tuilés**

## **Table des matières**
- **[I - Introduction](#i---introduction)**
- **[I.1 - Système d'information géographique](#i1---système-dinformation-géographique)**
    -  **[1.1.1 - Acquisition des données géographiques](#i11---acquisition-des-données-géographiques)**
- **[I.2 - Données géographiques : vecteur et raster](#i2---données-géographiques--vecteur-et-raster)**
    - **[I.2.1 - Mode vecteur](#i21---mode-vecteur)**
        - **[I.2.1.1 - Généralités](#i211---généralités)**
        - **[I.2.1.2 - Table attribuaire](#i212---table-attribuaire)**
    - **[2.2 - Mode raster](#i22---mode-raster)**
        - **[I.2.2.1 - Généralités](#i221---généralités)**
    - **[I.2.3 - Le principe de couches](#i23---le-principe-de-couches)**
- **[I.3 - Tuiles](#i3---tuiles)**
- **[I.4 - Web Mapping](#i4---web-mapping)**
    - **[I.4.1 Architecture de la cartographie web](#i41---architecture-de-la-cartographie-web)**
- **[I.5. Tuiles vectorielles : symbologie](#i5---tuiles-vectorielles--symbologie)**
- **[I.6. Rasterisation](#i6---rasterisation)**
- **[II - Etat de l'art des solutions techniques permettant d'effectuer une rasterisation de flux de vecteurs tuilés à l'IGN](#ii---etat-de-lart-des-solutions-techniques-permettant-deffectuer-une-rasterisation-de-flux-de-vecteurs-tuilés-à-lign)**
- **[II.1 - Objectif](#ii1---objectif)**
-  **[II.2 - Solutions techniques](#ii2---solutions-techniques)**
    - **[II.2.1 - Librairie Javascript : ol-map-screenshot](#ii21---librairie-javascript--ol-map-screenshot)**
        -  **[II.2.1.1 - Tests](#ii211---tests)**
        -  **[II.2.1.2 - Conclusion](#ii212---conclusion)**
    - **[II.2.2 - L'outil' : Print Maps](#ii22---loutil--print-maps)**
        -  **[II.2.2.1 - Tests](#ii221---tests)**
        -  **[II.2.2.2 - Tests](#ii222---conclusion)**
    - **[II.2.3 - La librairie : Ink Map](#ii23---la-librairie--ink-map)**
        -  **[II.2.3.1 - Tests](#ii231---tests)**
        -  **[II.2.3.2 - Conclusion](#ii232---conclusion)**

## **Glossaire**
**OGC :** Open Géospatial Consortium

**JSON :** Javascript Object Notation est un format d'échange léger de données

**SIG :** Système d'information géographique

**MapBox :** entreprise américaine spécialisée dans la cartographie en ligne

**API :** Application Programming Interface

**Javascript :** Langage de programmation principalement utilisé dans le cadre du développement de pages web intéractives.

**OpenLayers :** Librairie javascript permettant de créer des cartes géographiques web

**Librairie informatique :** c'est un ensemble de classes et de fonctions déjà pré-codées par des développeurs et  accessible publiquement généralement dans une dépôt, utilisable dans le développement des applications selon le besoin.

**PDF :** Portable Document Format est un format de document très utilisée sur différents appareils informatiques(tablettes, ordinateurs, téléphones portables)

**NPM :** Node Package Manager est un gestionnaire de librairies javascript
**PBF :** Protocol Buffer Binary Format

# **I - Introduction**
Les données géographiques vectorielles permettent de représenter les entités du monde réel sous forme de dessin. Ces données géographiques peuvent être délivrées côté client sous formes de dalles lorsqu'une requête est émise. Cela s'appelle des tuiles vectorielles. L'utilisation de tuiles vectorielles offre une rapidité d'accès aux données car ces tuiles sont pré calculées ou fabriquées à la volée lorsqu'une requête est émise, et une symbolisation côté client. L'utilisateur peut donc appliquer le style désiré côté client en créant son fichier de style. Avec l'utilisation de plus en plus fréquente des cartes sur le web et sur des applications mobiles, l'utilisateur peut parfois avoir besoin d'obtenir une image de cette carte personnalisée (avec son style) ou de l'imprimer. Cela consiste donc à rasteriser un flux de tuiles vecteurs. Actuellement il n'existe pas de solutions techniques à l'IGN permettant de répondre à ce besoin. L'objectif pour nous donc sera d'effectuer des propositions de différentes solutions techniques permettant de faire une rasterisation de flux de vecteurs tuilés à travers cet état de l'art.


## **II. - Données géographiques : vecteur et raster**

Il existe deux manières de représenter les données géographiques de manière numérique à savoir : le mode vecteur et le mode raster.

### **II.1 - Mode vecteur**
#### **II.1.1 - Généralités**
Les données géographiques en mode vecteur permettent de modéliser le monde réel à travers des objets. Un objet est caractérisé par deux éléments : les informations qui lui sont associées ou données attributaires et sa forme ou sa géométrie. La géométrie est constituée d’un ou plusieurs points interconnectés. Un point est une position dans l’espace. Il existe trois principaux types de géométrie : point, ligne et polygone. Si un objet dispose d’un seul point, alors sa forme est un point. Si un objet dispose de plusieurs points qui ne forment pas de forme géométrique fermée, alors sa forme est une ligne. Si un objet dispose de plusieurs points interconnectés formant une forme géométrique close, alors sa forme est un polygone.

![Les différentes formes de vecteurs](schema_vecteur_formes.png)

> source : https://www.emse.fr/tice/uved/SIG/Glossaire/co/vecteur_mode.html

#### **II.1.2 - Table attribuaire**
Les objets peuvent être par exemple des maisons, des routes ou arbres. Ils sont stockés 
dans des bases de données avec leurs coordonnées spatiales et leurs données attributaires ou métadonnées. Les données ou tables attributaires permettent de décrire les propriétés de l'entité. Les objets sont regroupés par thème dans les tables. Par exemple on regroupe toutes les entités représentant des batiments dans une même table. Une table est composé de lignes et de colonnes. Chaque colonne représente une caractéristique de l'entité comme la surface, la hauteur ou la date de construction d'un bâtiment. Chaque ligne représente une entité.

![table attributaire](table_attributaire.png)


### **II.2 - Mode raster**
#### **II.2.1 - Généralités**
Les données géographiques en mode raster sont des images constitués de plusieurs pixels organisés sous forme de grilles en lignes et colonnes. Un pixel est l'unité de base de la définition d'une image numérique matricielle. A chaque pixel est associé une ou plusieurs valeurs numériques décrivant les caractéristiques de l'espace telles que la température, l'altitude ou la végétation. 

![raster](raster_pix.jpg)

> source : https://naturagis.fr/cartographie-sig/difference-vecteur-raster/


### **II.3 - Le principe de couches**
Si l’on souhaite représenter différents types d’objets on utilise le principe de superposition 
de couches qui consiste à disposer différentes couches d’objets les unes sur les autres afin de constituer une carte. Chaque couche regroupes les données appartenant à une même thématique ou classe d'objets (immeubles, routes etc...)

![superposition des couches](superposition_des_couches.jpg)

> source : https://www.cc-molsheim-mutzig.fr/decouvrir/cartes.htm

### **II.4 - Tuiles**
Les tuiles (rasters ou vecteurs) sont des paquets de données géographiques prédécoupées en forme de dalles par le serveur, prêtes à être transférées lorsqu’une requête est émise. Elles peuvent avoir différentes tailles : 64x64, 256x256, 512x512 pixels. Les services web utilisent le plus souvent des tailles de 256x256. Ces tuiles sont produites par le serveur en fonction de l’échelle de visualisation. On appelle cela le principe de la pyramide. À chaque niveau de zoom, des tuiles spécifiques sont fournis. Les tuiles présentent plusieurs avantages d'utilisation dont :
- La rapidité d'acès à la donnée lors d'une requête car les tuiles sont prédécoupées à l'avance par le serveur ou fabriqué à la volée et stocker dans le cache
- La possibilité de personnalisation du style côté client (pour les tuiles vectorielles)


![Tableau récapitulatif](vector_tiles_pyramid_structure.png)
> source : https://docs.qgis.org/3.16/fr/docs/user_manual/working_with_vector_tiles/vector_tiles_properties.html


### **II.5 - Cartographie web**
Une carte géographique est une représentation graphique d'un espace géographique. Avec l'évolution des technologies et d'internet, le besoin d'affichage de cartes géographiques sur tous types d'écrans devient de plus en plus demandé par les utilisateurs et cela est possible grâce au Web mapping. Le web mapping ou cartographie web est la forme de cartographie qui fait usage d’internet afin de concevoir, traiter, produire et publier des cartes géographiques[^2]. Ces communications sont possibles grâce à un ensemble de règles appelées protocole. L’OGC est une organisation internationale qui implémente des standards pour les services et le contenu géospatial, le traitement de données géographiques et les formats d’échange.
Parmi les spécifications, les plus couramment utilisés à l'IGN sont :

- **Web Feature Service (WFS)** : Permet au moyen d’une URL formatée, d’interroger des 
serveurs cartographiques afin de manipuler des objets géographiques vectoriels. Les opérations de manipulations permettent de : créer de nouveaux objets, effacer, récupérer, rechercher ou mettre à jour des objets.[^3]:


-  **Web Map Service (WMS)** : Permet de mettre à disposition d’utilisateurs distants des images géoréférencées, via une simple requête HTTP, à partir de données sources raster (image) ou vecteur. [^4]

- **Web Map Tile Service (WMTS)** : Permet d'obtenir des cartes géo-référencées tuilées à partir d'un serveur. Ce service est comparable au Web Map Service (WMS) mais tandis que le WMS permet de faire des requêtes nécessitant une certaine puissance de calcul côté serveur à chaque requête, le WMTS met l'accent sur la performance et ne permet de requêter que des images pré-calculées (tuiles) par le serveur. [^5]


- **Tile Map Service (TMS)** : Le service TMS est comme le service WFS. Il Transmet des données géographiques vectorielles mais sous formes de tuiles vecteurs.[^6]


![Protocoles](tablea_comparaison_protocoles.PNG)



### **II.6 - Architecture de la cartographie web**
La cartographie web se base sur une architecture client/serveur:
- Client : Ici généralement représenté par un navigateur web, permet de visualiser les données géographiques transmises depuis le serveur
- Serveur : Traite les données géographiques et les transmet


La communication s’effectue de la manière suivante :
- Le client envoie une requête pour l’affichage d’une carte web géographique
- Le serveur reçoit la requête
- Le serveur extrait les données nécessaires à la constitution de la carte web géographique à partir de la base de données
- Le serveur transmets les données géographiques
- La carte géographique web est constituée à partir des données géographiques reçues du serveur [^7]:

![architecture cartographie web](architecture_cartographie_web.png)


### **II.8 - Flux de vecteurs tuilés**
L'on parle de flux de vecteurs tuilés lorsqu'il y'a un serveur qui délivre des tuiles vectorielles lorsque des requêtes seront émise par le serveur.

Architecture avec utilisation du service de tuilage : La communication s’effectue de la
manière suivante :
- Le client envoie une requête pour l’affichage d’une carte web géographique
- Le serveur reçoit la requête
- Le serveur extrait les données nécessaires à la constitution de la carte web 
géographique à partir de la base de données
- Le serveur sélectionne les tuiles en fonction du niveau de zoom si elles avaient déjà été chargée ou sinon les 
fabrique à la volée par rapport à l’échelle de visualisation et la zone concernée et les transmet au client au format PBF
- Le serveur transmet les tuiles permettant de fabriquer la carte web côté client
- La carte géographique web est constituée à partir des tuiles vecteurs reçues du serveur
- Les tuiles sont mise en cache sur le serveur[^7]

![flux de vecteurs tuilés](flux_de_vecteurs_tuilés.png)


### **II.10 - Tuiles vectorielles : Format PBF**

Le format PBF est un format développé par google permettant de sérialiser des données structurées. Le PBF est simple d'utilisation, performant et a été conçu pour remplacer à long terme le format XML[^8]. OSM c'est donc basé sur ce format pour stocker et échanger les données géographiques vecteurs. Les données géographiques sont stockées au format PBF d'OSM sous forme de messages et organisés en blocs. On retrouve deux types de bloc dans les fichiers PBF à savoir :
- OSMHeader : bloc d'entête et de métadonnées permettant de décrire les propriétés des blocs et des messages.
- OSMData : bloc contenant les données géospatiales

On distingue 3 types de données géospatiales que l'on peut retrouver dans un fichier PBF:
- Des noeuds (points)
- Des chemins (lignes)
- Des relations (polygones)[^9]

Les tuiles sont donc servies au format PBF d'OSM lorsqu'une requête est envoyée en fonction du niveau de zoom demandé. Elles sont compressées en utilisant le format PBF pour les rendre plus légères et plus faciles à transmettre.

![Transmission des tuiles](transmission_pbf.png)

### **II.10 - Tuiles vectorielles : symbologie**

Comme dans notre étude nous nous intéréssons principalement aux tuiles vectorielles, il est plus que nécessaire de parler de symbologie. La symbolologie est l'ensemble d'éléments (palette de couleurs, polices d'écriture, icônes...), utilisé afin de donner une apparence visuelle à la carte et ainsi mettre en valeur les informations en fonction de leur importance. L'un des avantages comme on le disait plus haut des tuiles vectorielles est qu'elles offrent la possibilité à un utilisateur de créer sa propre symbologie côté client à travers la création d'un fichier de style. Le fichier de style va permettre de représenter chaque entité comme le souhaite l'utilisateur à travers des règles de styles bien définis. Plusieurs normes permettent de créer une symbologie côté client à savoir :
- [MapBox GL JS](https://docs.mapbox.com/mapbox-gl-js/style-spec/) : document de style au format JSON créé par MapBox.
- [Carto CSS](https://cartocss.readthedocs.io/en/latest/) : syntaxe similaire au CSS, permettant de créer un style pour des données géographiques.

[Explication du fichier de style](explication_fichier_de_style.png)


**Le fichier de style** est appliqué côté client comme montré dans la figure ci-dessous :


![Illustration application du fichier de style](illustration_application_fichier_de_style.png)

## **II.10 - Rasterisation**
De manière globale, la rasterisation est un procédé qui consiste à convertir une image vectorielle en une image matricielle destinée à être affichée sur un écran ou imprimée par un matériel d'impression. Dans le cadre des SIG, la rastérisation est le passage du mode vecteur au mode raster : c'est la conversion de vecteurs (point, polygone, ligne) en une grille matricielle de pixels où chaque pixel comprend une valeur.

![rasterisation](rasterisation.png)
> source : https://www.researchgate.net/figure/Principe-de-la-rasterisation-conversion-du-format-vecteur-vers-le-format-raster_fig1_342344729

Dans notre cadre nous utilisons des tuiles vectorielles. La rasterisation consistera donc à effectuer une conversion de tuiles vecteurs vers des tuiles rasters. Les tuiles vecteurs sont servies au format pbf en fonction de l'échelle de visualisation. Les outils que nous présenterons doivent permettre de convertir des tuiles au format pbf vers des tuiles au format png avec le même style que l'utilisateur aura définit et sans pour autant qu'il y ait dégradation des informations.

![outil de rasterisation](outil_de_rasterisation.png)

&nbsp;
# **III - Etat de l'art des solutions techniques permettant d'effectuer une rasterisation de flux de vecteurs tuilés à l'IGN**
## **II.1 - Objectif**
Avec l'utilisation de plus en plus fréquente des cartes sur le web et sur des applications mobiles, le besoin parfois d'obtenir une image de cette carte ou de l'imprimer peut vraiment être problématique du fait de la qualité de rendu de l'image qui peut être parfois très floue. Actuellement il n'existe pas de solutions techniques à l'IGN permettant de répondre à ce besoin. L'objectif pour nous donc sera d'effectuer des propositions de différentes solutions techniques permettant de faire une rasterisation de flux de vecteurs tuilés à travers cet état de l'art.

## **II.2 - Solutions techniques**
### **II.2.1 - Librairie Javascript : ol-map-screenshot**
ol-map-screenshot est une librairie javascript disponible sur npm, que l'on peut utiliser pour obtenir une image (capture d'écran) d'une carte web au format PNG, JPEG. Vous trouverez une demo de l'utilisation la librairie [ici](https://jmmluna.github.io/ol-map-screenshot/demo/)

- **Caractéristique de la librairie :**
    - Image de la carte customisables à travers des options
    - Prise en charge des formats JPEG, PNG
    - Métadonnées de capture d'écran dans le résultat
    - Rendu de la barre d'échelle

- **Options de rendu de l'image :**
    - dimension : Taille de l'image souhaitée en milimètres (longueur et largeur)
    - L'échelle de la carte
    - Longueur de la barre d'échelle de la carte
    - Format d'exportation de l'image
    - Résolution d'écran

#### **II.2.1.1 - Tests**
Nous avons pu testé l'outil avec différentes résolutions afin de vérifier que celle-ci fournis une image de très bonne qualité.
- **Test - 1 :** Rendu de l'image avec comme paramètres :
    - Résolution : 72ppp
    - dimensions : 400x240 "mm"
![Test de rendu d'image avec une résolution de 72](ol-map-screenshot-72.png)

- **Test - 2 :** Rendu de l'image avec comme paramètres :
    - Résolution : 300ppp
    - dimensions : 400x240 "mm"
![Test de rendu d'image avec une résolution de 300](ol-map-screenshot-300.png)

#### **II.2.1.2 - Conclusion**
- **Avantages :**
    - Intégration facile et intuitive dans le code
    - Basée sur OpenLayers
    - Options de customisation
    - Le rendu est très satisfaisant
    - Génération du rendu rapide pour les très hautes résolutions
- **Désavantages :**
    - Faible utilisation de la librairie (source : github)

### **II.2.2 - L'outil : Print Maps**
Print Maps est une application web conçu par un développeur nommé Matthew Petroff. Il a constaté qu'il était très difficile de pouvoir imprimer une carte géographique du fait de la faible résolution de celle. Il a donc conçu cet outil permettant d'obtenir des images de cartes géographiques avec une haute résolution au format PNG ou PDF. Contrairement aux autres outils, cet outil utilise les source de données provenant de Mapbox. Vous trouverez la démo de l'outil [ici](https://printmaps.mpetroff.net/)

- **Caractéristique de la librairie :**
    - Image de la carte customisables à travers des options
    - Prise en charge des formats PNG, PDF
    - Rendu de la barre d'échelle

- **Options de rendu de l'image :**
    - dimension : Taille de l'image souhaitée en milimètres ou en inch (longueur et largeur)
    - Format de rendu (PNG, PDF)
    - choix du style de la carte
    - Format d'exportation de l'image
    - Résolution d'écran

#### **II.2.2.1 - Tests**
Nous avons testé l'outil avec différentes résolutions afin de vérifier que celle-ci fournis une image de très bonne qualité.
- **Test - 1 :** Rendu de l'image avec comme paramètres :
    - Résolution : 72ppp
    - dimensions : 400x240 "mm"
![Test de rendu d'image avec une résolution de 72](print-map-72.png)

- **Test - 2 :** Rendu de l'image avec comme paramètres :
    - Résolution : 300ppp
    - dimensions : 400x240 "mm"
![Test de rendu d'image avec une résolution de 300](print-map-300.png)

#### **II.2.2.2 - Conclusion**
- **Avantages :**
    - Populaire auprès des développeurs (source : github)
    - Options de customisation
    - Le rendu est très satisfaisant et de haute qualité
    - Libre de droit
    - Génération du rendu rapide pour les hautes résolutions
- **Désavantages :**
    - N'est pas implémentée avec OpenLayers mais plutôt avec MapBox
    - N'a pas d'API et nécessite donc de copier le code et de le modifier


### **II.2.3 - La librairie : Ink Map**
Ink Map est une librairie javascript basée sur open Layers capable de produire des images de carte. InkMap a été développé par camptocamp une entreprise basée en France, et a été entièrement financée par le ministère français de l'écologie. Elle a été développée afin d'obtenir des cartes de haute résolution imprimable à partir du navigateur. Vous trouverez la démo de la librairie [ici](https://camptocamp.github.io/inkmap/main/)

- **Caractéristique de la librairie :**
    - Image de la carte customisables à travers des options
    - Prise en charge du format PNG

- **Options de rendu de l'image :**
    - Tableau de couche d'objets
    - dimension : Taille de l'image souhaitée en milimètres ou en inch (longueur et largeur)
    - Résolution d'écran
    - Longitude et latitude du centre de la carte
    - Vue de la barre d'échelle
    - projection

#### **II.2.3.1 - Tests**
Nous avons testé l'outil avec différentes résolutions afin de vérifier que celle-ci fournis une image de très bonne qualité.
- **Test - 1 :** Rendu de l'image avec comme paramètres :
    - Résolution : 72ppp
    - dimensions : 400x240 "mm"
![Test de rendu d'image avec une résolution de 72](inkmap-test-72.png)

- **Test - 2 :** Rendu de l'image avec comme paramètres :
    - Résolution : 255ppp
    - dimensions : 400x240 "mm"
![Test de rendu d'image avec une résolution de 250](inkmap-test-250.png)

#### **II.2.3.2 - Conclusion**
- **Avantages :**
    - Intégration facile et intuitive dans le code
    - Populaire auprès des développeurs (source : github)
    - Options de customisation
    - Le rendu est satisfaisant
    - Validé par le ministère français de l'écologie
    - Basée sur OpenLayers
- **Désavantages :**
    - Génération du rendu lente pour les très hautes résolutions

### **II.2.4 - OpenLayers : print-to-scale**
OpenLayers propose différentes exemples pour son utilisation sur son site internet. Parmis les exemples on retrouve print-to-scale, qui démontre comment l'on peut obtenir une image d'une carte ave différentes mise en page en se basant sur la bibliothèque jspdf. Le rendu est un PDF de l'image de la carte produite au format JPEG et insérée par la suite dans le PDF. Vous trouverez la démo de la librairie [ici](https://openlayers.org/en/latest/examples/print-to-scale.html)

- **Caractéristique de la l'outil :**
    - Image de la carte customisables à travers des options
    - Prise en charge du format PDF

- **Options de rendu de l'image :**
    - dimension : Taille de l'image souhaitée en millimètre (longueur et largeur)
    - Résolution d'écran
    - échelle
    - Vue de la barre d'échelle

#### **II.2.4.1 - Tests**
Nous avons testé l'outil avec différentes résolutions afin de vérifier que celle-ci fournis une image de très bonne qualité.
- **Test - 1 :** Rendu de l'image avec comme paramètres :
    - Résolution : 72ppp
    - dimensions : 400x240 "mm"\
[Test de rendu d'image avec une résolution de 72](print-to-scale-72.pdf)

- **Test - 2 :** Rendu de l'image avec comme paramètres :
    - Résolution : 255ppp
    - dimensions : 210x297 "mm"\
[Test de rendu d'image avec une résolution de 300](print-to-scale-300.pdf)

#### **II.2.4.2 - Conclusion**
- **Avantages :**
    - Intégration facile
    - Options de customisation
    - Basée sur OpenLayers
    - - Génération du rendu rapide pour les très hautes résolutions
- **Désavantages :**
    - Rendu pas très satisfaisant (du fait que l'image est d'abord généré en JPEG puis inséré dans le pdf)


&nbsp;
# **Sources**
[^1]: https://fr.wikipedia.org/wiki/Syst%C3%A8me_d'information_g%C3%A9ographique
[^2]: https://fr.wikipedia.org/wiki/Cartographie_en_ligne
[^3]: https://geoservices.ign.fr/documentation/services/api-et-services-ogc/donnees-vecteur-wfs-ogc
[^4]: https://geoservices.ign.fr/documentation/services/api-et-services-ogc/images-wms-ogc
[^5]: https://geoservices.ign.fr/documentation/services/api-et-services-ogc/images-tuilees-wmts-ogc
[^6]: https://geoservices.ign.fr/documentation/services/api-et-services-ogc/tuiles-vectorielles-tmswmts
[^7]: https://www.sigterritoires.fr/index.php/geoserver-avance-le-tuilage-principes/
[^8]: https://en.wikipedia.org/wiki/Protocol_Buffers
[^9]: https://wiki.openstreetmap.org/wiki/PBF_Format