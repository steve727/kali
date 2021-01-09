# Update kali
    apt-get update
    apt-get upgrade
    apt-get dist-upgrade
    
    sudo apt update && sudo apt full-upgrade -y
    
    sudo apt autoremove -y
    
# Set static ip
    sudo vim /etc/network/interfaces

        auto eth0
        iface eth0 inet static
        address 192.168.254.9/24
        gateway 192.168.254.254
        
    sudo systemctl restart networking.service
    
    ip a 
    
    sudo rm /etc/resolv.conf
    sudo vim /etc/resolv.conf
    
        nameserver 8.8.8.8
        
    sudo chattr +i /etc/resolv.conf

        
    
    
