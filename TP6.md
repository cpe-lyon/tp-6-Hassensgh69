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

