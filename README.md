# Infrastructure décentralisée de Drave Développement

Ce projet permet d'installer, de mettre à jour et d'assurer la conformité des nodes de l'infrastructure décentralisée de Drave Développement.

Chaque draveur-e-s peut y partager des ordinateurs ou des serveurs auto-hébergés qui hébergent les services de Drave Développement.

## diagramme

![déploiement](https://docs.google.com/drawings/d/e/2PACX-1vTa78mF1kfxvuAnbf4CuvN8c0V2arBZDebamzPuV3rOY3BiMqzobTFue9L4Z4jXE_NMHRQiIcCZbtAr/pub?w=960&h=720)

# Démarrage d'une node de draveur-e-s

## Avant de commencer

Pré-requis:

- assure toi d'être dans l'équipe Github: https://github.com/orgs/dravedev/people
- assure toi d'avoir sur ta station de travail (le lanceur du script de configuration):

  -  ansible 2.9+
  -  ta clée ssh privée associée à ta clée publique de ton utilisateur github

NOTE: Nous prenons pour acquis que ton utilisateur local sur ton lanceur est le même que ton usager sur github et est le même que le nom du propriétaire de chaque node.

Si votre usager local sur votre lanceur ne correspond pas, vous devez corriger la situation en configurant la variable `owner` passée à Ansible.

* choisi le nom de ta node (convention est un terme terminologique de la drave traditionnelle)
  * http://abcstrategies.com/lexique-draveurs-cageux-bucherons/
* demande la création de la node comme enregistrement DNS dynamique synthétique sur le domaine drave.dev
  * un nom d'usager et mot de passe te sera retourné pour configurer ton client DNS dynamique

## Inscription d'une node au réseau de Drave Développement

* Installe Ubuntu 20.04 sur l'ordinateur que tu veux dédier comme node
* clone le répository devops
* installe les dépendances

    ansible-galaxy install -r requirements.yml

## configuration des draveurs

Le fichier `draveurs.yml` contient les noms des utilisateurs des personnes qui ont signé le [Serment des Draveur-e-s](https://serment.drave.dev) et qui sont autoriser à rejoindre le réseau. Le nom d'usager est celui présent sur Github.

## configuration de l'inventaire

* modifie inventory.yml pour ajouter ta node sous `all:hosts`:

     <HOSTNAME>:
       domain: <HOSTNAME>.drave.dev
       private_ip: 10.0.0.<IP non-alloué par d'autres nodes>
       ansible_host: <IP sur ton réseau local>
       ansible_user: <nom usager à utiliser pour accéder à la node en ssh>

* ajoute ton nom d'usager dans `children` avec ta (ou tes) node(s) définies dans `all:hosts`:

  children:
    <USERNAME>:
      hosts:
        <HOSTNAME>:


Pour déclarer l'ip de vos nodes, vous pouvez le faire dans `~/.ssh/config`:

    Host <HOSTNAME>
        HostName <REACHABLE_IP>
        User <USERNAME>
        ForwardAgent yes

## Initialisation:

Si tes nodes n'ont pas d'utilisateur avec droits sudo portant le même nom que l'utilisateur lançant ansible, tu peux utiliser le playbook `setup.yml`

Appliquer le playbook d'initialisation des utilisateurs:

    ansible-playbook setup.yml --ask-become-pass

D'autres options sont disponibles pour effacer des usagers ajoutés accidentellement:

    ansible-playbook setup.yml -e user=DRAVEUR_NAME -e delusers=OLDUSER1,OLDUSER2 --ask-become-pass

## Installation

Lancer le playbook pour configurer vos nodes et les joindre au réseau vpncloud de Drave Développement:

    ansible-playbook sites.yml

Si ton utilisateur local n'est pas le même que le nom d'usager Github / le nom de compte créé sur la node, tu peux spécifier la variable owner:

    ansible-playbook -e owner=TON_NOM_USAGER sites.yml

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

    ssh alligator.local

et ensuite connecte toi à la node du vpncloud

    ssh 10.0.0.77

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
