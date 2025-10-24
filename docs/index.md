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
  <em>Esteban TOUZET</em><br>
</div>





---


# 🌍 Agence Frankfurt

<div class="hero" style="background: linear-gradient(90deg, #003366, #0055aa); color: white; padding: 2em 1.5em; border-radius: 14px; text-align: center; margin-bottom: 2em;">
  <h1 style="margin: 0; font-size: 2.2em;">Projet CUB – Infrastructure réseau</h1>
  <p style="font-size: 1.1em; margin-top: 0.6em;">Documentation officielle de l’agence <strong>Frankfurt</strong><br>
  <em>BTS SIO option SISR – Bloc 2 et 3</em></p>
  <a href="https://estebantouzet.github.io/frankfurt/" target="_blank" style="background: white; color: #003366; padding: 0.5em 1em; border-radius: 6px; text-decoration: none; font-weight: bold;">🌐 Consulter le site</a>
</div>

## 🏢 Présentation du projet

**CUB** est une entreprise fondée en 2010, spécialisée dans l’**incubation de startups** autour de valeurs fortes de **solidarité** et de **développement durable**.  
Elle met à disposition des professionnels des espaces collaboratifs : **salles de réunion**, **bureaux partagés**, **séminaires** et **formations** accessibles via une plateforme web sécurisée.

L’entreprise dispose d’un **réseau international** d’agences situées à Paris (siège), Anvers, Barcelone, Hong-Kong, Los Angeles, Corse… et **Frankfurt**.  
Chaque agence bénéficie d’une **adresse IPv4 publique dédiée** issue du préfixe `192.36.0.0/16`, attribué par le **RIPE NCC**, et gère son propre pare-feu local.  
CUB est reconnue comme un **LIR (Local Internet Registry)** et administre le domaine :  
➡️ `cub.sioplc.fr`

---

<div class="admonition tip">
  <p class="admonition-title">🎯 Objectif de l’agence Frankfurt</p>
  <p>Déployer, sécuriser et documenter l’infrastructure réseau locale de l’agence selon la stratégie définie par la DSI de CUB, en suivant les recommandations de l’<strong>ANSSI</strong>.</p>
</div>

### Objectifs techniques
- Concevoir une **segmentation réseau** (LAN, DMZ, VLAN d’administration).  
- Établir un **plan d’adressage IPv4** clair et extensible.  
- Configurer les **commutateurs, pare-feux et serveurs locaux**.  
- Garantir la **sécurité et la haute disponibilité** du réseau.  
- Fournir une **documentation technique complète** sur GitHub Pages.

---

## 🧩 Architecture et organisation

Le **siège de Paris** centralise la politique informatique de CUB et assure la cohérence entre toutes les agences.  
Chaque **Service Informatique de Proximité (SIP)** – dont celui de Frankfurt – gère :

- L’**assistance aux utilisateurs** sur site,  
- La **maintenance de l’infrastructure réseau**,  
- La **supervision des serveurs**,  
- La **mise à jour de la documentation technique**.

---

## 🖥️ Informations réseau – Agence Frankfurt

| Élément | Adresse / VLAN | Description |
|----------|----------------|-------------|
| **LAN Frankfurt** | `192.168.6.0/24` | Réseau interne |
| **DMZ Frankfurt** | `192.36.6.0/24` | Services accessibles depuis l’extérieur |
| **VLAN Administration** | `192.168.66.192/28` | Gestion et supervision |
| **Pare-feu (FW6)** | `192.168.66.254 / 192.36.6.254 / 192.36.253.60` | Sécurité et routage |
| **WAN** | `192.36.253.0/24` | Interconnexion inter-agences |

---

## 🧭 Structure de la documentation

<div class="grid cards" markdown>
- **Phase d’analyse et de maquettage**  
  Étude du réseau actuel, définition des besoins, conception logique et physique.
- **Phase de prototypage et mise en œuvre**  
  Configuration des équipements et déploiement de la solution.
- **Phase de validation (recette)**  
  Tests de conformité, sécurité et performance.
- **Documentation finale**  
  Procédures, schémas, et configurations validées.
</div>

---

<div style="text-align: center; margin-top: 2.5em;">
  <p><em>Réalisé par l’agence <strong>Frankfurt</strong> – BTS SIO SISR</em></p>
  <a href="https://cubdocumentation.sioplc.fr" target="_blank" style="color:#0055aa; text-decoration:none;">🔗 Documentation CUB officielle</a>
</div>
