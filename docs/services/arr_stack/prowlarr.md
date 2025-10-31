# Prowlarr

- **PUID/PGID = 1000** : Correspond à un utilisateur et groupe non-root.

- **prowlarr_data** : Volume externe pour stocker la configuration de Prowlarr

- **media** : Réseau commun permettant à Prowlarr de communiquer avec Sonarr et Radarr via DNS interne

## Docker Compose
```yaml
---
services:
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

# Réseau partagé
networks:
  media:
    external: true

# Volumes de données
volumes:
  prowlarr_data:
    external: true
```

## Configuration
### Radarr/Sonarr
<img width="400" alt="Prowlarr Indexers Configuration" src="https://github.com/user-attachments/assets/e1bb4f2a-75e1-4471-8f67-c8dba0bf5ccf" />
