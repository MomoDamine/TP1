# Guide d'installation et de configuration de la Raspberry Pi

Ce dépôt fournit un guide complet pour installer et configurer une Raspberry Pi, en mettant l'accent sur les étapes essentielles pour les débutants et les utilisateurs avancés.

## Objectifs

- Installer le système d'exploitation Raspbian sur une carte SD.
- Configurer le réseau pour accéder à la Raspberry Pi.
- Configurer le port série pour la communication.
- Effectuer les mises à jour du système.
- Installer Python et vérifier son fonctionnement.

## Matériel requis

- Une Raspberry Pi (modèle 3 ou plus récent).
- Une carte SD (minimum 8 Go) avec un adaptateur pour PC.
- Un ordinateur avec un lecteur de carte SD.
- Un câble Ethernet (ou une connexion Wi-Fi).
- Un câble micro-USB pour l'alimentation.
- Un écran et un clavier (optionnels pour une configuration sans écran).

## Étapes d'installation

### 1. Télécharger l'image de Raspbian

- Rendez-vous sur le site officiel de Raspberry Pi : [https://www.raspberrypi.com/software/](https://www.raspberrypi.com/software/).
- Téléchargez l'outil Raspberry Pi Imager ou l'image Raspbian ("Raspberry Pi OS").

### 2. Flasher la carte SD

- Installez l'outil Raspberry Pi Imager sur votre ordinateur.
- Lancez l'outil et sélectionnez :
  - **Système d'exploitation** : Raspberry Pi OS (32 bits recommandé).
  - **Stockage** : Votre carte SD.
- Cliquez sur "Write" pour flasher l'image sur la carte SD.

### 3. Préparer la carte SD pour SSH

- Éjectez et reconnectez la carte SD.
- Créez un fichier vide nommé `ssh` (sans extension) à la racine de la carte SD.
- Pour le Wi-Fi, créez un fichier `wpa_supplicant.conf` avec le contenu suivant :
  ```
  country=FR
  ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
  update_config=1

  network={
      ssid="NomDeVotreWiFi"
      psk="MotDePasseWiFi"
  }
  ```

### 4. Configurer une IP statique avant le démarrage

- Modifiez le fichier `cmdline.txt` à la racine de la carte SD.
- Ajoutez à la fin de la ligne existante (en un seul bloc, sans sauts de ligne) :
  ```
  ip=192.168.1.100::192.168.1.1:255.255.255.0:rpi:eth0:off
  ```
  Remplacez `192.168.1.100` par l'adresse IP souhaitée, `192.168.1.1` par l'adresse de la passerelle, et `255.255.255.0` par le masque de sous-réseau approprié.

### 5. Démarrer la Raspberry Pi

- Insérez la carte SD dans la Raspberry Pi.
- Connectez l'alimentation pour démarrer l'appareil.

## Configuration réseau

### 1. Configurer une IP statique

- Modifiez le fichier de configuration :
  ```
  sudo nano /etc/dhcpcd.conf
  ```
- Ajoutez les lignes suivantes :
  ```
  interface eth0
  static ip_address=192.168.1.100/24
  static routers=192.168.1.1
  static domain_name_servers=192.168.1.1
  ```
- Redémarrez le service :
  ```
  sudo systemctl restart dhcpcd
  ```


### 2. Connexion via SSH

- Trouvez l'adresse IP de la Raspberry Pi (via votre routeur ou un outil comme `nmap`).
- Connectez-vous via SSH :
  ```
  ssh pi@<ip_address>
  ```
  Mot de passe par défaut : `raspberry`.

## Configuration du port série

### 1. Activer le port série

- Lancez l'outil de configuration :
  ```
  sudo raspi-config
  ```
- Accédez à **Options d'interface > Port série**.
- Activez l'interface série mais désactivez la console sur le port série.

### 2. Tester le port série

- Installez l'outil `minicom` :
  ```
  sudo apt install minicom
  ```
- Testez le port :
  ```
  minicom -b 9600 -o -D /dev/serial0
  ```

## Mises à jour du système

### 1. Mettre à jour les paquets

- Exécutez les commandes suivantes :
  ```
  sudo apt update
  sudo apt upgrade -y
  ```

### 2. Nettoyer les fichiers inutiles

- Supprimez les paquets inutilisés :
  ```
  sudo apt autoremove -y
  ```

## Installation de Python

### 1. Vérifier la version préinstallée

- Vérifiez la version de Python :
  ```
  python3 --version
  ```

### 2. Installer pip (gestionnaire de paquets Python)

- Installez pip :
  ```
  sudo apt install python3-pip
  ```

### 3. Installer des bibliothèques Python (Exemple)

- Installez une bibliothèque, par exemple `numpy` :
  ```
  pip3 install numpy
  ```

### 4. Tester Python

- Lancez Python :
  ```
  python3
  ```
- Testez un script simple :
  ```python
  print("Bonjour, Raspberry Pi !")
  ```

## Conclusion

Ce guide couvre l'installation et la configuration d'une Raspberry Pi, y compris la configuration réseau, l'activation du port série, les mises à jour du système et l'installation de Python. Ces compétences constituent une base solide pour des projets Raspberry Pi plus avancés.

