## Dell Optiplex 5050

### Informations générales
- **Rôle** : `Serveur Docker`
- **Hostname** : `sth-prd-02`
- **Modèle** : `Dell OptiPlex 5050`
- **OS** : `Ubuntu 24.04.3 LTS`
- **IP Principale** : `192.168.0.137/24`

### Spécifications matérielles
- **CPU** : `Intel Core i5-6500T @ 2.50GHz (4 cores, 4 threads)`
- **RAM** : `8 Go`
- **Stockage** :
    - **Boot** : `119.2 Go SSD (sdb)`
    - **Disques** : `223.6 Go SSD (sda)`
    - **Capacité utilisable** : `57 Go (/) + 220 Go (/mnt/torrents)`

### Réseau
- **Interface principale** : `enp0s31f6`
- **Vitesse** : `1Gbps`

### Configuration système

#### Docker
- **Version Docker** : `28.5.1`
- **Version Docker Compose** : `v2.40.3`
- **Stacks déployées** :
    - arr_stack
    - download_stack
    - media_stack
    - utilities_stack
    - vpn_stack
    - cloudflare-tunnel
    - portainer

#### Mounts
- `192.168.0.10:/mnt/tank/docker_backup` → `/mnt/nfs_docker-backup` (5.1 To)

#### Services/Fonctionnalités spéciales
- Scripts automation :
    - `/usr/local/bin/backup_script/` - Backup Docker (03:00 AM)
    - `/usr/local/bin/nas-backup-wol_script/` - WoL Dell T130 (02:45 AM)
    - `/usr/local/bin/nas-backup-shutdown_script/` - Shutdown Dell T130 (05:00 AM)
