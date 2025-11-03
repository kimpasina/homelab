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

### Paramètres qBittorrent

| Downloads | Connection |
|:---:|:---:|
| <img width="400" alt="image" src="https://github.com/user-attachments/assets/8c78a98f-a2f3-413d-b7a0-208174032601" /> | <img width="400" alt="image" src="https://github.com/user-attachments/assets/d6b145a0-f5ed-4bbe-b0ef-0c4557608ed0" /> |

| Speed | BitTorrent |
|:---:|:---:|
| <img width="400" alt="image" src="https://github.com/user-attachments/assets/336f2597-0439-4002-b2f5-7e36348c512a" /> | <img width="400" alt="image" src="https://github.com/user-attachments/assets/ef1cbf1f-fd62-49fe-8414-a91beb496bd2" /> |

| Advanced |
|:---:|
| <img width="400" alt="image" src="https://github.com/user-attachments/assets/65fa98cb-b697-4bb3-8936-6f5e6c4a3794" /> |

### Vérification du fonctionnement du VPN

On peut vérifier si le VPN et le port forwarding fonctionnent correctement en téléchargeant un magnet link sur [ipleak.net](https://ipleak.net)

| Test ipleak.net | IP du container |
|:---:|:---:|
| <img width="400" alt="image" src="https://github.com/user-attachments/assets/429da5e9-a777-4cfe-8dd8-ad81f9c6fc3c" /> | <img width="400" alt="image" src="https://github.com/user-attachments/assets/bd83c1cc-c5c1-41bc-bf29-53b4bd636878" /> |


