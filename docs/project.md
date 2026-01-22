---
layout: default
title: "Description du projet"
permalink: /project/
classes: full-width
---
<nav>
  <a href="{{ '/index.html' | relative_url }}">Accueil</a> |
  <a href="{{ '/project/' | relative_url }}">Description du projet</a>
</nav>

## Contexte
La diffusion, la valorisation et la découvrabilité de contenu scientifique
reposent aujourd'hui sur des bases de données bibliographiques, des moteurs de
recherche et des archives numériques. Avec ces infrastructures, les métadonnées
scientifiques sont de l'or pour propulser l'indexation et l'accès aux
publications.

Par ailleurs, le développement de la science s'inscrit dans un contexte
multilingue. Les résumés traduits dans des langues autres que la langue
principale de rédaction constituent souvent le point d'entrée principal
pour les lecteurs non natifs, facilitant ainsi la diffusion et l'appropriation
des connaissances. Des métadonnées multilingues sont donc essentielles à la
découvrabilité des contenus scientifiques, mais aussi au développement de la
traduction automatique, la recherche d'information multilingue et la génération
automatique de mots-clés. 

Ainsi, la qualité, la structure et l'alignement des métadonnées scientifiques
représentent un enjeu important pour la diffusion des connaissances et pour le
développement de méthodes automatiques adaptées au langage scientifique.


## Problématique
La traduction automatique pour les articles scientifiques constitue un levier
important pour rendre la recherche accessible à des communautés non locutrices
de la langue de rédaction originale. Cependant, cette
discipline repose sur la disponibilité de données multilingues alignées, en
particulier dans des domaines spécialisés.

Les travaux effectués dans le cadre du projet MaTOS
(Bawden et al., 2025), 
illustre les efforts pour développer des ressources, des méthodes et des
métriques librement disponibles pour la traduction automatique
spécialisée. Dans ce contexte, Peng et al. (2024) ont créé un corpus composés de
thèses dans le domaine du traitement automatique des langues, utilisant le
moteur de recherche [these.fr](these.fr), qui « recense l'ensemble des thèses de
doctorat soutenues en France depuis 1985 »
(The, 2026).

Dans le système universitaire québécois, où les thèses et mémoires sont
généralement écrites en français,
une grande proportion de ces publications disposent également de résumés en
anglais, ce qui constitue une excellente source de données pour la traduction
automatique. Par ailleurs, Piedboeuf et Langlais (2022) ont
exploité 
la plateforme de dépôt institutionnel de l'Université de Montréal
[Papyrus](https://umontreal.scholaris.ca/home) afin d'assembler un
jeu de données pour la génération automatique de mots-clés. Étant donné que ces
travaux furent réalisés en 2022, et considérant les efforts continus de
l'équipe de Papyrus pour numériser et enrichir les documents scientifiques
antérieurs à la plateforme, il apparaît pertinent d'exploiter cette ressource
afin de prolonger les initiatives du projet MaTOS dans le contexte des
publications universitaires québécoises.

## Proposition et objectifs
Ce projet vise à répondre à la question suivante : dans quelle mesure les
métadonnées multilingues disponibles sur Papyrus sont-elles suffisamment
complètes et alignées pour être utilisées comme ressource d'entraînement en
traduction automatique spécialisée?

Plus précisément, ceci portera sur la création d'un jeu de données propre et
structuré 
contenant des métadonnées de mémoires et de thèses universitaires disponibles
sur la plateforme Papyrus. Ce travail vise deux objectifs principaux :
- Initier l'effort québécois de création de jeux de données spécialisé pour
  la traduction automatique, dans la continuité des travaux du projet MaTOS;
- Dresser un portrait de l'état et de la qualité des métadonnés disponibles sur la plateforme Papyrus.

Ce jeu de données contiendra des résumés alignés en plusieurs langues, les
mots-clés associés, ainsi que des informations bibliographiques pertinentes
telles que les dates de publication, la langue de rédaction et les noms des
auteurs. Compte tenu de la structure actuelle des métadonnées disponibles sur
Papyrus, nous devrons effectuer de la reconnaissance automatique de la langue
afin de bien classifier les éléments. Pour ce faire, nous nous baserons sur la
méthodologie proposée par Piedboeuf et Langlais (2022).

Dans sa finalité, ce jeu de données pourra également être exploité pour la
génération de mots-clés, 
ainsi que pour d'autres tâches de traitement automatique du langage. Par
exemple, il pourrait servir de banc d'essai pour des outils d'extraction
d'information à partir de documents au format PDF. Toutefois, ces applications
secondaires se situent en dehors de la portée du présent projet.

## Planification
- **Semaines 4-5** : Extraction des données et conception de la
  structure du jeu de données;
- **Semaines 6-8** : Nettoyage, normalisation, alignement et détection
  de la langue; 
- **Semaines 9-10** : Analyse de la qualité des métadonnées (données
  manquantes, différences entre le français et l'anglais, couverture des
  sujets, ...); 
- **Semaine 11** : Documentation et finalisation du jeu de données;
- **Semaines 12-13** : Écriture et révision. Préparation de la
  présentation.



## Références
- 2026\. theses.fr, Le moteur de recherche des thèses françaises.

- Rachel Bawden, Maud Bénard, Éric de la Clergerie, José Cornejo Cárcamo, Nicolas Dahan, Manon Delorme,
Mathilde Huguin, Natalie Kübler, Paul Lerner, Alexandra Mestivier, Joachim Minder, Jean-François
Nominé, Ziqian Peng, Laurent Romary, Panagiotis Tsolakis, Lichao Zhu, and François Yvon. 2025.
MaTOS: Machine Translation for Open Science. In Proceedings of Machine Translation Summit XX:
Volume 2, pages 103–104, Geneva, Switzerland. European Association for Machine Translation.

- Ziqian Peng, Rachel Bawden, and François Yvon. 2024. à propos des difficultés de traduire automatiquement
de longs documents. In 35èmes Journées d’Études Sur La Parole (JEP 2024), volume 1 : articles longs et
prises de position, pages 2–21, Toulouse, France. ATALA & AFPC.

- Frédéric Piedboeuf and Philippe Langlais. 2022. A new dataset for multilingual keyphrase generation. In
Thirty-Sixth Conference on Neural Information Processing Systems Datasets and Benchmarks Track. 