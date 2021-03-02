---
template: BlogPost
path: /ssh-backup
date: 2021-03-02T12:40:26.640Z
title: Sauvegarder son site à distance via SSH
thumbnail: /assets/ssh.png
---
Deux lignes simples pour sauvegarder la base sql et le dossier d'installation de son site/blog/wiki sur un serveur distant :

```bash
# Téléchargement de la base de donnée
ssh admin@XX.XX.XXX.XXX "mysqldump -u dbuser -p dbname | gzip -9" > dbDump-`date +%Y-%m-%d`.sql.gz

# Téléchargement du dossier web
ssh admin@XX.XX.XXX.XXX "tar -C /var/lib/nethserver/vhost/wiki/ -zc test" > dossierWiki-`date +%Y-%m-%d`.tar
```
