# Scripting PowerShell — Gestion de fichiers

![](../../media/logo-cub.png)

*11/03/2026*

## PARTIE 1 — Analyse sans IA

### Indiquer les paramètres d'entrée et les fonctions présentes

**Paramètres d'entrée :**

- `[string]$SourcePath`
- `[string]$ArchivePath`
- `[int]$DaysOld`
- `[string]$LogFile`

**Fonctions :**

- `function Write-Log`
- `function Ensure-Directory`
- `function Archive-OldFiles`
- `function Remove-EmptyFolders`

### Schématiser l'ordre d'exécution du script

1. Définition des paramètres du script
2. Fonction permettant d'écrire un message dans le fichier de log
3. Fonction qui vérifie si un dossier existe et le crée si nécessaire
4. Fonction qui archive les fichiers trop anciens
5. Fonction qui supprime les dossiers vides dans le dossier source

### Résumer son rôle général

Le rôle général : **archiver les fichiers anciens** et **nettoyer les dossiers vides** du dossier source, en journalisant toutes les actions dans un fichier de log.

## PARTIE 2 — Utilisation libre de l'IA

### Commenter chaque ligne du script et expliquer son fonctionnement détaillé

Script `FileMaintenance-Esteban.ps1` (voir ci-dessous, commenté intégralement) :

```powershell
# Définition des paramètres du script
param (
    # Chemin du dossier source contenant les fichiers à analyser
    [Parameter(Mandatory=$true)]
    [string]$SourcePath,

    # Chemin du dossier où seront archivés les fichiers anciens
    [Parameter(Mandatory=$true)]
    [string]$ArchivePath,

    # Nombre de jours avant qu'un fichier soit considéré comme ancien
    # Valeur par défaut : 30 jours
    [int]$DaysOld = 30,

    # Chemin du fichier de log pour enregistrer les actions du script
    [string]$LogFile = "C:\Logs\FileMaintenance.log"
)

# Force les erreurs à être traitées par try/catch
$ErrorActionPreference = "Stop"
Set-StrictMode -Version Latest

# Fonction permettant d'écrire un message dans le fichier de log
function Write-Log {
    param (
        # Message à écrire dans le log
        [string]$Message,

        # Niveau du message (INFO, ERROR, WARNING, etc.)
        [string]$Level = "INFO"
    )

    try {
        # Génère un timestamp (date + heure)
        $timestamp = Get-Date -Format "yyyy-MM-dd HH:mm:ss"

        # Construit la ligne de log
        $entry = "$timestamp [$Level] $Message"

        # Ajoute la ligne dans le fichier de log
        Add-Content -Path $LogFile -Value $entry -ErrorAction Stop
    }
    catch {
        # Évite les silences : on affiche au moins l'erreur
        Write-Error "Impossible d'écrire dans le log '$LogFile' : $($_.Exception.Message)"
    }
}

# Fonction qui vérifie si un dossier existe et le crée si nécessaire
function Ensure-Directory {
    param ([string]$Path)

    try {
        # Si le dossier n'existe pas
        if (!(Test-Path $Path)) {

            # Création du dossier
            New-Item -ItemType Directory -Path $Path -Force | Out-Null

            # Écriture dans le log
            Write-Log "Création dossier : $Path"
        }
    }
    catch {
        Write-Log "Erreur création dossier '$Path' : $($_.Exception.Message)" "ERROR"
        Write-Error "Erreur création dossier '$Path' : $($_.Exception.Message)"
        throw
    }
}

# Fonction qui archive les fichiers trop anciens
function Archive-OldFiles {

    # Détermine la date limite (aujourd'hui - nombre de jours définis)
    $limitDate = (Get-Date).AddDays(-$DaysOld)

    # Récupère tous les fichiers du dossier source (et sous-dossiers)
    # dont la date de dernière modification est plus ancienne que la limite
    $files = Get-ChildItem -Path $SourcePath -File -Recurse |
             Where-Object { $_.LastWriteTime -lt $limitDate }

    # Parcourt chaque fichier trouvé
    foreach ($file in $files) {
        try {
            # Reconstitue le chemin relatif du fichier par rapport au dossier source
            $relativePath = $file.FullName.Substring($SourcePath.Length)

            # Construit le chemin de destination dans le dossier d'archive
            $destinationFile = Join-Path $ArchivePath $relativePath

            # Récupère le dossier de destination
            $destinationDir = Split-Path $destinationFile

            # Vérifie que le dossier de destination existe
            Ensure-Directory $destinationDir

            # Déplace le fichier vers l'archive
            Move-Item -Path $file.FullName -Destination $destinationFile -Force

            # Enregistre l'action dans le log
            Write-Log "Fichier archivé : $($file.FullName)"
        }
        catch {
            Write-Log "Erreur archivage '$($file.FullName)' : $($_.Exception.Message)" "ERROR"
            Write-Error "Erreur archivage '$($file.FullName)' : $($_.Exception.Message)"
            # On continue avec les autres fichiers
        }
    }
}

# Fonction qui supprime les dossiers vides dans le dossier source
function Remove-EmptyFolders {

    # Récupère tous les dossiers en partant du plus profond
    # (tri décroissant pour éviter les conflits de suppression)
    $directories = Get-ChildItem -Path $SourcePath -Directory -Recurse |
                   Sort-Object FullName -Descending

    # Parcourt chaque dossier
    foreach ($dir in $directories) {
        try {
            # Vérifie si le dossier est vide
            if ((Get-ChildItem $dir.FullName | Measure-Object).Count -eq 0) {

                # Supprime le dossier vide
                Remove-Item -Path $dir.FullName -Force

                # Enregistre l'action dans le log
                Write-Log "Dossier supprimé : $($dir.FullName)"
            }
        }
        catch {
            Write-Log "Erreur suppression dossier '$($dir.FullName)' : $($_.Exception.Message)" "ERROR"
            Write-Error "Erreur suppression dossier '$($dir.FullName)' : $($_.Exception.Message)"
            # On continue avec les autres dossiers
        }
    }
}

try {
    # Vérifie que le dossier contenant le fichier de log existe
    Ensure-Directory (Split-Path $LogFile)

    # Vérifie que le dossier d'archive existe
    Ensure-Directory $ArchivePath

    # Lance l'archivage des fichiers anciens
    Archive-OldFiles

    # Supprime les dossiers devenus vides
    Remove-EmptyFolders

    # Message final dans le fichier de log
    Write-Log "Traitement terminé."
}
catch {
    Write-Log "Erreur critique : $($_.Exception.Message)" "ERROR"
    Write-Error "Erreur critique : $($_.Exception.Message)"
    exit 1
}
```

### Énumérer au moins une critique du script actuel

En cas d'erreur, aucune notification n'est émise. Le script se termine brutalement ou poursuit son exécution sans fournir d'indication sur la nature de l'erreur rencontrée. Cette observation découle d'une discussion avec mon maître de stage.

### Proposer une réponse à la critique énoncée précédemment

Ce qui a été rajouté par l'IA pour la **gestion d'erreurs** :

- Toutes les erreurs deviennent détectables (`$ErrorActionPreference="Stop"`, `StrictMode`)
- Chaque étape critique est protégée avec `try/catch`
- Chaque erreur est loggée + affichée (`Write-Log` + `Write-Error`)
- Les erreurs locales n'arrêtent pas tout, sauf celles qui rendent le script inutilisable
- En cas d'échec critique, le script s'arrête proprement avec `exit 1`
