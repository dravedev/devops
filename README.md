# Infrastructure décentralisée de Drave Développement

## quécéça?

Ce projet permet d'installer, de mettre à jour et d'assurer la conformité des nodes de l'infrastructure décentralisée de Drave Développement.

Chaque draveur-e-s peut y partager des ordinateurs ou des serveurs auto-hébergés qui hébergent les services de Drave Développement.

Chaque draveur-e-s est responsable de l'administration de sa propre node mais permet à d'autres draveur-e-s de déployer des services.

## ça mange quoi en hiver?

ce dépôt de sources permet de configurer:

* les secrets liés à une node (seulement connus par le draveur propriétaire de la node)
* l'accès à la node par les autres draveurs
* l'endurcissement de la sécurité de la node
* le lien entre la node, son ip publique et le nom de domaine sur drave.dev
* la connectivité à d'autres nodes

et éventuellement (en cours), le déploiement du logiciel de gestion de grappes (Kubernetes) qui permettra aux draveurs de gérer l'ensemble des nodes comme un infonuage décentralisé.

## connaissances requises

* familiarité avec l'administration système
* familiarité avec ansible
* familiarité avec la réseautique

Besoin d'aide?

Nous avons des sessions de devops hebdomadaire pour t'aider à faire le déploiement.

## diagramme

![déploiement](https://docs.google.com/drawings/d/e/2PACX-1vTa78mF1kfxvuAnbf4CuvN8c0V2arBZDebamzPuV3rOY3BiMqzobTFue9L4Z4jXE_NMHRQiIcCZbtAr/pub?w=960&h=720)

dans le texte, les équivalents français:

* deployment station: lanceur
* New node: node

## Pré-requis

* assure toi d'être dans l'équipe Github: https://github.com/orgs/dravedev/people
  * nous allons migrer à autre nom système de gestion de sources une fois l'infrastructure déployé
* choisi le nom de ta node
  * la convention est un terme terminologique de la drave traditionnelle
  * http://abcstrategies.com/lexique-draveurs-cageux-bucherons/
* demande la création de la node comme enregistrement DNS dynamique synthétique sur le domaine drave.dev
  * un nom d'usager et mot de passe te sera retourné pour configurer ton client DNS dynamique

## configuration de ton environnement de développement sur le lanceur

- assure toi d'avoir sur ta station de travail (le lanceur du script de configuration):

  -  ansible 2.9+
  -  ta clée ssh privée associée à ta clée publique de ton utilisateur github

* clone le répository devops

    git@github.com:dravedev/devops.git

* installe les dépendances

    ansible-galaxy install -r requirements.yml

## installation sur la node

* Installe Ubuntu 20.04 sur l'ordinateur que tu veux dédier comme node du réseau drave

## configuration des draveurs

Le fichier `draveurs.yml` contient les noms des utilisateurs des personnes qui ont signé le [Serment des Draveur-e-s](https://serment.drave.dev) et qui sont autoriser à rejoindre le réseau. Le nom d'usager est celui présent sur Github.

## configuration de l'inventaire inventory.yml

Si ton usager local sur ton lanceur ne correspond pas, tu peux spécifier un usager alternatif en configurant la variable `owner` passée à Ansible.

* modifie inventory.yml pour ajouter ta node sous `all:hosts`:

     <NOM_NODE>:

* ajoute ton nom d'usager dans `children` avec ta (ou tes) node(s) définies dans `all:hosts`:

  children:
    <NOM_USAGER_NODE>:
      hosts:
        <NOM_NODE>:


## configuration des variables propres à ta node

création d'un répertoire

    mkdir -p ./host_vars/$NOM_NODE
    touch ./host_vars/$NOM_NODE/$NOM_NODE.yml
    touch ./vault/$NOM_NODE.yml

configure les valeurs des variables spécifique à ta node

    domain: $NOM_NODE.drave.dev
    private_ip: $IP_VPN_NODE
    dyndns: true
    inadyn:
        username: "{{ vault_inadyn_username }}"
        password: "{{ vault_inadyn_password }}"

configure les secrets dans ```./vault/$NOM_NODE.yml```

    ---
    vault_inadyn_username: <NOM_USAGER_DNS_DYNAMIQUE>
    vault_inadyn_password: <MOT_DE_PASSE_DNS_DYNAMIQUE>

encryption du fichier de secret (choisi ton mot de passe quand demandé)

    ansible-vault encrypt ./vault/$NOM_NODE.yml

une fois encryptée, tu peux éditer avec:

    ansible-vault edit ./vault/$NOM_NODE.yml

Par défaut, la configuration défault de Ansible (ansible.cfg) lit de ./.vault_password, un fichier qui est à créer. Ce fichier doit contenir le mot de passe choisi ci-haut pour encrypter le vault. Ce fichier est exclu du git par .gitignore et ne devrait donc exister que localement sur votre machine.

## accès à la node

Tu peux créer le nom correspondant dans ton fichier de configuration ssh (`~/.ssh/config`):

    Host <NOM_NODE>
        HostName <IP_NODE>
        User <NOM_USAGER_NODE>
        ForwardAgent yes

ALTERNATIVEMENT

Si le nom réseau de la node ou l'IP différe sur le réseau local du nom de la node, tu peux configurer avec ansible_host:

    -e ansible_host=<IP_NODE>

Si le nom d'usager sur la node différe de ton nom actuel, tu peux modifier avec ansible_user:

    -e ansible_user=<NOM_USAGER_NODE>


## Initialisation usager ansible (une seule fois, possiblement optionnel)

Si tes nodes n'ont pas d'utilisateur avec droits sudo portant le même nom que l'utilisateur lançant ansible, tu peux utiliser le playbook `setup.yml` pour en mettre en place un:

Appliquer le playbook d'initialisation des utilisateurs:

    ansible-playbook setup.yml --ask-become-pass

D'autres options sont disponibles pour effacer des usagers ajoutés accidentellement:

    ansible-playbook setup.yml -e user=DRAVEUR_NAME -e delusers=OLDUSER1,OLDUSER2 --ask-become-pass

## Installation


Lancer le playbook pour configurer vos nodes et les joindre au réseau vpncloud de Drave Développement:

    ansible-playbook sites.yml

Si ton utilisateur local n'est pas le même que le nom d'usager Github / le nom de compte créé sur la node, tu peux spécifier la variable owner:

    ansible-playbook -e owner=$NOM_USAGER_NODE sites.yml

Les tâches basés sur les rôles ont aussi des libellés (tags) qui permettent de rouler seulement un sous-ensemble:

    ansible-playbook sites.yml -t reseau_drave

## Logs

Voir

    /var/log/vpncloud-drave.stats
    /var/log/vpncloud-drave.log


## Connecte toi à une autre node par ssh

démarre ssh-agent:

    ssh-add

connecte toi à ta node

    ssh $NOM_NODE

et ensuite, sur ta node, connecte toi à d'autres node du vpncloud

    ssh $IP_VPN_NODE

## Partage ta node

Si il s'agit d'une nouvelle node, tu as ajouté des information au code. Pour les partager:

* Crée une nouvelle branche
* Commit tes changements localement
* Pousse ta branche à Github
* Crée un pull request sur Github
* informe le groupe de ton nouveau serveur

# Contribution

* Rejoins la discussion dans le salon de discussion RocketChat: https://rocketchat.drave.quebec/channel/devops
* Sélectionne un problème à résoudre: https://github.com/dravedev/devops/issues
