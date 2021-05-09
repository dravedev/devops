# why ipfs

    https://github.com/WebOfTrustInfo/rwot1-sf/blob/master/topics-and-advance-readings/ipfs-keychain.md

# installation

need the v0.8.0 of ipfs binary which includes a daemon:

https://docs.ipfs.io/install/ipfs-updater/#install-updater
https://dist.ipfs.io/#ipfs-update

# firewall


https://medium.com/pinata/how-to-deploy-an-ipfs-node-on-digital-ocean-c59b9e83098e
    # peers
    sudo ufw allow 4001/tcp
    # api
    sudo ufw allow 5001/tcp
    # gateway
    sudo ufw allow 8080/tcp
    # pubsub
    sudo ufw allow 8081/tcp

https://medium.com/textileio/tutorial-setting-up-an-ipfs-peer-part-ii-67a99cd2c5

    ipfs config Addresses.Swarm '["/ip4/0.0.0.0/tcp/4001", "/ip4/0.0.0.0/tcp/8081/ws", "/ip6/::/tcp/4001"]' --json
    ipfs config --bool Swarm.EnableRelayHop true

# setup

https://github.com/lucas-clemente/quic-go/wiki/UDP-Receive-Buffer-Size

    sysctl -w net.core.rmem_max=2500000

.config/systemd/user/ipfs.service

    [Unit]
    Description=InterPlanetary File System (IPFS) daemon
    Documentation=https://docs.ipfs.io/
    After=network.target

    [Service]
    # https://docs.ipfs.io/how-to/configure-node/#profiles
    # with public IP, use --profile server
    ExecStart=/usr/local/bin/ipfs daemon --init --migrate
    Restart=on-failure
    TimeoutStartSec=infinity
    KillSignal=SIGINT

    [Install]
    WantedBy=default.target

https://github.com/ipfs/go-ipfs/tree/master/misc#systemd

    loginctl enable-linger $USER
    systemctl --user enable ipfs
    systemctl --user start ipfs

# own bootstrap list

https://docs.ipfs.io/how-to/modify-bootstrap-list/

# debugging

    IPFS_LOGGING=verbose ipfs daemon

# with --mount
setup directories (from ipfs mount):

  > sudo mkdir /ipfs /ipns
  > sudo chown $(whoami) /ipfs /ipns


https://github.com/ipfs/go-ipfs/blob/master/docs/fuse.md

    /etc/fuse.conf

    user_allow_other

    ipfs config --json Mounts.FuseAllowOther true


https://dist.ipfs.io/#ipfs-update

# join swarm

https://medium.com/textileio/tutorial-setting-up-an-ipfs-peer-part-i-de48239d82e0



# testing swarm

    ipfs swarm peers

# exporting files

https://dnslink.io/


add files:

    ipfs add -r public/

keep root directory ipfs and create dns record https://domains.google.com/registrar/drave.dev/dns:

    _dnslink.drave.dev.	60	IN	TXT	"dnslink=QmRw8y6rsYcmBQmSdecQC5Qiek6CgqMZ28Xk3zp1X6BE1F"

list it:

    ipfs ls /ipns/drave.dev

# publishing to http

https://medium.com/textileio/tutorial-setting-up-an-ipfs-peer-part-ii-67a99cd2c5

# cluster creation

https://cluster.ipfs.io/documentation/collaborative/setup/

# about snap distribution


it's possible to install:

    sudo snap install ipfs

however this fails doing a filesystem mounti
