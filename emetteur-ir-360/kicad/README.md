# Conception KiCad

Ce dossier contient les documents de conception électronique de l'émetteur infrarouge 360°.

## Documents présents

- `schema-principe-v0.1.svg` : première représentation fonctionnelle de l'architecture.
- `ir-360.kicad_pro` : fichier projet KiCad.
- `ir-360.kicad_sch` : feuille de schéma KiCad initialisée.
- `ir-360.kicad_pcb` : fichier PCB KiCad initialisé.

**État actuel :** les fichiers KiCad sont des bases de travail, pas encore une conception électronique complète ou validée. Le schéma natif doit encore recevoir les symboles, les connexions, les références et les empreintes réelles. Le PCB ne contient pas encore le placement ni le routage définitifs. Le fichier projet `.kicad_pro` est lui aussi à contrôler et à valider à l'ouverture dans KiCad ; ne pas le considérer comme vérifié.

## Architecture électrique à saisir

- 8 branches indépendantes, chacune composée d'une résistance de 22 Ω / 1 W en série avec une LED Vishay TSAL6200.
- Retour commun des cathodes commandé par un MOSFET N Infineon IRLML6344TRPBF en low-side.
- Résistance série de grille : 100 Ω.
- Résistance de rappel Gate-GND : 10 kΩ.
- Connecteur JST-XH 3 broches : GND / +5V / IR.
- Condensateur électrolytique Panasonic EEU-FC1A102 : 1000 µF / 10 V entre +5V et GND.
- Condensateur céramique KEMET C320C104K5R5TA : 100 nF / 50 V X7R entre +5V et GND.

Les détails, liens vers les fiches techniques et calculs préliminaires figurent dans le [README du sous-projet](../README.md).

## Règles et points de contrôle

- Vérifier le brochage exact du IRLML6344 dans la fiche Infineon avant d'associer l'empreinte SOT-23.
- Vérifier anode/cathode et orientation de l'empreinte TSAL6200.
- Respecter la polarité du condensateur électrolytique.
- Prévoir une distribution +5V et GND dimensionnée pour le courant de crête des huit branches.
- Vérifier le courant impulsionnel répétitif des TSAL6200 avec la modulation IR réellement utilisée.
- Vérifier la tenue en impulsions des résistances 22 Ω / 1 W.
- Contrôler les largeurs de pistes, le connecteur et l'échauffement du MOSFET.
- Exécuter l'ERC du schéma puis le DRC du PCB avant fabrication.

Le schéma SVG est une illustration de principe et ne remplace pas le schéma KiCad natif.
