# Infrastructure décentralisée de Drave Développement

Ce projet permet d'installer, de mettre à jour et d'assurer la conformité des nodes de l'infrastructure décentralisée de Drave Développement.

Chaque draveur-e-s peut y partager des nodes qui sont des ordinateurs ou des serveurs auto-hébergés.

## Inscription d'un draveur

Vous devez avoir un utilisateur github avec une clef ssh dans votre compte.

Ajouter le nom de votre utilisateur dans la liste des draveur-e-s dans `draveurs.yml`

## Inscription d'une node

Générerez-vous une clef:

    vpncloud genkey

Mettez la clef privée dans les variables de votre host de node (`vpn_private_key`). La partie publique ira dans le fichier `nodes.yml`.

Ajouter les informations de la node dans le fichier `nodes.yml`:

    name: <nom de la node>
    domain: <url ou ip public>
    vpn_port: <no de port (optionnel)>
    private_ip: <ip sur le réseau vpn>
    public_key: <clef publique  vpncloud>
    proprietaire: <propriétaire de la node (optionnel)>

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
