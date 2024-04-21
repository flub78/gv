# Facturation

Dans GVV chaque club ou association pouvait configurer sa facturation à l'aide d'un module php. Cela nécessitait l'intervention de quelqu'un maîtrisant php ce qui peut poser problème à certains clubs.

Dans la nouvelle version, je vais essayer de mettre à disposition une facturation directement configurable par le trésorier sans programmation. 

## Définitions

La configuration de la facturation sera basé sur les concepts suivants.

### Produits

 Ils définissent ce qui peut être facturé
  
### Tarifs 

Ils définissent à quel prix un produit est facturé. Ils contiennent un prix et une date à partir de laquelle ce prix est facturé. Lors de la facturation on applique le dernier tarif à la date de facturation. Il est possible de préparer des tarifs pour l'avenir mais ils ne seront appliqués qu'à leur date de mise en oeuvre.

Ce mécanisme permet d'appliquer des tarifs différents pour une période donnée (concours, stage), mais nous verront plus tard qu'il y a d'autres mécanismes pour faire cela (ils impliquent la création de produit spécifique, comme remorqué-concours, etc)

### Membre

C'est quelqu'un connu du système qui peut-être facturé. Il peut aussi être propriétaire d'un planeur ce qui modifie les règles de facturation.

Un membre a une catégorie de facturation: standard, moins-de-25-ans, senior, militaire, chômeur, étudiant, convention-CE, etc. 

### Vol

C'est ce qui déclenche l'opération de facturation. Il contient certaines informations qui peuvent être utilisées pour la facturation. Le nom du pilote, d'un payeur tierce et d'un pourcentage de facturation (si on continue avec la facturation partagée ...).

Il contient une durée, une durée d'utilisation du moteur, un mode de lancement, une altitude et/ou une durée de remorqué en minute, le terrain de départ.

Le vol à également une catégorie de facturation: standard, VI, vol d'essai, concours. Et l'information DC (double commande).

### La machine

Elle a également des informations utilisées pour la facturation: club, privé, banalisé

Elle contient également des pointeurs vers des produits facturables: heure de vol pégase, heure de vol DC, prix de l'heure au forfait, etc. A voir si on peut simplifier à l'aide des règles de facturation.

## Les règles de facturation

Les règles de facturation sont créés par le trésorier pour configurer la facturation.

Elle comprennent deux parties: le déclencheur et la définition de ce qu'il faut facturer.

### Les déclencheurs

Les déclencheurs sont des filtres qui déterminent sil la règle s'applique à un vol. Quand plusieurs règles s'appliquent à un vol, il y a facturation de plusieurs produits.

Note: A voir à l'usage mais pour simplifier il pourra être utile de classer les déclencheurs par priorité et d'avoir un mécanisme pour stopper l'algorithme dés qu'un premier déclencheur sera activé.

Le déclencheur est une expression logique

Ex:

vol=standard

vol=concours et pilote.moins-de-25-ans

vol=standard et machine=banalisé et pilote/=machine.propriétaire

vol=standard et temps_moteur > 0

vol=standard et altitude_rem > 350 et altitude_rem < 550

### Les actions

Des actions de facturation, elles sont constituées d'un produit et d'une quantité.
le plus souvent la quantité est directement une durée ou quantité mais cela peut être une expression comme max (temps de vol en minute, 180)
ou ((Alt rem - 500) / 100).

Des actions de débit de compte de ticket.


## Les tickets

Les tickets sont un moyen de facturer d'avance une certaine quantité de vols, treuillés, remorqués à prix avantageux.

Ils vont nécessiter des actions spécifiques avec le problèmes des facturation multiple. Ex: il reste une heure de vol achetée d'avance à un pilote et il réalise un vol d'une heure trente.

## Les vols partagés

Le mécanisme complexifie beaucoup la facturation. A voir si on veut le garder.

## La facturation dependant d'achats antérieurs (les forfaits)

Il faut pouvoir déclencher une règle si le membre à acheté un produit spécifique dans l'année.
Ou ajouter une catégorie de facturation au pilote (pilote au forfait de telle catégorie)

# Conclusion partielle et temporaire.

On voit que la facturation reste un problème complex.

On en arrive à imposer au trésorier de la décrire dans un language spécifique. C'était une des raisons de la configuration en php, au moins dans ce cas on disposait de la souplesse et de la puissance d'un vrai language de programmation.

Ce dont j'ai peur avec l'approche par règle c'est qu'on ne dispose pas de la souplesse des conditions imbriquées et qu'on soit obligé de multiplier les règles pour couvrir tout les cas.

```
    Si condition 1 alors
        Si condition 2 alors
            facturation produit X
        Fin

        Si condition 3 ou condition 4 alors
            facturation produit Y
        Sinon
            facturation produit Z
        Fin
    Sinon
        facturation produit A
    Fin
```

traduit en règle cela donnerait:

```
si condition 1 et condition 2 facturation produit X
Si condition1 et condition 3 facturation produit Y
si condition 1 et condition 4 facturation produit Y
si condition 1 et non condition 3 et non condition 4 facturation produit Z
si non condition 1 facturation produit A

```

On voit que l'approche par règle est plus compliquée à exprimer et qu'il est plus difficile d'être sûr d'avoir couvert tout les cas... Et encore l'exemple ci-dessus est beaucoup plus simple que les cas réels.

Peut-être que la bonne approche est de fournir toutes les primitives pour construire les conditions et les actions mais de garder une structure de conditions imbriquées. Soit textuelle soit à partir de bloc de construction comme scratch.

Ou peut-être que la bonne approche est de fournir quand même des petits morceaux de php 