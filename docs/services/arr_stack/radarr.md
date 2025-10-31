# Radarr

- **PUID/PGID = 1000** : Correspond à un utilisateur et groupe non-root. Permet à Radarr de lire/écrire dans le NAS à partir du partage NFS (nfs_data)

- **radarr_data** : Volume externe pour stocker la configuration de Radarr

- **nfs_data** : Mount d'un share NFS qui donne accès à Radarr de télécharger des fichiers dans le NAS (voir [à venir](lien-vers-config-nfs))

- **media** : Réseau commun permettant à Radarr de communiquer avec Prowlarr et autres services via DNS interne

## Liens utiles

- [GitHub officiel Radarr](https://github.com/Radarr/Radarr)
- [Servarr Wiki - Radarr](https://wiki.servarr.com/radarr)

## Docker Compose
```yaml
---
services:
  # Radarr - Gestion des films
  radarr:
    image: lscr.io/linuxserver/radarr:latest
    container_name: radarr
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=America/Montreal
    volumes:
      - radarr_data:/config
      - nfs_data:/data
    ports:
      - 7878:7878
    restart: unless-stopped
    networks:
      - media

# Réseau partagé
networks:
  media:
    external: true

# Volumes de données
volumes:
  radarr_data:
    external: true
  nfs_data:
    external: true
```

## Configuration

<img width="2347" alt="image" src="https://github.com/user-attachments/assets/0c5d8525-657e-421d-8082-886c57b43e07" />

- `/data/downloads/radarr` : Dossier de téléchargement où qBittorrent sauvegarde les fichiers bruts. Les films téléchargés restent ici avec leurs noms d'origine jusqu'à ce que Radarr les traite

- `/data/movies` : Dossier de destination finale où Radarr crée des hardlinks des films avec un format de fichier standardisé et une structure organisée. Les hardlinks permettent d'éviter la duplication des fichiers tout en maintenant le seeding dans qBittorrent

| qBittorrent | Plex |
|:---:|:---:|
| <img width="400" alt="image" src="https://github.com/user-attachments/assets/d96531d6-ce06-4d98-8482-ec46aee29af1" /> | <img width="400" alt="image" src="https://github.com/user-attachments/assets/0cec2042-87ff-4384-bf64-b41d0cae458c" /> |
