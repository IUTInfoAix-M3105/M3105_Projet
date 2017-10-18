# Contraintes de réalisation
Ce projet constitue dans votre parcours une expérience de réalisation logicielle complexe. Il est l'occasion 
pour nous de vous faire expérimenter les pratiques de développement que vous retrouverez dans l'univers professionnel. 
Ce document a donc pour objet de vous donner l'ensemble des contraintes techniques que vous devrez respecter durant ce 
projet. L'évaluation prenant en compte ces consignes, vous devez impérativement les prendre en compte pour ne pas 
perdre des points bêtement.

## Instructions concernant la réalisation du projet
* L'utilisation d'un outil de gestion de versions est obligatoire (voir instruction ci-après).
* Les tests unitaires doivent être réalisés pour valider chaque fonctionnalité programmée tout au long du développement 
du projet.
* Votre base de code devra toujours être dans un état sain (le code compile et les tests passent). Pour 
vérifier cela au fur et à mesure des ajouts des contributions, vous utiliserez de manière plus ou moins transparente l'outil d'intégration continue TravisCI (qui est déjà actif pour votre projet mais dont il faudra personnaliser le badge pour visualiser facilement l'état).
* Le respect des conventions de nommage du langage Java est **impératif**. Pour garantir le respect de ces aspects 
esthétiques, les IDE vous fournissent tous des outils de nettoyage, veillez à bien les utiliser avant le moindre partage de code.
* Le programme devra gérer proprement les exceptions. Ne jamais lever une exception et ne rien faire !
* Le code doit être “propre” (au sens retenu dans l'ouvrage Clean Code de Robert C. Martin). La lisibilité et 
l'expressivité du code ne sont pas une option. La modélisation objet et le respects des bonnes pratiques de 
programmation (principes SOLID entre autre) font partie des règles que vous devez impérativement mettre en oeuvre. 
Pour mesurer votre dette technique, vous pouvez mettre en oeuvre des outils comme `SonarQube` ou `CodeClimate`.

## Instruction concernant la gestion de version
* La livraison finale de votre code devra se faire sur un dépôt GitHub privé. Ce dépôt contiendra les noms de 
famille de chaque membre accolés par ordre alphabétique. Par exemple, si les noms sont Dupont, Martin, Durand et Cornet, 
ce dernier s'appellera : `m3105-projet-CornetDupontDurantMartin`
* Le projet devra disposer d'une branche `master` contenant uniquement les `release`. Dans cette branche, chaque 
`release` fera l'objet d'un tag dont le nom donnera une indication sur la version de votre logiciel (par exemple, v0.1.1 
ou v0.1.1alpha).
* Le projet disposera d'une branche de développement appelée `develop`. C'est dans cette branche que devront être 
intégrées les fonctionnalités au fil de leur développement.
* Chaque fonctionnalité sera développée dans une branche dédiée. Le nom de la branche de fonctionnalité doit être au 
maximum porteuse de sens.
* Au sein d'une branche de fonctionnalité, chaque `commit` doit être le plus petit possible. Un commit qui fait 
10 modifications sans lien devra être découpé en 10 `commits`. Chaque commit sera accompagné d'un message permettant 
de comprendre l'objet de la modification.
* L'intégration d'une branche de fonctionnalité devra impérativement être faite en utilisant le mecanisme 
des `pull-request` pour permettre une revue de code systématique.
* Pour simplifier au maximum le graphe des révisions de votre dépot, quand vous travaillez à plusieurs sur la même 
branche de fonctionnalité, l'usage de la commande `rebase` est conseillé par rapport à la commande `merge`.

## Contraintes de rendu pour le livrable final
* La construction ainsi que le cycle de vie du logiciel devront être gérés avec Maven.
* L'archive contiendra un dossier `src` avec l'intégralité du code. Aucun fichier binaire exécutable ne doit être dans l'archive.
* Le modèle de projet fournit, est configuré pour créer un Jar executable auto-contenu (les dépendances sont contenues 
dans l'archive). Pour produire votre jar, il suffit d'éxécuter la commande `mvn package`. L'archive sera située dans le 
dossier `target/` de votre projet. Il sera executable avec la commande suivante : `java -jar M3105_Projet-1.0-SNAPSHOT.jar`
* Un dossier `test` contenant les tests unitaires devra également être inclus dans l'archive avec les instructions pour 
leur exécution.
* Le dossier `doc` devra contenir la documentation complète du code source (JavaDoc).
