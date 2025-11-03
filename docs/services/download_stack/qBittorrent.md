# qBittorrent

- **PUID/PGID = 1000** : Correspond à un utilisateur et groupe non-root. Permet à qBittorrent de lire/écrire dans le NAS à partir du partage NFS (nfs_data)

- **qbittorrent_data** : Volume externe pour stocker la configuration de qBittorrent

- **nfs_data** : Mount d'un share NFS qui donne accès à qBittorrent de télécharger des fichiers dans le NAS (voir [à venir](lien-vers-config-nfs))

- **/mnt/torrents:/incomplete** : Dossier local temporaire pour les téléchargements en cours. qBittorrent déplace automatiquement les fichiers vers leur destination finale dans le NAS quand le téléchargement est complété

- **network_mode: "service:gluetun"** : qBittorrent utilise le réseau de Gluetun. Tout le trafic de qBittorrent passe par le VPN

- **depends_on: "gluetun"** : qBittorrent dépend de gluetun pour fonctionner. Le container ne fonctionnera pas si gluetun ne fonctionne pas

## Liens utiles

- [GitHub officiel qBittorrent](https://github.com/qbittorrent/qBittorrent)

## Docker Compose
```yaml
---
services:
  # qBittorrent - Client torrent
  qbittorrent:
    image: lscr.io/linuxserver/qbittorrent:latest
    container_name: qbittorrent
    network_mode: "service:gluetun"
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=America/Montreal
      - WEBUI_PORT=8080
    volumes:
      - qbittorrent_data:/config
      - nfs_data:/data
      - /mnt/torrents:/incomplete
    depends_on:
      - gluetun
    restart: unless-stopped

# Volumes de données
volumes:
  qbittorrent_data:
    external: true
  nfs_data:
    external: true
```

## Configuration

