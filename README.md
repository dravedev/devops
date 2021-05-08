# Infrastructure décentralisée de Drave Développement

Ce projet permet d'installer, de mettre à jour et d'assurer la conformité des nodes de l'infrastructure décentralisée de Drave Développement.

Chaque draveur-e-s peut y partager des ordinateurs ou des serveurs auto-hébergés qui hébergent les services de Drave Développement.

Le fichier `draveurs.yml` contient les noms des utilisateurs des personnes qui ont signé le [Serment des Draveur-e-s](https://serment.drave.dev) et qui sont autoriser à rejoindre le réseau.

Cependant, vous pouvez utiliser ce code pour créer votre propre communauté ou pour administrer votre propre réseau de serveurs. Il suffit de modifier `draveurs.yml`

# Démarrage d'une node de draveur-e-s

## Prérequis et préparation de la node sur la machine cible

* Installer Ubuntu 20.04.
* Ajouter une résolution de DNS pour <HOSTNAME>.drave.dev vers l'IP publique de votre accès Internet auquel la machine cible est connecté
  * vpncloud va ouvrir le port 3120 automatiquement en configurant votre routeur en utilisant UPnP

## Inscription d'une node

Exemple d'inventaire (inventaire):

    [initialisation]
    <HOSTNAME> ansible_host= <HOSTNAME>.local ansible_user=<VOTRE USAGER>

    [owned_nodes]
     <HOSTNAME>

dans nodes.yml:

   nodes:
     - name: <HOSTNAME>
       domain: <HOSTNAME>.drave.dev
       private_ip: 10.0.0.<IP non-alloué autrement>

## Prérequis sur le lanceur

  - ansible 2.9+
  - avoir localement la clef ssh de votre utilisateur github

Installer les roles et collections requises:

    ansible-galaxy install -r requirements.yml

## Initialisation des nodes

Si vos nodes n'ont pas d'utilisateur avec droits sudo portant le même nom que l'utilisateur lançant ansible, vous pouvez utiliser le playbook `setup.yml`

Fournir un inventaire d'initialisation ```inventaire```

    [initialisation]
    NODE_NAME ansible_host=NODE_LAN_IP_OR_HOSTNAME  ansible_user=INIT_USERNAME

    [owned_nodes]
    NODE_NAME

## Initialisation

Appliquer le playbook d'initialisation des utilisateurs:

    ansible-playbook -i inventaire setup.yml --ask-become-pass

S'assurer d'avoir la clef ssh de votre draveur sur votre lanceur ansible.

## Installation

Lancer le playbook:

    ansible-playbook -i INVENTAIRE sites.yml

## Autres

D'autres options sont disponibles pour effacer des usagers ajoutés accidentellement:

    ansible-playbook -i INVENTAIRE setup.yml -e user=DRAVEUR_NAME -e delusers=OLDUSER1,OLDUSER2 --ask-become-pass