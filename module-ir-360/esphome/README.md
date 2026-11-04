# Configuration ESPHome du module IR

Ce dossier contient la configuration ESPHome du module installé dans une salle et chargé de piloter la carte driver IR.

## Fichiers prévus

- `module-ir-360.yaml` : configuration ESPHome principale du module.
- `secrets.yaml` : secrets locaux (Wi-Fi, API, OTA), à ne jamais versionner.
- Éventuellement des fichiers `.yaml` complémentaires pour séparer les paramètres communs et les fonctions.

## Responsabilités du firmware

- Configurer la sortie logique IR reliée à l'entrée de commande de la carte driver.
- Générer les trames infrarouges correspondant aux commandes des ventilateurs.
- Exposer le module à Home Assistant.
- Fournir au minimum son état de disponibilité et les informations utiles au diagnostic.

## Avant de figer le YAML

Les éléments suivants restent à préciser et ne sont volontairement pas inventés dans un fichier de configuration :
- modèle exact de carte ESP32 et broche GPIO utilisée ;
- protocole IR des ventilateurs (fréquence porteuse, codage, timings et trames) ;
- commandes nécessaires et éventuelles répétitions ;
- nommage des entités Home Assistant et paramètres réseau.

Les secrets doivent être placés dans un fichier local ignoré par Git ou dans le mécanisme de secrets ESPHome. Ne jamais committer de mot de passe ou de clé API.
