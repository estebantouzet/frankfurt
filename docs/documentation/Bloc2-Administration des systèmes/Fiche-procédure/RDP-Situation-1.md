# Fiche procédure protocole RDP

## Installation du bureau à distance – Protocole RDP

### Activer le Bureau à distance sous Windows Server :

Dans le **`Gestionnaire de serveur > Serveur local`**, le paramètre Bureau à distance permet d’activer RDP. Par défaut désactivé, il empêche les connexions entrantes, mais le serveur peut toujours se connecter en RDP à une autre machine.

![](../../../media/bloc2/AdminSys/Situation1-12.1.png)

En cliquant sur *Activé* ou *Désactivé*, on ouvre l’interface de configuration du Bureau à distance. Pour autoriser les connexions, il faut cocher **Autoriser les connexions à distance à cet ordinateur**.

![](../../../media/bloc2/AdminSys/Situation1-13.png)

**Activer le Bureau à distance sous Windows Server :**
`Paramètres → Système → Bureau à distance →` **`Activer`**. 

Ouvrir le bureau à distance :

![](../../../media/bloc2/AdminSys/Situation1-14.png)

**Test de la connexion RDP pour le compte “adminssh” :**
Le type de compte *adminssh* a été modifié, car par défaut il est en *utilisateur standard*. Pour permettre l’accès en RDP, il doit être configuré en **compte administrateur**.

![](../../../media/bloc2/AdminSys/Situation1-15.png)

![](../../../media/bloc2/AdminSys/Situation1-16.png)

Entrer votre identifiant et mot de passe : 

![](../../../media/bloc2/AdminSys/Situation1-17.png)