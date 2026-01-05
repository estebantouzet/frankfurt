# Situation 6 : Filtrage protocolaire

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

## 1. Création des règles de filtrage
### Règle 1

| N° | Interface | Sens | IP source | Port source | IP destination | Port destination | Protocole | Statut | Action |
|---|----------|------|----------|-------------|---------------|------------------|-----------|--------|--------|
| 1 | 192.168.66.254 | Entrer | 192.168.6.0/24 | * | 192.36.6.20 | 80, 443 | TCP | Nouvelle | Autoriser |
|   | 192.168.66.254 | Entrer | 192.168.6.0/24 | * | 192.36.6.20 | 22 | TCP | Nouvelle | Autoriser |


### Règle 2

| N° | Interface | Sens | IP source | Port src | IP dest | Port dest | Protocole | Statut | Action |
|---|----------|------|----------|----------|--------|-----------|-----------|--------|--------|
| 2 | 192.168.66.254 | Entrer | 192.168.6.192/28 | * | * | * | * | Nouvelle | Autoriser |


### Règle 3

| N° | Interface | Sens | IP source | Port src | IP dest | Port dest | Protocole | Statut | Action |
|---|----------|------|----------|----------|--------|-----------|-----------|--------|--------|
| 3 | 192.168.66.254 | Entrer | 192.168.6.0/25 | * | * | 123 | UDP | Nouvelle | Autoriser |


### Règle 4

| N° | Interface | Sens | IP source | Port src | IP dest | Port dest | Protocole | Statut | Action |
|---|----------|------|----------|----------|--------|-----------|-----------|--------|--------|
| 4 | 192.168.66.254 | Entrer | 192.168.6.0/24 | * | 192.168.66.254 | 443 | TCP | Nouvelle | Autoriser |


### Règle 5

| N° | Interface | Sens | IP source | IP dest | Port | Protocole | Statut | Action |
|---|----------|------|----------|--------|------|-----------|--------|--------|
| 5 | 192.168.66.254 | Entrer | 192.168.6.10 / .11 / .12 | * | 53 | DNS | Nouvelle | Autoriser |


### Règle 6

| N° | Interface | Sens | IP source | IP dest | Port | Protocole | Statut | Action |
|---|----------|------|----------|--------|------|-----------|--------|--------|
| 6 | 192.168.66.254 | Entrer | 192.168.6.0/24 | 192.36.0.0/18 | 80,443 | TCP | Nouvelle | Autoriser |


### Règle 7

| N° | Interface | Sens | IP source | IP dest | Ports | Protocole | Statut | Action |
|---|----------|------|----------|--------|-------|-----------|--------|--------|
| 7 | 192.168.66.254 | Entrer | 192.36.0.0/18 | 192.36.6.20/24 | 80,443,20,21 | TCP | Nouvelle | Autoriser |


### Règle 8

| N° | Interface | Sens | IP source | IP dest | Port | Protocole | Statut | Action |
|---|----------|------|----------|--------|------|-----------|--------|--------|
| 8 | 192.168.66.254 | Entrer | 192.36.0.0/18 | 192.36.6.10 / .11 | 53 | TCP, UDP | Nouvelle | Autoriser |


### Règle 9 – Accès Internet

| N° | Interface | Sens | IP source | IP dest | Ports | Protocole | Statut | Action |
|---|----------|------|----------|--------|-------|-----------|--------|--------|
| 9 | 192.168.66.254 | Entrer | 192.168.6.0/24 | Internet | 80,443 | TCP | Nouvelle | Autoriser |




## 2. Proposer une amélioration de la proposition de filtrage initiale.

