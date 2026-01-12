# IV - Redondance de l’active directory

![](../../../media/logo-cub.png)

## Prérequis

![](../../../media/schema-logique-cub.png)

*Ducumentation en ligne : [https://cubdocumentation.sioplc.fr](https://cubdocumentation.sioplc.fr)*
<br>

## Adressage 

| **Service**                           | **Nombre d’hôtes** | **Adresse réseau** | **Masque de sous-réseau** | **Adresse de diffusion** | **Description VLAN** |
|--------------------------------------|--------------------|--------------------|----------------------------|--------------------------|----------------------|
| Production                           | 120                | 192.168.6.0        | 255.255.255.128            | 192.168.6.127            | VLAN 56              |
| Client 1                             | 32                 | 192.168.6.128      | 255.255.255.192            | 192.168.6.191            | VLAN 10              |
| Administration systèmes et réseaux   | 6                  | 192.168.6.192      | 255.255.255.240            | 192.168.6.207            | VLAN 20              |

___

## Schéma logique – Agence Frankfur

![](../../../media/bloc2/ExploitationServ/Activite0-1.png)

___
## Packet tracert - Agence Frankfurt
<br>

![](../../../media/packet-tracert-v1.jpg)
<br>

<div style="text-align:center; margin-top:20px;">
  <a href="https://drive.google.com/file/d/1tqY2a5OSuL46RE_DEkwUXmWYONAYMvJb/view?usp=share_link" 
     style="display:inline-block;
            background:#e7e7e9;
            color:#0096FF;
            padding:11px 25px;
            border-radius:10px;
            text-decoration:none;
            font-weight:50;
            box-shadow:0 0 12px rgba(0,0,0,0.5);
            transition:all 0.3s ease;"
     onmouseover="this.style.background='#dcdce0'; this.style.color='#003d80';"
     onmouseout="this.style.background='#e7e7e9'; this.style.color='#0096FF';">
     🔗 Cliquer pour télécherger le paket tracert
  </a>
</div>
<br>

## Redondance de l’active directory

### Supprimer les fonctionnalités DNS et AD

![](../../../media/bloc2/AdminSys/Situation5-24.png)

### Pointer le serveur primaire dans l’onglet DNS du serveur secondaire

![](../../../media/bloc2/AdminSys/Situation5-25.png)
 
### Mettre le serveur secondaire dans le domaine “local.frankfurt.cub.sioplc.fr”

![](../../../media/bloc2/AdminSys/Situation5-26.png)

![](../../../media/bloc2/AdminSys/Situation5-34.png)


### Ajouter un contrôleur de domaine à un domaine existant

![](../../../media/bloc2/AdminSys/Situation5-27.png)

![](../../../media/bloc2/AdminSys/Situation5-28.png)

### Mot de passe du mode restauration : “Cub_007”

![](../../../media/bloc2/AdminSys/Situation5-29.png)
 
### Répliquer depuis : “Serveur Primaire”

![](../../../media/bloc2/AdminSys/Situation5-30.png)

![](../../../media/bloc2/AdminSys/Situation5-31.png)

**Installation en cours …**
