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


## Exercice 4. Donner un accès à Internet au client

1. La première chose à faire est d’autoriser l’IP forwarding sur le serveur (désactivé par défaut, étant
donné que la plupart des utilisateurs n’en ont pas besoin). Pour cela, il suffit de décommenter la ligne
net.ipv4.ip_forward=1 dans le fichier /etc/sysctl.conf. Pour que les changements soient pris en
compte immédiatement, il faut saisir la commande sudo sysctl -p /etc/sysctl.conf.
Vérifiez avec la commande sysctl net.ipv4.ip_forward que la nouvelle valeur a bien été prise en
compte.

![image](https://user-images.githubusercontent.com/80455696/193464316-610bf5e9-812a-47c1-b7dc-025073c7c565.png)

2. Ensuite, il faut autoriser la traduction d’adresse source (masquerading) en ajoutant la règle iptables
suivante :
sudo iptables --table nat --append POSTROUTING --out-interface enp0s3 -j MASQUERADE

![image](https://user-images.githubusercontent.com/80455696/193530643-1d667cdc-a795-42f5-a6cd-c15cb68a8020.png)

TEST de ping sur le client :

![image](https://user-images.githubusercontent.com/80455696/193530747-f60866ab-3943-483d-94fa-1f55383ada1e.png)

## Exercice 5. Installation du serveur DNS

2. A ce stade, Bind n’est pas configuré et ne fait donc pas grand chose. L’une des manières les simples de le configurer est d’en faire un serveur cache : il ne fait rien à part mettre en cache les réponses de serveurs externes à qui il transmet la requête de résolution de nom.
Nous allons donc modifier son fichier de configuration : /etc/bind/named.conf.options. Dans ce
fichier, décommentez la partie forwarders, et à la place de 0.0.0.0, renseignez les IP de DNS publics comme 1.1.1.1 et 8.8.8.8 (en terminant à chaque fois par un point virgule). Redémarrez le serveur
bind9.

![image](https://user-images.githubusercontent.com/80455696/193532204-9de6e73f-ae46-42b9-9b62-7434dd0bcf39.png)

3. Sur le client, retentez un ping sur www.google.fr. Cette fois ça devrait marcher ! On valide ainsi la configuration du DHCP effectuée précédemment, puisque c’est grâce à elle que le client a trouvé son serveur DNS.

![image](https://user-images.githubusercontent.com/80455696/193532430-06a18248-f607-4603-ad2b-f9e464d0a04b.png)


4. Sur le client, installez le navigateur en mode texte lynx et essayez de surfer sur fr.wikipedia.org (bienvenue dans le passé...)

On installer lynx avec la commande sudo apt install lynx puis on le lance. Pour lancer wikipedia il faut tapper lynx wikipedia.org

![image](https://user-images.githubusercontent.com/80455696/193533729-56b8a734-8f67-4722-a44c-140d0f2c6433.png)

## Exercice 6. Configuration du serveur DNS pour la zone tpadmin.local

1. Modifiez le fichier /etc/bind/named.conf.local et ajoutez les lignes suivantes :

![image](https://user-images.githubusercontent.com/80455696/193534580-5e3745a5-f529-4b50-a6b2-8a44dc5a4a90.png)

2. Créez une copie appelée db.tpadmin.local du fichier db.local.

![image](https://user-images.githubusercontent.com/80455696/193534978-8434273f-2f46-4eb1-a336-c38f12c2a5cd.png)

Dans le nouveau fichier, remplacez toutes les références à localhost par tpadmin.local, et l’adresse 127.0.0.1 par l’adresse IP du
serveur.

Fichier original :

![image](https://user-images.githubusercontent.com/80455696/193535554-beae2cc5-5f53-4a1f-a44f-53c6470f5697.png)

Fichier avec modification : 

![image](https://user-images.githubusercontent.com/80455696/193535637-540e45a3-f385-4e0d-a72c-9f83d6f8bfec.png)

3. Maintenant que nous avons configuré notre fichier de zone, il reste à configurer le fichier de zone inverse, qui permet de convertir une adresse IP en nom.
Commencez par rajouter les lignes suivantes à la fin du fichier named.conf.local :

![image](https://user-images.githubusercontent.com/80455696/193536312-d1484c00-99ae-47d8-9a3c-2265d8cca106.png)

Créez ensuite le fichier db.192.168.100 à partir du fichier db.127, et modifiez le de la même manière que le fichier de zone. Sur la dernière ligne, faites correspondre l’adresse IP avec celle du serveur :

![image](https://user-images.githubusercontent.com/80455696/193540395-0b7ef4a2-0919-475a-b77a-4a3faa6c2a5d.png)

4. Utilisez les utilitaires named-checkconf et named-checkzone pour valider vos fichiers de configuration :

On peut voir qu'a la suite des commandes on nous renvoit un "ok", ce qui veut dire que tout fonctionne correctement.

![image](https://user-images.githubusercontent.com/80455696/193541181-cc05caa0-26a7-416c-802d-1666d3bf5505.png)

Modifiez le fichier /etc/systemd/resolved.conf et décommentez la section DNS.


5. Redémarrer le serveur Bind9. Vous devriez maintenant être en mesure de « pinguer »les différentes
machines du réseau.

![image](https://user-images.githubusercontent.com/80455696/194234235-12cfa52e-01bb-4a9b-833d-27ec2d43dfd8.png)

Conclusion du tp :

On peut desormais ping le serveur depuis le client en utilisant son nom de domaine " SRV.tpadmin.local "avec la commande suivante ` ping SRV.tpadmin.local `. Dans le cadre du tp on est censé aussi ping du serveur vers le client en utilisant so n nom de domaine "SRV.tpadmin.local"  mais j'ai rencontré quelque problème qui ont fait que ca na pas été possible. 

![image](https://user-images.githubusercontent.com/80455696/194234604-6c81a4b9-d81c-408d-b4c8-75049802b0a8.png)
