# Tuto geoclimate

## Présentation
GeoClimate est une boîte à outils géospatiale open source permettant de calculer un
ensemble de paramètres liés au climat décrivant un territoire (indicateurs morphologiques
tels que le Sky View Factor, classifications urbaines telles que le Local Climate Zones, etc.).  
  
GeoClimate utilise des entrées vectorielles. Des flux de travail spécifiques ont été
développés pour utiliser automatiquement OpenStreetMap et les bases de données
françaises BD Topo v2.2.  

Il y a deux manières d’utiliser Geoclimate, soit en passant par une interface de ligne de
commande, soit en passant par une console Groovy.  
  
 Ce tuto permet d’utiliser Geoclimate sur Windows.  
 
## Pré-requis: Installation de Groovy 3.0.7 

1) Télécharger Groovy 3.0.7  
https://groovy.jfrog.io/artifactory/dist-release-local/groovy-windows-installer/groovy-3.0.7/
2) Installer Groovy 3.0.7

## Open Street Map

### Fichier de configuration 
Ici on utilise les données Open Street Map en entrée.  
  
__Attention__: au folder en output : ici “F:/GEOCLIMATE/output”, il faut créer le répertoire avant
“output” avant de faire tourner le code.   
  
Copier le script ci-dessous dans un bloc note. Et l’enregistrer en .json. Ici il se nomme
vitre_config_file_osm.json. (penser à adapter le nom de la ville et les fichiers)

```
{
"description": "Processing OSM data",
"geoclimatedb": {
"folder": "F:/GEOCLIMATE",
"name": "geoclimate_db_test;AUTO_SERVER=TRUE",
"delete": true
},
"input": {
"osm": [
"Vitré"
]
},
"output": {
"folder": "F:/GEOCLIMATE/output"
},
"parameters": {
"rsu_indicators": {
"indicatorUse": [
"LCZ",
"TEB",
"URBAN_TYPOLOGY"
],
"svfSimplified": true,
"estimateHeight": true
}
}
}
```

### Lancer la console Groovy
  
<img width="700" alt="Capture1" src="https://user-images.githubusercontent.com/85685916/161057702-1135503e-0a89-4654-8259-2c878c4e390e.PNG">
  
__Attention__ : Bien vérifier en bas à droite la version de Groovy : 3.0.7 sinon geoclimate ne
fonctionne pas.  
  
Entrer le code suivant :  
  
```
@GrabResolver(name='orbisgis', root='https://nexus.orbisgis.org/repository/orbisgis/')
@Grab(group='org.orbisgis.orbisprocess', module='geoclimate', version='1.0.0-SNAPSHOT')
import org.orbisgis.orbisprocess.geoclimate.Geoclimate
def process = Geoclimate.OSM.workflow
process.execute(configurationFile:'F:/GEOCLIMATE/vitre_config_file_osm.json')
```
  
__Attention__ : Bien entrer le chemin d’accès du fichier de configuration
  
<img width="700" alt="Capture2" src="https://user-images.githubusercontent.com/85685916/161057981-8dcf15d2-f30b-4d81-9e61-7a57406fe048.PNG">
  
### Résultats
  
<img width="700" alt="Capture3" src="https://user-images.githubusercontent.com/85685916/161058142-39f21dc4-3064-4a5f-ab05-0a2a80f4817a.PNG">  
  
Création automatique d’un fichier osm_zoneétude où sont les résultats.  
  
<img width="700" alt="Capture4" src="https://user-images.githubusercontent.com/85685916/161058503-b8e766d3-54e6-41d4-bf70-39f6440267a8.PNG">
  
Ci-dessus, les couches en sortie de geoclimate. On peut ensuite les mettre sous QGIS. Ici, le fichier rsu_lcz.geojson.
  
<img width="700" alt="Capture5" src="https://user-images.githubusercontent.com/85685916/161058723-ad806d72-8679-4a90-aca1-9750b93e8eee.PNG">
  
