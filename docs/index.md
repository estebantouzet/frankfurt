 # Documentation de l'agence CUB Frankfurt

 Bienvenue sur la documentation CUB Hong-Kong produite en Markdown et avec Mkdocs...

 # 🌐 Agence Frankfurt – Documentation technique

<div style="background: linear-gradient(90deg, #004aad, #007bff); color: white; padding: 1.2em 1.5em; border-radius: 12px; margin-top: 1em;">
  <h2 style="margin-top: 0;">Projet CUB – Mise en place de l’infrastructure réseau</h2>
  <p>Documentation officielle de l’agence <strong>Frankfurt</strong>, réalisée dans le cadre du BTS SIO option SISR.</p>
</div>

## 🏢 Contexte du projet

Créée en **2010**, l’entreprise <strong>CUB</strong> est spécialisée dans l’incubation de startups partageant les valeurs de **solidarité** et de **développement durable**.  
Elle propose à ses clients des solutions d’hébergement et de collaboration à travers une plateforme web et un réseau international d’agences situées à **Paris (siège)**, **Anvers**, **Barcelone**, **Hong-Kong**, **Los Angeles**, **Corse**… et **Frankfurt**.

Chaque agence dispose de sa propre **adresse IPv4 publique** et d’un **pare-feu local**, reliés à un préfixe global attribué par le **RIPE NCC (192.36.0.0/16)**.  
CUB est ainsi considérée comme un **LIR (Local Internet Registry)** et gère son domaine principal :  
➡️ `cub.sioplc.fr`

## 💼 Objectif de l’agence Frankfurt

L’agence Frankfurt a pour mission de **concevoir, déployer et documenter** l’infrastructure réseau locale de son site, en respectant les standards définis par la DSI de CUB.  
Cette documentation présente l’ensemble des éléments techniques du projet, depuis la **phase d’analyse** jusqu’à la **mise en œuvre et la recette**.

### Les objectifs principaux :

- Mettre en place une **segmentation réseau** sécurisée (LAN, DMZ, VLAN d’administration, etc.).
- Définir un **plan d’adressage IPv4** propre à l’agence Frankfurt.
- Configurer et documenter les **commutateurs**, **pare-feux** et **serveurs locaux**.
- Appliquer les **recommandations de l’ANSSI** pour la sécurité du réseau interne.
- Produire une documentation claire et complète sur **GitHub Pages** à destination des administrateurs.

## 🌍 Architecture globale

Le **siège de Paris** centralise la gestion du système d’information et assure la cohérence technique entre toutes les agences.  
Chaque **SIP (Service Informatique de Proximité)**, dont celui de Frankfurt, est responsable de :

- l’**assistance aux utilisateurs locaux**,  
- la **maintenance de l’infrastructure réseau**,  
- la **supervision des serveurs** et services associés,  
- la **mise à jour de la documentation technique**.

## 🔧 Composition du site Frankfurt

| Élément | Adresse / VLAN | Description |
|----------|----------------|-------------|
| LAN Frankfurt | `192.168.6.0/24` | Réseau interne |
| DMZ Frankfurt | `192.36.6.0/24` | Services publics (Web, DNS, etc.) |
| VLAN Admin | `192.168.66.192/28` | Administration réseau |
| Pare-feu (FW6) | `192.168.66.254` / `192.36.6.254` / `192.36.253.60` | Sécurisation et routage |
| WAN | `192.36.253.0/24` | Lien inter-agences |

## 🧭 Organisation de la documentation

Cette documentation est structurée selon les grandes étapes du projet :

1. **Phase d’analyse et de maquettage** – Étude des besoins, segmentation et schémas réseau.  
2. **Phase de prototypage et de mise en œuvre** – Configuration matérielle et logicielle.  
3. **Phase de validation (recette)** – Tests de bon fonctionnement et conformité.  
4. **Documentation technique finale** – Guides, configurations, et procédures administratives.

---

<div style="text-align: center; margin-top: 2em;">
  <em>Projet réalisé par l’agence <strong>Frankfurt</strong> – BTS SIO SISR</em><br>
  <a href="https://estebantouzet.github.io/frankfurt/" target="_blank" style="color: #007bff; text-decoration: none;">🔗 Accéder au site de documentation</a>
</div>


