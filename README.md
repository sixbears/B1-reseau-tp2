# B1 Réseau 2018 - TP2

## Exploration locale en solo

### Affichage d’informations sur la pile TCP/IP locale

J’ai ouvert PowerShell et tapé la commande « ipconfig /all »
Interface WIFI :
Carte réseau sans fil wifi
Adresse MAC : Adresse physique 84-A6-C8-06-FF-9A
Adresse IP : 10.33.2.81
Adresse de réseau : 10.33.0.0
Adresse de broadcast : 10.33.3.255
Interface Ethernet :
Carte Ethernet Ethernet
Adresse MAC : Adresse 8C-89-A5-05-8C-88
Adresse IP : Pas d’adresse IP
Adresse de réseau : 
Adresse de broadcast : 

Dans les informations de la carte Wifi, en tapant « ipconfig /all », on trouve l’adresse IP de « passerelle par défaut ».

Pour trouver les informations sur la carte IP, en utilisant l’interface graphique, il suffit d’ouvrir le centre de réseau et partage en cliquant droit sur l’icône du wifi dans le menu en bas à droite de l’écran. J’ai ensuite cliqué sur « modifier les paramètres de la carte ». Il suffit ensuite de faire un clic droit sur la carte voulu puis cliquer sur « statut » et aller dans « détails ».

La Gateway est un nœud agissant comme pivot, elle permet de sortir du réseau (du réseau Ingésup vers Internet par exemple).


### Modifications des informations

#### Modification d’adresse IP – pt.1	

La première IP du réseau est : 10.33.0.1 et la dernière est : 10.33.3.254 car 10.33.3.255 est l’adresse de broadcast et 10.33.3.253 est l’adresse gateway

Pour changer l’adresse IP de la carte wifi, en utilisant l’interface graphique, il suffit d’ouvrir le centre de réseau et partage en cliquant droit sur l’icône du wifi dans le menu en bas à droite de l’écran. J’ai ensuite cliqué sur la connexion wifi sur laquelle j’étais connecté, en l’occurrence celle d’YNOV, puis sur « propriétés ». Il faut ensuite sélectionner le protocole Internet version 4 et cliquer sur propriétés. Il suffit ensuite de cocher « Utiliser l’adresse IP suivante » et écrire l’adresse IP, le masque, la passerelle et le serveur DNS voulus.

#### Nmap

![screen_Nmap](/Images/1.png)


#### Modification d’adresse IP – pt.2

A partir des résultats de la commande Nmap fait précédemment nous savons quelles adresses sont utilisées et nous pouvons donc choisir une adresse inutilisée et appliquer la manip vue au-dessus. 

## Exploration locale en duo

### Prérequis

firewall désactivés

### Cablage

Hop

### ?création du réseau

Hop

### modification d'adresses IP

On place les 2 réseaux dans le meme masqeu de sous réseau
Pour vérifier que les changements d’adresse ip ont fait effets, on tape la commande ipconfig et on regarde les configurations carte ethernet.
/20 masque sous réseaux : 255.255.240.0
Envoi d’une requête 'Ping'  172.16.15.1 avec 32 octets de données :
Réponse de 172.16.15.1 : octets=32 temps=1 ms TTL=128
Réponse de 172.16.15.1 : octets=32 temps<1ms TTL=128
Réponse de 172.16.15.1 : octets=32 temps<1ms TTL=128
Réponse de 172.16.15.1 : octets=32 temps<1ms TTL=128
/24 masque sous réseaux : 255.255.255.0
 Envoi d’une requête 'Ping'  172.16.15.1 avec 32 octets de données :
Réponse de 172.16.15.1 : octets=32 temps=2 ms TTL=128
Réponse de 172.16.15.1 : octets=32 temps=2 ms TTL=128
Réponse de 172.16.15.1 : octets=32 temps=2 ms TTL=128
Réponse de 172.16.15.1 : octets=32 temps<1ms TTL=128
Pour le plus petit masque de sous réseau on a choisi le /30 (masque : 255.255.255.252) parce qu’après ça marche plus, il n’y a pas assez d’adresse disponible dans le masque réseau.

### Utilisation d'un des 2 comme gateway
Serveur :
Aller dans réseau et partage, connexion wifi internet, propriété, partage, cocher « autoriser d’autres utilisateurs du réseau à se connecté via la connexion internet de cet ordinateur », connexion réseau domestique : Ethernet
Client :
J’ai rentré l’adresse IP du serveur en tant que passerelle. Et mis en adresse DNS 8.8.8.8 qui est l’adresse du serveur de google. . 
Pour pouvoir se connecter il fallait entrer 8.8.8.8 en DNS. 
Nous avons réussi à nous connecter à internet par la suite. 


### Petit chat privé
* PC1 (serveur)
`nc.exe -l -p 8888`
* PC2 (client)
`nc.exe 172.16.15.22  8888`

### firewall
Pour pouvoir se ping avec les pare feux activés
Paref-feu windows, paramètre avancé, règles de traffic entrant/sortant, activer la règle « partage de connexion internet (traffic entrant/sortant DHCPv4) »
Pour pouvoir communiquer avec netcat avec les pare feux activés sur le Pc serveur.
Paref-feu windows, paramètre avancé, nouvelle règle, port, et spécifié dans « ports locaux spécifiques » 8888, cocher autoriser la connexion, définir le nom et terminer.

### Wire shark 
Ping :

![screen_Nmap](/Images/2.png)  
  
pc1/pc2
![screen_Nmap](/Images/3.png)  
  
Netcat
![screen_Nmap](/Images/4.png)  

#### Manipulations d'autres outils/protocoles coté client 

### DHCP

En executant ipconfig /all, dans le reseau wifi on trouve les informations suivantes :
* Serveur DHCP . . . . . . . . . . . . . : 192.168.1.254
* Bail expirant. . . . . . . . . . . . . : mardi 8 janvier 2019 16:32:00

Pour changer d’adresse ip en ligne commande il faut ouvrir l’invite de commande en mode administrateur et puis entrer:
netsh interface ipv4 set address name="Wi-Fi" static 192.168.1.8 255.255.255.0 192.168.1.254
Dans mon cas, je souhaite dans mon réseau Wi-Fi choisir comme nouvel  ipv4 192.168.1.8 avec 255.255.255.0 en masque de sous-réseau et 192.168.1.254 en passerelle par défaut.

### DNS

En executant ipconfig /all, dans le reseau wifi on trouve les informations suivantes :
* Serveurs DNS. . .  . . . . . . . . . . : 192.168.1.254

* nslookup google.com
C:\Users\Hugo>nslookup google.com
Serveur :   bbox.lan
Address:  192.168.1.254

Réponse ne faisant pas autorité :
Nom :    google.com
Addresses:  2a00:1450:4007:805::200e
          216.58.204.142

*nslookup ynov.com
C:\Users\Hugo>nslookup ynov.com
Serveur :   bbox.lan
Address:  192.168.1.254

Réponse ne faisant pas autorité :
Nom :    ynov.com
Address:  217.70.184.38
* reverse lookup 78.78.21.21
C:\Users\Hugo>nslookup 78.78.21.21
Serveur :   bbox.lan
Address:  192.168.1.254

Nom :    host-78-78-21-21.mobileonline.telia.com
Address:  78.78.21.21

*reverse lookup 96.16.54.88
C:\Users\Hugo>nslookup 96.16.54.88
Serveur :   bbox.lan
Address:  192.168.1.254

Nom :    a96-16-54-88.deploy.static.akamaitechnologies.com
Address:  96.16.54.88
