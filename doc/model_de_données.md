# Le model ou schéma de données

## Principes généraux

GVV a souffert de quelques erreurs dans la conception du schéma de données.

L'utilisation d'identifiants utilisateur composés de l'initiale de prénom et du nom a conduit à quelques situations génantes lorsqu'une faute d'orthographe a été faite dans le nom d'un utilisateur et qu'il n'était pas possible de le modifier (c'était une clé primaire).

L'utilisation de l'immatriculation des machines comme clé primaire a aussi posé problème quand il c'est agit d'exporter les données vers un autre système (GESASSO). Les immatriculations de la même machine ont été saisie avec des différences de majuscule/minuscule et de tiret. Conséquence les deux systèmes ne reconnaissait pas la même machine. Si l'immatriculation de la machine n'avait pas été une clé primaire, il aurait été facile de s'adapter à l'orthographe utilisée dans l'autre système.

Si des suppressions ou l'éditions de données peuvent rendre le shéma incohérent, il est important de les controler systématiquement avec les règles update/delete restrict/cascade pour interdire la situation. Cela doit fair partie de la validation de GV qu'un utilisateur mal intentioné doté des droits admin ne puisse pas rendre le schéma incohérent.  

Le modèle utilisé dans GVV pour la compta posait problème. Les soldes étaient systématiquement recalculés depuis l'origine des temps. Cela rendait la comptabilité sensible à des modifications des livres dans le passé. Bien sûr, ces modifications étaient interdites mais elles dépendaient du fait que le trésorier n'avait pas oublié de mettre à jour la date de gel.

Il me semble plus judicieux de:

* forcer la création d'une sauvegarde "avant_cloture_exercise_20xx"
* recopier toutes les opérations de l'exercise dans des comptes "opérations_cloturées"
* Mettre à jour le solde initial de l'exercise et le prendre en compte dans le solde du compte.

### Sauvegarde - Restauration

Comme le logiciel le schema des base de données évolue et doit être versionné. Les nouvelles fonctionnalités s'appuient sur de nouvelles tables ou champs et ne fonctionnent que si le schema est à jour.

Sous git les révisions du logiciel sont sauvegardées et la version du schéma correspond à l'ensemble des procédures de migration contenues dans une révision données.

Cela implique:

- de faire migrer la base de données lors de la mise à jour du logiciel.

Que faire en cas de tentative de restauration d'une base de données avec un schéma antérieur au logiciel courant ? Le nombre de migration dans la table migrations pourrait être utilisé comme marqueur de version mais il est possible de compacter les migrations donc d'en réduire le nombre ...

Une fois détécté il prévenir l'utilisateur.  

## Schéma de la base de données