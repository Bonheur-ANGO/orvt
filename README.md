ORVT

## Introduction

### 1 - Données géographiques : vecteur et raster
On appelle donnée géographique une donnée contenant une référence à un lieu. Il existe deux manières de représenter les données géographiques de manière numérique à savoir: le mode vecteur et le mode raster.

#### 1.1 Mode vecteur
Les données géographiques en mode vecteur permettent de modéliser le monde réel à travers des objets. Un objet est caractérisé par deux éléments : les informations qui lui sont associées ou données attributaires et sa forme ou sa géométrie. La géométrie est constituée d’un ou plusieurs points interconnectés. Un point est une position dans l’espace. Il existe trois principaux types de géométrie : point, ligne et polygone. Si un objet dispose d’un seul point, alors sa forme est un point. Si un objet dispose de plusieurs points qui ne forment pas de forme géométrique fermée, alors sa forme est une ligne. Si un objet dispose de plusieurs points interconnectés formant une forme géométrique close, alors sa forme est un polygone.

![Les différentes formes de vecteurs](schema_vecteur_formes.png)

Les objets peuvent être par exemple des maisons, des routes ou arbres. Ils sont stockés 
dans des bases de données avec leurs coordonnées spatiales et leurs données attributaires, et sont organisés en couches d'information représentant une classe d'objets. Une couche comporte un ou plusieurs objets de la même classe et du même type graphique (point, ligne ou surface).

![table attributaire](table_attributaire.png)


#### 1.1 Mode raster
Les données géographiques en mode raster sont des images matricielles constitués de plusieurs pixels organisés sous forme d'une grille en ligne et colonne. Un pixel est l'unité de base de la définition d'une image numérique matricielle. A chaque pixel est associé une ou plusieurs valeurs numériques décrivant les caractéristiques de l'espace.