## Supermicro 2U CSE-825TQC-R1K03LPB

### Informations générales
- **Rôle** : `NAS Principal`
- **Hostname** : `sth-prd-01`
- **Modèle** : `Assemblage custom (Gigabyte B450M DS3H V2)`
- **OS** : `TrueNAS SCALE - ElectricEel 24.10.1`
- **IP Principale** : `192.168.0.10/24`

### Spécifications matérielles
- **CPU** : `AMD Ryzen 3 2200G with Radeon Vega Graphics (4 cores, 4 threads)`
- **RAM** : `15 Go`
- **Stockage** :
    - **Boot** : `111 Go SSD (boot-pool)`
    - **Disques** : `4x 4 To`
    - **Configuration RAID** : `RAID 10 (2x Mirror)`
    - **Capacité utilisable** : `~7.25 To (pool: tank)`

### Réseau
- **Interface principale** : `enp5s0`
- **Vitesse** : `1Gbps`

### Configuration système

#### ZFS Pools
- **boot-pool** : 111 Go
- **tank** : 7.25 To
    - mirror-0 : 2x 4 To
    - mirror-1 : 2x 4 To

#### Services/Fonctionnalités spéciales
- Partage NFS vers 192.168.0.137 (docker_backup)
- Tâches de réplication vers 192.168.0.11 (Dell T130)
- Snapshots automatiques quotidiens

