from pathlib import Path
import re

src = Path("/mnt/data/README.md").read_text(encoding="utf-8")

# Minimal intrusive enhancement: preserve structure/text while adding pictograms.
replacements = [
    ("# Vulnérabilités Active Directory (PoC)", "# 🔐 Vulnérabilités Active Directory (PoC)"),
    ("## Mise en place du lab Active Directory", "## 🏗️ Mise en place du lab Active Directory"),
    ("### Vue générale", "### 🌐 Vue générale"),
    ("### Importation des OVA & Création des VM", "### 📦 Importation des OVA & Création des VM"),
    ("### Setup du DC (DC01)", "### 🏛️ Setup du DC (DC01)"),
    ("### Setup de SRV01", "### 🗄️ Setup de SRV01"),
    ("### Setup de PC01 (Windows11.ova)", "### 💻 Setup de PC01 (Windows11.ova)"),
    ("### Finalisation config DC01", "### 🔧 Finalisation config DC01"),
    ("### Snapshots", "### 📸 Snapshots"),
]

out = src
for old, new in replacements:
    out = out.replace(old, new)

# add icons to bullets carefully
out = out.replace("- 3 VM Windows", "- 🪟 3 VM Windows")
out = out.replace("- DC01 : le contrôleur de domaine", "- 🏛️ DC01 : le contrôleur de domaine")
out = out.replace("- SRV01 : un serveur", "- 🗄️ SRV01 : un serveur")
out = out.replace("- PC01 : un ordinateur (client)", "- 💻 PC01 : un ordinateur (client)")
out = out.replace("- 2 VM Unix/Linux", "- 🐧 2 VM Unix/Linux")
out = out.replace("- 1 VM Kali Linux", "- 🐉 1 VM Kali Linux")
out = out.replace("- 1 Routeur/parefeu PfSense", "- 🔥 1 Routeur/parefeu PfSense")

# add warning icons
out = out.replace("**LIRE ATTENTIVEMENT TOUTES LES ÉTAPES AVANT DE COMMENCER.**", "⚠️ **LIRE ATTENTIVEMENT TOUTES LES ÉTAPES AVANT DE COMMENCER.**")
out = out.replace("**NE PAS FAIRE D'ACTIONS MANUELLES TELLES QUE RENOMMER LES MACHINES OU AJOUTER DES RÔLES.**", "❌ **NE PAS FAIRE D'ACTIONS MANUELLES TELLES QUE RENOMMER LES MACHINES OU AJOUTER DES RÔLES.**")

# add icons to setup steps
out = out.replace("1. Allumer la VM DC01, installer Windows", "1. 🚀 Allumer la VM DC01, installer Windows")
out = out.replace("2. Choisir l'installation personnalisée", "2. ⚙️ Choisir l'installation personnalisée")
out = out.replace("3. Utiliser le mot de passe", "3. 🔐 Utiliser le mot de passe")
out = out.replace("4. Se connecter et installer les VM Tools / Guest Additions puis redémarrer", "4. 🧰 Se connecter et installer les VM Tools / Guest Additions puis redémarrer")
out = out.replace("5. Ouvrir PowerShell en admin", "5. 💻 Ouvrir PowerShell en admin")
out = out.replace("6. Désactivation via stratégie registre", "6. 🛡️ Désactivation via stratégie registre")
out = out.replace("7. Utiliser la commande suivante", "7. 🤖 Utiliser la commande suivante")
out = out.replace("8. Le script va faire redémarrer le serveur.", "8. 🔁 Le script va faire redémarrer le serveur.")
out = out.replace("9. Répéter les étapes 5 & 7", "9. 🔁 Répéter les étapes 5 & 7")
out = out.replace("10. Le serveur va de nouveau redémarrer.", "10. 🔁 Le serveur va de nouveau redémarrer.")

# snapshots
out = out.replace("- Une fois que toutes les VMs sont configurées, faire un snapshot !", "- 📸 Une fois que toutes les VMs sont configurées, faire un snapshot !")

# kali section typo cleanup minimal
out = out.replace("## Setup Kali- Une fois la session ouverte, installer les VM Tools / Guest Additions puis redémarrer.", "## 🐉 Setup Kali Linux\n\n- 🧰 Une fois la session ouverte, installer les VM Tools / Guest Additions puis redémarrer.")

dest = Path("/mnt/data/README_EDITED_PICTOS.md")
dest.write_text(out, encoding="utf-8")

print("Fichier README édité généré :", dest)
