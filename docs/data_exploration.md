---
layout: default
title: "Exploration des données"
permalink: /data_exploration/
classes: full-width
---
<nav>
  <a href="{{ '/index.html' | relative_url }}">Accueil</a> |
  <a href="{{ '/project/' | relative_url }}">Description du projet</a> |
  <a href="{{ '/update/' | relative_url }}">Rapport d'avancement</a>
</nav>


### Données manquantes
Le tableau suivant résume les éléments manquants pour chaque attribut extrait.

| Attribut | Nombre de champs manquants |
| -------- | -------------------------- |
| title            |     0 |
| author           |     0 |
| advisor          |   238 |
| accesionned      |     0 |
| available        |  1192 |
| issued           |     0 |
| abstract         | 15980 |
| orcid            | 31462 |
| handle           |     0 |
| doi              |    10 |
| uri              | 33818 |
| keyword          |   313 |
| doc_type         |     1 |
| doc_language     |  2024 |
| discipline       |     2 |
| degree_grantor   |     9 |
| degree_level     |     0 |
| degree_name      |     2 |

Le dépôt systématique des mémoires et 
thèses de l'Université de Montréal sur Papyrus commença en [2009](https://boite-outils.bib.umontreal.ca/trouver-evaluer/depot-institutionnel-papyrus).
Les documents précédent cette année sont ajoutés manuellement par l'équipe de Papyrus; 
les documents imprimés doivent être numérisés. 

### Exploration des données

#### `title`
Le champs `title` contient le titre du document. La langue de cet attribut 
fut annotée grâce à lingua-py. Tous les documents ont un et un seul 
titre.

La figure suivante montre la distribution des langues des titres. 

<p align="center">
  <img src="{{ '/assets/images/title_lang.png' | relative_url }}" style="width:60%;">
</p>

Nous voyons que la grande majorité des titres sont en français.

La figure
suivante montre la distribution des 10 disciplines les plus fréquentes dans
toutes les données selon la langue des titres. Nous voyons, par exemple, que l'antropologie publie dans beaucoup de langues.


<p align="center">
  <img src="{{ '/assets/images/disc_title_lang.png' | relative_url }}" style="width:60%;">
</p>


