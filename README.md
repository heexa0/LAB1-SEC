# LAB1-SEC
# LAB1 — Sécurité Mobile avec Mobexler

> **Objectif du lab :** Prendre en main un environnement de test d'intrusion mobile prêt à l'emploi, et comprendre comment auditer des applications Android dans un cadre éthique et contrôlé.

---

## Vue d'ensemble

### Qu'est-ce que Mobexler ?

Mobexler est une **machine virtuelle dédiée au pentest mobile**. Elle cible principalement les plateformes Android (et iOS), et embarque nativement l'ensemble de la chaîne d'outils nécessaire à un audit de sécurité applicatif mobile.

Distribuée sous forme d'image `.ova`, elle s'importe directement dans **VirtualBox** ou **VMware** et tourne sur tout système hôte (Windows, macOS, Linux) sans configuration préalable.

---

## Pourquoi Mobexler plutôt qu'une installation manuelle ?

Configurer un environnement de pentest mobile from scratch pose plusieurs défis :

- Multiplicity d'outils à installer séparément (ADB, Frida, Burp, apktool, jadx…)
- Conflits fréquents entre versions de dépendances
- Configuration fastidieuse du proxy et des certificats sur l'émulateur
- Nécessité de patcher ou rooter certains APKs manuellement

**Mobexler élimine ces frictions** : tous les outils sont déjà installés, configurés et opérationnels. On importe la VM, on la démarre, et on peut commencer à auditer immédiatement.

---

## Arsenal technique inclus

| Domaine | Outils disponibles |
|---|---|
| Décompilation / Analyse statique | `jadx`, `apktool`, `dex2jar`, `androguard` |
| Instrumentation dynamique | `Frida`, `objection`, `Xposed Framework` |
| Interception réseau | `Burp Suite`, `mitmproxy`, `Wireshark` |
| Interaction avec l'émulateur | `adb`, Android SDK tools |
| Reverse engineering bas niveau | `Ghidra`, `strings`, `binwalk` |
| Exploitation | `Metasploit`, `drozer` |
| Environnement général | `Python3`, `Node.js`, `Git`, `VS Code` |

---

## Architecture du laboratoire

```
┌──────────────────────────────────────────────────┐
│                   Machine Hôte (Windows)          │
│                                                  │
│    ┌─────────────────────────────┐               │
│    │    Émulateur Android        │               │
│    │    (Android Studio AVD)     │               │
│    └──────────────┬──────────────┘               │
│                   │ ADB over TCP                 │
│    ┌──────────────▼──────────────┐               │
│    │       VM Mobexler           │               │
│    │   IP locale : 10.0.2.15    │               │
│    │   Gateway hôte : 10.0.2.2  │               │
│    └─────────────────────────────┘               │
└──────────────────────────────────────────────────┘
```

La VM Mobexler communique avec l'émulateur tournant sur l'hôte via **ADB over TCP**. Elle intercepte, analyse et manipule le trafic et le comportement de l'application cible sans avoir besoin d'un vrai appareil physique.

---

## Scénarios d'utilisation typiques

- **Apprentissage des vulnérabilités Android** — via des apps délibérément vulnérables comme DIVA (*Damn Insecure and Vulnerable App*)
- **Interception HTTPS** — en configurant Burp Suite comme proxy et en installant son certificat dans l'émulateur
- **Bypass de protections runtime** — hooking de fonctions avec Frida pour contourner la détection de root ou le SSL pinning
- **Analyse du code source** — décompilation d'APKs obfusqués avec jadx pour lire le Java/Kotlin sous-jacent
- **Audit des stockages locaux** — détection de données sensibles stockées en clair (SharedPreferences, SQLite, fichiers internes)

---

## Établir la connexion ADB

L'émulateur Android tourne sur la machine hôte Windows. Depuis Mobexler, on y accède via l'IP de la gateway VirtualBox.

**Étape 1 — Sur l'hôte Windows (invite de commandes) :**

```cmd
adb forward tcp:5555 tcp:5554
```

**Étape 2 — Depuis le terminal dans Mobexler :**

```bash
adb connect 10.0.2.2:5555
adb devices
```

> Si la connexion réussit, `adb devices` affiche l'émulateur comme périphérique connecté.

---

<img width="740" height="375" alt="image" src="https://github.com/user-attachments/assets/f22c44dc-a6f8-46f1-9666-efa2b3b4a274" />
<img width="285" height="307" alt="image" src="https://github.com/user-attachments/assets/58341f5d-c7c7-4912-a76b-89b3b1cd29bf" />

<img width="429" height="425" alt="image" src="https://github.com/user-attachments/assets/c04b7dcd-454f-4321-b75a-f7277f032eb0" />

> Les captures suivantes documentent les différentes étapes de réalisation :

| Étape | Description |
|---|---|
| `devices` | Vérification des périphériques ADB détectés |
| `demarrage` | Démarrage de la VM Mobexler |
| `acceuil` | Interface d'accueil de la distribution |
| `snapshot` | Snapshot de la VM avant manipulation |
| `ip` | Vérification de la configuration réseau |
| `installation de mobexler` | Processus d'installation |
| `import-mobexler` | Import de l'image OVA dans VirtualBox |
| `hash` | Vérification d'intégrité du fichier téléchargé |

---

## Ressources

- [Site officiel Mobexler](https://mobexler.com)
- Documentation OWASP Mobile Security Testing Guide (MSTG)
- [DIVA Android App](https://github.com/payatu/diva-android) — application d'entraînement

---
