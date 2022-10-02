## Exercice 1. Adressage IP (rappels)

Vous administrez le réseau interne 172.16.0.0/23 d’une entreprise, et devez gérer un parc de 254 machines
réparties en 7 sous-réseaux. La répartition des machines est la suivante :


- Sous-réseau 1 : 38 machines/ ADDRESSE SOUS RESEAU = 172.16.0.0/26 / ADDRESSE BROADCAST = 172.16.0.63 / 1ère et dernière adresse machine : 172.16.0.1 ; 172.16.0.62
- Sous-réseau 2 : 33 machines/ ADDRESSE SOUS RESEAU = 172.16.0.64/26 / ADDRESSE BROADCAST = 172.16.0.127 / 1ère et dernière adresse machine : 172.16.0.65 ; 172.16.0.126
- Sous-réseau 3 : 52 machines/ ADDRESSE SOUS RESEAU = 172.16.0.128/26 / ADDRESSE BROADCAST = 172.16.0.191 / 1ère et dernière adresse machine : 172.16.0.129 ; 172.16.0.190
- Sous-réseau 4 : 35 machines/ ADDRESSE SOUS RESEAU = 172.16.0.192/26 / ADDRESSE BROADCAST = 172.16.0.255 / 1ère et dernière adresse machine : 172.16.0.193 ; 172.16.0.254
- Sous-réseau 5 : 34 machines/ ADDRESSE SOUS RESEAU = 172.16.1.0 /26 / ADDRESSE BROADCAST = 172.16.1.63 / 1ère et dernière adresse machine : 172.16.1.1 ; 172.16.1.62
- Sous-réseau 6 : 37 machines/ ADDRESSE SOUS RESEAU = 172.16.1.64 /26 / ADDRESSE BROADCAST = 172.16.1.127 / 1ère et dernière adresse machine : 172.16.1.65 ; 172.16.1.126
- Sous-réseau 7 : 25 machines/ ADDRESSE SOUS RESEAU = 172.16.1.128 /27 / ADDRESSE BROADCAST = 172.16.1.159 / 1ère et dernière adresse machine : 172.16.1.129 ; 172.16.1.158

## Exercice 2. Préparation de l’environnement

1. VM éteintes, utilisez les outils de configuration de VirtualBox pour mettre en place l’environnement
décrit ci-dessus.

Configuration du serveur avec deux cartes réseau (DCHP et reseau local ) :

