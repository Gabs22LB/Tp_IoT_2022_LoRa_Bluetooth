# Tp_IoT_2022_LoRa_Bluetooth

## 1. Receiver :
### Rôle et fonctionnement
 
 Ce dispositif est d'abord programmé pour se connecter au broker MQTT, une étape cruciale qui lui permet de récupérer les paramètres de connexion LoRa envoyés par le sender. Ces informations, comprenant la fréquence, le facteur d'étalement et la largeur de bande, sont essentielles pour synchroniser les configurations LoRa entre l'émetteur et le récepteur. Une fois ces paramètres récupérés, le receiver procède à l'initialisation de son module LoRa en conséquence. Grâce à cette configuration préalable, il est parfaitement équipé pour détecter et recevoir les paquets de données transmis par le sender. Ce processus illustre l'interaction harmonieuse entre les technologies MQTT et LoRa dans notre système, où MQTT sert de canal pour échanger des informations de configuration, permettant ainsi une communication LoRa efficace et précise pour la transmission des données.

### Explication du code:


## 2. Sender :

Nous avons configuré le sender, celui-ci envoie les informations de connexion LoRa (Fréquence 870MHz, Sf= 12 et Sb= 125KHz) en MQTT, ensuite il envoie de la donnée en LoRa.

