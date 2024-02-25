# Tp_IoT_2022_LoRa_Bluetooth - Gabin Le Breton / Erwan Drean

## 1. Receiver :
### Rôle et fonctionnement
 
 Ce dispositif est d'abord programmé pour se connecter au broker MQTT, une étape cruciale qui lui permet de récupérer les paramètres de connexion LoRa envoyés par le sender. Ces informations, comprenant la fréquence, le facteur d'étalement et la largeur de bande, sont essentielles pour synchroniser les configurations LoRa entre l'émetteur et le récepteur. Une fois ces paramètres récupérés, le receiver procède à l'initialisation de son module LoRa en conséquence. Grâce à cette configuration préalable, il est parfaitement équipé pour détecter et recevoir les paquets de données transmis par le sender. Ce processus illustre l'interaction harmonieuse entre les technologies MQTT et LoRa dans notre système, où MQTT sert de canal pour échanger des informations de configuration, permettant ainsi une communication LoRa efficace et précise pour la transmission des données.

### Explication du code:
#### Initialisation et Connexion WiFi :
Le code démarre par initialiser la communication série et configurer le module ESP32 pour se connecter au réseau WiFi spécifié. Cela est accompli grâce aux instructions WiFi.begin(ssid, password); qui utilisent les identifiants du réseau définis en amont. Une boucle surveille l'état de la connexion et affiche un message dans la console série une fois que la connexion est établie.

#### Configuration LoRa :
Avant de recevoir des données via LoRa, il est nécessaire de configurer le module LoRa avec les bons paramètres (fréquence, facteur d'étalement, largeur de bande). Ces paramètres sont initialement obtenus via MQTT, ce qui assure que l'émetteur et le récepteur opèrent sur les mêmes configurations. La fonction lora() est appelée après la réception des paramètres via MQTT pour initialiser le module LoRa avec ces valeurs.

#### Connexion au Broker MQTT et Souscription :
Le receiver se connecte ensuite au broker MQTT en utilisant client.setServer(mqtt_broker, mqtt_port); et s'abonne au topic spécifique avec client.subscribe(topic);. Cela lui permet de recevoir des messages contenant les paramètres de configuration LoRa envoyés par le sender.

#### Réception et Traitement des Messages MQTT :
Lorsqu'un message arrive sur le topic MQTT auquel le receiver est abonné, la fonction callback est invoquée. Cette fonction extrait les paramètres de configuration LoRa du message, affiche ces valeurs pour vérification, puis appelle lora() pour reconfigurer le module LoRa si nécessaire.

#### Réception des Paquets LoRa :
Dans la boucle principale loop(), le receiver écoute les paquets LoRa en utilisant LoRa.parsePacket(). Si un paquet est détecté, les données sont lues et traitées. Le contenu est interprété comme des valeurs à virgule flottante représentant les données envoyées par l'émetteur, et la force du signal (RSSI) est également récupérée pour évaluer la qualité de la réception.

## 2. Sender :
### Rôle et fonctionnement
Initialement, le sender établit une connexion au réseau WiFi et se connecte au broker MQTT en utilisant les identifiants fournis. Une fois cette connexion établie, il procède à l'envoi des informations de configuration LoRa, notamment la fréquence à 870MHz, le facteur d'étalement (Sf) à 12 et la largeur de bande (Sb) à 125KHz, à travers un message MQTT. Cette étape est cruciale car elle permet de s'assurer que le receiver sera configuré pour écouter sur les mêmes paramètres LoRa, garantissant ainsi une communication synchronisée entre les deux dispositifs.

Suite à la transmission des paramètres de configuration via MQTT, le sender passe à l'étape suivante de son processus, qui consiste à envoyer des données en utilisant la technologie LoRa. Pour ce faire, il initialise le module LoRa avec les paramètres spécifiés, puis crée un paquet de données, ici illustré par des valeurs à virgule flottante, qui sont converties en un format d'octets grâce à l'utilisation d'une union. Ces données sont ensuite transmises à travers le réseau LoRa. 

### Explication du code:
#### Connexion WiFi :
Le sender commence par établir une connexion WiFi en utilisant les identifiants ssid et password. Cette étape est fondamentale pour permettre au dispositif de communiquer avec le broker MQTT sur Internet.

#### Configuration et Connexion MQTT :
Une fois connecté au réseau WiFi, le sender configure le client MQTT en spécifiant le serveur MQTT (mqttServer), le port (mqttPort), et, si nécessaire, les identifiants utilisateur (mqttUser, mqttPassword). Il tente ensuite de se connecter au broker MQTT. En cas de succès, un message de confirmation est affiché dans la console série.

#### Envoi des Paramètres LoRa via MQTT :
Avant de transmettre des données réelles, le sender publie un message sur un topic MQTT spécifique. Ce message contient les paramètres de configuration LoRa (fréquence, facteur d'étalement, largeur de bande) qui seront utilisés par le receiver pour s'aligner sur la même configuration de communication.

#### Initialisation et Configuration LoRa :
Le module LoRa est initialisé avec LoRa.begin(870000000) pour opérer à la fréquence de 870 MHz. Le facteur d'étalement et la largeur de bande sont également configurés. Ces paramètres assurent que le sender et le receiver communiquent de manière cohérente sur le réseau LoRa.

#### Transmission de Données LoRa :
Le sender crée un paquet de données, ici composé de deux valeurs à virgule flottante. Ces valeurs sont stockées dans une union, permettant une manipulation aisée des données sous forme d'octets pour l'envoi via LoRa. Le paquet est ensuite envoyé, marquant la transmission effective des données à travers le réseau LoRa.
