# Fiche procédure – Gérer les Droits des utilisateurs sur un dossier 

## 1. Voir les permissions actuelles d’un dossier

```bash
$folder = "C:\TestPerms"
Get-Acl -Path $folder | Format-List -Property Path, Owner, Access
```

`Access` liste les règles (qui a quoi : Read, Modify, FullControl...).

## 2. Ajouter des droits à un utilisateur (exemples)

### Exemple A — Donner FullControl (contrôle total) à *`MONDOMAINE\jdupont`*

```bash
$folder = "C:\TestPerms"
$acl = Get-Acl -Path $folder

$rule = New-Object System.Security.AccessControl.FileSystemAccessRule(
    "MONDOMAINE\jdupont",            # nom d'utilisateur ou groupe
    "FullControl",                   # droits (FullControl, Modify, ReadAndExecute, Read, Write)
    "ContainerInherit, ObjectInherit", # héritage : dossiers + fichiers
    "None",                          # propagation
    "Allow"                          # type : Allow ou Deny
)

$acl.AddAccessRule($rule)
Set-Acl -Path $folder -AclObject $acl
```

**Remplace** *`MONDOMAINE\jdupont`* *`par MACHINE\user`* pour un utilisateur local (*ex:* *`.\jdupont`*) ou *`jdupont`* si approprié.

### Exemple B — Donner ReadAndExecute (lecture/exécution)

```bash
# mêmes variables $folder et $acl que ci-dessus
$rule = New-Object System.Security.AccessControl.FileSystemAccessRule("MONDOMAINE\jdupont","ReadAndExecute","ContainerInherit, ObjectInherit","None","Allow")
$acl.AddAccessRule($rule)
Set-Acl -Path $folder -AclObject $acl
```

## 3. Supprimer des droits d’un utilisateur

!!! warning "Attention"
    La suppression via objet nécessite de reconstruire la règle exacte (même flags). Si ça ne fonctionne pas, utiliser icacls (voir plus bas).


```bash
$folder = "C:\TestPerms"
$acl = Get-Acl -Path $folder

$rule = New-Object System.Security.AccessControl.FileSystemAccessRule("MONDOMAINE\jdupont","FullControl","ContainerInherit, ObjectInherit","None","Allow")

# Retire la règle correspondante
$removed = $acl.RemoveAccessRule($rule)

# Applique
Set-Acl -Path $folder -AclObject $acl
```

Si *`RemoveAccessRule`* ne supprime pas (cas fréquent si la règle n'est pas strictement identique), utilise *`icacls`* :

Avec icacls (plus simple pour supprimer toutes les permissions d’un user) :

```bash
icacls "C:\TestPerms" /remove "MONDOMAINE\jdupont"
```

## 4. Sauvegarder et restaurer les ACL (très utile avant les changements)

**Sauvegarder**

```bash
Get-Acl -Path "C:\TestPerms" | Export-Clixml -Path "C:\Sauvegarde\TestPerms_ACL.xml"
```

**Restaurer**

```bash
$acl = Import-Clixml -Path "C:\Sauvegarde\TestPerms_ACL.xml"
Set-Acl -Path "C:\TestPerms" -AclObject $acl
```

*`Export-Clixml`*/*`Import-Clixml`* préservent la structure de l’ACL.

## 5. Vérifier le résultat

```bash
Get-Acl -Path "C:\TestPerms" | Format-List -Property Access
```
Vérifie que l’utilisateur apparaît avec le bon droit (FullControl, Modify, etc.).

## 6. Alternative rapide : icacls (utile et simple)

Donner **Modify** (modifier) : 
```bash
icacls "C:\TestPerms" /grant "MONDOMAINE\jdupont:(OI)(CI)M"
```

Supprimer l’utilisateur :
```bash
icacls "C:\TestPerms" /remove "MONDOMAINE\jdupont"
```

Explication rapide des flags :

* `(OI)` = Object Inherit (hérite aux fichiers)
* `(CI)` = Container Inherit (hérite aux sous-dossiers)
* `M` = Modify, *`F`* = Full, *`RX`* = Read & Execute, *`R`* = Read, *`W`* = Write

## 8. Mini-scénario complet (exécuté sur un dossier test)

```bash
# Variables
$folder = "C:\TestPerms"
$user   = "MONDOMAINE\jdupont"

# 1) Sauvegarde ACL
Get-Acl -Path $folder | Export-Clixml -Path "$env:USERPROFILE\Desktop\TestPerms_ACL_backup.xml"

# 2) Ajouter le droit Lecture/Exécution
$acl = Get-Acl -Path $folder
$rule = New-Object System.Security.AccessControl.FileSystemAccessRule($user,"ReadAndExecute","ContainerInherit, ObjectInherit","None","Allow")
$acl.AddAccessRule($rule)
Set-Acl -Path $folder -AclObject $acl

# 3) Vérifier
Get-Acl -Path $folder | Format-List -Property Access
```