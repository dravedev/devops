# Infrastructure décentralisée de Drave Développement

Ce projet permet d'installer, de mettre à jour et d'assurer la conformité des nodes de l'infrastructure décentralisée de Drave Développement.

Chaque draveur-e-s peut y partager des nodes qui sont des ordinateurs ou des serveurs auto-hébergés.


## Préparation de la node

Installer Ubuntu 20.04.

## Prérequis

  - ansible 2.9+

Installer les roles et collections requises:
    ansible-galaxy install -r requirements.yml

## Initialisation des nodes

Si vos nodes n'ont pas d'utilisateur avec droits sudo portant le même nom que l'utilisateur lançant ansible, alors:

Fournir un inventaire d'initialisation

    [initialisation]
    NODE_NAME ansible_host=NODE_LAN_IP  ansible_user=INIT_USERNAME ...

Appliquer le playbook d'initialisation des utilisateurs:

    ansible-playbook -i INVENTAIRE setup.yml

S'assurer d'avoir la clef ssh de votre draveur sur votre lanceur ansible.


## Usage

Fournir un inventaire:

    [initialisation]
    NODE_NAME ansible_host=NODE_LAN_IP  ansible_user=INIT_USERNAME ...

Lancer le playbook:

    ansible-playbook -i INVENTAIRE sites.yml
