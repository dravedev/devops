# microk8s

## contexte

microk8s est la distribution de Canonical pour alléger le déploiement en utilisant Kubernetes.

## installation

Installe microk8s:

```
sudo snap install microk8s --classic --channel=1.21
```

Configure ton utilisateur pour pouvoir administrer Kubernetes:

```
sudo usermod -a -G microk8s $USER
sudo chown -f -R $USER ~/.kube
sudo su - $USER
```

Joint ta nouvelle node à la grappe en exécutant sur un des nodes dans la grappe existante la commande suivante:

```
microk8s add-node
```

sur la nouvelle node, exécute la sortie de la ligne de commande précédente.

## accède l'interface web

### copy config to your client

```
microk8s config
```

into: ~/.kube/config

### allow browser insecure localhost

chrome://flags/#allow-insecure-localhost

### renvoie de port de kube à votre machine

```
sudo kubefwd svc -n kube-system
```

utilisé le port ci-haut pour créer un tunnel ssh:

```
ssh -L 30777:localhost:30777 alligator.local
```

ouvrir https://localhost:30777