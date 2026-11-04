# Carte driver IR

Ce dossier regroupe la conception électronique de la carte qui pilote les huit LED infrarouges.

## Contenu

- [Projet KiCad existant](./kicad/README.md) : schéma, PCB, bibliothèques et exports.
- Les nomenclatures (BOM), fichiers de fabrication et documents de validation seront ajoutés ici au fur et à mesure.

## Architecture retenue

- 8 LED Vishay TSAL6200, chacune avec sa résistance série de 22 Ω / 1 W.
- MOSFET N Infineon IRLML6344TRPBF en commutation low-side.
- Commande logique 3,3 V depuis le module ESP32.
- Alimentation LED externe 5 V, masse commune avec l'ESP32.
- Connecteur JST-XH 3 broches : GND / +5V / IR.

Les valeurs et précautions de dimensionnement sont détaillées dans le [dossier général de l'émetteur IR](../README.md).

## Livrables à venir

- [ ] Schéma KiCad complet et vérifié (ERC).
- [ ] PCB placé et routé (DRC).
- [ ] Nomenclature avec références fabricant et empreintes.
- [ ] Fichiers Gerber et perçage.
- [ ] Plan d'assemblage et procédure de test.

**Attention :** les fichiers KiCad actuellement présents sont des bases de travail et ne constituent pas encore un schéma ou un PCB prêts à fabriquer.
