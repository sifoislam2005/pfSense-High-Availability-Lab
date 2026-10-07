# 🛡️ pfSense High Availability (HA) Lab

Documentation complète et guide étape par étape pour la mise en place d'une infrastructure à **Haute Disponibilité (HA)** sur pfSense en utilisant **CARP**, la **synchronisation XMLRPC** et le **NAT Sortant Avancé (Outbound NAT)**.

---

## 📌 Présentation du Projet

Ce Lab démontre comment concevoir et déployer une architecture de pare-feu redondante à haute disponibilité. Dans un environnement de production, cette configuration garantit une continuité de service maximale (zero downtime) en basculant automatiquement le trafic du nœud Maître (Master) vers le nœud de Secours (Backup) en cas de panne matérielle ou réseau.

---

## 🛠️ Infrastructure & Topologie

- **Hyperviseur :** VMware Workstation / ESXi
- **Nœuds Pare-feu :** 2x Machines Virtuelles pfSense (Master & Backup)
- **VIP (IP Virtuelles) :** Gérées via le protocole CARP pour les interfaces WAN, LAN et DMZ
- **Synchronisation :** Protocole XMLRPC via une interface dédiée (pfsync)

### Plan d'Adressage Réseau

| Interface | IP Master | IP Backup | IP Virtuelle (CARP VIP) | Masque de sous-réseau |
| :--- | :--- | :--- | :--- | :--- |
| **WAN** | `192.168.253.10` | `192.168.253.20` | `192.168.253.200` | `/24` |
| **LAN** | `192.168.1.2` | `192.168.1.3` | `192.168.1.1` | `/24` |
| **DMZ** | `192.168.2.2` | `192.168.2.3` | `192.168.2.1` | `/24` |
| **SYNC** | `10.10.10.1` | `10.10.10.2` | — | `/30` |

---

## 🚀 Fonctionnalités Clés Configurées

1. **CARP (Common Address Redundancy Protocol) :**
   - Configuration des IP Virtuelles partagées pour les interfaces WAN, LAN et DMZ.
   - Ajustement des valeurs de Skew (`0` pour le Master, `100` pour le Backup) pour définir la priorité des nœuds.

2. **Synchronisation d'État et de Configuration (XMLRPC & pfsync) :**
   - Mise en place d'un lien dédié point-à-point pour la synchronisation.
   - Réplication automatique des règles de pare-feu, de la configuration NAT, des alias et des paramètres système du Master vers le Backup.

3. **Redondance du NAT Sortant (Outbound NAT) :**
   - Passage du mode NAT automatique au mode **Hybride / Manuel**.
   - Mappage du trafic sortant (LAN/DMZ) sur l'IP Virtuelle **WAN CARP VIP** (`192.168.253.200`) au lieu de l'IP physique de l'interface, garantissant le maintien des sessions réseau lors d'un basculement.

---

## 🔍 Diagnostic & Résolution de Problème

Lors du déploiement, une erreur de synchronisation XMLRPC est survenue en raison d'un conflit de protocole sur le port 443 :
- **Problème :** Le nœud Master tentait de communiquer en `HTTP` alors que le nœud Backup écoutait en `HTTPS`.
- **Cause Racine :** Incohérence des paramètres du webConfigurator entre les deux pare-feux.
- **Résolution :** Alignement de la configuration sur le protocole HTTPS et le port de synchronisation sur les deux nœuds, résolvant ainsi l'erreur d'authentification XMLRPC.

---

## ✅ Tests de Basculement & Validation

- **Vérification du Statut CARP :** Validation du rôle `MASTER` sur le pare-feu principal et du rôle `BACKUP` sur le second.
- **Test de Synchronisation :** Création d'alias et de règles de filtrage sur le Master avec vérification de leur réplication instantanée sur le Backup.
- **Simulation de Panne :** Extinction forcée du nœud Master ; basculement immédiat et transparent des adresses VIP vers le nœud Backup sans perte de connectivité.

---

## 📄 Documentation PDF

Retrouvez le rapport complet et détaillé de ce Lab (avec captures d'écran) ici :
➡️ [`pfSense_HA_Lab_Report.pdf`](./pfSense_HA_Lab_Report.pdf)

---

## 👤 Auteur

**[Ton Prénom et Nom]**
- **Email :** ton.email@domain.com
- **LinkedIn :** [linkedin.com/in/votre-profil](https://linkedin.com/in/votre-profil)
- **GitHub :** [github.com/votre-username](https://github.com/votre-username)
