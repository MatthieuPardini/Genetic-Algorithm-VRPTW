# Présentation
Ce projet a été réalisé dans le cadre d'un cours, lors de ma dernière années à l'Université de Technologie de Troyes.

Il traite une problématique de logistique urbaine, à l'aide d'une méthode exacte (mais limitée) et d'une méthode approchée (fournissant de bons résultats). Ce projet a été mené avec le langage python, en utilisant également le logiciel Microsoft Excel.

# Problématique

Le problème du voyageur de commerce (TSP – Traveling-Sales Problem) recherche un itinéraire de voyage optimal, partant d'un point initial, passant par un ensemble de points une et une seule fois et revenant au point initial.

Voici un exemple de données avec 40 points, le point 0 étant le point initial duquel partir :

Dans notre cas les points sont des clients qu'un camion partant d'un dépôt doit livrer
La fonction-objectif, à minimiser, peut être : la distance, le coût de transport...
Ici nous avons étudié un cas particulier : celui où les clients ont une fenêtre de temps


La fonction-objectif définissant notre problème est la somme des coûts des trajets que réalise la tournée
optimale plus le coût de pénalité de retard si le client n’est pas visité à temps.
Ces fenêtres de temps permettent de complexifier le problème puisque on se retrouve avec des restrictions
concernant le moment où un sommet peut être visité. Le terme « fenêtres de temps souples » signifie que
nous pouvons visiter un client après la fin de sa fenêtre de temps mais cela engendrera des coûts
supplémentaires.
Ainsi, ce problème consiste à trouver un itinéraire optimal qui visite tous les sommets une seule fois, en
respectant les contraintes de fenêtres de temps souples et en minimisant la fonction-objectif. 

# Résolution

## - Méthode exacte
## - Méthode approchée
## - Méthode approchée avec capacité

# Résultats

