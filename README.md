# zfs-askpassword@.service

Use systemd-askpassword before zfs-mount but after zpool-import to prompt for passwords

## Installation:

Save the 2 files as /etc/systemd/system/zfs-askpassword@.service and /etc/systemd/system/zfs-remount.service

## Declaring the encryption root in your pool

Replace the slashes with hypers, so `your_pool/encryption_root` would becomes `your_pool-encryption_root` in the examples below.

Then run:

    # systemctl daemon-reload && systemctl enable zfs-askpassword@escaped-path-to-the-encryption_root && systemctl enable zfs-remount
    # systemctl enable "zfs-askpassword@your_pool-your-encryption_root.service

## Using at boot time

# On arch, using mkinitrdcpio.conf, edit /etc/mkinitcpio.conf to have:

    FILES=(... whatever you already have ... /etc/systemd/system/emergency.service /etc/systemd/system/zfs-askpassword.service /usr/lib/systemd/system/systemd-ask-password-console.path /usr/lib/systemd/system/sysinit.target.wants/systemd-ask-password-console.path /usr/lib/systemd/system/systemd-ask-password-console.service)

    BINARIES=(... whatever you already have .. then add . console.path systemd-ask-password systemd-tty-ask-password-agent)

Then recreate your initrd with mkinitrdcpio:

    mkinitcpio --config /etc/mkinitcpio.conf --kernel $VERSION$LOCALVERSION --generate $WHERE$INITRD

# On ubuntu, the same should work, but the file paths could be different

Instead of mkinitrdcpio, use:

    mkinitramfs -o $INITRD $VERSION -v