#### `author`
Tous les documents ont au moins un auteur d'associé. En fait, un seul document contient 
plus d'un auteur: [La résistance des Montagnais à l'usurpation des rivières à saumon par les euro-canadiens du XVIIe au XXe siècle, par Anne-Marie Panasuk et Jean-René Proulx (1981)](https://hdl.handle.net/1866/42292).

Le tableau suivant montre les 10 noms d'auteurs ayant le plus de publications, et les 
disciplines associées. Un même nom peut correspondre à plusieurs personnes différentes.

| Auteur              |   Publications | Disciplines                                                                                              |
|:--------------------|---------------:|:---------------------------------------------------------------------------------------------------------|
| Gagnon, François    |              6 | Criminologie, Histoire, Microbiologie et immunologie, Sciences de la communication, Sciences économiques |
| Cloutier, Geneviève |              6 | Littérature, Littérature comparée, Relations industrielles, Service social, Urbanisme                    |
| Martin, Isabelle    |              5 | Anthropologie, Droit, Psychologie, Théologie pratique                                                    |
| Bélanger, Mathieu   |              5 | Philosophie, Science politique, Sciences vétérinaires, Urbanisme                                         |
| Côté, Catherine     |              5 | Criminologie, Psychologie, Science politique, Sciences biomédicales                                      |
| Gauthier, Nicolas   |              5 | Biochimie, Littératures de langue française, Physique, Études françaises                                 |
| Roy, Mathieu        |              5 | Anthropologie, Géographie, Psychologie, Santé publique                                                   |
| Roy, Julie          |              5 | Criminologie, Nutrition, Sciences biomédicales, Sciences pharmaceutiques, Études cinématographiques      |
| Tremblay, Annie     |              5 | Biologie moléculaire, Criminologie, Science politique, Sciences pharmaceutiques                          |
| Bergeron, Luc       |              4 | Administration de l'éducation, Sciences pharmaceutiques, Sciences vétérinaires, Théologie                |


#### `advisor`
L'attribut `advisor` nomme les directions de recherche pour une 
publication donnée.

La Figure suivante montre la distribution des 238 documents sans direction de recherche.

<p align="center">
  <img src="{{ '/assets/images/missing_advisor_year.png' | relative_url }}" style="width:60%;">
</p>

Les documents sans direction de recherche se trouvent principalement avant 
2009, avec une occurrence en 2010, et une autre en 2019.

La figure suivante montre la distribution du nombre de direction de 
recherche par document. Nous voyons que, bien que 1 ou 2 directions de 
recherche sont les plus populaires, il existe plusieurs documents avec 3 directions ou plus.
<p align="center">
  <img src="{{ '/assets/images/distrib_advisors.png' | relative_url }}" style="width:60%;">
</p>

En allant plus loin, la Figure suivante montre la distribution 
disciplines ayant des publications avec 3 ou plus directions de 
recherche. Nous voyons que ce sont les sciences vétérinaires qui 
possèdent le plus de publications avec 3 ou plus directions de recherche,
mais que ce phénomène est aussi présent dans plusieurs départements.
<p align="center">
  <img src="{{ '/assets/images/disci_direct.png' | relative_url }}" style="width:80%;">
</p>

Il est intéressant de mentionner que le [formulaire du DIRO](https://diro.umontreal.ca/programmes-cours/cycles-superieurs/maitrise-en-informatique/formulaires-pour-la-maitrise/) 
pour la désignation d'un directeur et co-directeur de recherche ne 
permet qu'un directeur et un co-directeur; ce formulaire est le même 
depuis 2022. Or, nous voyons avec la Figure suivante que le DIRO accepta
quelques publications avec trois ou plus directeurs de recherche depuis 
2022, bien que cela soit rare. Nous voyons aussi que ce phénomène existe 
depuis plusieurs années.

<p align="center">
  <img src="{{ '/assets/images/info_advisors.png' | relative_url }}" style="width:60%;">
</p>

#### `accesionned`
L'attribut `accesionned` montre la date de soumission du document dans la base
de données de Papyrus, Scholaris; tous les documents ont donc nécessairement une valeur
dans ce champs.

<p align="center">
  <img src="{{ '/assets/images/access_per_year.png' | relative_url }}" style="width:60%;">
</p>

Nous voyons que la quantité de documents soumis sur Papyrus augmenta 
nettement après 2009, lorsque le dépôt systématique des thèses et mémoires 
sur cette plateforme fut instaurée.

#### `available`
L'attribut `available` indique la data à laquelle le document devient 
disponible publiquement. Une date manquante signifie simplement que le 
document n'était pas encore publique au moment de l'extraction des données.

<p align="center">
  <img src="{{ '/assets/images/avaialble_per_year.png' | relative_url }}" style="width:60%;">
</p>


#### `issued`
L'attribut `issued` spécifie la date de publication officielle, correspondant à  
la date de dépôt final du mémoire ou de la thèse.

<p align="center">
  <img src="{{ '/assets/images/nb_doc_per_year.png' | relative_url }}" style="width:60%;">
</p>

Avec le graphique précédant, nous voyons que le nombre de publications explosa au milieu 
des années 1980. Cependant, il est intéressant de noter que ce nombre reste plutôt
stable depuis les 20 dernières années.

#### `abstract`
L'attribut `abstract` donne le ou les résumés fournis dans les métadonnées
du document. Nous utilisons langdetect afin d'annoter la langue des résumés, 
puisque cette information n'est pas disponible directement avec les données
de Papyrus.

##### Données manquantes
La Figure suivant montre la distribution des 15,980 documents sans résumé.

<p align="center">
  <img src="{{ '/assets/images/missing_abstracts_year.png' | relative_url }}" style="width:60%;">
</p>

Les champs `absract` manquants se retrouvent tous avant 2009, 
inclusivement.

##### Résumés multiples
Les Figures suivantes montrent, à gauche, la distribution des documents avec
plusieurs résumés au travers des années et, à droite, la distribution du nombre
de résumés par document. Nous voyons que la montée en importance d'avoir plusieurs
résumés correspond à la monté en popularité du Web et de l'interconnectivité. 
Nous voyons aussi que la très grande majorité des publications ont exactement
deux résumés, mais certains peuvent en avoir jusqu'à six.
<p align="center">
  <img src="{{ '/assets/images/abs_year.png' | relative_url }}" style="width:48%;">
  <img src="{{ '/assets/images/nb_abs.png' | relative_url }}" style="width:48%;">
</p>

La Figure suivante montre la distribution des langues utilisées dans les résumés.
Nous voyons que ce sont le français et l'anglais qui sont utilisés en très grande
majorité.
<p align="center">
  <img src="{{ '/assets/images/distrib_langue_abstract.png' | relative_url }}" style="width:60%;">
</p>

La Figure suivante montre les combinaisons de langues les plus populaires pour les résumés.
Nous voyons que la combinaison (anglais, français) est la plus populaire. Nous remarquons
aussi les 15 combinaisons (anglais, anglais) et les 7 (français, français).
<p align="center">
  <img src="{{ '/assets/images/combi_lang_abs.png' | relative_url }}" style="width:85%;">
</p>

La Table suivante montre les résumés des documents ayant des combinaisons de langues (anglais, 
anglais), ainsi que la similarité entre les deux résumés (utilisant l'algorithme de Gestalt grâce à
la librairie [difflib.SequenceMatcher](https://docs.python.org/3/library/difflib.html)). Nous 
remarquons que les résumés sont presque toujours identiques, sauf à une occasion, où l'auteur insèra 
deux résumés différents (mais relativement similaires) en anglais.

|   doc_id |   issued_year | abstract_1                        | abstract_2                        |   similarity |
|---------:|--------------:|:----------------------------------|:----------------------------------|-------------:|
|     2481 |          2025 | Preserving and restoring peopl... | Preserving and restoring peopl... |         1    |
|     2483 |          2024 | This study addresses the criti... | This study addresses the criti... |         1    |
|     2518 |          2025 | The market for private seniors... | The market for private seniors... |         1    |
|     2521 |          2025 | Vascular dysfunction and patho... | Vascular dysfunction and patho... |         1    |
|     2523 |          2025 | Introduction: Aging is the mai... | Introduction: Aging is the mai... |         1    |
|     2525 |          2025 | CEGEP teachers are one of a ki... | CEGEP teachers are one of a ki... |         1    |
|     2529 |          2025 | Despite the success of cancer ... | Despite the success of cancer ... |         1    |
|     2595 |          2025 | Acute hypoxemic respiratory fa... | Acute hypoxemic respiratory fa... |         1    |
|     2598 |          2024 | In eukaryotes, chromosomes are... | In eukaryotes, chromosomes are... |         1    |
|     2600 |          2025 | This study is based on Sébasti... | This study is based on Sébasti... |         1    |
|     2601 |          2024 | Studies have shown that swimmi... | Studies have shown that swimmi... |         1    |
|     2603 |          2024 | For over a decade now, cell ph... | For over a decade now, cell ph... |         1    |
|     2604 |          2025 | The diverse shapes and archite... | The diverse shapes and archite... |         1    |
|     2929 |          2024 | As machine learning models gro... | As machine learning models con... |         0.79 |
|    14830 |          2021 | Introduction: The adequate man... | Introduction: The adequate man... |         1    |


La Table suivante montre les résumés utilisant la combinaison (français, français). Encore une fois,
ces résumés sont très similaires. Pour 9364, la différence entre les deux résumés se résume à la 
présence de caractères d'échappements "\r\n" dans le second résumé, non présents dans le premier.
Pour 30365, les deux résumés présentent de plus grande différences. Dans ce cas, la différence 
pourrait être causée à la numérisation du document; étant donné que cette thèse fut déposée en 1998
en format papier. Dans les autres cas, les erreurs peuvent être attribuées aux utilisateurs ajoutant
les documents dans la base de données de Papyrus.

|   doc_id |   issued_year | abstract_1                        | abstract_2                        |   similarity |
|---------:|--------------:|:----------------------------------|:----------------------------------|-------------:|
|     9364 |          2024 | Ce projet de recherche consist... | Ce projet de recherche consist... |        0.987 |
|     9486 |          2023 | Ce mémoire étudie le problème ... | Ce mémoire étudie le problème ... |        1     |
|    16366 |          2020 | Les ravins de thermo-érosion c... | Les ravins de thermo-érosion c... |        1     |
|    26776 |          2013 | L’acidose lactique du Saguenay... | L’acidose lactique du Saguenay... |        1     |
|    27400 |          2013 | Cette thèse cible l’étude d’un... | Cette thèse cible l’étude d’un... |        1     |
|    30365 |          1998 | Malgré la diversité grandissan... | Malgré la diversité grandissan... |        0.701 |
|    33634 |          2002 | Ce mémoire s'intéresse à l'eff... | Ce mémoire s'intéresse à l'eff... |        1     |


##### Exploration du découpage des résumés
Ce jeu de données assume que les utilisateurs déposant le document dans la 
base de données de Papyrus inscrivent les résumés multiples dans des champs 
différents. Cette section vise à vérifier s'il y a des erreurs quant au 
découpage de résumés, où des résumés différents seraient ajoutés dans le même 
champs. La Figure suivant montre la distribution du nombre de mots dans les 
résumés en anglais et en français.
<p align="center">
  <img src="{{ '/assets/images/avg_len_abs.png' | relative_url }}" style="width:60%;">
</p>

La moyenne du nombre de mots pour les résumés en anglais est de 315.1 mots, et, en français, de 357.4 mots. La différence entre la moyenne anglaise et la moyenne française correspond à une augmentation
de 13.41% pour le français, ce qui est attendu dans des textes parallèles entre ces deux langues.

Pour détecter ceci, s'il y a des erreurs dans le découpage des résumés par 
les utilisateurs, nous 
utilisons une heuristique simple. Nous découpons tous les résumés en tiers.
Ensuite, nous utilisons OpenLID-v2 indépendamment sur chaque tier afin de 
déterminer leur langue. Enfin, nous comparons la langue du résumé entier 
et les langues des tiers. Les exemples contenant au moins une différence 
sont vérifiés manuellement. La majorité du temps, les différences sont
causées par la citation du titre d'une oeuvre dans une langue différente que 
celle du résumé. 

Par contre, d'autres différences sont belles et bien des erreurs d'utilisateurs.
- [Ce document](http://hdl.handle.net/1866/22256) a entré deux fois le résumé 
anglais; une fois seul, et une fois avec le résumé français.
- [Ce document](http://hdl.handle.net/1866/10648) a mis les mot-clés dans un 
champs `abstract`.

35 résumés n'ont pas de langue annotée. Dans ces 35 résumés, seulement cinq 
ont une différence dans le *test des tiers*. L'absence de ces annotations 
de langue sont causés soit par un taux de confiance trop bas de la part de
OpenLID-v2. 

- TODO: vérifier les abstract très long

#### `orcid`
ORCID, organisisme créé en 2012, permet de donner un identifiant à une
personne faisant de la recherche. Dans les données extraites, 31,462 
documents n'ont pas d'ORCID associé; 2,418 en ont un, principalement à 
partir de 2016, la Figure suivante montre ce phénomène.
<p align="center">
  <img src="{{ '/assets/images/orcid_per_year.png' | relative_url }}" style="width:60%;">
</p>

La Figure suivante montre les 20 disciplines où les identifiants 
ORCID sont le plus utilisés. Nous voyons que, bien que les sciences 
biomédicales possèdent le plus grand nombre d'ORCID, cet identifiant est
utilisé dans beaucoup de disciplines, autant en sciences humaines qu'en 
sciences pures.

<p align="center">
  <img src="{{ '/assets/images/disciplines_orcid.png' | relative_url }}" style="width:60%;">
</p>

#### `handle`
L'attribut `handle` correspond au lien Scholaris, la base de données de
Papyrus. Tous les documents ont donc nécessairement en élément dans 
ce champs.

#### `doi`
L'attribut `doi` correspond au *Digital Object Identifier* de ce 
document. Seulement 10 documents n'ont pas de DOI; voici leur 
répartition selon les années:

| Year | Missing DOIs |
|------|-------------|
| 1985 | 1 |
| 1987 | 1 |
| 1989 | 1 |
| 1992 | 1 |
| 1994 | 1 |
| 1999 | 1 |
| 2000 | 1 |
| 2010 | 1 |
| 2017 | 1 |
| 2018 | 1 |

Voici la distribution des DOIs manquants selon la discipline:

| Discipline | Missing DOIs |
|------------|-------------|
| Criminologie | 1 |
| Psychologie clinique | 1 |
| Sciences de l'activité physique | 1 |
| Sciences de l'information | 1 |
| Théologie | 4 |

#### `uri`
Le champs URI est un champs optionnel pour les publications de thèses et 
mémoires. 33,818 documents n'ont pas d'URI; 62 en ont. Ils sont
utilisés pour ajouter des documents externes reliés à une publication.

La Figure suivante montre la distribution de l'utilisation des URIs
par année. Cet attribut était plus populaire avant 2006.
<p align="center">
  <img src="{{ '/assets/images/uri_per_year.png' | relative_url }}" style="width:60%;">
</p>

Afin d'explorer cette coupure, la Figure suivante montre la 
répartition des disciplines utilisant des URIs avant 2006. Cette
distribution est bien répartie entre plusieurs disciplines différentes.
<p align="center">
  <img src="{{ '/assets/images/disciplines_uri_b4_2006.png' | relative_url }}" style="width:60%;">
</p>

Nous pouvons comparer ces résultats avec les disciplines utilisant
des URIs après 2006 :
<p align="center">
  <img src="{{ '/assets/images/disciplines_uri_after_2006.png' | relative_url }}" style="width:60%;">
</p>
Après 2006, seulement le département de musique utilise encore
des URIs. Nous pourrions expliquer ceci avec la montée du web. Par 
exemple, Git fut crée en 2006. C'est aussi en [2008](https://www.iso.org/standard/51502.html) que le format PDF devint le standard pour la publication de documents, permettant de facilement joindre et suivre
un lien à même un document. Ceci pourrait expliquer la diminution de 
l'utilisation des URIs dans les publications universitaires.

Une grande proportion des liens URI testés manuellement ne 
sont plus disponible.


#### `keyword`
La Figure suivante montre la distribution des 313 documents sans mots-clés au 
fil des années.

<p align="center">
  <img src="{{ '/assets/images/missing_kw_year.png' | relative_url }}" style="width:60%;">
</p>

Les documents sans mots-clés furent donc publiés entre 1971 et 2009 inclusivement, précédant le dépôt systématique dans Papyrus.


La Figure suivante présente le nombre moyen de mots-clés par document selon les années.
Depuis 2008, le nombre moyen de mots-clés tourne autour de 13 par document.

<p align="center">
  <img src="{{ '/assets/images/moy_kw_year.png' | relative_url }}" style="width:60%;">
</p>

Nous pouvons aussi visualiser la distribution du nombre de mots-clés par 
document.

<p align="center">
  <img src="{{ '/assets/images/distrib_kw.png' | relative_url }}" style="width:60%;">
</p>

Le [document](http://hdl.handle.net/1866/25398) contenant 119 mots-clés 
provient de 2020;
les mots-clés furent donc entrés par l'auteur. La même chose est vraie pour 
le [document](http://hdl.handle.net/1866/22570) avec 71 mots-clés provenant 
de 2014. Le [document](http://hdl.handle.net/1866/4362) avec 79 mots-clés 
provient quant à lui de 2007.

Les mots-clés présents dans le PDF de la publications ne sont pas toujours 
les mêmes que ceux disponibles dans les métadonnées; certains auteurs 
semblent en ajouter lors du dépôt dans Papyrus.

<p align="center">
  <img src="{{ '/assets/images/no_kw_year.png' | relative_url }}" style="width:60%;">
</p>

Nous voyons avec la Figure précédente que tous les documents n'ayant pas de 
mots-clés dans 
les métadonnées proviennent de 2009 ou avant. Nous avons échantillonés 
aléatoirement
5 publications sans mots-clés ([1984](http://hdl.handle.net/1866/1294), 
[1991](http://hdl.handle.net/1866/1574), [1993](http://hdl.handle.net/1866/1585), 
[2003](http://hdl.handle.net/1866/7825) et [2007](http://hdl.handle.net/1866/8197)), 
et aucun de ces documents ne possède de mots-clés dans le PDF de la 
publication.

La Table suivante montre les 20 mots-clés les plus utilisés:

| Mot-clé | Nombre d'occurences |
|:--------|--------------------:|
|Psychologie| 575|
|Québec| 533|
|Anthropologie| 451|
|Canada| 375|
|Philosophie| 370|
|Histoire| 350|
|Informatique| 280|
|Chimie| 262|
|Sociologie| 248|
|Identité| 235|
|France| 229|
|Droit| 227|
|Philosophy| 218|
|Physique| 214|
|Sciences biomédicales| 214|
|Montréal| 212|
|Inflammation| 207|
|Quebec| 204|
|Science politique| 202|
|Sciences infirmières| 199|
|:--------|--------------------:|

Dans ce tableau, nous pouvons noter que `Québec` et `Quebec` sont présent; on 
peut associer le second à l'anglais ou à une faute de frappe.
Nous voyons aussi que, parmis les 20 mots-clés les plus populaires se trouvent 
des mot-clés décrivant les disciplines les plus populaires. Il y a donc une grande 
tendance à inscrire le nom de la discipline de publication dans les mots-clés.
Il y a un total de 155,489 mots-clés différents utilisés au travers de Papyrus. 

Au total, 120,936 mots-clés ne sont présents qu'une seule fois.



#### `doc_language`
La Figure suivante montre la distribution des 2,024 champs manquants de 
l'attribut`doc_language` selon les années.

<p align="center">
  <img src="{{ '/assets/images/missing_doc_lang_year.png' | relative_url }}" style="width:60%;">
</p>

Nous voyons que ces champs manquant proviennent exclusivement de publications
soumisses avant 2009, soit avant la date de dépôt systématique dans Papyrus.

<p align="center">
  <img src="{{ '/assets/images/doc_lang.png' | relative_url }}" style="width:60%;">
</p>

La grande majorité des documents sont écrits en français. 4,677 documents
sont écrits en anglais, 23 en espgnol. Enfin, quelques documents
sont en allemand, en italien, en portuguais, en russe ou en grec.

À l'aide de la Figure suivante, nous pouvons voir que le phénomène de la 
publications en d'autres langues que le français ou l'anglais est bien
réparti au travers des années.
<p align="center">
  <img src="{{ '/assets/images/doc_lang_not_fr_en.png' | relative_url }}" style="width:60%;">
</p>

Avec la Figure suivante, nous voyons que la quantité de publications en
anglais ne fait qu'augmenter au fil des années.
<p align="center">
  <img src="{{ '/assets/images/eng_doc.png' | relative_url }}" style="width:60%;">
</p>



#### `doc_type`, `discipline`, `degree_grantor` et `degree_name`

Le tableau suivant montre la distibution de ces attributs manquants selon les années.
Les éléments `discipline` et `degree_name` manquants proviennent de deux documents; 
le premier en 1985, le second en 1994. Les champs manquants durant les années 2010 et 2025
peuvent être attribués à des erreurs commises par les auteurs, puisque ces publications furent
soumises après 2009.

| issued_year | doc_type | discipline | degree_grantor | degree_name |
| ----------- | -------- | ---------- | -------------- | ----------- |
| 1985        | 0        | 1          | 0              | 1           |
| 1994        | 0        | 1          | 1              | 1           |
| 2010        | 0        | 0          | 1              | 0           |
| 2025        | 1        | 0          | 7              | 0           |


Le tableau suivant montre la distribution des 20 disciplines les plus 
populaires au travers du corpus:

| Discipline | Nombre d'occurrences |
| -----------| -----|
|Sciences biomédicales | 1816 |
|Informatique | 1593 |
|Psychologie | 1450 |
|Chimie | 1110 |
|Anthropologie | 1088 |
|Philosophie | 968 |
|Physique | 950 |
|Histoire | 947 |
|Criminologie | 831 |
|Sociologie | 795 |
|Science politique | 788 |
|Sciences biologiques | 735 |
|Biologie moléculaire | 726 |
|Droit | 701 |
|Biochimie | 694 |
|Sciences vétérinaires | 684 |
|Microbiologie et immunologie | 676 |
|Sciences infirmières | 675 |
|Littératures de langue française | 644 |
|Relations industrielles | 555 |
| -----------| -----|

Ce sont les sciences biomédicales qui dominent, suivies de l'informatique et de la psychologie.
Il y a un total de 174 disciplines dans le corpus complet. Le tableau suivant montre
les 20 disciplines les moins publiées:

| Discipline | Nombre d'occurrences |
| -----------| -----|
|Chimie médicinale | 1 |
|Médecine sociale et préventive | 1 |
|Activité physique | 1 |
|Sciences biomédicales (Nutrition) | 1 |
|Médecine et chirurgie expérimentales | 1 |
|Pathologie | 1 |
|Sciences de l'éducation (Andragogie) | 1 |
|Aménagement (Histoire et théories) | 1 |
|Sciences vétérinaires (option sciences cliniques) | 1 |
|Sciences de la vision (Option sciences fondamentales et appliquées) | 1 |
|Études néo-helléniques | 1 |
|Psychologie-recherche et intervention (Psychologie du travail et des organisations) | 1 |
|Sciences pharmaceutiques (Chimie médicinale) | 1 |
|Sciences biomédicales (Bioéthique) | 1 |
|Sciences de l'éducation (Éducation comparée et fondements de l'éducation) | 1 |
|Sciences humaines appliquées (Option bioéthique) | 1 |
|Psychologie-recherche et intervention (Psychodynamique) | 1 |
|Sciences biomédicales (Immunologie) | 1 |
|Médecine du travail et d'hygiène du milieu | 1 |
|Littérature (Humanités numériques) | 2 |
| -----------| -----|

Parmis les 33,879 documents du corpus, 33,876 sont des thèses ou mémoires,
2 sont des travaux étudiants de niveau maitrîse ou doctorat, 1 est un film
ou vidéo de niveau maitrise. Le document manquant ne possède par de valeur 
dans ce champs.

Le tableau suivant montre la distribution des types non vides de diplôme octroyés :

| Diplôme | Nombre d'occurrences |
|--------|------|
|M. Sc. | 15850 |
|Ph. D. | 10430 |
|M.A. | 5435 |
|LL. M. | 806 |
|M. Sc. A. | 265 |
|M. Ps. | 253 |
|LL. D. | 202 |
|M.O.A. | 183 |
|M. Mus. | 146 |
|M. Urb. | 133 |
|D. Mus. | 62 |
|M.S.I. | 38 |
|D. Ps. | 34 |
|L. Th. | 10 |
|M.B.S.I. | 8 |
|D. Th. & Ph. D. | 5 |
|D. Th. | 4 |
|D. Psy. | 3 |
|M. Th. | 3 |
|M. Sc | 2 |
|Ll. M. | 1 |
|M. Trad. | 1 |
|M.A.P. | 1 |
|M. Ps | 1 |
|M.S. | 1 |
|M.A | 1 |
|--------|------|

Ces dipômes se résument dans ces deux niveaux universitaires:

| Niveau universitaire | Nombre d'occurrences |
|-----------------|------------|
|Maîtrise / Master's | 23136 |
|Doctorat / Doctoral | 10744 |
|-----------------|------------|

On retrouve donc deux fois plus de documents liés à des maîtrises qu'à des doctorats.

Le tableau suivant résume la distribution des valeurs non vides de l'attribut
`degree_grantor`:

| Émetteur du diplôme | Nombre d'occurences |
|-----|-----|
Université de Montréal |          33870 |
M.S. U. of California, Davis       | 1 |
|-----|-----|

Le diplôme émit par UC Davis provient d'un 
[mémoire de maîtrise en nutrition](https://umontreal.scholaris.ca/items/f1238b56-ff73-4f10-aaa1-76fef83dbcd2)
publié en 1985 par une étudiante de l'UdeM déposant un mémoire à l'Université 
Davis de Californie.