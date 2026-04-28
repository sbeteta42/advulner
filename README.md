# 🔐 Active Directory Vulnerabilities Lab (PoC)

> Environnement de laboratoire dédié aux démonstrations et travaux pratiques autour des vulnérabilités Active Directory dans un contexte pédagogique cybersécurité. :contentReference[oaicite:0]{index=0}

---

# 📚 Sommaire

- 🎯 Objectif du lab
- 🖥️ Architecture du laboratoire
- 📦 Préparation des machines virtuelles
- 🏢 Configuration du contrôleur de domaine (DC01)
- 🗄️ Configuration du serveur (SRV01)
- 💻 Configuration du poste client (PC01)
- 🐉 Configuration de Kali Linux
- 🔧 Finalisation Active Directory
- 📸 Snapshots & sauvegardes
- ⚠️ Consignes importantes

---

# 🎯 Objectif du lab

Ce laboratoire permet de mettre en place un environnement complet Active Directory vulnérable afin de réaliser des démonstrations et PoC autour des attaques suivantes :

- 🔓 Kerberoasting
- 🎟️ Pass-the-Hash
- 🧠 NTLM Relay
- 🕵️ Lateral Movement
- 🔥 Privilege Escalation
- 🧬 Délégation Kerberos
- 💀 Exploitation de mauvaises configurations AD

---

# 🖥️ Architecture du laboratoire

## 📌 Infrastructure

Le lab est composé des machines suivantes : :contentReference[oaicite:1]{index=1}

### 🪟 Machines Windows

| Machine | Rôle |
|---|---|
| 🏛️ DC01 | Contrôleur de domaine |
| 🗄️ SRV01 | Serveur membre |
| 💻 PC01 | Poste client |

### 🐧 Machines Linux

| Machine | Rôle |
|---|---|
| 🐉 Kali Linux | Machine d’attaque |
| 🔥 pfSense | Routeur / Pare-feu |

---

## 🌐 Réseau

Toutes les machines utilisent le même sous-réseau NAT de l’hyperviseur. :contentReference[oaicite:2]{index=2}

Le domaine Active Directory utilisé est :

```txt
FORMATION.LAN
