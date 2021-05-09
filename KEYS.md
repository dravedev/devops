
* chaque draveur a une clée IPFS privée de création de contenu
* cette clée est utilisé pour publiée l'entrée IPNS
* cette entrée IPNS pointe vers un répertoire de contenu qui peut être mis à jour
* le fait d'être dans le DNS avec une entrée TXT confirme l'appartenance à l'organisation


# key generation

create and export key

    ipfs key gen --type=rsa --size=2048 $USER

add DNS entry for user pointing to key

    rngadam.drave.dev.	60	IN	TXT	"dnslink=/ipns/k2k4r8pe3fd83npsehplboibt4xij42blvjg9s6b0bwo57qsq2o7y9xx"

export and backup securely

    ipfs key export $USER --output private/$USER.ipfs.key

add data to ipfs (the root directory CID is the $CID after)

    ipfs add -r public/$USER

publish the data on the CID:

    ipfs name publish --key=$USER $CID

run the daemon

    ipfs daemon

# updating

re-add:

    ipfs add -r public/rngadam

re-publish

    ipfs name publish --key=$USER $CID