**Note :**
**Analyse préalable de la table de filtrage proposée par le DSL.**
Règle 1 : Les sous-réseaux Production et Clients n'ont pas à utiliser le protocole SSH.
Règle 2 : Le VLAN d'Administration a accès à n'importe quelle adresse IP avec tout type de protocole. Bien que ce VLAN dispose d'autorisations plus amples, cela n'est pas pertinent. Les bonnes pratiques recommandent fortement d'interdire l'accès à Internet au poste d'administration
Règle 3 : OK
Règle 4 : Il manque une règle de protection de la passerelle (ensemble des interfaces du pare-feu).
Seul le sous-réseau Administration peut interroger sa passerelle et donc aller sur la page Web du pare-feu Stormshield
Règle 5 : OK
Règle 6 : OK

**Il manque une règle de blocage par défaut explicite.**

NB : La règle de blocage par défaut explicite avec journalisation préconisée par l'ANSSI est sujette à interprétation. L'entreprise Stormshield recommande généralement d'éviter cette règle car mal configurée, elle peut générer un bruit conséquent à l'intérieur des fichiers journaux et les rendre ainsi difficilement exploitables.

Ainsi cette règle n'est pertinente que si l'ensemble des protocoles bloqués générant du bruit inutile n'est pas journalisé au préalable (NETBIOS par exemple). C'est d'ailleurs ce que recommande clairement l'ANSSI dans son guide de configuration

