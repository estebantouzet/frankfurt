# Introduction 

Bienvenue sur la documentation CUB Frankfurt produite en Markdown et avec Mkdocs.


# 🌐 Agence Frankfurt

<div class="hero" style="background: linear-gradient(90deg, #004aad, #007bff); color: white; padding: 2em 1.5em; border-radius: 14px; text-align: center; margin-bottom: 2em;">
  <h1 style="margin: 0; font-size: 2.2em;">Projet CUB – Infrastructure réseau</h1>
  <p style="font-size: 1.1em; margin-top: 0.6em;">Documentation officielle de l’agence <strong>Frankfurt</strong><br>
  <em>BTS SIO option SISR – Bloc 2 et 3</em></p>
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

![](media/schema-logique-cub.png)

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


## 🗂️ Description des services – Agence Frankfurt

| Intitulé des services              | Description                                  | VLAN       | Nombre d’hôtes par service |
|-----------------------------------|----------------------------------------------|-------------|-----------------------------|
| **Production**                    | Réseau principal dédié aux services métiers. | VLAN 5X     | 60 hôtes                    |
| **Client 1**                      | Réseau réservé aux postes clients internes.  | VLAN 10     | 16 hôtes                    |
| **Administration systèmes & réseaux** | Réseau d’administration pour la gestion des équipements et serveurs. | VLAN 20 | 3 hôtes                     |


## Table de filtrage cisco 

### Table de filtrage – VLAN Client

| N° | Interface       | Sens   | IP source              | Port src | IP destination          | Port dest        | Protocole | Statut    | Action   |
|----|-----------------|--------|------------------------|----------|--------------------------|------------------|-----------|-----------|----------|
| 1  | 192.168.6.190   | Sortie | *                      | *        | *                        | *                | *         | *         | Autoriser |
| 2  | 192.168.6.190   | Entrée | *                      | *        | *                        | *                | *         | Établie   | Autoriser |
| 3  | 192.168.6.190   | Entrée | *                      | 68       | 255.255.255.255          | 67               | UDP       | Nouvelle  | Autoriser |
| 4  | 192.168.6.190   | Entrée | 192.168.6.128/26       | *        | 192.168.6.10 / .11       | 53               | UDP       | Nouvelle  | Autoriser |
| 5  | 192.168.6.190   | Entrée | 192.168.6.128/26       | *        | 192.168.6.0/25           | 22 / 3389        | TCP       | Nouvelle  | Bloquer   |
| 6  | 192.168.6.190   | Entrée | 192.168.6.128/26       | *        | 192.168.6.1/.2/.110/.111 | *                | *         | Nouvelle  | Autoriser |
| 7  | 192.168.6.190   | Entrée | 192.168.6.128/26       | *        | 192.168.6.192/28         | *                | *         | Nouvelle  | Bloquer   |
| 8  | 192.168.6.190   | Entrée | 192.168.6.128/26       | *        | 192.168.6.0/25           | *                | *         | Nouvelle  | Bloquer   |
| 9  | 192.168.6.190   | Entrée | 192.168.6.128/26       | *        | *                        | *                | *         | Nouvelle  | Autoriser |


### Table de filtrage – VLAN Production

| N° | Interface       | Sens   | IP source        | Port src | IP destination      | Port dest | Protocole | Statut   | Action   |
|----|-----------------|--------|------------------|----------|---------------------|-----------|-----------|----------|----------|
| 1  | 192.168.6.126   | Sortie | *                | *        | *                   | *         | *         | *        | Autoriser |
| 2  | 192.168.6.126   | Entrée | *                | *        | *                   | *         | *         | Établie  | Autoriser |
| 3  | 192.168.6.126   | Entrée | 192.168.6.0/25   | *        | 192.168.6.192/28    | *         | *         | Nouvelle | Bloquer   |
| 4  | 192.168.6.126   | Entrée | 192.168.6.0/25   | *        | 192.168.6.128/26    | *         | *         | Nouvelle | Bloquer   |
| 5  | 192.168.6.126   | Entrée | 192.168.6.0/25   | *        | *                   | *         | *         | Nouvelle | Autoriser |


### Table de filtrage – VLAN Administration

| N° | Interface       | Sens   | IP source          | Port src | IP destination        | Port dest | Protocole | Statut   | Action   |
|----|-----------------|--------|--------------------|----------|-----------------------|-----------|-----------|----------|----------|
| 1  | 192.168.6.206   | Sortie | *                  | *        | *                     | *         | *         | *        | Autoriser |
| 2  | 192.168.6.206   | Entrée | *                  | *        | *                     | *         | *         | Établie  | Autoriser |
| 3  | 192.168.6.206   | Entrée | 192.168.6.192/28   | -        | 192.168.6.0/25, .128/26 | -       | ICMP      | Nouvelle | Autoriser |
| 4  | 192.168.6.206   | Entrée | 192.168.6.192/28   | *        | 192.168.6.128/26      | *         | *         | Nouvelle | Autoriser |
| 5  | 192.168.6.206   | Entrée | 192.168.6.192/28   | *        | 192.168.6.0           | 22 / 3389 | TCP       | Nouvelle | Bloquer   |
| 6  | 192.168.6.206   | Entrée | 192.168.6.192/28   | *        | 192.168.6.1/.2/.110/.111 | *     | *         | Nouvelle | Autoriser |
| 7  | 192.168.6.206   | Entrée | 192.168.6.192/28   | *        | 192.168.6.10 / .11    | 53        | UDP       | Nouvelle | Bloquer   |
| 8  | 192.168.6.206   | Entrée | 192.168.6.192/28   | *        | 192.168.6.125         | 443       | TCP       | Nouvelle | Bloquer   |
| 9  | 192.168.6.206   | Entrée | 192.168.6.192/28   | *        | 192.168.6.0/25        | *         | *         | Nouvelle | Autoriser |
| 10 | 192.168.6.206   | Entrée | 192.168.6.192/28   | *        | *                     | *         | *         | Nouvelle | Autoriser |



> **Remarque :**  
> L’évolution du réseau est un élément primordial.  
> Le plan d’adressage doit permettre d’accueillir **au moins le double d’équipements** par rapport au recensement initial afin d’éviter toute saturation future.

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

<div style="text-align: center; margin-top: 2em;">
  <em>Projet réalisé par l’agence <strong>Frankfurt</strong> – BTS SIO SISR</em><br>
  <em>Esteban TOUZET & Joris TEXIER</em><br>
</div>




