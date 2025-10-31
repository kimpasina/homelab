# Docker Compose - arr_stack


- **PUID/PGID = 1000** : Correspond à un utilisateur et groupe non-root. Permet à Sonarr et Radarr de lire/écrire dans le NAS à partir du partage NFS (nfs_data)

- **Volumes externes** : Les volumes sont créés de façon externe, ce qui permet une convention de noms plus simple et une meilleure gestion

- **nfs_data** : Mount d'un share NFS qui donne accès à Sonarr et Radarr de télécharger des fichiers dans le NAS (voir [à venir](lien-vers-config-nfs))

- **media** : Réseau commun entre les applications pour leur permettre de communiquer. Un serveur DNS interne est créé et les applications peuvent communiquer entre elles en utilisant leur nom de container

  
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

  # Prowlarr - Gestionnaire d'indexeurs
  prowlarr:
    image: lscr.io/linuxserver/prowlarr:latest
    container_name: prowlarr
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=America/Montreal
    volumes:
      - prowlarr_data:/config
    ports:
      - 9696:9696
    restart: unless-stopped
    networks:
      - media

# Réseau partagé entre les services
networks:
  media:
    external: true

# Volumes de données
volumes:
  prowlarr_data:
    external: true
  sonarr_data:
    external: true
  radarr_data:
    external: true
  nfs_data:
    external: true
```