<table border="1" cellpadding="6" cellspacing="0" width="100%">
  <thead>
    <tr>
      <th>N°</th>
      <th>Interface</th>
      <th>Sens</th>
      <th>@IP src</th>
      <th>Port src</th>
      <th>@IP dest</th>
      <th>Port dest</th>
      <th>Protocole</th>
      <th>Statut</th>
      <th>Action</th>
    </tr>
  </thead>
  <tbody>

    <!-- Section 1 -->
    <tr>
      <td colspan="10"><strong>Section 1 – Règles d’autorisation à destination du pare-feu</strong></td>
    </tr>
    <tr>
      <td>6</td>
      <td>Règle 4</td>
      <td>Entrer</td>
      <td>192.168.6.192/28</td>
      <td>*</td>
      <td>192.168.66.254</td>
      <td>443</td>
      <td>TCP</td>
      <td>Nouvelle</td>
      <td>Autoriser</td>
    </tr>

    <!-- Section 3 -->
    <tr>
      <td colspan="10"><strong>Section 3 – Règles de protection du pare-feu</strong></td>
    </tr>
    <tr>
      <td>3</td>
      <td>New line</td>
      <td>Entrer</td>
      <td>*</td>
      <td>*</td>
      <td>192.36.253.60</td>
      <td>*</td>
      <td>*</td>
      <td>Nouvelle</td>
      <td>Bloquer</td>
    </tr>
    <tr>
      <td>6</td>
      <td>Règle 4</td>
      <td>Entrer</td>
      <td>*</td>
      <td>*</td>
      <td>192.168.66.254</td>
      <td>*</td>
      <td>*</td>
      <td>Nouvelle</td>
      <td>Bloquer</td>
    </tr>

    <!-- Section 4 -->
    <tr>
      <td colspan="10"><strong>Section 4 – Règles d’autorisation des flux métiers</strong></td>
    </tr>
    <tr>
      <td>2</td>
      <td>Règle 1</td>
      <td>Entrer</td>
      <td>192.168.6.0/24</td>
      <td>*</td>
      <td>192.36.6.20</td>
      <td>80,443</td>
      <td>TCP</td>
      <td>Nouvelle</td>
      <td>Autoriser</td>
    </tr>
    <tr>
      <td>3</td>
      <td>Règle 1</td>
      <td>Entrer</td>
      <td>192.168.6.192/28</td>
      <td>*</td>
      <td>192.36.6.20</td>
      <td>22</td>
      <td>TCP</td>
      <td>Nouvelle</td>
      <td>Autoriser</td>
    </tr>
    <tr>
      <td>4</td>
      <td>Règle 2</td>
      <td>Entrer</td>
      <td>192.168.6.192/28</td>
      <td>*</td>
      <td>*</td>
      <td>80,443</td>
      <td>TCP</td>
      <td>Nouvelle</td>
      <td>Bloquer</td>
    </tr>
    <tr>
      <td>5</td>
      <td>Règle 3</td>
      <td>Entrer</td>
      <td>192.168.6.0/25</td>
      <td>*</td>
      <td>*</td>
      <td>123</td>
      <td>UDP</td>
      <td>Nouvelle</td>
      <td>Autoriser</td>
    </tr>
    <tr>
      <td>7</td>
      <td>Règle 5</td>
      <td>Entrer</td>
      <td>192.168.6.10</td>
      <td>*</td>
      <td>*</td>
      <td>53</td>
      <td>DNS</td>
      <td>Nouvelle</td>
      <td>Autoriser</td>
    </tr>
    <tr>
      <td></td>
      <td>Règle 5</td>
      <td>Entrer</td>
      <td>192.168.6.11</td>
      <td>*</td>
      <td>*</td>
      <td>53</td>
      <td>DNS</td>
      <td>Nouvelle</td>
      <td>Autoriser</td>
    </tr>
    <tr>
      <td></td>
      <td>Règle 5</td>
      <td>Entrer</td>
      <td>192.168.6.12</td>
      <td>*</td>
      <td>*</td>
      <td>53</td>
      <td>DNS</td>
      <td>Nouvelle</td>
      <td>Autoriser</td>
    </tr>
    <tr>
      <td>8</td>
      <td>Règle 6</td>
      <td>Entrer</td>
      <td>192.168.6.0/24</td>
      <td>*</td>
      <td>192.36.0.0/18</td>
      <td>80,443</td>
      <td>TCP</td>
      <td>Nouvelle</td>
      <td>Autoriser</td>
    </tr>
    <tr>
      <td>9</td>
      <td>Règle 7</td>
      <td>Entrer</td>
      <td>192.36.0.0/18</td>
      <td>*</td>
      <td>192.36.6.20/24</td>
      <td>80,443,20,21</td>
      <td>TCP</td>
      <td>Nouvelle</td>
      <td>Autoriser</td>
    </tr>
    <tr>
      <td>10</td>
      <td>Règle 8</td>
      <td>Entrer</td>
      <td>192.36.0.0/18</td>
      <td>*</td>
      <td>192.36.6.10</td>
      <td>53</td>
      <td>TCP, UDP</td>
      <td>Nouvelle</td>
      <td>Autoriser</td>
    </tr>
    <tr>
      <td></td>
      <td>Règle 8</td>
      <td>Entrer</td>
      <td>192.36.0.0/18</td>
      <td>*</td>
      <td>192.36.6.11</td>
      <td>53</td>
      <td>TCP, UDP</td>
      <td>Nouvelle</td>
      <td>Autoriser</td>
    </tr>
    <tr>
      <td></td>
      <td>New line</td>
      <td>Entrer</td>
      <td>Internet</td>
      <td>*</td>
      <td>192.36.6.10 / 192.36.6.11</td>
      <td>53</td>
      <td>TCP</td>
      <td>Nouvelle</td>
      <td>Autoriser</td>
    </tr>
    <tr>
      <td>11</td>
      <td>Règle 9</td>
      <td>Entrer</td>
      <td>192.168.6.0/24</td>
      <td>*</td>
      <td>Internet</td>
      <td>80,443</td>
      <td>TCP</td>
      <td>Nouvelle</td>
      <td>Autoriser</td>
    </tr>

    <!-- Section 6 -->
    <tr>
      <td colspan="10"><strong>Section 6 – Règle d’interdiction finale</strong></td>
    </tr>
    <tr>
      <td>9</td>
      <td>*</td>
      <td>Entrer</td>
      <td>*</td>
      <td>*</td>
      <td>*</td>
      <td>*</td>
      <td>*</td>
      <td>Nouvelle</td>
      <td>Bloquer</td>
    </tr>

  </tbody>
</table>
