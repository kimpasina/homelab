# Sonarr

- **PUID/PGID = 1000** : Correspond à un utilisateur et groupe non-root. Permet à Sonarr de lire/écrire dans le NAS à partir du partage NFS (nfs_data)

- **sonarr_data** : Volume externe pour stocker la configuration de Sonarr

- **nfs_data** : Mount d'un share NFS qui donne accès à Sonarr de télécharger des fichiers dans le NAS (voir [à venir](lien-vers-config-nfs))

- **media** : Réseau commun permettant à Sonarr de communiquer avec Prowlarr et autres services via DNS interne

## Liens utiles

- [GitHub officiel Sonarr](https://github.com/Sonarr/Sonarr)
- [Servarr Wiki - Sonarr](https://wiki.servarr.com/sonarr)

## Docker Compose
```yaml
---
services:
  # Sonarr - Gestion des séries TV
  sonarr:
    image: lscr.io/linuxserver/sonarr:latest
    container_name: sonarr
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=America/Montreal
    volumes:
      - sonarr_data:/config
      - nfs_data:/data
    ports:
      - 8989:8989
    restart: unless-stopped
    networks:
      - media

# Réseau partagé
networks:
  media:
    external: true

# Volumes de données
volumes:
  sonarr_data:
    external: true
  nfs_data:
    external: true
```

## Configuration

<img width="2345" alt="image" src="https://github.com/user-attachments/assets/5bd910b9-9d4a-44eb-b0f2-fed0f6d27c41" />

- `/data/downloads/tv-sonarr` : Dossier de téléchargement où qBittorrent sauvegarde les fichiers bruts. Les épisodes téléchargés restent ici avec leurs noms d'origine jusqu'à ce que Sonarr les traite

- `/data/shows` : Dossier de destination finale où Sonarr crée des hardlinks des épisodes avec un format de fichier standardisé et une structure organisée (par série et saison). Les hardlinks permettent d'éviter la duplication des fichiers tout en maintenant le seeding dans qBittorrent




| qBittorrent | Plex |
|:---:|:---:|
| <img width="400" alt="image" src="https://github.com/user-attachments/assets/23b78ff8-a49b-440c-8115-9a95215afb38" /> | <img width="400" alt="image" src="https://github.com/user-attachments/assets/9f1136e3-462f-4859-aff6-0813cc970255" /> |
