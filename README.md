<img width="500" height="250" alt="Sound Monitoring Logo with Ear and Waveform" src="https://github.com/user-attachments/assets/cfc33550-3a46-4c92-adac-296a7554ecd4" />

# AcoustiX — Simulateur Web Interactif

> Livrable Proto 1 · UTT Filière Réseau & Télécom · Avril 2026

Simulateur de l'écosystème IoT AcoustiX en vue de dessus. Permet de démontrer la chaîne complète **capteur → passerelle edge → seuillage → alerte → bracelet vibrant** sans hardware, directement dans un navigateur.

---

## Démo rapide

```
Télécharger simulator.html → double-clic → ouvrir dans Chrome ou Firefox
```

Aucune installation. Aucune dépendance. Aucun serveur.

---

## Contexte

AcoustiX remplace la Salle Des Machines (SDM) statique des arrêts techniques industriels et nucléaires par une infrastructure IoT temps réel. Ce simulateur couvre le **Proto 1** :

| Composant simulé | Technologie réelle |
|---|---|
| Capteurs sonores | ESP32 + MEMS INMP441 |
| Passerelle edge | Raspberry Pi (FFT + seuillage) |
| Broker messages | Mosquitto MQTT |
| Base de données | InfluxDB |
| Dashboard | Grafana |
| Balises de zone | ESP32-C3 BLE advertising |
| Bracelets vibrants | ESP32 scan BLE + GPIO |

> **Principe fondamental** : la chaîne d'alerte est 100 % déterministe. Chaque décision est un calcul, pas une prédiction.

---

## Scénarios disponibles

| # | Scénario | Description |
|---|---|---|
| 1 | **Calme** | État nominal, tous capteurs ≤ 75 dB |
| 2 | **Meuleuse Salle A** | Source unique 92 dB, alerte ambre, bracelets zone A |
| 3 | **Multi-source Hall** | 3 sources simultanées ~88 dB, alerte hall |
| 4 | **Transition porte** | Suivi d'un ouvrier Salle A → Hall → Salle B |
| 5 | **Alarme incendie** | Tous capteurs > 98 dB, évacuation générale |

---

## Seuils d'alerte

```
≤ 85 dB  →  vert    (normal)
85–95 dB →  ambre   (attention)
> 95 dB  →  rouge   (alarme)
```

---

## Structure du fichier

```
simulator.html
└── <style>      Design system + animations CSS
└── <body>       Plan SVG du bâtiment + panneaux
└── <script>     Moteur de scénarios + log temps réel
```

Tout est dans un seul fichier HTML autonome, conformément aux contraintes du livrable.

---

## Stack & contraintes techniques

- **Vanilla JS** — aucun framework, aucun build
- **CSS animations** — 60 fps, transitions 600 ms
- **Données simulées** — valeurs dB générées avec `±2 dB` de bruit, rafraîchies toutes les 800 ms
- **Offline** — fonctionne sans connexion après chargement des CDN

---

## Lancer en local

```bash
git clone https://github.com/<org>/acoustix-simulator
open acoustix-simulator/simulator.html   # macOS
# ou
start acoustix-simulator/simulator.html  # Windows
```

---

## Lien avec les autres composants AcoustiX

Ce simulateur est développé en parallèle des autres livrables. Les formats de données respectent le protocole MQTT défini par l'équipe réseau (topics `capteur/<zone>/<id>`, payload JSON).

Quand le hardware est disponible, les scripts Python d'injection (`inject_scenarios.py`) remplacent le moteur JS en publiant les mêmes scénarios sur le vrai broker Mosquitto.

---

## Équipe

| Rôle | Responsable |
|---|---|
| Simulateur web + firmware + stack serveur | Jules |
| Protocole MQTT + firmware réseau | Japhet |
| Spec BLE + expérience utilisateur | Shéryl |

---

*Projet AcoustiX · UTT Réseau & Télécom · 2026*
