# Update kali
    apt-get update
    apt-get upgrade
    apt-get dist-upgrade

# Set static ip
    sudo vim /etc/network/interfaces

        auto eth0
        iface eth0 inet static
        address 192.168.0.100/24
        gateway 192.168.0.1
