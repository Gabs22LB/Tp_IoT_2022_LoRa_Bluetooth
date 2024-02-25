# Tp_IoT_2022_LoRa_Bluetooth

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

Nous avons configuré le sender, celui-ci envoie les informations de connexion LoRa (Fréquence 870MHz, Sf= 12 et Sb= 125KHz) en MQTT, ensuite il envoie de la donnée en LoRa.

