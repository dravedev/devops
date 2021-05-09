why ipfs

    https://github.com/WebOfTrustInfo/rwot1-sf/blob/master/topics-and-advance-readings/ipfs-keychain.md

install

    sudo snap install ipfs

https://github.com/lucas-clemente/quic-go/wiki/UDP-Receive-Buffer-Size

    sysctl -w net.core.rmem_max=2500000

.config/systemd/user/ipfs.service

    [Unit]
    Description=InterPlanetary File System (IPFS) daemon
    Documentation=https://docs.ipfs.io/
    After=network.target

    [Service]
    ExecStart=/snap/bin/ipfs daemon --init --migrate --mount
    Restart=on-failure
    TimeoutStartSec=infinity
    KillSignal=SIGINT

    [Install]
    WantedBy=default.target

https://github.com/ipfs/go-ipfs/tree/master/misc#systemd

    loginctl enable-linger $USER
    systemctl --user enable ipfs
    systemctl --user start ipfs

setup directories (from ipfs mount):

  > sudo mkdir /ipfs /ipns
  > sudo chown $(whoami) /ipfs /ipns


https://github.com/ipfs/go-ipfs/blob/master/docs/fuse.md

    /etc/fuse.conf

    user_allow_other


    ipfs config --json Mounts.FuseAllowOther true