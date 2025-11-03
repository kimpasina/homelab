# Docker Compose - stack_download

- **PUID/PGID = 1000** : Correspond à un utilisateur et groupe non-root. Permet à qBittorrent de lire/écrire dans le NAS à partir du partage NFS (nfs_data)

- **media** : Réseau commun avec les applications dans arr_stack

- **network_mode: "service:gluetun"** : qBittorrent utilise le réseau de Gluetun. Tout le trafic de qBittorrent passe par le VPN

- **depends_on: "gluetun"** : qBittorrent dépend de gluetun pour fonctionner. Le container ne fonctionnera pas si gluetun ne fonctionne pas

- **/mnt/torrents:/incomplete** : Dossier local temporaire pour les téléchargements en cours. qBittorrent déplace automatiquement les fichiers vers leur destination finale dans le NAS quand le téléchargement est complété

- **nfs_data** : Mount d'un share NFS qui donne accès à qBittorrent de télécharger des fichiers dans le NAS (voir [à venir](lien-vers-config-nfs))

## Docker Compose
```yaml
---
services:
  # Gluetun - Client VPN
  gluetun:
    image: qmcgaw/gluetun:latest
    container_name: gluetun
    devices:
      - /dev/net/tun:/dev/net/tun
    cap_add:
      - NET_ADMIN
    environment:
      - VPN_SERVICE_PROVIDER=airvpn
      - VPN_TYPE=wireguard
      - WIREGUARD_PRIVATE_KEY=xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
      - WIREGUARD_PRESHARED_KEY=uxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
      - WIREGUARD_ADDRESSES=1xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
      - FIREWALL_VPN_INPUT_PORTS=xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
    volumes:
      - gluetun_data:/gluetun
    networks:
      - media
    ports:
      - 8080:8080 # qBittorrent UI
      - 6881:6881/udp
      - 6881:6881/tcp
    restart: unless-stopped

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

# Réseau partagé
networks:
  media:
    external: true

# Volumes de données
volumes:
  nfs_data:
    external: true
  gluetun_data:
    external: true
  qbittorrent_data:
    external: true
```