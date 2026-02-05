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
l'utilisation d'OAI-PMH ne donnait qu'environ $400$ résultats, alors que (la
version en ligne de Papyrus)
[https://umontreal.scholaris.ca/collections/4aa0be61-e42e-428a-a844-623d48b3fd4c/search]
montre $1036$ documents correspondant.

Devant ces résultats, nous avons décidé d'intérogger directement le site web 
de Papyrus, d'en extraire les pages HTML correspondant aux thèses et mémoires,
et de structurer les metadonnées pertinentes dans un fichier JSON.

L'inconvénient avec cette stratégie est qu'il faut itérer sur tous les documents
disponibles sur le site web, puis de déterminer s'il s'agit d'une thèse ou d'un 
mémoire, et enfin d'extraire les métadonnées pertinentes. Les hyperliens des 
documents ont cette forme: `https://papyrus.bib.umontreal.ca/xmlui/handle/1866/{index}?show=full`,
où `{index}` est le numéro du document dans la base de données. L'algorithme utilisé pour 
l'extraction des metadonnées itère donc au travers de tous les documents disponibles, 
passant de l'index maximal au moment de la collecte, jusqu'à $0$. À notre 
connaissance, il n'existe pas de manière explicite de trouver l'index maximal; nous
utilisons donc une recherche manuelle approximative, suivie d'une heuristique afin
d'avoir un index maximal approximé.

Donc, jusqu'ici, le script pour extraire les données est complèt. Il suffit 
maintenant de l'exécuter afind d'extraire toutes les données.