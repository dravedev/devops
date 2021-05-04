

```
~/.kube/config
```

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