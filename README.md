# GV

Logiciel de gestion des vols - planeur, ULM, avion.

Ce project et la nouvelle version de [GVV](http://projets.developpez.com/projects/gvv/wiki/Documentation_utilisateur) programme de gestion d'aéroclub. Allez voir sur le site de [GVV](http://projets.developpez.com/projects/gvv/wiki/Documentation_utilisateur) pour une liste complète des fonctionnalités de celui-ci.

GVV a été déployé initiallement en 2011. Il a été écris en PHP, MySql et CodeIgniter. Il a donne encore satisfaction à plusieurs club mais il a veilli. Sa compatibilité avec PHP 8.x est douteuse, voir impossible à cause des modules utilisés qui ne sont plus maintenus. Sa couverture de test automatique est imparfaite rendant les modifications stressantes par peur d'introduire des regressions.

Quleques choix techniques ont été discutables:
* Une installation par association - c'était compliqué et a douragé de nombreux clubs, GV sera une plateforme multi - club.
* Des choix sur le modèle comptable discutables (recalcul sytématique des soldes des comptes depuis l'origine des temps)
* Des protections imparfaites sur le destruction d'élements référencés dans la base de données.
* L'hébergement du calendrier chez Goolle rendant l'installation complexe
* La configuration de la facturation par programmation. 

Donc, récement à la retraite, c'est le moment de recommencer tout cela sur de meilleures bases.

La nouvelle version est basé sur le projet [Multitenant](https://github.com/flub78/multitenant). C'est moi qui est créé les deux, Multitenant est juste la volonté d'isoler dans un projet à part les fonctionnalités de GVV ou GV qui pourraient être réutiliser dans un autre contexte. Il n'y a pas que les applications de gestion d'aéroclub qui ont besoin de gérer des membres, de leur envoyer des mails, de faire un peu de compta et de E-commerce.

## Organisation du projet

Pour l'instant ce projet ne contient que les fichiers qui lui sont spécifique. il ne peut pas être déployé seul. Il faut d'abord déployer Multitenant puis le compléter avec les fichiers de GV et remplacer les fichiers communs pour obtenir un projet déployable. Il y a un script pour faire cela.

Multitenant n'a pas encore atteint le niveau pour supporter facilement des applications comme GV, mais pour mettre en place les mécanismes de navigation utilisateur et d'interface graphique j'ai besoin d'une application plus céomplexe que multitenant. C'est facile de mettre en place une expérience utilisateur agréable quand vous ne gérez que quelques fonctions. Il faut une application plus complexes avec des rôles différent et de nombreux cas d'utilisation pour pouvoir concevoir une interface graphique agréable et efficace.

Donc pour l'instant c'est surtout sur ces aspects que GV va commencer à évoluer pendant que je complète multitenant. C'est uniquement une fois que multitenant contiendra tous les éléments non spéciques utilisés par GV que je basculerai sur le dévelopement actif de GV. La gestion des vols est l'objectif ultime, Multitenant en est sa plateforme.