![image](https://user-images.githubusercontent.com/80455696/193029055-1ede88a8-2bf7-4f5b-8d3c-07a6caa883f4.png)

Configuration du client avec une carte réseau :

![image](https://user-images.githubusercontent.com/80455696/193029550-3f4fe7c4-194e-4c9a-98e8-c1c2999b5242.png)

3. Pour desinstaller le cloud-init il faut tapper :
          sudo apt remove cloud-init

4. 

Nom du serveur :

![image](https://user-images.githubusercontent.com/80455696/193036429-2b31a9b2-2fc5-4fce-9b54-c075548a5f62.png)

Nom du client :

![image](https://user-images.githubusercontent.com/80455696/193036677-b999594e-880b-4408-ad51-37686e1154c9.png)


## Exercice 3. Installation du serveur DHCP

1. Sur le serveur, installez le paquet isc-dhcp-server. La commande systemctl status isc-dhcp-server devrait vous indiquer que le serveur n’a pas réussi à démarrer, ce qui est normal puisqu’il n’est pas encore configuré (en particulier, il n’a pas encore d’adresses IP à distribuer):

                    sudo apt install isc-dhcp-server                    


2. Un serveur DHCP a besoin d’une IP statique. Attribuez de manière permanente l’adresse IP 192.168.100.1 à l’interface réseau du réseau interne. Vérifiez que la configuration est correcte.

![image](https://user-images.githubusercontent.com/80455696/193046521-8fb629a0-2704-458e-817c-79c78a40164c.png)

3. 

Fichier dhcpd.conf :

![image](https://user-images.githubusercontent.com/80455696/193048899-f2e3b12d-18dd-4e81-825e-0f31d06cafda.png)

Les deux premières lignes sont :

Le default-lease-time qui est la durée en minutes ou en secondes pendant laquelle un périphérique réseau peut utiliser une adresse IP dans un réseau. L'adresse IP est réservée pour cet appareil jusqu'à l'expiration de la réservation.

Le max-lease-time qui est la durée maximal en minutes ou en secondes pendant laquelle un périphérique réseau peut utiliser une adresse IP dans un réseau.

4.Editez le fichier /etc/default/isc-dhcp-server afin de spécifier l’interface sur laquelle le serveur doit écouter. 

![image](https://user-images.githubusercontent.com/80455696/193051087-62e4b73d-32b5-44e0-9d1c-62b35b717bb0.png)

5. Validez votre fichier de configuration avec la commande dhcpd -t puis redémarrez le serveur DHCP (avec la commande systemctl restart isc-dhcp-server) et vérifiez qu’il est actif.

Pour redemarrer le systemctl il faut tapper la commande systemctl restart isc-dhcp-server et ensuite pour verifier si le systeme est actif on tappe la commande suivante systemctl status isc-dhcp-server

![image](https://user-images.githubusercontent.com/80455696/193054137-445033a6-f65a-4a14-9dd8-c6e85ccf841d.png)


7. La commande tail -f /var/log/syslog affiche de manière continue les dernières lignes du fichier
de log du système (dès qu’une nouvelle ligne est écrite à la fin du fichier, elle est affichée à l’écran).
Lancez cette commande sur le serveur, puis connectez la carte réseau du client et observez les logs
sur le serveur. Expliquez à quoi correspondent les messages DHCPDISCOVER, DHCPOFFER, DHCPREQUEST,
DHCPACK. Vérifiez que le client reçoit bien une adresse IP de la plage spécifiée précédemment.


![image](https://user-images.githubusercontent.com/80455696/193062013-460b9181-88c4-4631-b11a-52f4e3902386.png)

DHCPDISCOVER : C'est un paquet qui sert à détecter les serveurs DHCP disponibles.

DHCPOFFER : C'est un paquet qui répond à un paquet DHCPDISCOVER, qui inclut les premiers paramètres tels que l'adresse IP.

DHCPREQUEST : Cela correspond à une requête du client qui peut ainsi prolonger son bail.


8. Que contient le fichier /var/lib/dhcp/dhcpd.leases sur le serveur, et qu’afficle la commande dhcp-lease-list ? 


![image](https://user-images.githubusercontent.com/80455696/193459555-8b230bbe-3287-4357-b164-974704b15820.png)





Le fichier /var/lib/dhcp/dhcpd.leases est une base de données qui stocke toute les bails dhcp attribués aux machnies reliés au serveurs DHCP. La commande dhcp-lease-list permet de demander au serveur DHCP qu'elles sont les adresse IP DCHP actuellement attribués.


9. Vérifiez que les deux machines peuvent communiquer via leur adresse IP, à l’aide de la commande ping

Ping du serveur DHCP vers le client :

![image](https://user-images.githubusercontent.com/80455696/193459619-1cb9a750-db0c-457f-9043-771caef07a0d.png)

Ping du client vers le serveur DHCP : 

![image](https://user-images.githubusercontent.com/80455696/193459713-bfe6189e-d15d-4c66-81ee-591d5447944f.png)


10. Modifiez la configuration du serveur pour que l’interface réseau du client reçoive l’IP statique 192.168.100.20 :

![image](https://user-images.githubusercontent.com/80455696/193462605-358e754c-a2f0-49fb-9890-7e652c8af77d.png)

Ensuite il faut validez notre fichier de configuration avec la commande dhcpd -t puis redémarrez le serveur DHCP
(avec la commande systemctl restart isc-dhcp-server) et vérifiez qu’il est actif.
Et  cote client on tappe la commande dhclient -v pour mettre a jour la nouvelle confuguration de la carte réseau :

![image](https://user-images.githubusercontent.com/80455696/193462694-98158c73-6ce4-4645-a513-669769fc448f.png)
