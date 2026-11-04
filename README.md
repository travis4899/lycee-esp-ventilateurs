# Lycée – Pilotage des ventilateurs de salles de classe

Projet de réalisation d'un système connecté permettant de piloter les ventilateurs installés dans les salles de classe du lycée, avec une supervision et une commande centralisées depuis **Home Assistant**.

Le système repose sur des microcontrôleurs **ESP32** programmés avec **ESPHome**. L'objectif est de disposer de modules autonomes, reproductibles et faciles à maintenir, tout en conservant une gestion centralisée.

## Présentation du besoin

Chaque salle de classe est équipée de **9 ventilateurs**. Le projet vise à commander ces ventilateurs depuis une interface commune, sans intervention individuelle sur chaque appareil.

Le déploiement envisagé concerne à terme environ **50 salles**, soit jusqu'à **450 ventilateurs**.

## Architecture générale

Chaque salle disposera d'un module ESP32 exécutant ESPHome. Celui-ci assurera la commande locale des ventilateurs. Les modules seront intégrés au réseau IP du lycée et supervisés depuis Home Assistant.

```text
                    HOME ASSISTANT
              Supervision et commande centrale
                            |
                   Réseau IP du lycée
                            |
             +--------------+--------------+
             |              |              |
          ESP32          ESP32          ESP32       ...
         Salle 01       Salle 02       Salle 03
             |              |              |
        Émetteur(s)    Émetteur(s)    Émetteur(s)
        de commande    de commande    de commande
             |              |              |
          9 ventil.      9 ventil.      9 ventil.
          Salle 01       Salle 02       Salle 03

                    ... environ 50 salles
```

Le premier axe matériel étudié est une commande infrarouge. Un émetteur IR à diffusion horizontale est développé pour couvrir une salle sans devoir viser chaque ventilateur séparément.

## Pourquoi ESPHome et Home Assistant ?

- **ESPHome** permet de configurer et de mettre à jour les ESP32 de manière homogène.
- **Home Assistant** fournit l'interface centralisée, les automatismes et la supervision.
- Une configuration par salle permet de reproduire le déploiement et de faire évoluer progressivement l'installation.
- L'état de disponibilité de chaque module pourra être suivi depuis Home Assistant.

## Sous-projets matériels

### Émetteur infrarouge 360°

Le premier sous-projet matériel est une carte émettrice IR circulaire destinée à diffuser les commandes dans le plan horizontal.

Principaux choix de composants :
- 8 LED infrarouges Vishay TSAL6200 ;
- 8 résistances de limitation de 22 Ω / 1 W ;
- 1 MOSFET N Infineon IRLML6344TRPBF pour la commutation côté masse ;
- 1 résistance de grille de 100 Ω et 1 résistance de rappel de 10 kΩ ;
- 1 condensateur céramique KEMET 100 nF / 50 V X7R ;
- 1 condensateur électrolytique Panasonic 1000 µF / 10 V ;
- alimentation 5 V externe et connecteur JST-XH 3 broches.

La conception, les références fabricants, les liens vers les fiches techniques, les calculs préliminaires et les points de validation sont documentés dans le [dossier matériel de l'émetteur IR 360°](./hardware/emetteur-ir-360/README.md).

## Organisation du dépôt

```text
.
├── hardware/
│   └── emetteur-ir-360/
│       ├── kicad/       Schéma et circuit imprimé
│       └── mecanique/   Boîtier et pièces imprimées en 3D
├── README.md
└── LICENSE
```

Les firmwares ESPHome, les configurations de salles, les procédures de déploiement et les outils de maintenance seront ajoutés à mesure que le projet avancera.

## Étapes du projet

- [x] Créer le dépôt et son arborescence initiale.
- [x] Définir le principe Home Assistant / ESPHome.
- [x] Documenter le premier choix de composants de l'émetteur IR.
- [ ] Finaliser et vérifier le schéma électronique.
- [ ] Concevoir et router le PCB.
- [ ] Réaliser le boîtier imprimé en 3D.
- [ ] Tester la portée et la couverture IR dans une salle.
- [ ] Développer le firmware ESPHome du module de salle.
- [ ] Intégrer le module dans Home Assistant.
- [ ] Valider le fonctionnement des 9 ventilateurs d'une salle.
- [ ] Préparer le déploiement reproductible dans environ 50 salles.

## Licence

Ce projet est distribué sous les termes de la **GNU General Public License v3.0 (GPL-3.0)**. Le texte complet est disponible dans le fichier [LICENSE](./LICENSE).

Les composants, bibliothèques, documentations et matériels tiers conservent leurs licences et conditions propres.
