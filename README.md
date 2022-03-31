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
  





