Pour la typologie voir : https://github.com/orbisgis/geoclimate/wiki/LCZ-classification

## BD TOPO
  
Pour utiliser geoclimate avec la BD TOPO il est nécessaire de sélectionner certains fichiers
de celle-ci.  
  
lien de téléchargement de la bd topo v2-2 : http://files.opendatarchives.fr/professionnels.ign.fr/bdtopo/  
liste des fichiers nécessaires : https://github.com/orbisgis/geoclimate/tree/master/bdtopo_v2/src/test/resources/org/orbisgis/geoclimate/bdtopo_v2/sample_12174  

La BD TOPO sur l’Ille-et-Vilaine, est dispo dans mes fichiers (BD_TOPO_35.zip). Le choix de la zone d’intéret se fait dans le fichier de configuration.

## Fichier de configuration  
  
Copier le script ci-dessous dans un bloc note. Et l’enregistrer en .json. Ici il se nomme
rennes_config_file_bd_topo_v2.json :  

```
{
"description": "Processing BD Topo v2 data",
"input": {"bdtopo_v2": {
"folder": {
"path": "C:/Geoclimate/BD_TOPO_35",
"id_zones": ["35238"]
}
}
},
"output": {
"folder": "C:/Geoclimate/output"
},
"parameters": {
"rsu_indicators": {
"indicatorUse": [
"LCZ",
"TEB",
"URBAN_TYPOLOGY"
],
"svfSimplified": true
},
"grid_indicators": {
"x_size": 100,
"y_size": 100,
"rowCol": false,
"output" : "geojson",
"indicators" :[
"BUILDING_FRACTION",
"BUILDING_HEIGHT",
"WATER_FRACTION",
"VEGETATION_FRACTION",
"ROAD_FRACTION",
"IMPERVIOUS_FRACTION",
"LCZ_FRACTION"
]
}
}
}
```
   
__"C:/Geoclimate/BD_TOPO_35"__ : chemin d’accès de la BD_TOPO_35  
__"id_zones": ["35238"]__ : code INSEE de Rennes (possible de mettre plusieurs codes INSEE
ou de faire une bounding box)  
__C:/Geoclimate/output__ : chemin d’accès où s’enregistre les fichiers de sortie
  
### Lancer la console Groovy 
  
Entrer le code suivant :   
   
```
@GrabResolver(name='orbisgis', root='https://nexus.orbisgis.org/repository/orbisgis/')
@Grab(group='org.orbisgis.geoclimate', module='geoclimate', version='1.0.0-SNAPSHOT')
import org.orbisgis.geoclimate.Geoclimate
def process = Geoclimate.BDTopo_V2.workflow
process.execute(configurationFile:'F:/GEOCLIMATE/GEOCLIMATE_10_2021/rennes/BD_T
OPO_V2/rennes_config_file_bd_topo_v2.json')
```

__'F:/GEOCLIMATE/GEOCLIMATE_10_2021/rennes/BD_TOPO_V2/rennes_config_file_bd_topo_v2.json'__ : chemin d’accès au fichier de configuration
  
<img width="700" alt="Capture6" src="https://user-images.githubusercontent.com/85685916/161060677-8954857c-d977-450c-8022-9d0f0e244ddc.PNG">
  
### Résultats
  
<img width="700" alt="Capture7" src="https://user-images.githubusercontent.com/85685916/161060794-67ca42f3-b234-47af-a9ee-754a2ca7c96e.PNG">
  
## Comparaison des résultats avec en input Open Street Map et la BD TOPO
  
<img width="699" alt="Capture8" src="https://user-images.githubusercontent.com/85685916/161061002-08cac15f-0c43-46c0-931e-7ed8cb7b7524.PNG">
  
<img width="700" alt="Capture9" src="https://user-images.githubusercontent.com/85685916/161061069-4cdb1256-e392-4a0e-8134-9e4c76b970a2.PNG">
  
## Crédits 
  
Geoclimate : https://github.com/orbisgis/geoclimate  












