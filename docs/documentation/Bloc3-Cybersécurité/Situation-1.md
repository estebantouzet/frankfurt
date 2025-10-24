# Situation 1 : phase d’analyse préalable

![](../../media/logo-cub.png)

## Prérequis

![](../../media/schema-logique-cub.png)

*Ducumentation en ligne : [https://cubdocumentation.sioplc.fr](https://cubdocumentation.sioplc.fr)*
<br>

| **Service**                           | **Nombre d’hôtes** | **Adresse réseau** | **Masque de sous-réseau** | **Adresse de diffusion** | **Description VLAN** |
|--------------------------------------|--------------------|--------------------|----------------------------|--------------------------|----------------------|
| Production                           | 120                | 192.168.6.0        | 255.255.255.128            | 192.168.6.127            | VLAN 56              |
| Client 1                             | 32                 | 192.168.6.128      | 255.255.255.192            | 192.168.6.191            | VLAN 10              |
| Administration systèmes et réseaux   | 6                  | 192.168.6.192      | 255.255.255.240            | 192.168.6.207            | VLAN 20              |


## Packet tracert - Agence Frankfurt
<br>

![](../../media/packet-tracert-v1.jpg)
<br>

<div style="text-align:center; margin-top:20px;">
  <a href="https://drive.google.com/file/d/1L7Gp52YpPjjRhFdp9gp4L1sGORqAoCEK/view?usp=share_link" 
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

## 1. Pourquoi le RSSI a opté pour une solution UTM plutôt qu’un simple pare-feu stateful ?

Un pare-feu **stateful** se limite principalement aux couches 3 et 4 du modèle OSI. Il inspecte les adresses IP, les ports et l’état des connexions, mais ne va pas plus loin. En revanche, une solution **UTM (Unified Threat Management)** agit jusqu’à la couche 7 (applicative) et intègre plusieurs briques de sécurité en une seule appliance : antivirus, antispam, IDS/IPS, filtrage web et prévention des fuites de données

Cela permet de mieux contrer les menaces modernes (malwares, attaques ciblées, ransomwares), avec une administration centralisée et plus simple pour le RSSI.


## 2. Donner deux arguments en faveur d’un boîtier UTM Stormshield, par rapport à ceux proposés par des entreprises concurrents telles que Palo Alto ou checkPoint.

**Souveraineté numérique :** Stormshield est une entreprise française (filiale d’Airbus), soumise au droit français et européen (RGPD, loi informatique et libertés). Contrairement à certains concurrents américains, ses solutions ne sont pas soumises à l’extraterritorialité du droit US 

**Certification ANSSI :** Le pare-feu Stormshield SNS est certifié EAL4+ et qualifié ANSSI niveau standard. Cela garantit un haut niveau de sécurité validé par un audit officiel, un critère de confiance important pour les OIV et les entreprises sensibles
