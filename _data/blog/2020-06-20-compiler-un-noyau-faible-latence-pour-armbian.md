---
template: BlogPost
path: /OPLowLatency
date: 2020-06-20T09:40:48.913Z
title: Compiler un noyau faible latence pour Armbian
thumbnail: /assets/RealTimeLinux.jpg
---


## Téléchargement des outils de compilation d'Armbian

```
git clone https://github.com/armbian/build ~/build-OPZynth
```

## Téléchargement du patch RT

Télécharger le [dernier patch RT](https://wiki.linuxfoundation.org/realtime/start). En ce moment le noyau mainline pour OP One est basé sur la [branche 5.4 du dépot Megous](https://github.com/megous/linux/tree/orange-pi-5.4).

Télécharger le [dernier patch 5.4](http://cdn.kernel.org/pub/linux/kernel/projects/rt/5.4/) et le décompresser dans le dossier ~/build-OPZynth/patch/kernel/sunxi-current (OPOne) 

Remarque : ce sera dans /home/davy/build-OPZynth/patch/kernel/rockchip64 pour l'OP4

## Modification éventuelle des option de compilation 

Éventuellement, on pourrait modifier des [options](https://github.com/armbian/build/blob/master/lib/configuration.sh) dans le fichier lib.config

```
mkdir ~/build-OPZynth/userpatches
nano ~/build-OPZynth/userpatches/lib.config
```

Comme par exemple :

```
[[ $LINUXFAMILY == sunxi && $BRANCH == dev ]] && 
KERNELBRANCH="tag:v5.4.44"
```
Attention, cela ne marchera pas dans notre cas précis car ce tag n'existe pas pour le dépot sunxi-current / megous

Remarque : On pourrait aussi enregistrer le fichier de configuration du Kernel une fois configuré.

```
nano ~/build-OPZynth/userpatches/linux-$KERNELFAMILY-$KERNELBRANCH.config
```

## Compilation le kernel

```
./compile.sh  BOARD=orangepione BRANCH=current KERNEL_ONLY=yes KERNEL_CONFIGURE=yes
```

Il faut ensuite choisir le low latency (3) car la compilation du Full RT ne marche pas.
