# Update kali
    apt-get update
    apt-get upgrade
    apt-get dist-upgrade
    
    sudo apt update && sudo apt upgrade -y 
    sudo apt update && sudo apt full-upgrade -y
    
    sudo apt autoremove -y
    
    sudo apt autoclean -y
    sudo apt clean -y

# Add a normal user
    sudo useradd -m -G sudo -s /bin/bash steve
    sudo passwd steve
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

# Simple python http server in any directory
    
    sudo python -m SimpleHTTPServer 80
    
# Bettercap
    sudo apt update
    sudo apt install golang git build-essential libpcap-dev libusb-1.0-0-dev libnetfilter-queue-dev
    go get -u github.com/bettercap/bettercap
    
    Location:
    go/bin/bettercap
    
    sudo bettercap -caplet https-ui
    
    /usr/local/share/bettercap/caplets/http-ui.cap
        
# airmon-ng
    sudo airmon-ng check kill
    sudo airmon-ng start wlan1
    sudo iwconfig
    sudo airodump-ng -c 9 wlan1mon
    sudo airodump-ng wlan1mon
    
# bettercap usage
    bettercap --iface wlan1mon
    wifi.recon on
    wifi.show
    
    set ticker.commands 'clear; net.show; events.show 10'
    net.probe on
    ticker on

# Kismet
    git clone https://www.kismetwireless.net/git/kismet.git
        
        sudo apt-get install build-essential git libmicrohttpd-dev zlib1g-dev libnl-3-dev libnl-genl-3-dev libcap-dev libpcap-dev 
        libncurses5-dev libnm-dev libdw-dev libsqlite3-dev
        
        sudo apt install build-essential git libwebsockets-dev pkg-config zlib1g-dev libnl-3-dev libnl-genl-3-dev libcap-dev libpcap-dev libnm-dev libdw-dev libsqlite3-dev libprotobuf-dev libprotobuf-c-dev protobuf-compiler protobuf-c-compiler libsensors4-dev libusb-1.0-0-dev python3 python3-setuptools python3-protobuf python3-requests python3-numpy python3-serial python3-usb python3-dev python3-websockets librtlsdr0 libubertooth-dev libbtbb-dev

    cd kismet
    ./configure
    make
    
    sudo usermod -aG kismet $USER
    
# Probequest
    sudo pip3 install --upgrade probequest
    
    sudo airmon-ng start wlan1
    sudo probequest -i wlan1mon 
    sudo airodump-ng wlan1mon
    
# hcxtools
    git clone https://github.com/ZerBea/hcxdumptool.git
    cd hcxdumptool
    make
    sudo make install
    
    sudo apt install libcurl4-openssl-dev
    
    git clone https://github.com/ZerBea/hcxtools.git
    cd 
    make
    sudo make install
    
    sudo airmon-ng check kill
    sudo airmon-ng start wlan1
    sudo hcxdumptool -i wlan1mon -o galleria.pcapng --enable_status=1
    
    sudo hcxpcaptool -E essidlist -I identitylist -U usernamelist -z galleriaHC.16800 galleria.pcapng-0
    hashcat -m 16800 galleriaHC.16800 -a 0 --kernel-accel=1 -w 4 --force 'topwifipass.txt'
    
    sudo hcxdumptool -i wlan1mon -o testing.pcapng 
 
 # seclists
    sudo apt -y install seclists
