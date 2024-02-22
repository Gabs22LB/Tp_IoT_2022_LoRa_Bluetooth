# Tp_IoT_2022_LoRa_Bluetooth

## 1. Receiver :
 
Nous avons configuré le receiver, celui-ci se connecte au brocker MQTT pour récupérer les informations de connexions Lora poussés par le Sender. Une fois les informations récupérer celui-ci peut configurer et lancer Lora pour recevoir les paquets transmis par le Sender.

## 2. Sender :

Nous avons configuré le sender, celui-ci envoie les informations de connexion LoRa (Fréquence 870MHz, Sf= 12 et Sb= 125KHz) en MQTT, ensuite il envoie de la donnée en LoRa.

## 3. Ensuite...

### 1. Ajouter une fonction permettant de switcher entre Sender et Receiver

### 2. Sur le sender, échangez la partie LoRa par du bluetooth (ou BLE)

### 3. Refaire la partie cliente pour répondre au bluetooth

