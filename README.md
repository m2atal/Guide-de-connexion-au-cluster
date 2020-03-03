Nous avons subi un certain nombres de problèmes en M2 ATAL sur l'année 2018-2019.
Ces problèmes étaient au final assez évitables mais nous n'avions pas toutes les cartes en mains dès le début pour y faire face.
Pour éviter que ça se répète, voici quelques conseils et remarques.
En sachant que vous n'allez pas subir les même problèmes que nous, donc il va vous falloir innover de ce point de vue.

# Concernant tout le monde

## Petit guide de ~~communication~~ survie en milieu étudiant

1. Quand un problème est constaté (par exemple surcharge de travail), confronter (gentiment) l'intervenant.
2. Si vous n'arrivez pas à engager une discussion autour du problème, n'insistez pas, contactez l'intéressé par mail.
3. Si ça ne donne rien, parlez en au responsable de formation.

Selon les cas, l'étape 2. peut être court-circuitée.
Les mails semblent mieux fonctionner, peut-être parce que le destinataire a plus le temps d'ingérer et de comprendre le message.

Gardez à l'esprit que les responsables de formation n'ont pas de pouvoir de décision particulier sur les autres intervenants.
Typiquement, si les intervenants refusent d'admettre un problème, c'est juste parce qu'ils ne le voient pas.
C'est donc à vous de leur faire voir.

# Spécifique à Nantes

