https://www.digitalocean.com/community/tutorials/how-to-install-node-js-on-ubuntu-20-04

    curl -sL https://deb.nodesource.com/setup_14.x -o nodesource_setup.sh
    sudo bash nodesource_setup.sh
    sudo apt-get install -y nodejs gcc g++ make
    sudo npm install -g @hyperspace/cli
    hyp drive sync hyper://46ca522e624739408b325fcc25c630be00146a66beebf095d96103a2558585f0// test