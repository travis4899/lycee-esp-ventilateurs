# Conception KiCad

Ce dossier contiendra les fichiers natifs KiCad du projet :

- `ir-360.kicad_pro` : paramètres du projet.
- `ir-360.kicad_sch` : schéma électronique.
- `ir-360.kicad_pcb` : circuit imprimé.

## Conventions de conception

- Un seul projet KiCad pour le schéma et le PCB.
- PCB circulaire Ø60 mm.
- 8 branches LED indépendantes, chacune avec sa résistance série.
- Commande commune des cathodes par MOSFET low-side.
- Connecteur JST-XH 3 broches avec brochage GND / +5V / IR.
- Trou central de fixation M3.

Les fichiers de conception seront ajoutés après validation du schéma et des empreintes.
