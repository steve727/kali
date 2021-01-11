# Update kali
    apt-get update
    apt-get upgrade
    apt-get dist-upgrade
    
    sudo apt update && sudo apt full-upgrade -y
    
    sudo apt autoremove -y
    
    sudo apt autoclean -y
    sudo apt clean -y

# Add a normal user
    useradd -m -G sudo -s /bin/bash steve
    passwd steve
    echo '%sudo ALL=(ALL) NOPASSWD: ALL' >> /etc/sudoers    
    
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

# Boot without gui
    systemctl get-default
    sudo systemctl set-default multi-user.target

# Simple python http server
    
    sudo python -m SimpleHTTPServer 80
    
# Bettercap
    apt install bettercap
    
# airmon-ng
    airmon-ng check kill
    airmon-ng start wlan1
    iwconfig
    sudo airodump-ng -c 9 wlan1mon
    
# bettercap
    bettercap --iface wlan1mon
    wifi.recon
    
