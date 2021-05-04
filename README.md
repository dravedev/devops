# Infrastructure décentralisée de Drave Développement

Ce projet permet d'installer, de mettre à jour et d'assurer la conformité des nodes de l'infrastructure décentralisée de Drave Développement.

Chaque draveur-e-s peut y partager des ordinateurs ou des serveurs auto-hébergés qui hébergent les services de Drave Développement.

Le fichier `draveurs.yml` contient les noms des utilisateurs des personnes qui ont signé le [Serment des Draveur-e-s](https://serment.drave.dev) et qui sont autoriser à rejoindre le réseau.

 Cependant, vous pouvez utiliser ce code pour créer votre propre communauté ou pour administrer votre propre réseau de serveurs. Il suffit de modifier `draveurs.yml`

# Démarrage d'une node de draveur-e-s

## Inscription d'une node

Générerez-vous une clef:

    vpncloud genkey

Mettez la clef privée dans les variables de votre host de node (`vpn_private_key`). La partie publique ira dans le fichier `nodes.yml`.

Ajouter les informations de la node dans le fichier `nodes.yml`:

   nodes:
     - name: alligator
       domain: alligator.drave.dev
       private_ip: 10.0.0.1
       public_key: 5HPdhX88rbSrHk4l2ddr2spxzxFEZQAGM92hwsDRgHF
       proprietaire: rngadam

## Prérequis et préparation de la node

Installer Ubuntu 20.04.

## Prérequis sur le lanceur

  - ansible 2.9+
  - avoir localement la clef ssh de votre utilisateur github

Installer les roles et collections requises:

    ansible-galaxy install -r requirements.yml

## Initialisation des nodes

Si vos nodes n'ont pas d'utilisateur avec droits sudo portant le même nom que l'utilisateur lançant ansible, vous pouvez utiliser le playbook `setup.yml`

Fournir un inventaire d'initialisation

    [initialisation]
    NODE_NAME ansible_host=NODE_LAN_IP  ansible_user=INIT_USERNAME ...

    [owned_nodes]
    NODE_NAME

Appliquer le playbook d'initialisation des utilisateurs:

    ansible-playbook -i INVENTAIRE setup.yml -e user=DRAVEUR_NAME -e delusers=OLDUSER1,OLDUSER2 --ask-become-pass

S'assurer d'avoir la clef ssh de votre draveur sur votre lanceur ansible.


## Installation

Fournir un inventaire:

    [initialisation]
    NODE_NAME ansible_host=NODE_LAN_IP  ansible_user=INIT_USERNAME \
      vpncloud_private_key: PRIVATE_KEY ...

Lancer le playbook:

    ansible-playbook -i INVENTAIRE sites.yml
