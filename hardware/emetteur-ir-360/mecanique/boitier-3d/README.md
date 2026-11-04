# Boîtier imprimé en 3D

Ce dossier est dédié à la conception CAO et à la fabrication du boîtier de l'émetteur IR 360°.

## Contraintes d'intégration

- PCB circulaire de Ø60 mm.
- Huit LED TSAL6200 réparties tous les 45°, orientées horizontalement vers l'extérieur.
- Maintien des LED et de leurs pattes pliées à 90° sans faire porter les efforts sur les soudures.
- Accès au connecteur JST-XH 3 broches.
- Dégagement du trou central de fixation M3.
- Protection de la carte et maintien de l'assemblage.

## Organisation des fichiers

- `source/` : fichiers natifs de CAO (FreeCAD, Fusion 360, OpenSCAD ou autre, à choisir).
- `stl/` : exports STL prêts à trancher.
- `3mf/` : projets d'impression 3MF si utiles.
- `docs/` : plans cotés, vues d'assemblage et notes.

Les fichiers de conception seront ajoutés une fois le logiciel CAO et la géométrie validés. Aucun format CAO n'est imposé à ce stade.
