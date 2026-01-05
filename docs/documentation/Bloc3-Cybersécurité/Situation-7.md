# Situation 7 : Configuration d’un SIEM 

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


## 1. Définir la notion de SIEM et son rôle dans un SOC
Un **SIEM (Security Information and Event Management)** est une solution de sécurité qui permet de **centraliser, collecter, corréler et analyser les événements de sécurité** provenant de différentes sources du système d’information (serveurs, postes de travail, équipements réseau, applications, etc.).

Dans un **SOC (Security Operations Center)**, le SIEM joue un rôle central :
* il regroupe les journaux de sécurité de l’ensemble de l’infrastructure,
* il détecte les anomalies, attaques ou comportements suspects grâce à des règles et des corrélations d’événements,
* il génère des alertes contextualisées pour faciliter l’analyse,
* il aide au respect des exigences réglementaires (PCI-DSS, GDPR, NIST, etc.).

Avec Wazuh, le SIEM permet aussi la détection des vulnérabilités, l’évaluation de la configuration de sécurité (benchmarks CIS) et la surveillance continue des systèmes, ce qui réduit le temps de réponse aux incidents

## 2. Différence entre un EDR et un XDR
Un EDR (Endpoint Detection and Response) est une solution de sécurité qui se concentre exclusivement sur les points d’extrémité tels que les ordinateurs et les serveurs. Son objectif est de détecter les menaces avancées sur ces équipements, d’analyser les comportements suspects et de permettre une réponse rapide aux incidents. 

Un XDR (eXtended Detection and Response) est une évolution de l’EDR qui élargit la détection et la réponse à l’ensemble du système d’information. Il ne se limite pas aux endpoints mais prend également en compte les équipements réseau, les services cloud, les applications et des sources externes de renseignement sur les menaces. Le XDR permet ainsi de corréler les événements provenant de différentes couches de l’infrastructure afin d’identifier plus efficacement les attaques complexes et coordonnées.

## 3. Rôle des composants de la solution SIEM/XDR Wazuh
La solution Wazuh repose sur plusieurs composants qui fonctionnent ensemble pour assurer la surveillance de la sécurité. Les agents Wazuh sont installés sur les postes de travail et les serveurs et ont pour rôle de collecter les journaux, de surveiller l’intégrité des fichiers, de détecter les vulnérabilités et de transmettre ces informations au serveur central. Le serveur Wazuh analyse les données reçues à l’aide de règles et de décodeurs afin d’identifier des indicateurs de compromission connus et de générer des alertes de sécurité. Il permet également l’administration et la gestion des agents à distance. L’indexeur Wazuh stocke et indexe l’ensemble des événements et des alertes afin de permettre des recherches rapides et une analyse historique. Enfin, le tableau de bord Wazuh constitue l’interface web utilisée par les analystes du SOC pour visualiser les alertes, suivre l’état de la sécurité, analyser les vulnérabilités et vérifier la conformité aux normes.
