# Émetteur infrarouge 360°

## Objectif

Réaliser un émetteur infrarouge piloté par ESPHome/ESP32, destiné à commander les climatiseurs de la salle.

La carte doit diffuser les ordres IR sur 360° dans le plan horizontal afin de couvrir la pièce sans devoir orienter précisément l'émetteur.

## Architecture prévue

- PCB circulaire de diamètre 60 mm.
- 8 LED infrarouges Vishay TSAL6200 réparties tous les 45°, orientées radialement vers l'extérieur.
- Une résistance de limitation de courant dédiée à chaque LED.
- Commande des LED par un MOSFET canal N AO3400A en commutation côté masse (low-side).
- Entrée de commande logique 3,3 V provenant d'un GPIO de l'ESP32.
- Alimentation externe 5 V pour les LED ; masse commune avec l'ESP32.
- Connecteur JST-XH 3 broches : GND, +5V, IR.
- Condensateurs de découplage sur l'alimentation de la carte.
- Un trou central de fixation M3.

## Arborescence

- `kicad/` : schéma électronique, circuit imprimé et projet KiCad.
- `mecanique/` : informations relatives au boîtier et au maintien des LED.

## Connectique

| Broche | Signal | Fonction |
|---:|---|---|
| 1 | GND | Masse commune |
| 2 | +5V | Alimentation de puissance des LED |
| 3 | IR | Signal de commande depuis un GPIO ESP32 |

La puissance des LED est fournie directement par l'alimentation 5 V sur la carte. Le courant des LED ne doit pas transiter par les pistes d'alimentation de la carte ESP32.

## Points à valider avant fabrication

- Dimensionnement définitif des résistances en fonction de la tension d'alimentation, de la tension directe des TSAL6200 et des limites de courant impulsionnel répétitif du fabricant.
- Vérification du courant total et du dimensionnement des pistes, du connecteur et du MOSFET.
- Vérification de l'empreinte et du brochage du MOSFET AO3400A.
- Vérification de la polarité et de l'orientation des LED.
- Validation mécanique du pliage des pattes à 90° et de leur maintien dans le boîtier.
- Essais de portée et de couverture dans la salle.

**Attention :** les valeurs de composants évoquées pendant la conception restent prévisionnelles. Elles devront être vérifiées avant de considérer le PCB comme prêt à fabriquer.

## État du projet

Initialisation de l'arborescence et de la documentation. Le schéma et le routage KiCad seront ajoutés dans une prochaine étape.