## Connection ssh sur les serveurs du Mans
Courte explication sur comment se connecter en ssh à skinner.
Pour plus de détail, voir [https://wiki.univ-nantes.fr/doku.php?id=etudiants:restreint:bastion_out]()

### Sur un réseau non bloqué

L'identifiant du mans est de la forme s123456.
```sh
ssh identifiant@transit.univ-lemans.fr
ssh skinner
```

Ou en une ligne (nécessite cependant de taper deux fois le mdp) :
```sh
ssh -A -t -l identifiant_univ_lemans transit.univ-lemans.fr ssh -A skinner
```

### Sur le réseau de la fac

Pour passer à travers le réseau de la fac, établir un tunnel ssh entre bastion et transit puis se connecter en l'utilisant :
```sh
ssh -f -N -L:port_local:transit.univ-lemans.fr:22 identifiant_univ_nantes@bastion.etu.univ-nantes.fr
ssh -A -t identifiant_lemans@localhost -p port_local ssh -A skinner
```

Où
 - port_local est un port TCP libre supérieur à 1024 (basiquement un numéro quelquonque entre 1024 et 65535)
 - identifiant\_univ\_nantes est le numéro d'étudiant de nantes, de la forme E123456L

Possible en une ligne, en sachant qu'il n'est pas nécessaire de réétablir le tunnel ssh s'il est déjà en place et que tenter de réétablir un tunnel peut faire planter la connexion :
```sh
ssh -f -N -L:port_local:transit.univ-lemans.fr:22 identifiant_univ_nantes@bastion.etu.univ-nantes.fr && ssh -A -t identifiant_lemans@localhost -p port_local ssh -A skinner
```
En utilisant cette commande, il faut donc entrer une fois le mot de passe univ nantes et deux fois le mot de passe univ lemans.

## Récuperer un fichier des serveurs du mans sur votre machine

Vous allez probablement avoir besoin de récuperer un graphe ou un PDF depuis les cluster sauf que GRANDE TRISTESSE les serveurs n'ont pas d'interface graphique et ouvrir le joli dessin que vous avez fait est inouvrable...
Afin de récuperer le fichier et l'ouvrir sur votre PC il vous faudra utiliser la commande SCP de bash

Pour se faire vous avez besoin de deux terminaux:
 - un connecté à skinner
 - un autre pour votre PC



Dans le terminal ou se trouve skinner placez vous dans le dossier qui contient le fichier que vous voulez récuperer et faite
```sh
scp le_nom_du_fichier.extension identifiant_lemans@transit.univ-lemans.fr:le_nouveau_nom_du_fichier.extension
```
Où
 - le_nom_du_fichier.extension correspond au nom du fichier tel qu'il est inscrit sur skinner ainsi que son extension
 - identifiant_lemans est exactement le meme que pour la connexion ssh
 - le_nouveau_nom_du_fichier.extension est le nouveau nom du fichier il n'est pas nécessairement le meme que le_nom_du_fichier mais c'est quand même mieux. L'extension doit evidement rester la même
 
 En utilisant cette commande, il faut donc entrer une fois le mot de passe univ lemans.
### sur le réseau de la fac
Ensuite assurez vous que vous êtes bien toujours connecté à la passerelle __**bastion**__ et depuis le second terminal faites
```sh
scp -P port_local identifiant_lemans@localhost:le_nouveau_nom_du_fichier.extension le_chemin_absolue_de votre_PC
```
Où
 - port_local doit être précisement le meme que celui pour votre connexion ssh
 - identifiant_lemans est le meme que pour la connexion ssh
 - le_nouveau_nom_du_fichier.extension doit etre le même que plus haut
 - le_chemin_absolue_de votre_PC est la zone ou vous voulez stocker votre fichier en chemin absolue pour linux par exemple ce sera /home/Dupont/Documents/
 
 En utilisant cette commande, il faut donc entrer une fois le mot de passe univ lemans.

### Sur un réseau non bloqué
Depuis le second terminal faites

```sh
scp identifiant_lemans@transit.univ-lemans.fr:le_nouveau_nom_du_fichier.extension le_chemin_absolue_de votre_PC
```

Où
 - identifiant_lemans est le meme que pour la connexion ssh
 - le_nouveau_nom_du_fichier.extension doit etre le même que plus haut
 - le_chemin_absolue_de votre_PC est la zone ou vous voulez stocker votre fichier en chemin absolue pour linux par exemple ce sera /home/Dupont/Documents/
 
 En utilisant cette commande, il faut donc entrer une fois le mot de passe univ lemans.

Et voila vous avez récuperer votre fichier!

## Envoyer un fichier sur les serveurs du mans depuis votre machine

Vous allez probablement avoir besoin d'envoyer aussi des fichiers
Afin de récuperer le fichier et l'ouvrir sur votre PC il vous faudra utiliser la commande SCP de bash encore une fois

Pour se faire vous avez toujours besoin de deux terminaux:
 - un connecté à skinner
 - un autre pour votre PC

Dans un premier temps prenez le terminal connecté a votre PC
### sur le réseau de la fac
assurez vous que vous êtes bien toujours connecté à la passerelle __**bastion**__ et faites
```sh
scp -P port_local le_chemin_de votre_fichier identifiant_lemans@localhost:le_nouveau_nom_du_fichier.extension 
```
Où
 - port_local doit être précisement le meme que celui pour votre connexion ssh
 - identifiant_lemans est le meme que pour la connexion ssh
 - le_nouveau_nom_du_fichier.extension est le nom du fichier que vous souhaitez (oui vous pouvez renomer vos fichier)
 - le_chemin_absolue_de votre_fichier est le chemin a emprunter depuis le répertoire courant jusqu'a votre fichier a envoyer
 
 En utilisant cette commande, il faut donc entrer __une fois le mot de passe univ lemans__.

### Sur un réseau non bloqué
C'est bien plus simple faites

```sh
scp le_chemin_absolue_de votre_fichier identifiant_lemans@transit.univ-lemans.fr:le_nouveau_nom_du_fichier.extension 
```

Où
 - identifiant_lemans est le meme que pour la connexion ssh
 - le_nouveau_nom_du_fichier.extension doit etre le même que plus haut
 - le_chemin_absolue_de votre_fichier est le chemin a emprunter depuis le répertoire courant jusqu'a votre fichier a envoyer
 
 En utilisant cette commande, il faut donc entrer __une fois le mot de passe univ lemans__.

Ensuite prenez le terminal ou se trouve skinner et faites :
```sh
scp identifiant_lemans@transit.univ-lemans.fr:le_nouveau_nom_du_fichier.extension le_nom_du_fichier.extension
```
Où
 - le_nom_du_fichier.extension correspond au nom du fichier tel que vous voulez le nommer sur skinner
 - identifiant_lemans est exactement le meme que pour la connexion ssh
 - le_nouveau_nom_du_fichier.extension doit etre exactement le meme que celui de la premiere commande scp 
 
 En utilisant cette commande, il faut donc entrer __une fois le mot de passe univ lemans__.


Et voila vous avez envoyer votre fichier!


## Salles et emploi du temps
### Salles sans visio
Les salles de tp du CIE n'ayant pas de visio, il est typiquement préférable que chacun amène sa machine et que les tps aient lieu dans des salles avec visio.

Gardez tout de même l'oeil sur l'emploi du temps, puisque de temps en temps, sans raison apparente, des cours apparaissent dans des salles sans visio.

### Ouverture des salles
En plus des badges activés pour la salle U3, il est possible de désigner un étudiant référent, à même de récupérer un badge universel à l'accueil en échange de sa carte d'étudiant.
Pour mettre ça en place, voir avec le responsable de Nantes.
