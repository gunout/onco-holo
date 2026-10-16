# 🏥 ONCO-HOLO PRO

<div align="center">

**Plateforme d'assistance clinique en oncologie thoracique**
Hologramme 3D · IA auto-apprenante · RCP collaborative WebRTC · Comptes-rendus HL7/FHIR

[![Statut](https://img.shields.io/badge/statut-prototype-orange)]()
[![Version](https://img.shields.io/badge/version-v6.0-cyan)]()
[![Licence](https://img.shields.io/badge/licence-MIT-green)]()
[![WebRTC](https://img.shields.io/badge/WebRTC-natif-purple)]()
[![Three.js](https://img.shields.io/badge/Three.js-r128-blue)]()

⚠️ **Prototype de démonstration — Ne constitue pas un dispositif médical**

</div>

---

## 📋 Table des matières

- [Aperçu](#-aperçu)
- [Démo](#-démo)
- [Fonctionnalités](#-fonctionnalités)
- [Structure du dépôt](#-structure-du-dépôt)
- [Architecture](#-architecture)
- [Installation](#-installation)
- [Utilisation](#-utilisation)
- [Rôles utilisateurs](#-rôles-utilisateurs)
- [Modules détaillés](#-modules-détaillés)
- [Stack technique](#-stack-technique)
- [Conformité réglementaire](#-conformité-réglementaire)
- [Limitations](#-limitations)
- [Roadmap](#-roadmap)
- [Contribution](#-contribution)
- [Licence](#-licence)
- [Contact](#-contact)

---

## 🎯 Aperçu

**ONCO-HOLO PRO** est un tableau de bord clinique de démonstration destiné à
l'assistance diagnostique et thérapeutique en oncologie thoracique. Il combine :

- 🫁 Une **visualisation holographique 3D** interactive du cancer du poumon (stades I → IV)
- 🧠 Un **moteur de décision clinique** avec scores TNM, ECOG, RECIST 1.1
- 💊 Des **recommandations thérapeutiques** alignées sur les guidelines (NCCN, ESMO)
- 🎥 Un **mode RCP collaboratif** avec visioconférence WebRTC temps réel
- 📈 Un module d'**IA auto-apprenante** par feedback médecin
- 📝 Une **génération automatique de comptes-rendus** (PDF, HL7 FHIR R4)

> ⚠️ **Ce projet est un prototype UX/UI de démonstration.** Il ne constitue pas
> un dispositif médical au sens du règlement MDR 2017/745 et ne peut en aucun
> cas être utilisé pour un diagnostic réel.

---

## 🎬 Démo

### Accès en ligne

```
🌐 https://gunout.github.io/onco-holo/
```

*(si GitHub Pages est activé sur la branche `main` / `gh-pages`)*

### Accès local

```bash
git clone https://github.com/gunout/onco-holo.git
cd onco-holo
python3 -m http.server 8000
# Ouvrir http://localhost:8000
```

---

## ✨ Fonctionnalités

### 🔐 Authentification
- Écran de login avec **4 rôles** (Oncologue, Radiologue, Interne, Admin)
- **2FA** par code OTP 6 chiffres
- Session timer (8h)
- Traçabilité RGPD

### 🫁 Module de visualisation 3D
- Rendu holographique temps réel (Three.js / WebGL)
- Reconstruction anatomique : poumons, trachée, bronches, ganglions
- Tumeur primaire avec **nécrose centrale** et **spicules** dynamiques
- Métastases pulmonaires et extra-pulmonaires pulsantes
- Vues : **3D libre · Axial · Coronal · Sagittal**
- Annotations 3D projetées sur la scène
- Sliders interactifs : densité tumorale, nombre de métastases, opacité tissu sain

### 🧠 IA auto-apprenante
- Modèle versionné (`v4.2.847`)
- Score de confiance affiché en temps réel
- **4 boutons de feedback** médecin : ✓ Exact · ◐ Partiel · ✗ Erreur · ? Incertain
- Impact direct sur la précision du modèle (+0.3% / −0.4%)
- Buffer de cas avant ré-entraînement (seuil 1000)
- Déclenchement manuel du **ré-entraînement** avec animation
- Historique horodaté des feedbacks

### 🎥 Mode RCP Collaboratif (WebRTC)
- Visioconférence 4 participants (1 local + 3 pairs)
- **Caméra et micro réels** via `getUserMedia`
- **Partage d'écran** via `getDisplayMedia`
- Coupure micro / caméra indépendante par participant
- Détection de parole (pastille animée)
- **Enregistrement** de session avec timer
- Latence et statut ICE affichés

### 📅 Calendrier RCP
- Grille mensuelle navigable
- Événements marqués (violet = programmé, rouge = urgent)
- Détection du jour courant
- Modal de planification avec invitations
- Liste des RCP à venir

### 📊 Scores cliniques
- **TNM 8ème édition** (T, N, M)
- **ECOG Performance Status**
- **RECIST 1.1** (lésions cibles, somme des diamètres)
- **Volume tumoral** avec précision IA
- **Profil mutationnel** (EGFR, KRAS, ALK, PD-L1 TPS)
- **Marqueurs biologiques** (LDH, CA 15-3)

### 📈 Courbes de progression (Chart.js)
- Volume tumoral (12 mois)
- Marqueurs biologiques (LDH + CA 15-3)
- Fonction pulmonaire (VEMS)
- Distribution des métastases (doughnut)

### 📝 Comptes-rendus
- Génération automatique de CR oncologique
- Export PDF
- Export **HL7 FHIR R4**
- Signature électronique eIDAS (simulée)

---

## 📁 Structure du dépôt

```
onco-holo/
├── index.html          # Application complète (single-file)
├── README.md           # Ce fichier
├── LICENSE             # Licence MIT (à ajouter)
├── CHANGELOG.md        # Historique des versions (à ajouter)
└── docs/               # Documentation additionnelle (à ajouter)
    ├── CONTRIBUTING.md
    ├── DISCLAIMER.md
    └── screenshots/
```

> 📌 Le projet est actuellement **mono-fichier** (`index.html`) pour faciliter
> le déploiement et la démonstration. Une modularisation est prévue dans la roadmap.

---

## 🏗️ Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                     ONCO-HOLO PRO v6.0                      │
├──────────────┬──────────────┬──────────────┬────────────────┤
│   LOGIN      │  PATIENTS    │  VIEWPORT    │   DIAGNOSTIC   │
│  (Auth 2FA)  │  (Dossiers)  │  (3D + RCP)  │  (Charts/IA)   │
├──────────────┴──────────────┴──────────────┴────────────────┤
│                       BOTTOM BAR                            │
│           (Stades I-IV · Timeline · Actions)                │
├─────────────────────────────────────────────────────────────┤
│                       STATUS BAR                            │
│         (PACS · DICOM · HL7 · WebRTC · IA · MDR)            │
└─────────────────────────────────────────────────────────────┘
```

### Communication entre modules

```
Login ──► Session ──► Rôles ──► UI permissions
   │
   └──► User context ──► Comptes-rendus (signature RPPS)

Three.js ◄── Sliders ◄── Prompt Builder
   │
   └──► Snapshot ──► CR

WebRTC ◄── Signaling (à connecter) ◄── Pairs distants
   │
   └──► MediaRecorder ──► Enregistrement RCP

Feedback médecin ──► Buffer IA ──► Ré-entraînement ──► New model version
```

---

## 💾 Installation

### Prérequis

- Navigateur moderne supportant **WebRTC** et **WebGL 2.0**
  - Chrome 90+ · Firefox 88+ · Safari 15+ · Edge 90+
- Connexion **HTTPS** obligatoire pour `getUserMedia` / `getDisplayMedia`
  (ou `localhost` pour développement)

### Cloner et lancer

```bash
# 1. Cloner le dépôt
git clone https://github.com/gunout/onco-holo.git
cd onco-holo

# 2. Aucune dépendance npm requise — tout est en CDN
# Bibliothèques externes chargées :
#   - Three.js r128 (cdnjs)
#   - Chart.js 4.4.0 (cdnjs)

# 3. Lancer un serveur local (obligatoire pour WebRTC)
python3 -m http.server 8000
# ou
npx serve .
# ou
php -S localhost:8000
```

### Accès

Ouvrir : **http://localhost:8000/index.html**

### Déploiement en production

```bash
# Hébergement statique simple
# Copier index.html + README.md sur votre CDN/HDS
```

> ⚠️ Pour un hébergement en production médicale, un hébergeur **HDS**
> (Hébergeur de Données de Santé) certifié est obligatoire en France.

---

## 🎮 Utilisation

### 1. Connexion

1. Ouvrir l'application
2. Sélectionner le **rôle** (Oncologue / Radiologue / Interne / Admin)
3. Saisir identifiant + mot de passe
4. Saisir le **code 2FA** (6 chiffres)
5. Cliquer sur **"Se Connecter"**

### 2. Sélection d'un patient

- Cliquer sur une **carte patient** dans le panneau gauche
- La scène 3D, le prompt et les scores se mettent à jour

### 3. Manipulation de l'hologramme

| Action | Résultat |
|--------|----------|
| **Sliders gauche** | Modifient tumeur, métastases, opacité en temps réel |
| **Tags** | Ajoutent/retirent des modificateurs anatomiques |
| **Vues (toolbar)** | 3D · Axial · Coronal · Sagittal |
| **Stades I-IV** (bas) | Transforment la scène selon le stade |
| **Pause / Reset / Snapshot** | Contrôles d'animation |

### 4. Analyse diagnostique

Cliquer sur les onglets à droite :
- **📈 Charts** — 4 courbes de progression
- **📅 RCP** — Calendrier + planification
- **🧠 IA** — Feedback et ré-entraînement
- **Scores** — TNM, ECOG, RECIST, mutations
- **Thérapie** — Recommandations + contre-indications

### 5. Mode RCP

1. Cliquer sur **🎥 RCP LIVE** (toolbar) ou **🎥** (bottombar)
2. Autoriser l'accès caméra/micro (navigateur)
3. Les 3 pairs rejoignent automatiquement
4. Contrôles disponibles :
   - 🎙 **Micro** — activer/couper
   - 📷 **Caméra** — activer/couper
   - 📺 **Partager** — partage d'écran réel
   - ⏺ **Enregistrer** — démarrer l'enregistrement
   - ✕ **Fin RCP** — clôturer

### 6. Génération de compte-rendu

1. Onglet **CR** dans le panneau droit
2. Cliquer sur **⚡ Générer**
3. Options : **⬇ PDF** · **🔗 HL7** · **✓ Signer**

---

## 👥 Rôles utilisateurs

| Rôle | Icône | Accès |
|------|-------|-------|
| **Oncologue** | 👨‍⚕️ | Tous les modules · Signature CR · RCP |
| **Radiologue** | 🔬 | Imagerie · Scores · RCP (pas de signature thérapeutique) |
| **Interne** | 📚 | Consultation · Apprentissage (lecture seule CR) |
| **Admin** | ⚙️ | Gestion système · Logs · Utilisateurs |

Chaque rôle applique une **badge colorée** dans la top bar et conditionne
les actions disponibles.

---

## 🔬 Modules détaillés

### Module IA auto-apprenante

Le modèle commence en version `v4.2.847` avec une précision de `94.7%`.

**Règles d'apprentissage :**
```javascript
✓ Exact      → +0.3% précision
◐ Partiel    → +0.1% précision
✗ Erreur     → −0.4% précision + nouvelle version générée
? Incertain  →  0.0% (enregistré pour analyse)
```

**Déclenchement du ré-entraînement :**
- Automatique : à 1000 feedbacks
- Manuel : bouton **⚡ Déclencher ré-entraînement**
- Durée : ~20 étapes progressives
- Résultat : nouvelle version + précision +0.5%

### Module WebRTC

**Configuration ICE :**
```javascript
{
  iceServers: [
    { urls: 'stun:stun.l.google.com:19302' },
    { urls: 'stun:stun1.l.google.com:19302' }
  ]
}
```

**⚠️ En production**, un serveur **TURN** (coturn) et un serveur de
**signaling WebSocket** sont nécessaires pour traverser les NAT restrictifs.

### Module Comptes-rendus

Format de sortie :
- **Texte structuré** : patient, pathologie, imagerie, biologie, décision, suivi
- **PDF** : via export navigateur ou bibliothèque côté serveur
- **HL7 FHIR R4** : ressource `DiagnosticReport` + `Observation`

---

## 🛠️ Stack technique

| Couche | Technologie |
|--------|-------------|
| **3D** | Three.js r128 (WebGL) |
| **Graphiques** | Chart.js 4.4.0 |
| **Visioconférence** | WebRTC natif (`RTCPeerConnection`, `getUserMedia`, `getDisplayMedia`) |
| **Enregistrement** | MediaRecorder API |
| **UI** | HTML5 + CSS3 (Grid, Flexbox, Custom Properties) |
| **Logique** | Vanilla JavaScript ES6+ |
| **Persistance** | *(à connecter : IndexedDB, backend REST)* |
| **Signaling** | *(à connecter : WebSocket / Socket.io)* |

### Aucune dépendance npm

Le projet est **100% autonome** dans un seul fichier `index.html`.
Les bibliothèques externes sont chargées par CDN (cdnjs).

---

## ⚖️ Conformité réglementaire

### États actuels (démo)

| Norme | Statut |
|-------|--------|
| MDR 2017/745 (Classe IIa) | ❌ Non applicable (prototype) |
| RGPD | ⚠️ À documenter |
| HDS (Hébergement Santé) | ❌ Non hébergé |
| ISO 13485 | ❌ Non certifié |
| IEC 62304 | ❌ Non applicable |
| eIDAS | ⚠️ Signature simulée |

### Pour un usage clinique réel

1. **Marquage CE** — Classe IIa minimum
2. **Système qualité** ISO 13485
3. **Cycle de vie logiciel** IEC 62304
4. **Gestion des risques** ISO 14971
5. **Hébergement HDS** certifié
6. **Validation clinique** multicentrique
7. **ANSM** notification
8. **DPO** désigné (RGPD)
9. **Audit trail** complet et inviolable
10. **Chiffrement** AES-256 au repos, TLS 1.3 en transit

---

## ⚠️ Limitations

| Limitation | Détail |
|-----------|--------|
| **Signaling WebRTC** | Pas de serveur WebSocket — pairs distants simulés |
| **Backend** | Aucune persistance — données en mémoire |
| **Base patients** | 4 patients statiques de démonstration |
| **IA** | Modèle simulé — pas d'inférence réelle |
| **PACS/DICOM** | Interfaces affichées, non connectées |
| **HL7 FHIR** | Message simulé, non transmis |
| **Signature eIDAS** | Modal uniquement |
| **Sécurité** | Aucune authentification backend réelle |
| **Accessibilité** | Partielle (contrastes, ARIA à compléter) |

---

## 🗺️ Roadmap

### Version 6.1 (Q1)
- [ ] Serveur signaling WebSocket (Node.js + Socket.io)
- [ ] MediaRecorder avec export WebM
- [ ] IndexedDB pour persister les feedbacks IA

### Version 6.2 (Q2)
- [ ] Backend REST (FastAPI / Node.js)
- [ ] Base de données PostgreSQL + chiffrement
- [ ] Intégration PACS réelle (Orthanc)
- [ ] Export DICOM

### Version 7.0 (Q3-Q4)
- [ ] IA réelle (TensorFlow.js ou serveur d'inférence)
- [ ] Multi-utilisateurs simultanés
- [ ] Chat intégré dans la RCP
- [ ] Vidéo 1080p / audio HD
- [ ] Application mobile (React Native)

### Vision long terme
- [ ] Support multi-pathologies (sein, colon, prostate)
- [ ] Intégration dossier patient (DPI)
- [ ] Module de recherche clinique
- [ ] Marquage CE Classe IIa

---

## 🤝 Contribution

### Guidelines

1. Fork le projet
2. Créer une branche (`git checkout -b feature/ma-fonctionnalite`)
3. Commiter (`git commit -m 'feat: ajout de ma fonctionnalité'`)
4. Pusher (`git push origin feature/ma-fonctionnalite`)
5. Ouvrir une Pull Request

### Conventions de code

- **CSS** : variables CSS pour les couleurs, BEM pour les classes
- **JS** : ES6+, camelCase, commentaires en français pour le domaine médical
- **Commits** : `feat:`, `fix:`, `docs:`, `style:`, `refactor:`, `test:`

### Tests à effectuer avant PR

- [ ] Chrome, Firefox, Safari, Edge
- [ ] Mobile responsive
- [ ] WebRTC fonctionnel (avec caméra)
- [ ] Aucune erreur console
- [ ] Accessibilité clavier

---

## 📄 Licence

```
MIT License

Copyright (c) 2026 — gunout / ONCO-HOLO PRO

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```

> ⚠️ **Note médicale** : cette licence s'applique au **code source** uniquement.
> Toute utilisation clinique réelle nécessite un **marquage CE** et une
> **autorisation réglementaire** préalable, indépendamment de la licence
> logicielle.

---

## 📞 Contact

| Canal | Lien |
|-------|------|
| **Dépôt GitHub** | [github.com/gunout/onco-holo](https://github.com/gunout/onco-holo/) |
| **Issues** | [github.com/gunout/onco-holo/issues](https://github.com/gunout/onco-holo/issues) |
| **Discussions** | [github.com/gunout/onco-holo/discussions](https://github.com/gunout/onco-holo/discussions) |
| **Auteur** | [@gunout](https://github.com/gunout) |

---

## 🙏 Remerciements

- **Three.js** — moteur 3D
- **Chart.js** — visualisation de données
- **Communauté médicale** — guidelines NCCN, ESMO, ASCO
- **Contributeurs** — tous les professionnels de santé ayant participé aux tests

---

<div align="center">

**ONCO-HOLO PRO v6.0**

*Assistance clinique · Hologramme 3D · IA · RCP collaborative*

⚕️ **Prototype de démonstration — ne remplace pas le jugement clinique** ⚕️

Made with ❤️ for the medical community

</div>
