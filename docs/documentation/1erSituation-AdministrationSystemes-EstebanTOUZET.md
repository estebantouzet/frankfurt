# Préparation de la maquette et premiers paramétrages du serveur Windows2019

![](media/image1.png){width="3.123611111111111in"
height="1.3284722222222223in"}

**Situation 1 -- Administration des systèmes**

*[03/09/2025]{.underline}*

# Table des matières {#table-des-matières .TOC-Heading}

[Préparation de la maquette et premiers paramétrages du serveur
Windows2019
\[1\](#préparation-de-la-maquette-et-premiers-paramétrages-du-serveur-windows2019)](#préparation-de-la-maquette-et-premiers-paramétrages-du-serveur-windows2019)

[Prérequis : \[3\](#prérequis)](#prérequis)

[Sysprep : \[3\](#sysprep)](#sysprep)

[Comparaison SID \[5\](#comparaison-sid)](#comparaison-sid)

[Changement d'adresse \[5\](#changement-dadresse)](#changement-dadresse)

[Accès poste client \[6\](#accès-poste-client)](#accès-poste-client)

[Installer OpenSSH Server sur Windows
\[7\](#installer-openssh-server-sur-windows)](#installer-openssh-server-sur-windows)

[II. Configurer OpenSSH Server sur Windows
\[7\](#ii.-configurer-openssh-server-sur-windows)](#ii.-configurer-openssh-server-sur-windows)

[A. Démarrage automatique du serveur OpenSSH
\[7\](#démarrage-automatique-du-serveur-openssh)](#démarrage-automatique-du-serveur-openssh)

[B. Configuration d\'OpenSSH Server sur Windows
\[8\](#configuration-dopenssh-server-sur-windows)](#configuration-dopenssh-server-sur-windows)

[Connexion en ssh : \[10\](#connexion-en-ssh)](#connexion-en-ssh)

[Installation du bureau à distance -- Protocole RDP
\[10\](#installation-du-bureau-à-distance-protocole-rdp)](#installation-du-bureau-à-distance-protocole-rdp)

[Activer le Bureau à distance sous Windows Server :
\[10\](#\_Toc209598017)](#_Toc209598017)

## ![](media/image2.png){width="7.527777777777778in" height="4.022916666666666in"}Prérequis :

Source :
https://www.it-connect.fr/installer-et-configurer-openssh-server-sur-windows-server-2019/

## Sysprep :

Sysprep est l'**outil de préparation système de Windows**. Il permet de
préparer une machine qui va servir de « master », en vue d'un futur
déploiement.

Cet utilitaire se trouve dans **C:\\Windows\\System32\\Sysprep.**

![](media/image3.png){width="5.135294181977253in"
height="3.423529090113736in"}

Double cliquez sur « sysprep »

![](media/image4.png){width="3.308170384951881in"
height="2.4521489501312335in"}

Quand tout de passe bien, il vous affiche cette petite fenêtre qui reste
ouverte plusieurs minutes :

![](media/image5.png){width="3.2470581802274716in"
height="1.7036493875765528in"}

## Comparaison SID

Ouvrez l\'invite de commande en tant qu\'administrateur.

Tapez « whoami /user » pour voir votre **SID** d\'utilisateur actuel.

![](media/image6.png){width="5.535416666666666in"
height="0.8666666666666667in"}![](media/image7.png){width="4.863495188101488in"
height="0.3015102799650044in"}

**[Mon SID :]{.underline}**
S-1-5-21-1430976166-1236960807-3649249825-500

**[SID de mon camarade]{.underline}** :
5-1-5-21-324986168-2068038747-1270508929-500

## Changement d'adresse

![](media/image8.png){width="2.8805555555555555in"
height="3.8875in"}Afin d'être conforme aux règles en vigueur sur les
adressages, je change mon adresse IP, mon masque ainsi que ma
passerelle.

![](media/image9.png){width="5.0368055555555555in"
height="2.213888888888889in"}Commande : « ipconfig »

### Accès poste client

**[Autorisation des postes clients : autoriser les requetes
icmp]{.underline}**

1.  Dans le volet gauche, clique sur **Règles de trafic entrant**.

2.  Dans le volet droit, clique sur **Nouvelle règle...**.

3.  Choisis **Personnalisée** puis **Suivant**.

4.  Dans **Programme**, laisse \"Tous les programmes\" → **Suivant**.

5.  Dans **Protocole et ports** :

-   Dans **Type de protocole**, sélectionne **ICMPv4**

-   Il est également possible aussi de choisir **ICMPv6** si nécessaire.

6.  Clique sur **Suivant** jusqu'à **Action** → choisis **Autoriser la
    connexion**.

7.  Sélectionne les profils (Domaine, Privé, Public) selon le besoin.\
    Généralement **Privé + Domaine** suffisent.

8.  Donne un nom à la règle (ex : *Autoriser ICMP Ping*) → **Terminer**.

## Installer OpenSSH Server sur Windows

A partir d\'une console PowerShell ouverte en tant qu\'administrateur,
la commande suivante permet l\'installation :

Commande : Add-WindowsCapability -Online -Name
OpenSSH.Server\~\~\~\~0.0.1.0

![](media/image10.png){width="4.425036089238845in"
height="0.7376432633420822in"}

## II. Configurer OpenSSH Server sur Windows

### Démarrage automatique du serveur OpenSSH

Pour commencer la configuration, nous allons démarrer le serveur OpenSSH
d\'une part, et d\'autre part nous allons le configurer en démarrage
automatique.

Plutôt que le faire à partir de la console \"Services\", je vous propose
d\'exécuter deux commandes PowerShell, ce sera plus efficace. Voici
l\'état actuel du service \"OpenSSH SSH Server\" :

![](media/image11.jpeg){width="6.0in" height="1.0743055555555556in"}

Pour démarrer le service \"sshd\" correspondant à \"OpenSSH SSH
Server\", on va utiliser la commande suivante :

Start-Service -Name \"sshd\"

Ensuite, pour le modifier et définir le mode de démarrage sur
\"Automatique\" au lieu de \"Manuel\" :

Set-Service -Name \"sshd\" -StartupType Automatic

La commande ci-dessous vous permettra de vérifier qu\'il est bien en
cours d\'exécution.

Get-Service -Name \"sshd\"

![OpenSSH Server sur Windows](media/image12.png){width="6.0in"
height="1.6701388888888888in"}

### Configuration d\'OpenSSH Server sur Windows

Sur Windows, la configuration d\'OpenSSH est stockée à l\'emplacement
ci-dessous où l\'on va trouver le fichier sshd_config :

%programdata%\\ssh\\

Au sein du fichier sshd_config, nous allons retrouver les options
classiques d\'OpenSSH. Nous pouvons le configurer de la même façon
qu\'on le ferai la [configuration SSH sous
Linux](https://www.it-connect.fr/chapitres/openssh-configuration-du-serveur-ssh/).

Attention tout de même : à ce jour, il y a des options non supportées
comme les options X11. La liste complète des options non supportées est
disponible sur le site Microsoft : [OpenSSH - Options non
supportées](https://docs.microsoft.com/fr-fr/windows-server/administration/openssh/openssh_server_configuration#not-supported)

-   **Modifier le port d\'écoute SSH**

Pour modifier le port d\'écoute par défaut (recommandé) et utiliser un
autre port que le n°22, il faut modifier l\'option \"Port\". **Pour
modifier le fichier de configuration, vous devez ouvrir l\'éditeur de
texte en tant qu\'administrateur pour avoir le droit d\'enregistrer le
fichier**.

Ensuite, il faut décommenter la ligne \"*#Port 22*\" et changer le
numéro de port, comme ceci :

![OpenSSH Server sur Windows -
Configuration](media/image13.jpeg){width="6.0in"
height="1.8444444444444446in"}

**Après chaque modification de la config, il est indispensable de
redémarrer le service SSH** pour charger les nouveaux paramètres.
PowerShell permet de le faire facilement :

Restart-Service \"sshd\"

A la suite de la modification du port, si la connexion SSH ne fonctionne
pas, regardez du côté du pare-feu Windows. En effet, lors de
l\'installation d\'OpenSSH Server, une règle est créée pour autoriser
les connexions sur le port 22.

Voici la commande pour créer une règle de pare-feu qui autorise les
connexions entrantes sur le port 222 :

New-NetFirewallRule -Name sshd -DisplayName \'OpenSSH Server (sshd) -
Port 222\' -Enabled True -Direction Inbound -Protocol TCP -Action Allow
-LocalPort 222

(Auparavant, j'ai créé un utilisateur « adminssh » afin d'utiliser celui
a la connexion SSH.)

Dans le fichier de configuration, il faut commenter les deux lignes
suivantes, sinon la directive DenyGroups ne fonctionne pas :

\# Match Group administrators

\# AuthorizedKeysFile
\_\_PROGRAMDATA\_\_/ssh/administrators_authorized_keys

A la suite, ajoutez la ligne suivante :

DenyGroups administrateurs

Redémarrez le service SSH et tentez de vous connecter à votre serveur
avec un compte administrateur :** l\'accès doit être refusé !**

### Connexion en ssh :

Commande : ssh adminssh@172.16.56.2 -p -222

![](media/image14.png){width="6.916666666666667in"
height="0.5472222222222223in"}

## Installation du bureau à distance -- Protocole RDP

\[\]{#\_Toc209598017 .anchor}**[Activer le Bureau à distance sous
Windows Server :]{.underline}**\
Dans le **Gestionnaire de serveur \> Serveur local**, le
paramètre *Bureau à distance* permet d'activer RDP. Par défaut
désactivé, il empêche les connexions entrantes, mais le serveur peut
toujours se connecter en RDP à une autre machine.

![](media/image15.png){width="6.0in" height="2.4618055555555554in"}

En cliquant sur *Activé* ou *Désactivé*, on ouvre l'interface de
configuration du Bureau à distance. Pour autoriser les connexions, il
faut cocher **Autoriser les connexions à distance à cet ordinateur**.

![](media/image16.png){width="2.317361111111111in"
height="2.776388888888889in"}

**Activer le Bureau à distance sous Windows Server :**\
Paramètres → Système → Bureau à distance → **Activer**. 

Ouvrir le bureau à distance :

![](media/image17.png){width="4.1841797900262465in"
height="2.6210247156605426in"}

**Test de la connexion RDP pour le compte "adminssh" :**\
Le type de compte *adminssh* a été modifié, car par défaut il est
en *utilisateur standard*. Pour permettre l'accès en RDP, il doit être
configuré en **compte administrateur**.

![](media/image18.png){width="5.576388888888889in" height="3.26875in"}

![](media/image17.png){width="4.839058398950131in"
height="4.213182414698163in"}

Entrer votre identifiant et mot de passe :

![](media/image19.png){width="6.0in" height="4.554166666666666in"}
