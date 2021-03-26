## Playbook Ansible d'initialisation de serveurs

### Prérequis :
- [Ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html)
- [nickjj.user](https://galaxy.ansible.com/nickjj/user) et [geerlingguy.docker](https://galaxy.ansible.com/geerlingguy/docker)

### Variables :
Éditer group_vars/all

### Démarrer l'installation :
ansible-playbook -i inventory.ini playbook.yml

### Démarrer à un endroit précis du playbook :
Utiliser le nom d'un "tags:" présent dans playbook.yml.
exemple : ansible-playbook -i inventory.ini -t install-docker playbook.yml