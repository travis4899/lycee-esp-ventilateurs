# Conception KiCad

Ce dossier contient les documents de conception électronique.

## Documents présents

- `schema-principe-v0.1.svg` : premier schéma fonctionnel de principe, destiné à valider l'architecture avant la saisie native dans KiCad.

## Fichiers KiCad à venir

- `ir-360.kicad_pro` : paramètres du projet.
- `ir-360.kicad_sch` : schéma électronique natif.
- `ir-360.kicad_pcb` : circuit imprimé.

## Architecture représentée

- 8 branches LED indépendantes, chacune avec sa résistance série.
- Commande commune des cathodes par MOSFET low-side AO3400A.
- Résistance de grille de 100 Ω et pull-down de 10 kΩ.
- Connecteur JST-XH 3 broches : GND / +5V / IR.
- Condensateurs de filtrage : 470 à 1000 µF et 100 nF entre +5V et GND.

## Points à valider

La résistance de 27 Ω / 0,5 W par LED est une valeur de départ, pas une validation définitive. Il faudra vérifier le courant impulsionnel admissible des TSAL6200 dans les conditions réelles de modulation, la puissance impulsionnelle des résistances, le brochage du AO3400A et les courants dans le connecteur et les pistes.

Le schéma SVG est un document de revue fonctionnelle, pas encore un schéma KiCad prêt à générer une netlist. La saisie native KiCad et la vérification ERC restent à faire.
