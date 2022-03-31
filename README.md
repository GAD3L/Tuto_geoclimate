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
_Attention_: au folder en output : ici “F:/GEOCLIMATE/output”, il faut créer le répertoire avant
“output” avant de faire tourner le code.   
Copier le script ci-dessous dans un bloc note. Et l’enregistrer en .json. Ici il se nomme
vitre_config_file_osm.json. (en gras: à changer en fonction de la ville d’étude et fichier de
sortie)  

''
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
''

