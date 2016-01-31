---
title: Fix NFS on Arch/Manjaro for Vagrant
date: 09/07/2014

I tried to use a Vagrant box on my Manjaro system, but it failed miserably on the NFS portion during startup.  It turns out that NFS isn't cut-and-dry on Arch.  Luckily, the fix is pretty easy.

Using the trusty, but often cryptic and wordy [NFS Arch Wiki](https://wiki.archlinux.org/index.php/NFS), I managed to get 99% of the way there.  Here are the basic steps:

    sudo pacman -S nfs-utils
    sudo systemd enable nfs-server.service
    sudo systemd enable nfs-client.service

You obviously only need client or server as your situation merits.  However this didn't do it.  Since I'm using manjaro, it didn't come with a key component that chef was looking for in order to determine which system I was running.  All I had to do to fix this was add an empty file, like so:

    sudo touch /etc/arch-release

And, magically, it works!


# Maybe this is what I really did:

Make the nfs directories:

     sudo mkdir -p /srv/nfs4/rs/{stock,kraken}

Enable the services so they start at boot

     systemctl enable nfs-client.target
     systemctl enable nfs-server.service
     systemctl enable rpcbind.service

Not sure about these ones:

     systemctl enable rpc-mountd
     systemctl enable rpc-idmapd
     systemctl enable nfs-server.target

And all "start" commands for the above as well:

     systemctl enable nfs-client.target
     systemctl enable nfs-server.service
     systemctl enable rpcbind.service
     ...

Also maybe:

     sudo pacman -S net-tools

# Other things I did for sure:

## exports

    # /etc/exports - exports(5) - directories exported to NFS clients
    #
    # Example for NFSv2 and NFSv3:
    #  /srv/home        hostname1(rw,sync) hostname2(ro,sync)
    # Example for NFSv4:
    #  /srv/nfs4      hostname1(rw,sync,fsid=0)
    #  /srv/nfs4/home   hostname1(rw,sync,nohide)
    # Using Kerberos and integrity checking:
    #  /srv/nfs4        *(rw,sync,sec=krb5i,fsid=0)
    #  /srv/nfs4/home   *(rw,sync,sec=krb5i,nohide)
    #
    # Use `exportfs -arv` to reload.
    /srv/nfs4/ 192.168.33.1(rw,fsid=root,no_subtree_check)
    /srv/nfs4/rs/stock  192.168.33.10(rw,no_subtree_check,nohide)
    /srv/nfs4/rs/kraken 192.168.33.10(rw,no_subtree_check,nohide)

## fstab

    # /etc/fstab: static file system information.
    #
    # Use 'blkid' to print the universally unique identifier for a
    # device; this may be used with UUID= as a more robust way to name devices
    # that works even if disks are added and removed. See fstab(5).
    #
    # <file system> <mount point>   <type>  <options>       <dump>  <pass>
    #
    UUID=99257ca2-4aed-4dbe-92ef-9cc0031b5ea2 / ext4 defaults,noatime,discard 0 1
    tmpfs /tmp tmpfs defaults,noatime,mode=1777 0 0
    /home/trevor/rs/stock  /srv/nfs4/rs/stock none bind 0 0
    /home/trevor/rs/kraken /srv/nfs4/rs/kraken none bind 0 0
