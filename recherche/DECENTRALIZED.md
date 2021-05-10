# Centre de données décentralisés par et pour la communauté Drave Développement

## Introduction

Drave Développement mets de l'avant ses membres comme une équipe d'administrateurs systèmes pour figurer l'auto-hébergement et le nuage décentralisé pour le Québec (et la francophonie).

En utilisant les mêmes technologies logiciels libres qui gèrent les grands infonuages comme Kubernetes de Google) et particulièrement leur implémentation simplifiée pour l'informatique en périphérie (edge computing: IoT, microcloud) microk8s (micro-kubernetes de Canonical) et des réseaux virtualisés pairs à pairs comme VpnCloud, on travaille à créer un centre de données décentralisé qui pourra:

* rouler des nodes d'applications distribuées
* des services fédérés
* des applications avec haute disponibilité.

Ceci permettra de créer des grappes (clusters) virtuels pour des communautés (association, fédération, régions, consortiums).

Ces communautés mutualisent leurs ressources d'infrastructure et de compétences techniques entre elles par des accords commerciaux.

La fédération de services décentralisées permet de mettre en place la redondance de services et la durabilité des données.

Au centre de notre stratégie, le déploiement de services avec un service d'authentification et d'authorisation unifié (Identity Provider) et fédérable accompagnée de gestion de profils usager stockable en Pods SOLID.


## copy config to your client

```
microk8s config
```

into: ~/.kube/config

## allow browser insecure localhost

chrome://flags/#allow-insecure-localhost

## port forward using ssh

```
ssh -L 30777:localhost:30777 alligator.local
```

# instructions that don't work yet

## forwarding ports from remote cluster

```
brew install txn2/tap/kubefwd
```

```
sudo kubefwd svc -n kube-system
```

## accessing page
```
scp alligator.local:/var/snap/microk8s/current/certs/ca.crt .
```

https://github.com/ubuntu/microk8s/issues/1046

## VPN Cloud

```
sudo apt install vpncloud
sudo vpncloud -l 9090 --password helloworld --ip 10.0.0.76 --fix-rp-filter
```