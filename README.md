## Playbook Ansible d'initialisation de serveurs

### Ce que ça fait :
- Crée les comptes des administrateurs via une liste dans une variable (sudoer sans mot de passe à fournir)
- Importe les clés publiques SSH des administrateurs via GitHub puis les associe aux comptes des administrateurs
- Installe Docker et docker-compose

### Prérequis :
- [Ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html)
- [nickjj.user](https://galaxy.ansible.com/nickjj/user), [geerlingguy.pip](https://galaxy.ansible.com/geerlingguy/pip), [geerlingguy.docker](https://galaxy.ansible.com/geerlingguy/docker) et [ypsman.hostname](https://galaxy.ansible.com/ypsman/hostname)

### Variables :
Éditer [group_vars/all](group_vars/all)

### Démarrer l'installation :
```ansible-playbook -i inventory.ini -u nom_utilisateur_github playbook.yml```

### Démarrer à un endroit précis du playbook :
Utiliser le nom d'un "tags:" présent dans playbook.yml

exemple : ```ansible-playbook -i inventory.ini -u nom_utilisateur_github -t install-docker playbook.yml```