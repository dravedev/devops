# Installation, mise à jour et conformité de l'infrastructure de Drave Développement

## Usage

    # Installe les rôles externes
    ansible-galaxy install -r requirements.yml


    # Setup minimal sur une machine "neuve"
    #   - configurer l'inventaire
    #   - configurer les credentials initiaux
    ansible-playbook -i inventaire setup.yml \
        -e '{ "draveur" : "DRAVEUR" , "init_hosts": "INIT_HOSTS" }'  \
        -e @draveurs.yml \
        INVENTAIRE_ET_CREDENTIALS


    # Playbook idempotent permettant d'installer, de mettre à jour et d'assurer la conformité de l'infrastructure de Drave Développment
    ansible-playbook sites.yml \
       INVENTAIRE_ET_CREDENTIALS
