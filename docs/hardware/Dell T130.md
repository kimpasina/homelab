## Dell PowerEdge T130

- **Rôle** : `NAS replication backup`
- **Modèle** : `Dell PowerEdge T130`
- **OS** : `TrueNAS CORE 13`
- **IP Principale** : `192.168.0.5/24`
- **CPU** : `Intel Xeon E3-1220 v5`
- **RAM** : `16 Go - 2133 MHz ECC`
- **Stockage** :
    - **Boot drive** : `1x SSD 128 Go`
    - **Disques** : `4x 2 To`
    - **Capacité** : `~8 To usable`

---

### BIOS & Power Management
#### System BIOS Settings • System Profile Settings
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
