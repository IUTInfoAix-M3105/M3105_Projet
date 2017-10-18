# HyperFrisette – un réseau ferré alternatif

**Date de rendu : 20 novembre 2017 à 23h59**  
**Date de soutenance : 23 novembre 2017**

*Le non-respect d’une des consignes ci-dessous impliquera une pénalité de 3 points minimum sur la note du projet.*

### Généralités
-  Vous travaillerez par équipe de 4 ou 5 personnes et de manière collaborative en utilisant Git et en partageant le travail entre les membres de l'équipe sur GitHub.
-  Chaque membre de l'équipe fera des commits du code qu'il a écrit. L'équipe enseignante va en tenir compte lors de l'évaluation.
-  Le dépôt du projet de votre équipe devra être __privé__ (accessible uniquement par les membres de l'équipe).
-  Attention aux [modalités de déroulement des soutenances](ModalitesSoutenance.md) !

### Remarques

La note du projet prendra en compte :

-  Le respect des consignes du sujet et des [contraintes de réalisation](ContraintesRealisation.md).
-  La modélisation objet de votre application. Les correcteurs seront particulièrement attentifs à l’utilisation appropriée des patrons de conception que vous aurez mis en œuvre
-  La propreté et la lisibilité du code ainsi que tout ce qui facilitera sa compréhension par les correcteurs (noms des variables, commentaires, modularité, etc ).
-  La compilation de votre code devrait marcher sur tout type de machine - pensez à tester avant le rendu final.
-  La correction et la cohérence du code.
-  La présentation de votre application durant la soutenance.

### Instructions

-  Il est **strictement interdit de copier du code** des projets de vos collègues. Attention : l’obfuscation du code n’est pas une solution, c’est très facile à détecter !
-  Vous pouvez aussi implémenter les bonus et/ou améliorations que vous aurez imaginés. Il est cependant conseillé de venir en parler avec votre enseignant afin d’en discuter. Dans tous les cas, il faut __d’abord implémenter__ les fonctionnalités demandées dans le sujet.

Au cours de votre travail de conception et de réalisation, pensez à respecter les principes vus dans vos cours de conception objet durant votre formation (SOLID, DRY, YAGNI, KISS, ...).

Les modalités de réalisation de certaines fonctionnalités ne sont pas forcément spécifiées afin de vous donner une certaine liberté dans la conception. En revanche, vous devez respecter les contraintes imposées dans le sujet et justifier tout choix qui les contredit.

## Présentation du projet

### Description du contexte

Vous êtes chargé de réaliser le système de gestion du tout nouveau réseau de trains sur coussin d'air du pays : **HyperFrisette**. Le système est composé de gares reliées entre elles par des chemins fer. Le réseau est utilisé par diverses entreprises ferroviaires (FCNS, EFNER,...) qui, pour chaque passage de train, sont facturées comme il se doit ! Des lignes reliant deux gares permettent de découper le pays dans des axes principales, par exemple Paris-Marseille, Nice-Bordeaux, Marseille-Lille, etc. Pour simplifier une ligne ne comporte que deux gares : la gare de départ et la gare d'arrivée. L'utilisation d'une ligne a un coût. Ainsi, pour une compagnie ferroviaire dont le train traversera plusieurs lignes, on calculera le prix total à payer qui pourra éventuellement dépendre de plusieurs paramètres -- taille de train, fidélité, quantité d'autres trains de la même compagnie circulant dans le réseau, les subventions que les villes de certaines gares très excentrées pourraient fournir pour baisser les coûts de péages, etc. Plusieurs lignes sont déjà définies et naturellement, d'autres peuvent être créées à la demande de l'utilisateur.

Notez que les trains sont propriété des compagnies ferroviaires et pas du réseau ferré. Votre système devra donc gérer uniquement la circulation des trains dans le réseau et la facturation de ceux-ci. Notamment le système devra affecter des lignes à des trains sur des créneaux horaires de façon à éviter les conflits. Pour cela la notion de *sillon horaire* est introduite : c'est la période durant laquelle une infrastructure donnée est affectée à la circulation d'un train entre deux points du réseau ferré. Pour vous faire une idée, vous pouvez avoir des explications en détails [ici](https://www.sncf-reseau.fr/fr/que-sont-les-sillons) et [là](https://fr.wikipedia.org/wiki/Sillon_horaire). Dans notre cas, la notion de sillon est simplifiée. Vous allez considérer la même planification horaire pour tous les jours. Par exemple si pour le trajet Paris-Marseille le sillon 10h-11h est réservé, aucun train ne circulera sur la ligne Paris-Marseille durant ce créneau et c'est valable pour chaque jour de la semaine. D'autre part, vu que les trains utilisant **HyperFrisette** sont à sustentation à coussins d'air, ils sont très rapides et on peut parcourir toute ligne du réseau dans l'espace d'une heure maximum. Cela simplifie votre modélisation : les créneaux horaires des sillons seront les mêmes (d'une heure) pour toutes les lignes de France.

### Fonctionnement général de l'application

Pour simplifier la tâche, vous n'aurez pas à gérer les aspects temps réel de l'application. La simulation du passage du temps dans votre système se fera par des itérations actionnées par l'utilisateur de l'application. Chaque itération est composée des étapes suivantes qui devront être réalisées séquentiellement :

1. *Demandes de trajets, déclaration d'incidents* : chaque compagnie ferroviaire a la possibilité de demander des trajets et doit spécifier les trains qui seront utilisés pour ces trajets. Les compagnies ferroviaires peuvent également déclarer des pannes sur des trajets en cours.
2. *Validation* : le contrôleur valide chaque trajet en affectant au train correspondant des sillons choisis par le système pour chacune des lignes réservées.
3. *Actionner le système* : les trains se déplacent d'une unité de temps (par exemple 10 minutes).
    - le système mets à jour l'emplacement des trains dans le réseau
    - les trains arrivant en gare changent d'état et ne sont plus considérés en déplacement
    - détection des incidents et action correspondante. Pour cela le système mets à jour le temps et vérifie si l'emplacement des trains correspond à l'affectation des sillons
4. *Affichage* : mise à jour des vues permettant d'afficher l'état actuel du système, les facturations.

Un utilisateur pourra simuler le fonctionnement du logiciel en déroulant étape par étape le scénario décrit ci-dessus.

Pour rendre le code plus compréhensible et simplifier la maintenance du projet, les parties «*traitement*» et «*affichage*» seront séparées.

- La couche *traitement* du package «`reseauferre.traitement`» : toutes les données et les traitements spécifiques (ex: ajouter des tronçons ferroviaires, ajouter des gares, ajouter des trains etc.) sont regroupés dans ce package. Les classes de cette couche ne concernent pas la partie graphique. Elles se contentent principalement d’effectuer des traitements métiers et de renvoyer des résultats.

-  La couche *graphique* du package «`reseauferre.affichage`» : cette couche gère l'affichage (interface utilisateur) et les actions de l’utilisateur (clics ou saisies au clavier).

## Grandes phases du projet
Pour réaliser ce projet, vous allez devoir découper votre travail en unités fonctionnelles plus simples. 
Votre solution devrait faire apparaître les jalons suivants :
1. Modélisation de l'univers métier (Gare, Ligne, Wagon, Train, ...)

2. Gestion de la planification/affectation des créneaux horaires et des sillons.

3. Calcul des coûts de péage et Facturation 

4. Simulation de l'évolution du système (déplacement des trains)

5. Visualisation de l'état du système

Pour repérer chacun de ces jalons dans l'historique de votre projet, vous pouvez créer une [*release*](https://help.github.com/articles/creating-releases/) sur Github.

## Éléments de mise en œuvre

### Modélisation de l'univers métier

La mise en œuvre du projet passera par la réalisation des classes et méthodes décrites ci-dessous.

#### Entreprises Ferroviaires
Une `EntrepriseFrroviaire` est caractérisée par des attributs de base (nom, numéro SIREN, etc). De plus différents types de ces entreprises existent : `EntrepriseFrroviairePassagers`, `EntrepriseFrroviaireCargo`, `EntrepriseFrroviaireAnimaux`. D'autres spécialisations d'entreprises peuvent s'ajouter ultérieurement. L'attribution des sillons, la facturation peuvent dépendre du type d'entreprise ferroviaire et des `Trains` qu'elle utilise.

#### Trains
Un `Train` est décrit par ses dimensions, vitesse maximale, le propriétaire (l'entreprise ferroviaire à laquelle il appartient), les wagons qui le composent. Plusieurs types de trains peuvent être distingués : de passagers, de marchandise, de transport de bétail, de transport de déchets, etc. Les `wagons` eux sont de types différents également : des voitures de passagers, des wagons pour les produits liquides, des wagons pour le bétail, etc. Vous proposerez un service flexible permettant la création et composition des trains à partir des wagons. Pour tout train on doit pouvoir calculer son coût d'utilisation à travers la méthode `getCout()`.

L'utilisateur (compagnie férroviaire), peut demander d'ajouter des options supplémentaires pour la fonctionnalité de déplacement : connexion au réseau Wi-Fi d'HyperFrisette, connexion au service de streaming vidéo d'HyperFrisette, etc. Ajoutez la méthode `deplacer()` dans la classe `Train` et pensez à la rendre suffisamment flexible pour prendre en compte toutes les options éventuelles. Pour mettre en ́evidence ces options, vous pourrez vous contenter d’un simple message d’affichage correspondant.

#### Lignes Ferroviaires
La création d'une `LigneFerroviaire` concrète est faite en amont de la simulation. Chaque ligne est constituée d'uniquement deux `Gares` représentant le départ et l'arrivée.

#### Sillon
Un sillon correspond à un laps de temps d'occupation d'une ligne ferroviaire. Les trains qui se déplacent dans le réseau doivent être affectés à des sillons et circuler en fonction des horaires imposés par ces sillons (sinon la collision des trains est garantie !). Pour ce faire, lorsqu'une compagnie ferroviaire a besoin d'un trajet, elle fait la demande d'une ou plusieurs lignes ferroviaires qui seront empruntées par un train, et précise également l'ordre dans lequel ces lignes seront empruntées. À la validation, s'il y a possibilité, le système attribuera au train correspondant des sillons des lignes ferroviaires demandées. Prenons un exemple où une compagnie demande le trajet Marseille-Paris dans le réseau contenant 4 lignes Marseille-Paris, Marseille-Bordeaux, Bordeaux-Paris et Paris-Lille. Dans ce cas, la compagnie pourrait demander deux lignes Marseille-Paris et Paris-Lille. Une autre possibilité serait de demander trois lignes Marseille-Bordeaux, Bordeaux-Paris, Paris-Lille. Dans le premier cas le système pourra attribuer le sillon 10h-11h pour la ligne Marseille-Paris et le sillon 11h-12h pour la ligne Paris-Lille. Une fois la demande prise en compte, le choix des sillons sera fait par le système. Chaque sillon a un coût d'utilisation (l'équivalent de la notion de péage).


#### Contrôleur
Cette entité représente en quelque sorte le regulateur du système qui veille au bon fonctionnement de celui-ci. Notamment, le contrôleur a la responsabilité d'affecter des sillons à des trains suivant différents scénarios. Au moins trois scénarios devront être intégrés :
- attribution suivant un algorithme glouton : le premier sillon disponible est attribué au premier trajet demandant l'utilisation de la ligne
- minimisation du prix pour les compagnies ferroviaires en attribuant les sillons les moins coûteux
- minimisation du temps de trajet

D'autres scénarios pourront être ajoutés ultérieurement dans votre application. Le choix du scénario sera fait par l'utilisateur (attention ce n'est pas la compagnie ferroviaire). Écrivez la méthode `attribuerSillons()` qui permettra pour un trajet demandé, de choisir et affecter les sillons suivant le scenario fixée par l'utilisateur (scenario qui peut changer/évoluer à tout moment). Le contrôleur s'assure également que les règles sont respectées et que les trains circulent bien en conformités aux sillons qui leur ont été affectés.

### Modèle économique
**HyperFrisette** gagne son pain en facturant les compagnies ferroviaires pour chaque passage de train sur une ligne. Les montants que chaque compagnie aura à payer pourront dépendre à la fois : du type de trains, de la vitesse maximale autorisée, du nombre et/ou tonnage des wagons des trains, des lignes ferroviaires et sillons réservés, de la fidelité de la compagnie ferroviaire,... . Toutes ces options peuvent être combinées entre elles par l'utilisateur. Le système de facturation est déjà assez complexe et avec le developpement du réseau, aura tendance à se complexifier davantage. Il faudra donc prévoir un mécanisme flexible de gestion de la facturation qui pourra être adapté ou enrichi par l'utilisateur.

### Visualisation du système – mode simplifié

Vous implémenterez un ensemble de vues permettant de représenter et modifier l'état interne du système. Votre programme devra en particulier intégrer les vues suivantes :

1. Une vue qui affiche l'ensemble du réseau à l'état actuel : les trains et leur situation dans le réseau, les gares

2. Une vue qui affiche le panneau des temps d'arrivée estimés dans les gares

3. Une vue qui affiche le panneau des temps de départ estimés dans les gares

4. Un panneau affichant les perturbations dans le réseau : retards, pannes dans les trains ou dans les gares

Afin de ne pas retarder le travail, dans un premier temps, vos différentes vues seront représentées par des messages appropriés affichés dans le terminal.

### Visualisation du système – IHM

*(CETTE PARTIE EST À RÉALISER EN DERNIER)*

Réalisez une interface graphique incluant les éléments suivants :

-   Les différentes vues du système présentées précédemment sous forme de fenêtres indépendantes.

-   La vue 1 sera une vue interactive, c’est sur cette vue que les boutons seront “cliquables”

-   Un menu d'application incluant : paramètres, documentation, à propos de, quitter l'application, etc.

-   La gestion des erreurs d'affichage.
