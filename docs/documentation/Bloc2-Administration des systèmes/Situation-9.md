# Scripting PowerShell — Gestion du service AD

![](../../media/logo-cub.png)

*25/03/2026*

## PARTIE 1 — Analyse sans IA

### Indiquer les paramètres d'entrée et les fonctions présentes

**Paramètres d'entrée :**

- `$InactiveDays`
- `$SearchBase`
- `$DisableAccounts`

**Fonctions :**

- `Write-Info`
- `Get-InactiveUsers`
- `Disable-InactiveUsers`

### Schématiser l'ordre d'exécution du script

1. Déclaration des paramètres
2. Import du module Active Directory
3. Définition des fonctions
4. Exécution de `Get-InactiveUsers`
5. Affichage du nombre de comptes trouvés
6. Si `-DisableAccounts` est activé → affichage de la liste + confirmation → désactivation + déplacement des comptes
7. Message de fin

### Résumer son rôle général

Ce script permet de **détecter les comptes utilisateurs inactifs** dans Active Directory, d'afficher un rapport simple (nombre de comptes) et de **désactiver et déplacer** ces comptes si demandé.

**Objectif : renforcer la sécurité du SI en évitant les comptes inutilisés.**

## PARTIE 2 — Utilisation libre de l'IA

### Commenter chaque ligne du script et expliquer son fonctionnement détaillé

**Résumé du fonctionnement du script :**

**Objectif principal** : Audit des comptes utilisateurs inactifs dans Active Directory avec possibilité de les désactiver et de les déplacer dans une OU dédiée.

**Fonctionnalités :**

- Recherche des comptes utilisateurs inactifs depuis X jours
- Affichage des résultats avec horodatage
- Confirmation obligatoire avant désactivation
- Déplacement automatique des comptes désactivés vers `OU=Comptes_Désactivés`

Script `ADAudit-Esteban.ps1` (voir ci-dessous, commenté intégralement) :

