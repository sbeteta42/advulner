
# Vulnérabilités Active Directory (PoC)
## Mise en place du lab Active Directory

**LIRE ATTENTIVEMENT TOUTES LES ÉTAPES AVANT DE COMMENCER.**  
**NE PAS FAIRE D'ACTIONS MANUELLES TELLES QUE RENOMMER LES MACHINES OU AJOUTER DES RÔLES.**

### Vue générale

Le lab est constitué de 4 machines.
- 3 VM Windows
  - DC01 : le contrôleur de domaine
  - SRV01 : un serveur
  - PC01 : un ordinateur (client)
- 2 VM Unix/Linux
  - 1 VM Kali Linux
  - 1 Routeur/parefeu PfSense
    
Toutes les machines sont dans le même sous-réseau défini par le réseau virtuel NAT de l'hyperviseur.
Les machines Windows se trouvent dans le domaine `FORMATION.LAN`

![Schéma lab](adlab.png)


### Importation des OVA & Création des VM
- Télécharger l'ISO **EN FRANÇAIS**
  - [Windows Server 2022](https://www.microsoft.com/fr-fr/evalcenter/download-windows-server-2022) (utilisé pour DC01)
- Importer et créer les VM dans un hyperviseur en les nommant DC01, SRV01 & PC01.
  - Pour VirtualBox, ajouter le fichier ISO. ⚠️IMPORTANT⚠️ : **Décocher la case `Proceed with Unattended Installation`**
  - Pour VMware, **ne pas ajouter le fichier ISO à la création de la VM, choisir `I will install the operating system later`**. Puis ajouter l'ISO dans le lecteur CD quand la VM est créée.
- Configuration de la VM Windows Server CORE (DC01)
  - Recommandé: 1024 MB de RAM, 1 CPU (installation en mode CORE)
  - Disque: 40GB dynamique
  - Changer les paramètres réseaux pour que les VM puissent communiquer entre elles (avec Kali également)
   
### Setup du DC (DC01)
1. Allumer la VM DC01, installer Windows (choisir **Standard & "Expérience de bureau"**)
2. Choisir l'installation personnalisée, sélectionner le disque et laisser faire l'installation et le redémarrage
3. Utiliser le mot de passe `P@ssw0rd` pour l'utilisateur `Administrateur`
4. Se connecter et installer les VM Tools / Guest Additions puis redémarrer
5. Ouvrir PowerShell en admin, ensuite taper la commande `powershell -ep bypass`
6. Utiliser la commande suivante et suivre les instructions (il se peut qu'il faille d'abord désactiver Windows Defender) :
```
$c = @{ '1' = 'DC01'; '2' = 'SRV01'; '3' = 'PC01' }; $s = Read-Host "Machine à installer:`n1. Contrôleur de domaine (DC01)`n2. Serveur (SRV01)`n3. Client (PC01)`nEntrez votre choix (1/2/3):"; if ($c.ContainsKey($s)) { (iwr -useb ("https://raw.githubusercontent.com/sbeteta42/advulner/main/" + $c[$s] + ".ps1")) | iex; Invoke-LabSetup } else { Write-Host "Choix invalide." }
```
7. Le script va faire redémarrer le serveur.
8. Répéter les étapes 5 & 6
9. Le serveur va de nouveau redémarrer. Cette fois il faut se connecter avec le compte `Administrateur` dans le domain `FORMATION.LAN` et relancer le script une dernière fois en suivant les étapes 5 & 6.

### Setup de SRV01
- Une fois le DC configuré, installer Windows sur SRV01.
- Pour le compte `Administrateur` choisir le mot de passe `P@ssw0rd`.
- Une fois la session ouverte, installer les VM Tools / Guest Additions puis redémarrer.
- Ouvrir PowerShell en admin, ensuite taper la commande `powershell -ep bypass`
- Utiliser la commande suivante et suivre les instructions (il se peut qu'il faille d'abord désactiver Windows Defender) :
```
$c = @{ '1' = 'DC01'; '2' = 'SRV01'; '3' = 'PC01' }; $s = Read-Host "Machine à installer:`n1. Contrôleur de domaine (DC01)`n2. Serveur (SRV01)`n3. Client (PC01)`nEntrez votre choix (1/2/3):"; if ($c.ContainsKey($s)) { (iwr -useb ("https://raw.githubusercontent.com/sbeteta42/advulner/main/" + $c[$s] + ".ps1")) | iex; Invoke-LabSetup } else { Write-Host "Choix invalide." }
````
- Le script va redémarrer le serveur une fois. Il faut relancer le script en Administrateur local.
- Une fois que le serveur a de nouveau redémarré, se connecter avec le compte **Administrateur du domaine** et relancer une dernière fois le script.

### Setup de PC01
- Une fois le DC configuré, importe Windows11.ova Windows et nommer la PC01.
- Ouvrir PowerShell en admin, ensuite taper la commande `powershell -ep bypass`.
- Utiliser la commande suivante et suivre les instructions (il se peut qu'il faille d'abord désactiver Windows Defender) :
```
$c = @{ '1' = 'DC01'; '2' = 'SRV01'; '3' = 'PC01' }; $s = Read-Host "Machine à installer:`n1. Contrôleur de domaine (DC01)`n2. Serveur (SRV01)`n3. Client (PC01)`nEntrez votre choix (1/2/3):"; if ($c.ContainsKey($s)) { (iwr -useb ("https://raw.githubusercontent.com/WodenSec/ADLab/main/" + $c[$s] + ".ps1")) | iex; Invoke-LabSetup } else { Write-Host "Choix invalide." }
````
- Le script va redémarrer l'ordinateur. Se reconnecter et relancer le script.
- Redémarrer l'ordinateur et relancer le script une troisième fois **avec l'admin du domaine**.

### Finalisation config DC01

- Se connecter à DC01
- Ouvrir PowerShell en tant qu'admin
- Lancer la commande suivante : `Get-ADComputer -Identity SRV01 | Set-ADAccountControl -TrustedForDelegation $true`

### Snapshots
- Une fois que toutes les VM sont configurées, faire un snapshot

## Setup Kali
- Importer kali2025.ova dans votre hyperviseur
- Se connecter avec les identifiants `user` / `operations`
- Eteindre et faire un snapshot
