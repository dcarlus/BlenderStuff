# Installation des serveurs auto-hébergés

## Galène
Galène est un serveur de visioconférence open-source et gratuit. Attention, il nécessite des dépendances qui ne sont peut-être pas disponible pour un processeur ARM, ce qui écarte potentiellement son installation sur un Raspberry Pi. En revanche, aucun problème pour une installation sur un dispositif ou ordinateur équipé d'un processeur x86 ou x86_64 (Intel ou AMD).

Installer les outils pour compiler le langage Go sur une distribution Linux (cela sera la solution la plus simple pour mettre en place le serveur par rapport aux autres OS). A noter que Galène nécessite au minimum la version 1.13 de Go, une version supérieure conviendra également.

Télécharger ou cloner le dépôt GitHub officiel de Galène : https://github.com/jech/galene

Dans le dossier, taper la commande suivante :
`CGO_ENABLED=0 go build -ldflags='-s -w'`

Quand c'est fini, normalement c'est très rapide, suivre les instructions du fichier README.md pour créer et configurer le(s) groupe(s) (càd salon(s) de visioconférence), les comptes administrateur, ...

La configuration la plus simple consiste en la création d'un fichier `NOM_GROUPE.json` (remplacer `NOM_GROUPE` par le nom du groupe souhaité) dans le dossier `groups`, créer ce dossier si besoin, à ne pas confondre avec le dossier `group` déjà existant. Voici le contenu du fichier, en remplaçant les données par le nom d'utilisateur et son mot de passe souhaité.
```json
{
    "op": [{"username": "USER_NAME", "password": "USER_PASSWD"}],
    "presenter": [{}]
}
```

Une fois la configuration prête, lancer le serveur avec la commande `./galene` dans un terminal. Il devrait y avoir quelques logs suite au démarrage du serveur.

Pour tester si le serveur fonctionne, ouvrir un navigateur web et rentrer l'adresse du serveur ainsi que le port 8443 (TCP) ou 1194 (TCP/UDP). Par exemple, si le serveur tourne sur la même machine que pour le test, rentrer l'adresse `https://localhost:8443` ou bien `https://localhost:1194`. Notez que le `https://` est obligatoire pour chiffrer les communications, Galène n'autorisant pas un accès autre que chiffré tel que le protocole HTTP simple.

## Etherpad
Etherpad est un serveur pour écrire des documents collaborativement, c'est-à-dire que plusieurs personnes peuvent participer à l'écriture d'un document simultanément.

Etherpad a `nodejs >= 10.17.0` pour seule dépendance. Il faut donc l'installer avant de pouvoir lancer le serveur Etherpad.

Sous une distribution Debian/Ubuntu, l'installation d'Etherpad est d'une très grande simplicité. Il suffit en effet d'entrer les commandes indiquées sur le GitHub officiel du projet à l'adresse suivante : https://github.com/ether/etherpad-lite. Voici les commandes à rentrer successivement :

```bash
curl -sL https://deb.nodesource.com/setup_14.x | sudo -E bash -
sudo apt install -y nodejs
git clone --branch master https://github.com/ether/etherpad-lite.git
cd etherpad-lite
src/bin/run.sh
```

Le serveur devrait pouvoir se lancer. Pour vérifier son fonctionnement, lancer un navigateur web et rentrer l'adresse du serveur sur le port 9001. Si le serveur se trouve sur la même machine que pour le test, l'adresse est donc : `https://localhost:9001`.

### Installation d'un Raspberry Pi sur disque dur externe et Raspberry Pi OS
Le Raspberry Pi est un micro-ordinateur dont le disque dur est remplacé par une carte microSD sur les versions les plus récentes. Cependant, ce type de carte est prévu pour du stockage de fichiers. Par exemple des photos, vidéos sur un appareil photo, de la musique en plus sur un téléphone ou des logiciels dans une console de jeu vidéo. Ceci n'impliquant pas de trop nombreuses écritures. Un système d'exploitation écrit énormément de données sur le disque, comme par exemple les logs, les fichiers temporaires de programmes, etc. Ces écritures très nombreuses et répétées dans des cases mémoires identiques entraînent non seulement la dégradation prématurée de la mémoire flash, mais aussi peut rapidement rendre carte microSD corrompue et inutilisable.

Afin de pallier à ce gros problème rendant le Raspberry Pi peut fiable pour un usage de type serveur, il est possible d'installer un système d'exploitation sur un disque externe (HDD, SSD) lequel sera conçu pour supporter un système d'exploitation. Le Raspberry Pi n'a cependant pas été conçu pour démarrer sur un disque externe en USB. Si le modèle dont on dispose est un peu "vieux" (au moins avant fin 2020), il est nécessaire de flasher la mémoire morte du Raspberry Pi pour mettre à jour ce qu'on appelle un bootloader, un peu à la manière du BIOS. Le nouveau bootloader permettra de définir les périphériques depuis lesquels on autorise le démarrage (carte microSD, disque externe, ethernet...) ainsi que l'ordre des priorités.

Pour ce faire, une excellente vidéo explique parfaitement le principe pour un Raspberry Pi 4 (attention, la procédure est peut-être différente sur un autre modèle). En voici le lien : https://www.youtube.com/watch?v=JZ0UfofD0zs. Les étapes sont un peu longues mais il n'y a rien de compliqué.

A noter que depuis que cette vidéo a été postée, de petites modifications ont été apportées sur le nom des bootloader. Ainsi, il ne faudra pas choisir un bootloader "stable" comme dans la vidéo, mais "default". Cela correspond à un bootloader très stable qui peut être installé d'office dans les Rapsberry Pi sortis depuis. Le bootloader "default" du 3 septembre 2020 permet par exemple un démarrage depuis un disque externe. Il se peut que si le Raspberry Pi dont vous disposez a été produit après cette date, il dispose déjà d'un bootloader compatible avec le boot USB (il n'y aurait donc rien à faire).

Dernière précision, le Rapsberry Pi OS permet directement de booter via un disque USB. Ce n'est pas forcément le cas de tous les OS, comme par exemple ArchLinux ARM qui peut nécessiter des opérations supplémentaire, plus ou moins complexes.
