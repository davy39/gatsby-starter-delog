---
template: BlogPost
path: /bcd2000
date: 2020-05-23T15:43:24.295Z
title: Modifier le firmware du BCD2000
thumbnail: /assets/BCD2000_big.jpg
---
Le BCD2000 est une table de mixage pour DJ qui commence à dater un peu (2006) mais peu donc être achetée pour pas bien cher d'occasion (j'en ai acheté une à 10€ et l'autre à 20€), a l'air assez robuste et s'adapte facilement pour controler Mixxx (j'ai apporté [quelques modifs au script et fichier xml](https://github.com/davy39/mixxx-bcd2000) de Mixxx pour un meilleur support).

Cependant, ses drivers pour Windows ne supportent que les systèmes 32bits et donc pas les plus récents, et le noyau Linux ne prend en charge que les signaux midi et pas la partie audio.

On trouve bien [la branche audio d'un module pour Linux](https://github.com/anyc/snd-bcd2000/tree/audio), mais elle ne fonctionne que si le controleur est branché sur un port USB3, et l'acquisition de l'entrée micro saccade par moment (peut-être est-ce du au manque de puissance du système sur lequel je l'ai testée...). Bref, c'est pas mal mais pas idéal...

J'ai trouvé une option plus prometteuse : la possibilité de[ modifier le firmare du controleur ](https://github.com/CodeKill3r/BCD2000HIDplus)pour qu'il utilise des protocoles de communication plus standard. Cette solution est très peu documentée et nécessite de [flasher l'Eeprom 24C64](https://github.com/command-tab/ch341eeprom) présent dans l'électronique (je l'ai identifié,mais doit-on le déssouder pour cela ?). J'ai commandé [le nécessaire pour essayer](https://fr.aliexpress.com/item/4000851292510.html), au pire je flingue un contrôleur, au mieux j'ai la satisfaction de faire du neuf avec du vieux !

Je fais donc l'essai dès que je reçois ce qu'il me faut, et explique la démarche ici si j'y arrive.

A suivre !
