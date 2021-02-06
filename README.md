# Update Kali (Raspberry Pi4)  
    sudo apt update && sudo apt upgrade -y 
    sudo apt update && sudo apt full-upgrade -y
    sudo apt dist-upgrade
    
    sudo apt autoremove -y
    sudo apt autoclean -y
    sudo apt clean -y
    
# Prevent system from sleeping    
    sudo systemctl mask sleep.target suspend.target hibernate.target hybrid-sleep.target
  
# Passwordless sudo
    sudo apt install -y kali-grant-root && sudo dpkg-reconfigure kali-grant-root

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

# Install ssh
    apt install openssh-server
    mkdir /etc/ssh/default_keys
    mv /etc/ssh/ssh_host_* /etc/ssh/default_keys/
    dpkg-reconfigure openssh-server
    vim /etc/ssh/sshd_config
    systemctl enable ssh.service
    systemctl start ssh.service
    systemctl status ssh.service

# Boot without gui
    systemctl get-default
    sudo systemctl set-default multi-user.target

# Simple python http server in any directory
    
    sudo python -m SimpleHTTPServer 80
    
# Bettercap
    sudo apt install golang git build-essential libpcap-dev libusb-1.0-0-dev libnetfilter-queue-dev
    go get -u github.com/bettercap/bettercap
    
    /usr/local/share/bettercap/caplets/https-ui.cap
   
    bettercap -caplet https-ui --iface wlan1mon

    wifi.recon on
    set wifi.show.sort clients desc
    set ticker.commands 'clear; wifi.show'
    ticker on

    set ticker.commands 'clear; net.show; events.show 10'
    net.probe on
    ticker on

    cd /usr/share/bettercap/caplets
    vi https-ui.cap
        
# airmon-ng
    sudo airmon-ng check kill
    sudo airmon-ng start wlan1
    sudo iwconfig
    sudo airodump-ng -c 9 wlan1mon
    sudo airodump-ng wlan1mon
    
# bettercap usage
    bettercap -caplet https-ui --iface wlan0mon
    
    wifi.recon on
    
    set wifi.show.sort clients desc
    set ticker.commands 'clear; wifi.show'
    ticker on
    
    wifi.recon.channel 11
    
    set ticker.commands 'clear; net.show; events.show 10'
    net.probe on
    ticker on
    
    ## Set https user/pw:
    vi /usr/share/bettercap/caplets/https-ui.cap
    
    wifi.show.wps

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
    
# hcxdump / hcxtools
    git clone https://github.com/ZerBea/hcxdumptool.git
    cd hcxdumptool
    make
    sudo make install
    sudo apt-get install libcurl4-openssl-dev libssl-dev pkg-config zlib1g-dev
    
    git clone https://github.com/ZerBea/hcxtools.git
    cd 
    make
    sudo make install
   
    hcxdumptool -i wlan1mon -o test001.pcapng --enable_status=1 
    hcxpcaptool -E essidlist001 -I identitylist001 -U usernamelist001 test001.pcapng -o output001
    
    hashcat -m 16800 testcap -a 0 --kernel-accel=1 -w 4 --force 'rockyou.txt'
    
    https://wpa-sec.stanev.org/?
    
 # seclists
    sudo apt -y install seclists
    
 # Wifi Interface Settings
    iwconfig
    airmon-ng check kill
    ifconfig wlan0 down
    ifconfig wlan1 down
    iw reg set GY
    iw wlan1 set txpower fixed 30 
    macchanger -a wlan0
    ifconfig wlan0 up
    airmon-ng start wlan0
    ifconfig wlan0mon down
    macchanger -a wlan0mon
    ifconfig wlan0mon up

 # mdk4  
    sudo apt install mdk4
    
    ifconfig wlan1 down
    iw phy1 interface add mon0 type monitor
    iw phy1 interface add mon1 type monitor
    iw phy1 interface add mon2 type monitor
    iw wlan1 del
    ifconfig mon0 up
    ifconfig mon1 up
    ifconfig mon2 up
    mdk4 mon0 e -t [TARGET] -s 100
    mdk4 mon1 e -t [TARGET] -s 100
    mdk4 mon2 e -t [TARGET] -s 100
    
 # wpa supplicant
    vi /etc/wpa_supplicant.conf
        
        ctrl_interface=/var/run/wpa_supplicant
        ctrl_interface_group=0
        update_config=1
    	
    wpa_supplicant -Dwext -iwlan1 -c/etc/wpa_supplicant.conf –B
    wpa_cli
    
 # Wordlists
    sudo gzip -d /usr/share/wordlists/rockyou.txt.gz
    
 # wifite
    wifite --wps --ignore-locks --crack
    
