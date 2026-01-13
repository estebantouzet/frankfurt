# 5. Paramétrage et sécurisation du service AD

**Commande de base :**

## 1. Chargement du module Active Directory
```bash
Import-Module ActiveDirectory
```

## 2. Création d'une unité d'organisation (OU)
```bash
New-ADOrganizationalUnit -Name "Salle005" -Path "DC=local,DC=anvers,DC=cub,DC=sioplc,DC=fr"
```

## 3. Déplacer un ordinateur vers une unité d'organisation
```bash
PS C:\Users\Administrateur> Move-ADObject -Identity "CN=posteA,OU=Salle002,DC=local,DC=anvers,DC=cub,DC=sioplc,DC=fr" -TargetPath "OU=Salle001,DC=local,DC=anvers,DC=cub,DC=sioplc,DC=fr"
```

## 4. Création d'un utilisateur
```bash
New-ADUser -Name "Patricia Delouche" -GivenName "Patricia" -Surname "Delouche" -SamAccountName "pdelouche" -UserPrincipalName "pdelouche@local.anvers.cub.sioplc.fr" -Path "CN=Users,DC=local,DC=anvers,DC=cub,DC=sioplc,DC=fr" -AccountPassword (ConvertTo-SecureString "Provisoire_007" -AsPlainText -Force) -Enabled $true -ChangePasswordAtLogon $true
```

## 5. Création d'un groupe
```bash
New-ADGroup -Name "Développeurs" -GroupScope Global -Path "CN=Users,DC=local,DC=anvers,DC=cub,DC=sioplc,DC=fr"
```

## 6. Ajouter un utilisateur au groupe
```bash
Add-ADGroupMember -Identity "Développeurs" -Members "pdelouche"
```

## 7. Lister tous les comptes avec informations détaillées
```bash
Get-ADUser -Filter * -Property * | Select-Object Name, GivenName, Surname, SamAccountName, Enabled, PasswordLastSet, LastLogonDate
```


