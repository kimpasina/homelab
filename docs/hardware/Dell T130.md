## Dell PowerEdge T130

### Informations générales
- **Rôle** : `NAS Backup & Replication`
- **Hostname** : `sth-bckp-01`
- **Modèle** : `Dell PowerEdge T130`
- **OS** : `TrueNAS CORE 13.0-U6.7`
- **IP Principale** : `192.168.0.11/24`

### Spécifications matérielles
- **CPU** : `Intel Xeon E3-1220 v5`
- **RAM** : `16 Go - 2133 MHz ECC`
- **Stockage** :
    - **Boot** : `119 Go SSD (boot-pool)`
    - **Disques** : `4x 2 To`
    - **Configuration RAID** : `RAID 10 (2x Mirror)`
    - **Capacité utilisable** : `~3.6 To (pool: dozer)`

### Réseau
- **Interface principale** : `bge0`
- **Vitesse** : `1Gbps`

### Configuration système

#### ZFS Pools
- **boot-pool** : 119 Go
- **dozer** : 3.62 To
    - mirror-0 : 2x 2 To
    - mirror-1 : 2x 2 To

#### BIOS & Power Management
##### System BIOS Settings • System Profile Settings
- **System Profile** : `Custom`
- **CPU Power Management** : `OS DBPM`
- **Memory Frequency** : `Maximum Performance`
- **Turbo Boost** : `Enabled`
- **Energy Efficient Turbo** : `Disabled`
- **C1E** : `Enabled`
- **C States** : `Enabled`
- **Memory Refresh Rate** : `1x`
- **Uncore Frequency** : `Dynamic`
- **Energy Efficient Policy** : `Balanced Performance`
- **Number of Turbo Boost Enabled Cores for Processor 1** : `All`
- **Monitor/Mwait** : `Enabled`

> **Note** : L'activation de **C1E** et **C States** est nécessaire pour le Wake on LAN

#### NIC Configuration (Embedded NIC 1 Port 1)
- **Legacy Boot Protocol** : `PXE`
- **Wake On LAN** : `Enabled`

#### Services/Fonctionnalités spéciales
- Wake on LAN activé (C1E et C States requis)
- Utilisateur `shutdown` avec permissions sudoers (shutdown sans mot de passe)
- Script init pour ajouter `shutdown` dans sudoers"
- Réception des réplications depuis 192.168.0.10

### Notes
- Serveur actif uniquement de 03:45 à 05:00 quotidiennement