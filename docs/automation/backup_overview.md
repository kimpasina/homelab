# Backup & Replication – Vue d’ensemble

## Workflow

```text
02:45 - wol_script : Démarrage du backup (sth-bckp-01)
03:00 - docker-backup : Arrêt des stacks et backup des stacks vers le NAS (sth-prd-01)
03:00 à 05:00 - Snapshots & réplication TrueNAS (sth-prd-01)
05:00 - nas-backup-shutdown_script.sh : Shutdown du NAS de backup (sth-bckp-01)
```

---

## Serveurs impliqués

| Rôle                   | Hostname    | IP               | Modèle             |
| ---------------------- | ----------- | ---------------- | ------------------ |
| NAS Principal          | sth-prd-01  | 192.168.0.10/24  | Supermicro 2U      |
| NAS Backup             | sth-bckp-01 | 192.168.0.11/24  | Dell T130          |
| Orchestrateur / Docker | sth-prd-02  | 192.168.0.137/24 | Dell Optiplex 5050 |

---

## Scripts d’automation

> Les snapshots et la réplication **sont gérés par TrueNAS (sth-prd-01)**.

| Script                          | Heure | Serveur    | Description                                                                             | Documentation      |
| ------------------------------- | ----- | ---------- |-----------------------------------------------------------------------------------------| ------------------ |
| `wol_script.sh`                 | 02:45 | sth-prd-02 | Wake on LAN du backup                                                                   | [wol_script.md](docs/automation/wol_script.md)      |
| `docker-backup.sh`                | 03:00 | sth-prd-02 | Arrête tous les stacks Docker **sauf** `portainer_data`, `syncthing_shared`, `nfs_data` | —                  |
| `nas-backup-shutdown_script.sh` | 05:00 | sth-prd-02 | Shutdown du backup via SSH                                                              | [shutdown_script.md](docs/automation/shutdown_script.md) |

---

## Cron Jobs

```bash
# Sur sth-prd-02 (root)
0 3 * * * /usr/local/bin/backup_script/backup >> /var/log/docker-backup.log 2>&1
45 2 * * * /usr/local/bin/nas-backup-wol_script/wol_script.sh
0 5 * * * /usr/local/bin/nas-backup-shutdown_script/nas-backup-shutdown_script.sh
```

---

## Notes

* Utilisateur dédié `shutdown` sur le backup et peut uniquement éteindre le système.
* Authentification SSH par clé pour le user `shutdown` et script init dans le backup pour ajouter `shutdown` dans les sudoers.
* Les snapshots et la réplication sont exécutés via les **tasks natives TrueNAS** (récursives, planifiées, avec rétention).
* Le backup est actif uniquement entre **02:45 et 05:00**.
