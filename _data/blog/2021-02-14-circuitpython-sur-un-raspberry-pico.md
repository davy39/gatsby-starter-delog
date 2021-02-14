---
template: BlogPost
path: /pico
date: 2021-02-14T17:37:26.459Z
title: CircuitPython sur un Raspberry Pico
thumbnail: /assets/2021-01-21-raspberry-pi-pico-pinout.png
---
La compagnie Rasperry a sorti des nouveaux microprocesseurs du nom de [Raspberry Pico](https://www.raspberrypi.org/products/raspberry-pi-pico/). ils ont l'avantage d'être puissants pour le prix ([4€50](https://shop.mchobby.be/fr/pico-raspberry-pi/2025-pico-rp2040-microcontroleur-2-coeurs-raspberry-pi-3232100020252.html)), devraient bénéficier du support d'une très large communauté, et supportent le langage CircuitPython sur lequel on a déjà travaillé (sur SAMD21 et BlackPill).

## Installation de Circuitpython

Rien de plus simple, il suffit de télécharger[ la dernière version  de Circuitpy au format UF2](https://circuitpython.org/board/raspberry_pi_pico/) et de copier le fichier sur le Pico après l'avoir connecté via USB.

## Utilisation des librairies Adafruit

Comme d'habitude, on peut [télécharger le bundle de librairies Adafruit](https://circuitpython.org/libraries), pour pouvoir ensuite copier les librairies nécessaire dans le dossier lib du Pico. Attention : s'assurer que le bundle correspond bien à la version de Circuitpython installée précédemment sur le Pico (la v6 lors de la rédaction de cet article).

## Programmation avec Mu-editor

Pour installer mu-editor : 

```bash
sudo apt- install python3-pip
pip3 install mu-editor
```

Ou pour installer la version en développement de mu-editor :

`pip3 install git+https://github.com/mu-editor/mu.git`

Une fois Circuitpython Mu-editor reconnait le Pico comme un périphérique Adafruit CircuitPlayground. 

## Programmation avec Atom

Si vous êtes un afficionado de l'éditeur Atom, il existe un le package **language-circuitpython** (*Edit>Preferences>Install*) qui permet d'avoir accès au Serial et au Ploter.

Pour installer Atom sur un OS dérivé de Debian (Ubuntu, Mint...) :

```
wget -qO - https://packagecloud.io/AtomEditor/atom/gpgkey | sudo apt-key add -
sudo sh -c 'echo "deb \[arch=amd64] https://packagecloud.io/AtomEditor/atom/any/ any main" > /etc/apt/sources.list.d/atom.list'
sudo apt-get update
sudo apt-get install atom
```

## Exemple d'utilisation

un petit exemple pour la route, pour afficher les mesures d'un BME680 et les enregistrer sur une carte SD.

```python
# On déclare les librairies nécessaires 
# Attention de bien avoir copié adafruit_bme680.mpy et adafruit_sdcard.mpy dans le dossier lib
import board
import busio
import time
import adafruit_bme680
import digitalio
import adafruit_sdcard
import storage

#BME680 connecté en I2C sur les pins GP0 et GP1 du Pico
i2c = busio.I2C(scl=board.GP1, sda=board.GP0)
bme680 = adafruit_bme680.Adafruit_BME680_I2C(i2c)
# Indiquer ici la pression (hPa) mesurée au niveau de la mer
bme680.sea_level_pressure = 1013.25

#Carte SD connectée en SPI sur les pins GP2, GP3, GP4 et GP5 du Pico
spi = busio.SPI(clock=board.GP2, MOSI=board.GP3, MISO=board.GP4)
cs = digitalio.DigitalInOut(board.GP5)
#On déclare une carte SD connectée sur le SPI
sdcard = adafruit_sdcard.SDCard(spi, cs)
#On déclare un système de fichier VfsFat
vfs = storage.VfsFat(sdcard)
#On le monte dans un dossier /sd
storage.mount(vfs, "/sd")

#On lance une boucle qui tourne en permanence
while True:
    #On récupère un tuple comprenant tous les paramètres mesurés par le BME680
    result=(bme680.temperature,bme680.gas,bme680.relative_humidity,bme680.pressure,bme680.altitude)
    #On affiche le tuple sur le serial pour éventuellement pouvoir le "ploter"
    print(result)
    #On ouvre un fichier test.txt pour continuer à le remplir ("a"=append)
    with open("/sd/test.txt", "a") as f:
        #On y écrit nos paramètres, séparés d'un espace, et formatés avec plus ou moins de décimales
        f.write("%0.1f %d %0.1f %0.3f %0.2f\r\n" % result)
    #on attend 2 secondes avant de recommancer une mesure.
    time.sleep(2)
```
