# Gluetun

## Environnement

- **VPN_SERVICE_PROVIDER** : Fournisseur VPN utilisé (AirVPN dans ce cas)

- **VPN_TYPE** : Type de protocole VPN (WireGuard)

- **FIREWALL_VPN_INPUT_PORTS** : Ports autorisés à travers le firewall (port forwarding)

- **WIREGUARD_PRIVATE_KEY** : Information générée dans la configuration Wireguard du provider VPN

- **WIREGUARD_PRESHARED_KEY** : Information générée dans la configuration Wireguard du provider VPN

- **WIREGUARD_ADDRESSES** : Information générée dans la configuration Wireguard du provider VPN


- **gluetun_data** : Volume externe pour stocker la configuration de Gluetun

- **media** : Réseau commun permettant à Gluetun de communiquer avec arr_stack

## Liens utiles

- [GitHub officiel Gluetun](https://github.com/qdm12/gluetun)
- [AirVPN](https://airvpn.org)

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
      - WIREGUARD_PRESHARED_KEY=xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
      - WIREGUARD_ADDRESSES=xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
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

# Réseau partagé
networks:
  media:
    external: true

# Volumes de données
volumes:
  gluetun_data:
    external: true
```

## Configuration

| Configuration VPN |
|:---:|
| <img width="400" alt="image" src="https://github.com/user-attachments/assets/ec30cf9e-12f5-4bce-b536-e645f26e8a60" /> |
| <img width="400" alt="image" src="https://github.com/user-attachments/assets/3df970c3-0d0b-456e-85db-0af8db91f565" /> |
