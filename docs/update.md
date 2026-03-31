---
layout: default
title: "Rapport d'avancement"
permalink: /update/
classes: full-width
---
<nav>
  <a href="{{ '/index.html' | relative_url }}">Accueil</a> |
  <a href="{{ '/project/' | relative_url }}">Description du projet</a> |
  <a href="{{ '/update/' | relative_url }}">Rapport d'avancement</a>
</nav>

# Rapport d'avancement

## 25 mars 2026 - Analyse des données
Un total de 33,880 documents furent extraits le 14 février 2026. 

Étant donné la performance moyenne de l'annotation des languges, nous avons
décider d'utiliser le modèle [OpenLID-v2](https://huggingface.co/laurievb/OpenLID-v2), capable d'identifier 200 variétés de langues. Aussi, devant la 
difficulté d'annoter les mots-clés étant donné leur petite taille, 
ceux-ci ne sont plus annotés pour la langue.

L'exploration des données se trouve <a href="{{ '/data_exploration/' | relative_url }}">à cette page-ci</a>

## 11 mars 2026 - Détection de langues
Puisque les données extraites de Papyrus ne mentionnent pas la langue de
certains champs textuel, nous devons effectuer de la détection de langues 
afin de les identifier, rendant ce jeu de données utile à la traduction 
automatique. Cette tâche consiste donc à annoter les titres, les résumés ainsi
que les mots-clés, avec leur langue correspondante.

Pour les résumés, nous utilisons [langdetect](https://github.com/Mimino666/langdetect)
(v1.0.9). Cet outil utilise un modèle probabiliste baysien pour classifier des
chaînes de caractères. Malheureusement, pour de courtes chaînes, cet outil 
semble avoir de [moins bonnes performances](https://github.com/Mimino666/langdetect/issues/110).
Étant donné que les mots-clés et les titres ne contiennent que peu de 
charactères, nous utilisons [lingua-py](https://github.com/pemistahl/lingua-py) 
(v2.2.0). Cet outil fut crée afin de pallier au manque d'outils de
détection de la langue pour les chaînes de charactères relativement petites.
De plus, cet outil priorise `None` lorsqu'il n'est pas certain de la langue.


## 28 février 2026 - Extraction des données
Les données sont extraites. Étant donné quelques problèmes de réseaux, ceci a 
pris plusieurs essais avant de fonctionner, ainsi que 13h d'extraction pour 
récupérer le jeu de données final.


## 5 février 2026 - Complétion du script pour l'extraction des données
Les données de [Papyrus](https://umontreal.scholaris.ca/home) sont disponibles
selon deux approches principales:
- La première exploite des liens dynamiques du moteur de recherche interne à 
Papyrus. Par exemple : `https://umontreal.scholaris.ca/search?query="code+civil`.
Cette méthodes permet de faire des recherches avancées et rapides dans la base
de données, mais n'est pas idéale pour le moissonnage.
- La seconde exploite le protocole Open Access Initiative - Protocol for 
Metadata Harvesting (OAI-PMH). Ceci est conçu pour la récolte de métadonnée. 

Après un test rapide sur les thèses et mémoires du département de droit, 
l'utilisation d'OAI-PMH ne donnait qu'environ 400 résultats, alors que 
[la version en ligne de Papyrus](https://umontreal.scholaris.ca/collections/4aa0be61-e42e-428a-a844-623d48b3fd4c/search)
montre 1,036 documents correspondant.

Devant ces résultats, nous avons décidé d'intérogger directement le site web 
de Papyrus, d'en extraire les pages HTML correspondant aux thèses et mémoires,
et de structurer les metadonnées pertinentes dans un fichier JSON.

L'inconvénient avec cette stratégie est qu'il faut itérer sur tous les documents
disponibles sur le site web, puis de déterminer s'il s'agit d'une thèse ou d'un 
mémoire, et enfin d'extraire les métadonnées pertinentes. Les hyperliens des 
documents ont cette forme: `https://papyrus.bib.umontreal.ca/xmlui/handle/1866/{index}?show=full`,
où `{index}` est le numéro du document dans la base de données. L'algorithme utilisé pour 
l'extraction des metadonnées itère donc au travers de tous les documents disponibles, 
passant de l'index maximal au moment de la collecte, jusqu'à 0. À notre 
connaissance, il n'existe pas de manière explicite de trouver l'index maximal; nous
utilisons donc une recherche manuelle approximative, suivie d'une heuristique afin
d'avoir un index maximal approximé.

Donc, jusqu'ici, le script pour extraire les données est complèt. Il suffit 
maintenant de l'exécuter afin d'extraire toutes les données.