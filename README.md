# Présentation
Ce projet a été réalisé dans le cadre d'un cours, en binôme, lors de ma dernière année à l'Université de Technologie de Troyes.

Il traite une problématique de logistique urbaine, à l'aide d'une méthode exacte (mais limitée) et d'une méthode approchée (fournissant de bons résultats). Ce projet a été mené avec le langage python, en utilisant également le logiciel Microsoft Excel.

# Problématique

Le problème du voyageur de commerce (_TSP – Traveling-Sales Problem_) recherche un itinéraire de voyage optimal, partant d'un point initial, passant par un ensemble de points une et une seule fois et revenant au point initial.

Voici un exemple de données avec 40 points, le point 0 étant le point initial duquel partir :

![GrapheInstance40_1](https://user-images.githubusercontent.com/111661333/213656880-c92e76b1-a132-4d38-8d06-cee2793be67b.png)

Les points peuvent représenter des clients qu'un camion partant d'un dépôt doit livrer, avant de revenir au dépôt.

La fonction-objectif, à minimiser, peut être : la distance, le coût de transport...
Ici nous avons étudié un cas particulier : celui où les clients ont une fenêtre de temps dans laquelle ils souhaitent être livrés. Cela complexifie le problème. Il a été choisi de rendre cette contrainte "souple", c'est-à-dire de permettre au voyageur d'arriver chez un client après la fin de sa fenêtre de temps. Toutefois, cela engendre une pénalité, qui est un coût proportionnel au temps de retard . De plus, si le voyageur arrive chez un client avant le début de sa fenêtre de temps, il doit attendre avant de le livrer.

La fonction-objectif définissant notre problème est la somme des coûts de trajet d'une tournée auquel on y ajoute le coût de pénalité de retard si le client n’est pas visité à temps. L'itinéraire optimal est celui qui minimise cette fonction.

Pour simplifier la résolution du problème, deux hypothèses sont émises :
1. Distance entre les points = Temps de trajet = Coût du trajet.
2. Le temps de service chez un client est nul.

Voici comment sont formalisées les données que récupèrent les algorithmes :

![DonnéesVRPTW](https://user-images.githubusercontent.com/111661333/213749671-af00287b-db2c-4cac-9b55-1c9b6ca85674.png)

Comme nous pouvons le voir, il y a une autre contrainte : les clients demandent chacun une certain quantité de produit et le camion a une capacité limitée. Il s'agit d'un problème plus général que le _TSP_ : le problème de tournées de véhicules (_VRP – Vehicules Routing Problem_). Le camion ne peut généralement pas livrer tous les clients en une seule tournée et il doit effectuer des retours au dépôt.

Nous avons d'abord résolu le cas particulier du _TSP_ (pas de contrainte de capacité, une seule route optimale).
Puis nous avons transformé l'algorithme pour le généraliser en _VRP_ (capacité du véhicule limitée, plusieurs tournées).

# Résolution - Cas particulier TSP

## - Méthode exacte

Nous avons d'abord construit une modélisation mathématique linéaire du problème :

![TSPTW-Modélisation](https://user-images.githubusercontent.com/111661333/213663477-ea318cd1-d8f9-48d6-a6a6-e9dabb114c6a.png)

Nous avons ensuite codé cette modélisation en python en utilisant le solveur _pyscipopt_. Le code se trouve dans le fichier "TSPTW_Solveur.py".

Ce solveur trouve rapidement une solution pour des problèmes à 15 clients ou moins. Au-delà, la résolution nécéssite davantage de temps. Avec une limite de temps de quelques minutes, la meilleure solution du solveur est relativement mauvaise.

Il est donc nécessaire, pour des problèmes complexes, d'avoir recours à des méthodes approchées. Ici nous avons reproduit et adapté une métaheuristique répandue : l'algorithme génétique.


## - Méthode approchée

Le code se trouve dans le fichier "TSPTW_GA.py".

## Résultats

Nous avons testé notre algorithme sur des instances de 3 dimensions : 20 clients, 40 clients et 100 clients. Avec 3 instances par dimension, cela faisait 9 instances en tout.

Notre algorithme génétique comporte 4 paramètres (taille de population, nombre de générations, probabilité de mutation, part de la population initiale provenant d'une heuristique). Nous avons réalisé un nombre important d'exécutions avec différentes configurations de paramètres. À l'aide des résultats moyens, nous avons choisi le meilleur paramétrage.

Nous avons confirmé que l'algorithme converge vers des bonnes solutions, bien meilleures que celles du solveur avec une limite de temps du même ordre de grandeur.

Ci-dessous deux exemples d'exécutions pour l'instance présentée au début (Dimension 40) :

**1. Meilleure solution : 6827,78 - Temps d'exécution : 89,73s**


![GraphExécution1](https://user-images.githubusercontent.com/111661333/213675161-d069c648-8020-47c0-bf15-32dd6aa1e119.png)![exécution1](https://user-images.githubusercontent.com/111661333/213674978-02ebb3e9-9ff4-4837-9c88-d333c9e6858f.png)

Bleu : Meilleure solution trouvée, par génération

Orange : Valeur moyenne des solutions de la population, par génération


**2. Meilleure solution : 6764,32 - Temps d'exécution : 124,00s**

![GraphExécution2](https://user-images.githubusercontent.com/111661333/213675208-17580d52-729f-499a-8bea-aea6f80b983f.png)![exécution2](https://user-images.githubusercontent.com/111661333/213675194-7ee31bf1-363c-497a-aa57-fcb5c18df01b.png)

# Transformation en VRP

Il est possible de modifier ces derniers algorithmes pour les transformer en problèmes de tournées de véhicules.

Pour la partie méthode exacte (solveur), il faudrait ajouter quelques contraintes de capacité et ajouter une dimension 

Pour cela, un algorithme similaire au précédent est utilisé, avec quelques changements : le découpage en tournée et le calcul de la fonction-objectif s'aide de l'algorithme _SPLIT_.

Le code se trouve dans le fichier "VRPTW_GA.py".

Voici une exécution de cet algorithme : **Meilleure solution : 9924,57 - Temps d'exécution : 494,03s**
![GraphVRP](https://user-images.githubusercontent.com/111661333/213719756-e8e29aa3-feb2-477f-a006-7d5ec00af556.png)![VRPexecution](https://user-images.githubusercontent.com/111661333/213719817-3b0d0b3d-0613-4ed1-bab3-4a290b453496.png)

Il est nécessaire que la route soit suivie dans cet ordre : [[0, 2, 15, 21, 0], [0, 27, 18, 8, 5, 17, 16, 37, 0], [0, 28, 26, 12, 3, 34, 9, 33, 0], [0, 13, 6, 0], [0, 31, 10, 11, 19, 36, 7, 0], [0, 1, 30, 32, 20, 35, 29, 24, 0], [0, 22, 23, 39, 25, 4, 14, 38, 0]]
