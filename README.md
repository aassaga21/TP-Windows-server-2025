# Module 123 - Activer les services d'un serveur / Windows server OS administration - Advanced

Auteure: Alexandra ASSAGA
Date: 23.03.2026 - 26.03.2026

![GIT_Logo](https://hackmd.io/_uploads/r1MBSXJ2Wx.png)

---

## **Introduction**

Dans le cadre de ce travail pratique, nous avons mis en place une infrastructure réseau basée sur **Active Directory** afin de centraliser la gestion des utilisateurs, des machines et des stratégies de sécurité au sein d’un domaine Windows.

L’environnement déployé repose sur **Windows Server 2025** avec deux contrôleurs de domaine configurés pour assurer la **haute disponibilité** des services critiques. Cette architecture permet de garantir la continuité de service, notamment grâce à la réplication Active Directory et au mécanisme de **basculement (failover)** du serveur DHCP.

Par ailleurs, ce TP met en œuvre les **stratégies de groupe (GPO)** afin d’appliquer des configurations et des restrictions automatiquement aux utilisateurs, comme l’imposition d’un fond d’écran ou le blocage de certaines fonctionnalités système.

Ce projet permet ainsi de comprendre les mécanismes fondamentaux d’une infrastructure Windows en entreprise, incluant la gestion des identités, des services réseau et des politiques de sécurité.

---

## **Objectifs**

L’objectif principal de ce TP est de concevoir et de configurer une infrastructure Active Directory fonctionnelle et sécurisée, tout en assurant la disponibilité des services réseau.

Les objectifs spécifiques sont les suivants :

### Mise en place de l’infrastructure

* Installer et configurer deux serveurs Windows Server 2025
* Créer un domaine Active Directory (`git.local`)
* Organiser les objets dans des unités organisationnelles (OU)
* Intégrer un poste client au domaine

---

### Configuration des services réseau

* Installer et configurer le serveur DHCP
* Créer et paramétrer une plage d’adresses IP
* Mettre en place un mécanisme de **failover DHCP** entre deux serveurs
* Tester la tolérance aux pannes

---

### Gestion des ressources et des accès

* Créer un partage réseau accessible via un chemin UNC
* Configurer les permissions **SMB et NTFS**
* Résoudre les problèmes d’accès liés aux droits utilisateurs

---

### Déploiement des stratégies de groupe (GPO)

* Créer et configurer des GPO utilisateur
* Imposer un fond d’écran réseau
* Restreindre l’accès au panneau de configuration
* Bloquer l’utilisation de l’invite de commandes

---

### Vérification et validation

* Tester l’application des GPO sur un poste client
* Vérifier la distribution des adresses IP via DHCP
* Valider le bon fonctionnement du failover
* Utiliser des outils comme `gpresult` et `Test-Path`

---

## DAT (Document d'Architecture Technique)

---

## I. Réalisation de l'infrastructure

### 1. Mise en place de l’environnement

Dans cette première partie, nous avons procédé à la mise en place de l’infrastructure nécessaire au déploiement des stratégies de groupe (GPO).

#### 1.1 Téléchargement de l’image ISO

La première étape a consisté à télécharger l’image ISO du système d’exploitation :

* Système : Windows Server 2025
* Source : site officiel Microsoft
* Format : fichier .iso

[Téléchargement de Windows Server 2025](https://www.microsoft.com/fr-fr/evalcenter/download-windows-server-2025)
![image](https://hackmd.io/_uploads/S1b0ITzsZx.png)

Cette image a ensuite été utilisée pour créer les machines virtuelles.

#### 1.2 Création des machines virtuelles

Deux machines virtuelles ont été créées afin de simuler une infrastructure Active Directory redondante :

a. Contrôleurs de domaine
* DC1-AA (principal)
* DC2-AA (secondaire)

b. Caractéristiques techniques
* RAM : 4 Go
* CPU : 2 vCPU
* Disque : 60 GB
* Réseau : mode bridge / NAT

![image](https://hackmd.io/_uploads/rkzWDTGs-e.png)
![image](https://hackmd.io/_uploads/ByxW8O6Mi-l.png)

##### DC1: Installation et Configuration
![image](https://hackmd.io/_uploads/Hyz1vaMjZg.png)
![image](https://hackmd.io/_uploads/H1lxvTGobl.png)
![image](https://hackmd.io/_uploads/H1so_Tzi-g.png)
![image](https://hackmd.io/_uploads/SJi9wazj-l.png)
![image](https://hackmd.io/_uploads/BymswTMiWx.png)
![image](https://hackmd.io/_uploads/HJssvpGiZe.png)
![image](https://hackmd.io/_uploads/H1bnv6fsWx.png)
![image](https://hackmd.io/_uploads/Sys3wTMjWg.png)
![image](https://hackmd.io/_uploads/rydaw6ziZe.png)
![image](https://hackmd.io/_uploads/SyxAwpGjWe.png)
![image](https://hackmd.io/_uploads/HJJW_6Msbl.png)
![image](https://hackmd.io/_uploads/S1KZOTzobx.png)
![image](https://hackmd.io/_uploads/Hye4OTzsbx.png)
![image](https://hackmd.io/_uploads/BJIEO6Gobl.png)
![image](https://hackmd.io/_uploads/By6Vupfi-x.png)

- Fixer une ip statique au DC1
![image](https://hackmd.io/_uploads/Sk4ktpzjbl.png)
![image](https://hackmd.io/_uploads/ryGgFpzjbg.png)

- Désactivation de la Configuration de sécurité renforcée d'Internet Explorer (IE ESC)
![image](https://hackmd.io/_uploads/rkB-tazoZg.png)

- Désactivation de la complexité de mot de passe
![image](https://hackmd.io/_uploads/SJnWKpGo-e.png)
![image](https://hackmd.io/_uploads/B1_1dafjZe.png)

- Création de la règle de pare-feu qui autorise un ping sur le DC1
![image](https://hackmd.io/_uploads/r1Z3IafiZg.png)
![image](https://hackmd.io/_uploads/SJn0jiosbl.png)

##### DC2: Installation et Configuration
![image](https://hackmd.io/_uploads/HkZvKpGsZl.png)
![image](https://hackmd.io/_uploads/SyHPE6Mibx.png)
![image](https://hackmd.io/_uploads/B15sE6GsZx.png)
![image](https://hackmd.io/_uploads/Hk46E6zjbx.png)
![image](https://hackmd.io/_uploads/BkcTNTMoWx.png)
![image](https://hackmd.io/_uploads/rJZ0NpziWx.png)
![image](https://hackmd.io/_uploads/Hkw1HTzsZg.png)
![image](https://hackmd.io/_uploads/rkNxB6Msbe.png)
![image](https://hackmd.io/_uploads/HkrWr6Gs-g.png)
![image](https://hackmd.io/_uploads/rJVfHpfoWg.png)
![image](https://hackmd.io/_uploads/BJiMr6zi-x.png)
![image](https://hackmd.io/_uploads/H18Vr6Mi-l.png)
![image](https://hackmd.io/_uploads/Skv8rafibe.png)
![image](https://hackmd.io/_uploads/BkfdrTfoZe.png)
![image](https://hackmd.io/_uploads/HygYBpMsWl.png)
![image](https://hackmd.io/_uploads/r1UYH6fs-g.png)
![image](https://hackmd.io/_uploads/rJH9HTzjWg.png)

##### Windows 10: Installation et Configuration


---

## II. Déploiement d'Active Directory Domain Services, DNS et résolution de noms

### 1. Installation d’Active Directory

Sur le serveur principal (DC1-AA) :

#### DC1: Installation du rôle AD DS :

![image](https://hackmd.io/_uploads/rkgLWRGoZe.png)
![image](https://hackmd.io/_uploads/r1ILW0zibe.png)
![image](https://hackmd.io/_uploads/rkh8b0Gobg.png)
![image](https://hackmd.io/_uploads/rJMDZRzj-x.png)
![image](https://hackmd.io/_uploads/B1OvWAfsbx.png)
![image](https://hackmd.io/_uploads/rkCDWCMjWg.png)
![image](https://hackmd.io/_uploads/r1UuWCzs-e.png)
![image](https://hackmd.io/_uploads/B1jhrCzobe.png)
![image](https://hackmd.io/_uploads/HkRhB0MoZe.png)
![image](https://hackmd.io/_uploads/ByPTSAzibx.png)
![image](https://hackmd.io/_uploads/B1dsB0ziZg.png)
![image](https://hackmd.io/_uploads/ry_-IRGo-x.png)
![image](https://hackmd.io/_uploads/BkSRURMsWl.png)
![image](https://hackmd.io/_uploads/SJoR8CfoWg.png)
![image](https://hackmd.io/_uploads/BJQNvRGiZg.png)
![image](https://hackmd.io/_uploads/HkESwCMiZe.png)
![image](https://hackmd.io/_uploads/BJorvAMo-g.png)
![image](https://hackmd.io/_uploads/ry88PRzsbg.png)
![image](https://hackmd.io/_uploads/Hk-wwCMiZg.png)
![image](https://hackmd.io/_uploads/HJdvwCfs-x.png)
![image](https://hackmd.io/_uploads/BJbdw0zibx.png)
![image](https://hackmd.io/_uploads/BkNYPRfobx.png)
![image](https://hackmd.io/_uploads/ByHl_Czj-l.png)
![image](https://hackmd.io/_uploads/BJ5ZuCGjWe.png)
![image](https://hackmd.io/_uploads/ryEZvCzi-l.png)
![image](https://hackmd.io/_uploads/BJiJPRzsWl.png)
![image](https://hackmd.io/_uploads/SyPVdAMo-l.png)
![image](https://hackmd.io/_uploads/BygHdRGo-g.png)
![image](https://hackmd.io/_uploads/S15BOCfiZl.png)
![image](https://hackmd.io/_uploads/HkPI_CzjWe.png)
![image](https://hackmd.io/_uploads/Sk1O_Afs-l.png)
![image](https://hackmd.io/_uploads/S1add0fjWl.png)
![image](https://hackmd.io/_uploads/rJf3_0Mobl.png)
![image](https://hackmd.io/_uploads/ryDnuRzj-l.png)
![image](https://hackmd.io/_uploads/BJ2n_0zi-l.png)
![image](https://hackmd.io/_uploads/ByZ6uAzibx.png)
![image](https://hackmd.io/_uploads/Bk26dCMoWe.png)
![image](https://hackmd.io/_uploads/HyfR_CfoWg.png)
![image](https://hackmd.io/_uploads/HkX890fibe.png)
![image](https://hackmd.io/_uploads/HkYLqRfjZg.png)
![image](https://hackmd.io/_uploads/Byv0uAzoZe.png)
![image](https://hackmd.io/_uploads/Bk-ytAGjbe.png)
![image](https://hackmd.io/_uploads/HyPJYRfo-g.png)
![image](https://hackmd.io/_uploads/Syn1tAfobg.png)
![image](https://hackmd.io/_uploads/BJ4eY0zsbx.png)
![image](https://hackmd.io/_uploads/HyAgYAzjbl.png)
![image](https://hackmd.io/_uploads/rJcPtCGo-g.png)
![image](https://hackmd.io/_uploads/ry-_YRMoWe.png)

Sur le serveur secondaire (DC2-AA) :

**Sur DC2-AA : Ajout du second contrôleur de domaine**

- Rejoindre le domaine

![image](https://hackmd.io/_uploads/SyPYmafoZl.png)
![image](https://hackmd.io/_uploads/H1z97pfsZl.png)
![image](https://hackmd.io/_uploads/H1a9QTGsbg.png)
![image](https://hackmd.io/_uploads/Hyyhm6fjbg.png)
![image](https://hackmd.io/_uploads/SJsTmafoZe.png)
![image](https://hackmd.io/_uploads/ry3xN6GiZl.png)
![image](https://hackmd.io/_uploads/rJ2zVazsWe.png)
![image](https://hackmd.io/_uploads/SJmXVpfoWx.png)
![image](https://hackmd.io/_uploads/B15Q4pfsZl.png)
![image](https://hackmd.io/_uploads/SkRjr6fiZg.png)
![image](https://hackmd.io/_uploads/Bk8pBaMs-x.png)
![image](https://hackmd.io/_uploads/rybCrpfo-l.png)
![image](https://hackmd.io/_uploads/SkuABTGs-e.png)
![image](https://hackmd.io/_uploads/ryLyL6MoZg.png)
![image](https://hackmd.io/_uploads/BJDlITMiZx.png)
![image](https://hackmd.io/_uploads/SkkWLpMsZx.png)
![image](https://hackmd.io/_uploads/rJdZI6MiWl.png)
![image](https://hackmd.io/_uploads/H1vzUpzjZl.png)
![image](https://hackmd.io/_uploads/B1JmLTMoWe.png)
![image](https://hackmd.io/_uploads/Sy8XU6fjWe.png)
![image](https://hackmd.io/_uploads/HJsXUaGj-g.png)
![image](https://hackmd.io/_uploads/HJUE8afi-e.png)
![image](https://hackmd.io/_uploads/r1SH8afo-g.png)

---

## III. Installation et configuration du serveur DHCP

Afin d’automatiser l’attribution des adresses IP sur le réseau, le rôle DHCP a été installé sur le contrôleur de domaine principal DC1-AA.

#### 1. DC1:
a. **Installation**
![image](https://hackmd.io/_uploads/Hyiq5AGoZl.png)
![image](https://hackmd.io/_uploads/HJVs9Rfi-x.png)
![image](https://hackmd.io/_uploads/HJHs5Rfo-x.png)
![image](https://hackmd.io/_uploads/SJ87sRzo-e.png)
![image](https://hackmd.io/_uploads/rJzViAfoZe.png)
![image](https://hackmd.io/_uploads/S1JSo0zoZl.png)
![image](https://hackmd.io/_uploads/rJZ8oRMi-g.png)
![image](https://hackmd.io/_uploads/ryO8jAMj-x.png)
![image](https://hackmd.io/_uploads/HygDj0Mi-e.png)
![image](https://hackmd.io/_uploads/HkaDiRMsbl.png)
![image](https://hackmd.io/_uploads/rk1uiCMsWe.png)
![image](https://hackmd.io/_uploads/HkHOj0GoZe.png)
![image](https://hackmd.io/_uploads/rJxqOoAfo-e.png)

b. **Configuration**
![image](https://hackmd.io/_uploads/BJ8Yj0zsZx.png)
![image](https://hackmd.io/_uploads/r1nFj0Mjbx.png)
![image](https://hackmd.io/_uploads/HyKcsAGobl.png)
![image](https://hackmd.io/_uploads/B11oiAzi-e.png)
![image](https://hackmd.io/_uploads/Sy8jsAfsbx.png)
![image](https://hackmd.io/_uploads/HkrhsRGoWg.png)
![image](https://hackmd.io/_uploads/H1pns0zjbx.png)
![image](https://hackmd.io/_uploads/rkN6sRfibe.png)
![image](https://hackmd.io/_uploads/SJsTjAMiWe.png)
![image](https://hackmd.io/_uploads/rkx70jRziZl.png)
![image](https://hackmd.io/_uploads/rJcCsCGiZl.png)
![image](https://hackmd.io/_uploads/ryf1hRGobl.png)
![image](https://hackmd.io/_uploads/Bybl3RGjbe.png)

c. **Vérification avec un pc client Windows 10**:
![image](https://hackmd.io/_uploads/S1CxhCGsZg.png)
![image](https://hackmd.io/_uploads/S1vbnRGjbe.png)
![image](https://hackmd.io/_uploads/SyaZ2RMobl.png)

#### 2. DC2:
a. **Installation**
![image](https://hackmd.io/_uploads/BJP78Afibx.png)
![image](https://hackmd.io/_uploads/S1VN8AfsWl.png)
![image](https://hackmd.io/_uploads/HkHrI0Mi-l.png)
![image](https://hackmd.io/_uploads/H1hrU0zo-x.png)
![image](https://hackmd.io/_uploads/B18wLAzoWx.png)
![image](https://hackmd.io/_uploads/rJQuI0Mjbl.png)
![image](https://hackmd.io/_uploads/HJW5I0fsbl.png)
![image](https://hackmd.io/_uploads/SyGiUCfoWl.png)
![image](https://hackmd.io/_uploads/ByghUAfsZl.png)
![image](https://hackmd.io/_uploads/r1vMtAGjZx.png)

b. **Mise en place du cluster DHCP (Failover)**

Afin d’assurer la haute disponibilité, un mécanisme de basculement DHCP a été configuré entre :

* DC1-AA (principal)
* DC2-AA (secondaire)

![image](https://hackmd.io/_uploads/rkhEqJmibl.png)
![image](https://hackmd.io/_uploads/H12S9yXjbg.png)
![image](https://hackmd.io/_uploads/rJbyoyQoZe.png)
![image](https://hackmd.io/_uploads/ByWbaJXjbg.png)
![image](https://hackmd.io/_uploads/BkDEayQiWg.png)
![image](https://hackmd.io/_uploads/HJKLTk7s-e.png)
![image](https://hackmd.io/_uploads/HJN_6J7sWe.png)
![image](https://hackmd.io/_uploads/r1xSu4msbx.png)
![image](https://hackmd.io/_uploads/rybxtVXoWe.png)
![image](https://hackmd.io/_uploads/BJBItVQsWe.png)
![image](https://hackmd.io/_uploads/rJQloN7jWg.png)
![image](https://hackmd.io/_uploads/BkAFsNXo-e.png)
![image](https://hackmd.io/_uploads/SyQHpN7o-g.png)
![image](https://hackmd.io/_uploads/H1DSsHQiWl.png)
![image](https://hackmd.io/_uploads/SyPusSXsZl.png)
![image](https://hackmd.io/_uploads/ByQojHQiWx.png)

c. **Test de basculement**
Simulation de panne en arrêtant le service DHCP sur DC1
```
Stop-Service DHCPServer
```
![image](https://hackmd.io/_uploads/SkTlY2oo-g.png)
![image](https://hackmd.io/_uploads/rkHykfao-g.png)
![image](https://hackmd.io/_uploads/r19LYhso-l.png)
![image](https://hackmd.io/_uploads/BJExPGaobe.png)
![image](https://hackmd.io/_uploads/SyFVwG6iZx.png)

Vérification si le service DHCP esn toujours activé sur DC2 malgré l'arrêt sur le DC1:
![image](https://hackmd.io/_uploads/SJ32jhij-x.png)

---

## IV. Gestion des utilisateurs, groupes et Unités d'Organisation
![image](https://hackmd.io/_uploads/ryD06Cfobe.png)

**Intégration du poste client**

- Un poste client PC1-AA a été ajouté au domaine :
![image](https://hackmd.io/_uploads/rkm4qXmiZe.png)
![image](https://hackmd.io/_uploads/H1enp7QjZg.png)
![image](https://hackmd.io/_uploads/BJ01AmQj-x.png)
![image](https://hackmd.io/_uploads/SJ_bAXQiZe.png)

- Un utilisateur du post client PC1-AA a été ajouté au domaine :
![image](https://hackmd.io/_uploads/ByOSWS7o-g.png)
![image](https://hackmd.io/_uploads/BkI5WSXs-l.png)
![image](https://hackmd.io/_uploads/H1PZQS7o-g.png)
![image](https://hackmd.io/_uploads/HJMdmSQiZx.png)
![image](https://hackmd.io/_uploads/Bktb4BQiWx.png)

---

## V. Stratégies de groupe (GPO) et gestion centralisée

![image](https://hackmd.io/_uploads/rJoMRGJnWx.png)

1. Modules PowerShell requis
Le module GroupPolicy doit être importé sur le contrôleur de domaine :

```
Import-Module GroupPolicy
```

2. Création du partage réseau

2.1 Contexte
La GPO de fond d'écran nécessite un chemin UNC accessible par tous les postes clients. Le fichier fond.jpg est stocké dans le dossier Downloads de l'Administrateur sur le serveur.

Emplacement physique du fichier
```
C:\Users\Administrateur\Downloads\fond.jpg
```

2.2. Commandes de création du partage

```
# Récupérer l'ACL du dossier Downloads
$acl = Get-Acl "C:\Users\Administrateur\Downloads"

# Ajouter la règle de lecture pour Tout le monde
$rule = New-Object System.Security.AccessControl.FileSystemAccessRule(
    "Tout le monde",
    "ReadAndExecute",
    "ContainerInherit,ObjectInherit",
    "None",
    "Allow"
)
$acl.AddAccessRule($rule)
Set-Acl -Path "C:\Users\Administrateur\Downloads" -AclObject $acl

# Vérifier les permissions appliquées
Get-Acl "C:\Users\Administrateur\Downloads" | Format-List

```
![image](https://hackmd.io/_uploads/Skpn0MJ3Wx.png)

2.3. Vérification du partage

```
# Vérifier que le partage est créé
Get-SmbShare -Name "partage"

# Tester l'accès au fichier via chemin UNC
Test-Path "\\$env:COMPUTERNAME\partage\fond.jpg"

# Résultat attendu : True
```

![image](https://hackmd.io/_uploads/rJSF1QJn-x.png)

3. Création des GPO

3.1 GPO — Fond d'écran imposé
Cette GPO impose un fond d'écran identique à tous les utilisateurs. Le fichier image est servi depuis le partage réseau créé à l'étape 2.

```
# Créer la GPO
New-GPO -Name "GPO-FondEcran" -Comment "Impose un fond d'écran"

# Définir le chemin de l'image
Set-GPRegistryValue -Name "GPO-FondEcran" `
    -Key "HKCU\Software\Microsoft\Windows\CurrentVersion\Policies\System" `
    -ValueName "Wallpaper" -Type String `
    -Value "\\$env:COMPUTERNAME\partage\fond.jpg"

# Définir le style (2 = Étiré)
Set-GPRegistryValue -Name "GPO-FondEcran" `
    -Key "HKCU\Software\Microsoft\Windows\CurrentVersion\Policies\System" `
    -ValueName "WallpaperStyle" -Type String -Value "2"
```
![image](https://hackmd.io/_uploads/r1SKG7yhWe.png)
![image](https://hackmd.io/_uploads/B1dRf7y3-l.png)

3.2 GPO — Blocage du Panneau de configuration
Cette GPO empêche les utilisateurs d'accéder au Panneau de configuration Windows.

```
# Créer la GPO
New-GPO -Name "GPO-BlocagePanneauConfig" -Comment "Interdit panneau config"

# Activer le blocage
Set-GPRegistryValue -Name "GPO-BlocagePanneauConfig" `
    -Key "HKCU\Software\Microsoft\Windows\CurrentVersion\Policies\Explorer" `
    -ValueName "NoControlPanel" -Type DWord -Value 1
```
![image](https://hackmd.io/_uploads/HJ4f7mJ2bg.png)
![image](https://hackmd.io/_uploads/H1z7XQk3bl.png)

3.3. GPO — Blocage de l'invite de commandes
Cette GPO interdit l'accès à cmd.exe pour les utilisateurs standards.

```
# Créer la GPO
New-GPO -Name "GPO-BlocageCMD" -Comment "Interdit cmd.exe"

# Activer le blocage
Set-GPRegistryValue -Name "GPO-BlocageCMD" `
    -Key "HKCU\Software\Policies\Microsoft\Windows\System" `
    -ValueName "DisableCMD" -Type DWord -Value 1
```
![image](https://hackmd.io/_uploads/SJS4X713Zg.png)
![image](https://hackmd.io/_uploads/rkmH7Xynbl.png)

4. Liaison des GPO aux OUs

Les trois GPO sont de type Utilisateur (HKCU). Elles sont liées à l'OU USERS_GIT où se trouve l'utilisateur Alexandra.

```
$ou = "OU=USERS_GIT,OU=GIT,DC=git,DC=local"

New-GPLink -Name "GPO-FondEcran"            -Target "OU=USERS_GIT,OU=GIT,DC=git, DC=local" -LinkEnabled Yes
New-GPLink -Name "GPO-BlocagePanneauConfig" -Target "OU=USERS_GIT,OU=GIT,DC=git, DC=local" -LinkEnabled Yes
New-GPLink -Name "GPO-BlocageCMD"           -Target "OU=USERS_GIT,OU=GIT,DC=git, DC=local" -LinkEnabled Yes
```
![image](https://hackmd.io/_uploads/S1NLmXyhWl.png)
![image](https://hackmd.io/_uploads/B1m_mmynWg.png)
![image](https://hackmd.io/_uploads/rJ-qmXynZe.png)

5. Application des stratégies

5.1 Sur le contrôleur de domaine
```
gpupdate /force
```
![image](https://hackmd.io/_uploads/SJOwRMJnbx.png)

> **Résultat attendu :**
La mise à jour de la stratégie d'ordinateur s'est terminée sans erreur.
La mise à jour de la stratégie utilisateur s'est terminée sans erreur.

5.2 Sur le poste client PC1-AA
```
# Forcer la mise à jour depuis le serveur via PowerShell Remoting
Invoke-Command -ComputerName "PC1-AA" -ScriptBlock { gpupdate /force }

# Ou directement sur le poste client
gpupdate /force

```
![image](https://hackmd.io/_uploads/S1H8ymJhbx.png)
![image](https://hackmd.io/_uploads/BJZ34XJnWx.png)
![image](https://hackmd.io/_uploads/rkpVVQJ3Wl.png)
![image](https://hackmd.io/_uploads/ByFSNQk2bx.png)
![image](https://hackmd.io/_uploads/SJGDEXJnZx.png)
![image](https://hackmd.io/_uploads/SynwVQJ2be.png)

---

## Publication projet sur Github
![image](https://hackmd.io/_uploads/B1wmdqfnbe.png)
![image](https://hackmd.io/_uploads/SJAI_5G2bx.png)
![image](https://hackmd.io/_uploads/HkO9ucG3-g.png)
![image](https://hackmd.io/_uploads/r1ElKcG3bx.png)
![image](https://hackmd.io/_uploads/BJxPt5MhZg.png)
![image](https://hackmd.io/_uploads/rJDJc9Gh-e.png)
![image](https://hackmd.io/_uploads/ryTljqGnbl.png)
![image](https://hackmd.io/_uploads/ry4onqf2Wx.png)


---

## Conclusion
Ce projet illustre la mise en place d’une infrastructure robuste et réaliste, proche des environnements rencontrés en entreprise, avec une attention particulière portée à la disponibilité des services et à la sécurisation des accès.

