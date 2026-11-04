# Lycée – Pilotage des ventilateurs de salles de classe

Projet de réalisation d'un système connecté permettant de piloter les ventilateurs installés dans les salles de classe du lycée, avec une supervision et une commande centralisées depuis **Home Assistant**.

Le projet s'appuie sur des microcontrôleurs **ESP32** programmés avec **ESPHome**, afin de disposer de modules autonomes, faciles à déployer, à maintenir et à intégrer dans Home Assistant.

## Présentation du besoin

Chaque salle de classe est équipée de **9 ventilateurs**. L'objectif est de pouvoir les commander simplement, sans devoir intervenir individuellement sur chaque appareil.

Le déploiement envisagé concerne à terme environ **50 salles**, soit jusqu'à **450 ventilateurs**.

Le système devra permettre :

- de commander les ventilateurs d'une salle depuis une interface commune ;
- de centraliser le pilotage de toutes les salles dans Home Assistant ;
- de connaître, autant que possible, l'état de fonctionnement des équipements ;
- de déployer et maintenir les modules de commande de manière homogène ;
- de faire évoluer progressivement l'installation, salle par salle.

## Architecture générale envisagée

Chaque salle disposera d'un module ESP32 chargé de transmettre les commandes aux ventilateurs de la pièce.

**ESPHome** sera utilisé pour le firmware des ESP32. Les appareils seront intégrés à **Home Assistant**, qui assurera la centralisation des commandes, des automatismes et de l'interface de supervision.

Vue logique simplifiée :

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

Le moyen de commande local des ventilateurs sera défini et validé au fil du développement. Les premières études portent notamment sur une commande infrarouge, avec des émetteurs adaptés à la couverture de l'ensemble d'une salle.

## Pourquoi ESPHome ?

ESPHome permet de décrire le fonctionnement des modules dans des fichiers de configuration lisibles, de compiler et mettre à jour les firmwares, et d'intégrer nativement les équipements dans Home Assistant.

Cette approche doit faciliter :

- la reproduction d'une configuration identique dans de nombreuses salles ;
- la supervision centralisée des équipements ;
- les mises à jour et la maintenance ;
- l'ajout progressif de fonctions et d'automatismes.

## Organisation du dépôt

```text
.
├── hardware/
│   └── emetteur-ir-360/
│       ├── kicad/       Conception électronique KiCad
│       └── mecanique/   Boîtier et pièces imprimées en 3D
├── README.md
└── LICENSE
```

Les différents sous-projets, firmwares ESPHome, documentations de câblage et outils de déploiement seront ajoutés à mesure que l'architecture sera stabilisée.

## État du développement

Le projet est en phase de conception et de prototypage.

### Premiers travaux

- [x] Création du dépôt et de son arborescence initiale.
- [x] Définition du principe d'une commande centralisée Home Assistant / ESPHome.
- [ ] Valider le protocole de commande des ventilateurs.
- [ ] Réaliser et tester un premier émetteur de commande couvrant une salle.
- [ ] Développer le firmware ESPHome du module de salle.
- [ ] Intégrer le module dans Home Assistant.
- [ ] Valider le fonctionnement simultané des 9 ventilateurs d'une salle.
- [ ] Préparer une architecture reproductible pour le déploiement dans environ 50 salles.
- [ ] Documenter l'installation, la configuration et la maintenance.

## Licence

Ce projet est distribué sous les termes de la **GNU General Public License v3.0 (GPL-3.0)**. Le texte complet de la licence est disponible dans le fichier [LICENSE](./LICENSE).

Les composants, bibliothèques et matériels tiers éventuellement utilisés conservent leurs licences et conditions propres.
