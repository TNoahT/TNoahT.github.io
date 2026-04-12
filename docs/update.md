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

## 10 avril 2026

### Comparaison du sens des traductions
Afin d'avoir une idée de la qualité des traductions, nous utilisons un [système 
multilingue de vecteur sémantique pour les phrases](https://huggingface.co/sentence-transformers/paraphrase-multilingual-mpnet-base-v2)
(*multilingual sentence embedder*). Ce modèle permet de transformer des phrases 
ou des paragraphes en vecteurs à 768 dimensions. Nous avons décidé de 
travailler au niveau des phrases, puisque ceci nous permettra de pénaliser
plus sévèrement les détails ajoutés dans une language et non dans l'autre.

Pour chaque document ne contenant que deux résumés, un en français et l'autre 
en anglais, nous transformons chaque phrase d'un résumé en un vecteur. Les 
documents contenant des résumés dans d'autres langues sont laissés de côté 
pour cette analyse. Ceci nous laisse 17,389 documents à analyser.
Nous créons ensuite une matrice de similarité. 

Soit un document donnée contenant exactement un résumé français et un résumé
anglais. Soit F, l'ensemble des phrases française ( \( F = {f_1, \dots, f_m} \) )
et E l'ensemble des phrases anglaises ( \( E = {e_1, \dots, e_n} \) ). Chaque
phrase est encodée en un vecteur de 768 dimensions dans un espace sémantique
partagé.

Nous construisons une matrice de similarité S \in \mathbb{R}^{m \times n},
où chaque élément correspond à la similarité cosinus entre le vecteur
d'une phrase française et un phrase anglaise.

[
  S{ij} = \cos{\mathbf{f}_i, \mathbf{e}_j}
]

Afin de modéliser l'équivalence entre les phrases indépendamment de leur
ordre, nous utilisons un algorithme Hongrois. Ceci nous permet de trouver
l'alignement 1-à-1 des phrases tout en maximisant la similarité totale. 
Ceci permet de prendre en compte que l'ordre de l'information peut être 
différent en anglais et en français.

[
  \pi^* = \arg\max_{\pi} \sum_{(i,j) \in \pi} S_{ij},
]

où $\pi$ est un alignement entre deux phrases.

Avec cet alignement optimal, nous pouvous calculer la similarité moyenne
entre le résumé français en celui en anglais. Soit $M = {S_{i,j \mid (i,j) \in \pi^*}$
l'ensemble des similarités. Nous définissions la similarité moyenne :

\[
\mu = \frac{1}{|M|} \sum_{s \in M} s.
\]

Afin de capturer la 
qualité des correspondances les plus faibles tout en restant robuste aux 
valeurs aberrantes, nous utilison le 10e percentile de similarités, qui 
transmet plus d'information qu'un simple minimum :

\[
\rho = \text{percentile}_{10}(M)
\]

Pour prendre en compte les phrases non alignées, nous introduisons une 
pénalité de couverture:

\[
c = \frac{1}{2} \left( \frac{|M|}{m} + \frac{|M|}{n} \right),
\]

où \( m\) et \( n\) sont respectivement le nombre de phrases en français
et en anglais. Cette stratégie est inspirée de 
[BLEU](https://aclanthology.org/P02-1040.pdf) qui pénalise les omissions 
avec des facteurs de courverture ou de longueur.

Inspiré du score F1, nous combinons la qualité totale et la couverture
via une moyenne harmonique :

\[
F = \frac{2 \mu c}{\mu + c}
\]

Enfin, nous intégrons une pénalisation basée sur les correspondances les
plus faibles en utilisant \( \rho \), ce qui donne le score final :

[
\text{Score final} = F \cdot \sqrt{\max(\rho, 0)}
]

Ce score permet de capturer à la fois la similarité globale entre les 
résumés, leur couverture mutuelle, ainsi que la présence éventuelle
de correspondances faibles ou incohérentes. Le maximum fut choisi
pour éviter les nombres négatifs, qui rendaient l'équation instable.

Le tableau suivant montre les résultats de cette comparaison de résumés:

| Statistique | Similarité moyenne | 10e percentile | F        | Score final |
|:------------|-------------------:|:---------------|----------|-------------|
| Moyenne     |	0.875666           | 0.790935	      | 0.919420 | 0.817997    |
| Écart-type  | 0.059121	         | 0.100323       |	0.043393 | 0.086371    |
| Minimum     |	0.558599	         | 0.245455       |	0.620491 | 0.391569    |
| 25%	        | 0.842933	         | 0.733303	      | 0.895180 | 0.766784    |
| 50%	        | 0.887186	         | 0.808658	      | 0.927782 | 0.832494    |
| 75%	        | 0.919650	         | 0.865351 	    | 0.952054 | 0.882452    |
| Maximum	    | 1.000000	         | 1.000000	      | 0.996116 | 0.990332    |
|:------------|-------------------:|:---------------|----------|-------------|

En moyenne, les résumés sont assez équivalents dans les deux langues, avec un score 
final moyen de 0.817997. Certains documents ont un score final plutôt bas, alors
que d'autres approchent du score parfait. L'écart-type faible montre l'homogénéité
des données; les paires de résumés très différentes sont rares. Nous voyons que
75% des paires obtiennent un score final plus élevé que 0.766784, et 50% des paires
excèdent 0.832494, ce qui est un très bon score. 
La Figure suivante montre la distribution des scores finaux :

<p align="center">
  <img src="{{ '/assets/images/abs_final_scores.png' | relative_url }}" style="width:60%;">
</p>

Nous voyons une queue assez longue vers la gauche, montrant qu'il y a plusieurs 
exemples avec des traductions non fidèles. L'utilisation de ce corpus à des
fins de traduction automatique requièrerait un nettoyage des ces résumés afin
de traiter ce bruit.

Voici les résumés associés au [document](http://hdl.handle.net/1866/20257) avec le score le plus bas, soit 0.391569:

FR: `L’HTP est une maladie caractérisée par une élévation de la pression artérielle pulmonaire moyenne au-delà de 25 mmHg au repos. Dans le but de trouver un meilleur outil diagnostique pour cette affection, notre laboratoire a synthétisé puis expérimenté un radiotraceur nommé PulmoBind. Celui-ci s’avère être un dérivé synthétique de l’AM : un peptide vasodilatateur endogène de 52 acides aminés sécrété par de nombreux organes. Une fois couplé au 99mTechnétium, le PulmoBind permet par méthode scintigraphique la visualisation de la circulation pulmonaire métaboliquement active. Suite à notre étude de phase I, le PulmoBind s’est avéré sécuritaire et efficace. De plus, des analyses plus approfondies de la distribution spatiale ont démontré la capacité du PulmoBind à imager les divers gradients de la perfusion pulmonaire. À ce jour, aucun produit sur le marché n’emploie des données de captation et de distribution spatiale pour effectuer l’évaluation de la circulation respiratoire. Par conséquent, cette nouvelle méthodologie constitue une approche inédite et prometteuse pour le diagnostic de l’HTP et de son suivi.`

EN: `Background. The pulmonary circulation is submitted to large physiologic hemodynamic variations and possesses numerous important metabolic functions mediated through its vast endothelial surface. Unfortunately, no test is currently available that can directly evaluate endothelial integrity and pulmonary perfusion gradients at the same time. We developed PulmoBind, a chelated derivative of human adrenomedullin labeled with 99m-Tc for nuclear medicine SPECT imaging. By specifically binding to its endothelial receptors, abundantly expressed in human alveolar capillaries, PulmoBind can provide a non-invasive evaluation of pulmonary function that can help in the diagnosis of disorders affecting the pulmonary circulation. This thesis represents my doctoral work to determine the safety of PulmoBind in humans and its capacity to generate good quality imaging for diagnosis purposes. Methods. Twenty healthy participants were included into escalating doses groups of 5 mCi (n=5), 10 mCi (n=5) or 15 mCi (n=10) 99mTc-PulmoBind. Vital signs were closely monitored and safety biochemistry and hematology parameters obtained. SPECT imaging was serially performed and 99mTc-PulmoBind dosimetric and spatial distribution analysis accomplished. Imaging quality of the lungs was assessed serially. Results. Radiochemical purity of 99mTc-PulmoBind was greater than 95%. There were no safety concerns at the 3 dosages studied. Imaging revealed a predominant and prolonged lung uptake with mean peak pulmonary extraction of 58% ± 7% (mean ± SD) of the injected dose. PulmoBind was well tolerated and resulted in no clinically significant adverse events. The highest dose of 15 mCi provided a favorable dosimetric profile and excellent quality tomographic imaging of the lungs. The postural lung perfusion gradient was detectable. Indeed, dorsal activity was 18.1 ± 2.1% greater than ventral activity, and caudal activity was 25.7 ± 1.6% greater than cranial activity. The intensity of PB activity followed a normal distribution while macro aggregated albumin (MAA) activity was importantly skewed and bimodal. Conclusion. 99mTc-PulmoBind up to a dose of 15 mCi is safe and provides good quality lung perfusion imaging that can reveal the heterogeneity of the pulmonary perfusion. Therefore, the safety and efficacy of this agent could be tested in disorders of the pulmonary circulation such as pulmonary arterial hypertension.`

Nous voyons rapidement que le résumé anglais possède beaucoup plus 
d'information que celui en français; le score final de 0.391569 reflète 
cette différence sémantique importante entre ces deux résumés.

Similairement, voici les résumés associés au [document](http://hdl.handle.net/1866/32509) avec un score de 0.990332 :

FR: `Cette étude comparative examine le rôle des perceptions de corruption sur les efforts anti-corruption en Bulgarie et en Roumanie et le rôle de l'Union européenne dans cet effort depuis leur adhésion de 2007 jusqu'à aujourd'hui. L'étude se concentre sur le rôle des manifestations anti-corruption dans les deux pays et sur la manière dont ces mouvements populaires ont contribué à la création de nouveaux partis politiques.`

EN: `This comparative study examines the role of perceptions of corruption on anti-corruption efforts in Bulgaria and Romania and the role of the European Union in this effort since their accession in 2007 until today. The study focuses on the role of anti-corruption protests in the two countries and how these popular movements contributed to the creation of new political parties.`

Nous voyons que ces deux résumés sont très similaires, expliquant leur score 
commun de 0.990332.

### Regroupement de données (*clustering*) pour les mot-clés

Afin de pouvoir comprendre l'étendue de la recherche effectuée
à l'Université de Montréal, nous avons effectué un regroupement
non-supervisé des mots-clés du corpus Papyrus.

Étant donnée que plusieurs langues différentes sont utilisées
pour les mots-clés, nous devons utiliser un système multilingue 
de vecteurs sémantiques (*embedder*). Nous avons choisi le
même [modèle](https://huggingface.co/sentence-transformers/paraphrase-multilingual-mpnet-base-v2) que celui utilisé pour les résumés. Ce modèle permet de projecter tous les mots-clés 
du jeu de données dans un espace commun à 768 dimensions.

Ensuite, afin de pouvoir regrouper les données, nous appliquons
une méthode de réduction de dimensionnalité à ces vecteurs sémantiques. Afin de passer de 768 dimensions à 50, nous
utilisons *[Uniform Manifold Approximation and Projection](https://umap-learn.readthedocs.io/en/latest/index.html)* 
(UMAP).

La méthode choisie pour faire le 
regroupement des données, HDBSCAN, est optimal pour des données
d'entrée de [50 à 100 dimensions]((https://hdbscan.readthedocs.io/en/latest/faq.html)); au-delà de cela, les 
performances diminuent considérablement, d'où cette diminution 
de dimentionnalité préalable. Afin d'avoir des regroupement 
suffisamment importants, nous introduisons une taille minimale 
de 15 mots-clés par groupe.

Enfin, nous pouvons explorer les plus gros regroupements, ainsi
que les dix mots clés les plus populaires pour ces groupes. Le tableau 
suivant présente les dix plus gros regroupements en terme de nombre de
mots-clés :

| Numéro du regroupement | Taille | Mots-clés les plus populaires |
|-------|--------|----|
| 2061  | 1889  |  Grossesse, Douleur, Altérité, Locomotion, Régulation, Jeunes, Bien-être, Soins intensifs, Rein, sommeil |
| 270  | 833  |  Nutrition, Alimentation, nutrition, Diet, Food insecurity, Insécurité alimentaire, Sécurité alimentaire, Food, alimentation, Environnement alimentaire |
| 2077  | 712  |  Jacques Derrida, Hubert Aquin, Rousseau, Jean-Jacques, 1712-1778, Georges Bataille, Jean-Jacques Rousseau, Rousseau, Georges Didi-Huberman, Grandbois, Alain, 1900-1975, Pierre Perrault, Derrida, Jacques |
| 361  | 678  |  Génétique, Expression génique, Génomique, Gene expression, Genetics, Genetic, Thérapie génique, génétique, Génétique des populations, Genomics |
| 1749  | 626  |  Environnement, Environment, environnement, environment, Écologie, Ecology, écologie, ecology, Facteurs environnementaux, Environnement bâti |
| 1082  | 425  |  Cinéma, Cinema, cinéma, Cinématique, cinema, Film, film, cinématique, Cinéma expérimental, Experimental cinema |
| 429  | 417  |  Parents, Pratiques parentales, Parent, pratiques parentales, Parentalité, Parenting, Parenté, Parental involvement, Implication parentale, parents |
| 1630  | 405  |  Travail, Mémoire de travail, Emploi, Employment, Environnement de travail, Work, travail, work, Conditions de travail, Marché du travail |
| 1457  | 383  |  Cytokines, Cytokine, cytokines, Cytotoxicité, Cytosquelette, Cytométrie en flux, Flow cytometry, Cytokinesis, Cytotoxicity, Cyclopropane |
| 216  | 382  |  Sciences infirmières, Infirmières, Nurses, Nursing, Soins infirmiers, Infirmière, Formation infirmière, Nursing education, infirmières, nurses |



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