```powershell
# Déclaration des paramètres du script
param (
    # Paramètre obligatoire qui définit le nombre de jours d'inactivité pour considérer un compte comme inactif
    [Parameter(Mandatory=$true)]
    [int]$InactiveDays,

    # Paramètre optionnel qui définit la base de recherche dans l'Active Directory (domaine entier)
    [string]$SearchBase = "DC=local,DC=frankfurt,DC=cub,DC=sioplc,DC=fr",

    # Paramètre optionnel qui définit l'OU de destination pour les comptes désactivés
    [string]$TargetOU = "OU=Comptes_Désactivés,DC=local,DC=frankfurt,DC=cub,DC=sioplc,DC=fr",

    # Paramètre de type switch (booléen) qui, s'il est présent, désactivera les comptes inactifs trouvés
    [switch]$DisableAccounts
)

# Importation du module Active Directory nécessaire pour utiliser les commandes de gestion AD
Import-Module ActiveDirectory

# Déclaration de la fonction Write-Info pour afficher des messages avec horodatage
function Write-Info {
    # Déclaration du paramètre de la fonction : le message à afficher
    param ([string]$Message)
    # Création d'un horodatage au format AAAA-MM-JJ HH:MM:SS
    $timestamp = Get-Date -Format "yyyy-MM-dd HH:mm:ss"
    # Affichage du message préfixé avec l'horodatage
    Write-Host "$timestamp - $Message"
}

# Déclaration de la fonction Get-InactiveUsers pour rechercher les utilisateurs inactifs
function Get-InactiveUsers {

    # Calcul de la date limite en soustrayant le nombre de jours d'inactivité de la date actuelle
    $limitDate = (Get-Date).AddDays(-$InactiveDays)

    # Affichage d'un message informatif indiquant le critère de recherche
    Write-Info "Recherche des comptes inactifs depuis $InactiveDays jours"

    # Recherche des utilisateurs Active Directory qui répondent aux critères :
    # - Comptes activés (Enabled -eq $true)
    # - Dernière connexion antérieure à la date limite (LastLogonDate -lt $limitDate)
    # - Limités à l'OU spécifiée dans $SearchBase
    # - Récupération de la propriété LastLogonDate pour chaque utilisateur
    $users = Get-ADUser -Filter {Enabled -eq $true -and LastLogonDate -lt $limitDate} `
                        -SearchBase $SearchBase `
                        -Properties LastLogonDate

    # Retourne la collection des utilisateurs inactifs trouvés
    return $users
}

# Déclaration de la fonction Disable-InactiveUsers pour désactiver et déplacer les comptes inactifs
function Disable-InactiveUsers {
    # Déclaration du paramètre : collection d'utilisateurs à désactiver
    param ($Users)

    # Vérification si l'OU de destination existe
    try {
        $ouExists = Get-ADOrganizationalUnit -Filter "DistinguishedName -eq '$TargetOU'" -ErrorAction Stop
        if (-not $ouExists) {
            Write-Info "ERREUR : L'OU de destination '$TargetOU' n'existe pas."
            return
        }
    } catch {
        Write-Info "ERREUR : Impossible de vérifier l'OU de destination '$TargetOU'."
        return
    }

    # Boucle foreach qui parcourt chaque utilisateur dans la collection fournie
    foreach ($user in $Users) {
        try {
            # Désactivation du compte utilisateur en utilisant son identifiant SAM (nom de connexion)
            Disable-ADAccount -Identity $user.SamAccountName -ErrorAction Stop
            
            # Déplacement du compte vers l'OU des comptes désactivés
            Move-ADObject -Identity $user.DistinguishedName -TargetPath $TargetOU -ErrorAction Stop
            
            # Affichage d'un message confirmant la désactivation et le déplacement du compte
            Write-Info "Compte désactivé et déplacé : $($user.SamAccountName) → $TargetOU"
        } catch {
            Write-Info "ERREUR lors du traitement du compte $($user.SamAccountName) : $($_.Exception.Message)"
        }
    }
}

# Appel de la fonction Get-InactiveUsers pour récupérer la liste des comptes inactifs
$inactiveUsers = Get-InactiveUsers

# Affichage du nombre total de comptes inactifs trouvés
Write-Host "Nombre de comptes inactifs trouvés : $($inactiveUsers.Count)"

# Vérification si le paramètre $DisableAccounts a été spécifié (switch activé)
if ($DisableAccounts) {
    # Vérification si des comptes inactifs ont été trouvés
    if ($inactiveUsers.Count -gt 0) {
        # Affichage de la liste des comptes qui vont être désactivés
        Write-Host "Comptes qui vont être désactivés :"
        foreach ($user in $inactiveUsers) {
            Write-Host "  - $($user.SamAccountName) (Dernière connexion : $($user.LastLogonDate))"
        }
        
        # Demande de confirmation à l'administrateur
        $confirmation = Read-Host "Voulez-vous vraiment désactiver ces $($inactiveUsers.Count) comptes ? (O/N)"
        
        # Vérification de la réponse de l'utilisateur
        if ($confirmation -eq "O" -or $confirmation -eq "o") {
            # Si oui, appel de la fonction pour désactiver tous les comptes inactifs trouvés
            Disable-InactiveUsers -Users $inactiveUsers
        } else {
            # Annulation de l'opération
            Write-Info "Opération de désactivation annulée par l'utilisateur."
        }
    } else {
        Write-Info "Aucun compte inactif à désactiver."
    }
}

# Affichage d'un message indiquant la fin du script
Write-Info "Fin du script."
```

### Énumérer au moins une critique du script actuel

**Pas de confirmation :** La désactivation des comptes se fait sans demander confirmation à l'administrateur, ce qui peut entraîner une désactivation accidentelle de comptes importants.

### Proposer une réponse à la critique énoncée précédemment

**Nouvelles fonctionnalités de sécurité ajoutées :**

1. **Affichage détaillé** : liste tous les comptes qui seront désactivés avec leur dernière date de connexion
2. **Confirmation obligatoire** : demande explicitement à l'administrateur de confirmer avec `O` ou `N`
3. **Gestion du cas vide** : affiche un message si aucun compte inactif n'est trouvé
4. **Annulation propre** : permet d'annuler l'opération sans interrompre le script

Flux d'exécution avec confirmation :

```
Recherche → Affichage des résultats → [Si -DisableAccounts] →
Affichage liste → Confirmation → [Oui] Désactivation + Déplacement / [Non] Annulation
```

L'administrateur voit maintenant exactement quels comptes seront affectés avant de prendre une décision, ce qui élimine le risque de désactivation accidentelle de comptes importants.

## PARTIE 3 — Demande d'évolution

Nouvelle exigence de la DSI : tout compte utilisateur désactivé devra désormais être automatiquement **déplacé** dans l'OU `OU=Comptes_Désactivés,DC=cub,DC=local` afin d'assurer une meilleure organisation et traçabilité dans l'Active Directory.

Cette évolution est intégrée dans le script via le paramètre `$TargetOU` et la commande `Move-ADObject` dans la fonction `Disable-InactiveUsers` (voir script commenté ci-dessus).
