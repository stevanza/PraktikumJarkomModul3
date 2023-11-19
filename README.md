# PraktikumJarkomModul3
Kelompok E05
Stevanza Gian Maheswara - 5025221248
Fanza Khairan Pratama - 5025221305

1. Topologi
   ![image.png]( https://github.com/stevanza/PraktikumJarkomModul3/blob/main/NO1.png )

2. Buat domain
   riegel.canyon.yyy.com untuk worker Laravel
   granz.channel.yyy.com untuk worker PHP
   mengarah pada worker yang memiliki IP [prefix IP].x.1.
   
   Heiter
   apt-get update
   apt-get install bind9 -y
   
   Ubah konfigurasi
   nano /etc/bind/named.conf.local
   
   zone "canyon.e05.com" {
   	type master;
   	file "/etc/bind/canyon/canyon.e05.com";
   };
   
   zone "channel.e05.com" {
   	type master;
   	file "/etc/bind/channel/channel.e05.com";
   };
   
   ![image.png]( https://github.com/stevanza/PraktikumJarkomModul3/blob/main/NO2a.png )

   Buat dan copy directory ke db.local
   mkdir /etc/bind/canyon 
   mkdir /etc/bind/channel
   cp /etc/bind/db.local /etc/bind/canyon/canyon.e05.com
   cp /etc/bind/db.local /etc/bind/channel/channel.e05.com 
   
   Ubah konfigurasi canyon
   nano /etc/bind/canyon/canyon.e05.com

   ![image.png]( https://github.com/stevanza/PraktikumJarkomModul3/blob/main/NO2b.png )

   Ubah konfigurasi canyon
   nano /etc/bind/channel/channel.e05.com

   ![image.png]( https://github.com/stevanza/PraktikumJarkomModul3/blob/main/No2c.png )
   
   Restart bind9

   service bind9 restart
   
   Lakukan testing pada domain dan subdomain yang telah dibuat
   Himmel:
   
   echo nameserver 192.208.1.2 > /etc/resolv.conf
   
   Lakukan ping domain dan sub domain

   ![image.png]( https://github.com/stevanza/PraktikumJarkomModul3/blob/main/NO2d.png )
   ![image.png]( https://github.com/stevanza/PraktikumJarkomModul3/blob/main/NO2e.png )
   ![image.png]( https://github.com/stevanza/PraktikumJarkomModul3/blob/main/No2f.png )
   ![image.png]( https://github.com/stevanza/PraktikumJarkomModul3/blob/main/NO2g.png )

3. 3.1 DHCP Server
   echo nameserver 192.168.122.1 > /etc/resolv.conf
   apt-get update
   apt-get install isc-dhcp-server -y
   Menentukan interface
   
   nano /etc/default/isc-dhcp-server
   INTERFACESv4="eth0"
   
   ![image.png]( https://github.com/stevanza/PraktikumJarkomModul3/blob/main/NO3a.png )

   Konfigurasi DHCP Server

   nano /etc/dhcp/dhcpd.conf
   Kemudian isi dengan
   
   
   subnet 10.39.1.0 netmask 255.255.255.0 {  
   }
   
   subnet 10.39.2.0 netmask 255.255.255.0 {
   }
   
   subnet 10.39.3.0  netmask 255.255.255.0 {
     range 10.39.3.16 10.39.3.32;
     range 10.39.3.64 10.39.3.80;
     option routers 10.39.3.0;
     option broadcast-address 10.39.3.255;
     option domain-name-servers 10.39.1.2;
     default-lease-time 180;
     max-lease-time 5760;
   }
   
   subnet 10.39.4.0  netmask 255.255.255.0 {
     range 10.39.4.12 10.39.4.20;
     range 10.39.4.160 10.39.4.168;
     option routers 10.39.4.0;
     option broadcast-address 10.39.4.255;
     option domain-name-servers 10.39.1.2;
     default-lease-time 720;
     max-lease-time 5760; 
   }

   ![image.png]( https://github.com/stevanza/PraktikumJarkomModul3/blob/main/NO3b.png )

   Restart DHCP Server

   service isc-dhcp-server restart
   3.2 DHCP Relay
   apt-get update
   apt-get install isc-dhcp-relay -y
   
   Lakukan konfigurasi DHCP Relay
   
   nano /etc/default/isc-dhcp-relay
   SERVERS="192.208.1.1"  
   INTERFACES="eth1 eth2 eth3 eth4"
   OPTIONS=

   ![image.png]( https://github.com/stevanza/PraktikumJarkomModul3/blob/main/NO3c.png )
   
   Lakukan konfigurasi IP Forwarding

   nano /etc/sysctl.conf
   Kemudian isi dengan
   
   net.ipv4.ip_forward=1

   ![image.png]( https://github.com/stevanza/PraktikumJarkomModul3/blob/main/NO3d.png )
   
   Restart DHCP Relay

   service isc-dhcp-relay restart
   3.3 Client DHCP
   Ubah konfigurasi interface client
   
   nano /etc/network/interfaces
   Kemudian isi dengan
   
   auto eth0
   iface eth0 inet dhcp

   ![image.png]( https://github.com/stevanza/PraktikumJarkomModul3/blob/main/NO3e.png )
   
   Restart client dhcp
   Dengan cara GNS3 → klik kanan pada client DHCP → klik Stop → klik kanan kembali pada client DHCP → klik Start
   
   Cek apakah client mendapatkan IP yang sesuai

   ![image.png]( https://github.com/stevanza/PraktikumJarkomModul3/blob/main/NO3f.png )
   ![image.png]( https://github.com/stevanza/PraktikumJarkomModul3/blob/main/NO3g.png )
   ![image.png]( https://github.com/stevanza/PraktikumJarkomModul3/blob/main/NO3h.png )
   ![image.png]( https://github.com/stevanza/PraktikumJarkomModul3/blob/main/NO3i.png )

