# Émetteur infrarouge 360°

## 1. Rôle dans le projet

Cet émetteur est un sous-ensemble du projet de pilotage des ventilateurs des salles de classe du lycée.

L'objectif global est de commander **9 ventilateurs par salle**, avec un déploiement cible d'environ **50 salles** (jusqu'à 450 ventilateurs). Chaque salle disposera à terme d'un module ESP32 exécutant ESPHome. Les équipements seront supervisés et commandés de manière centralisée par Home Assistant.

Ce sous-projet concerne la réalisation d'un émetteur infrarouge à diffusion horizontale, destiné à transmettre les commandes aux appareils de la salle sans devoir viser individuellement chaque ventilateur.

## 2. Objectifs de la carte

- Émettre les commandes infrarouges sur 360° dans le plan horizontal.
- Utiliser huit LED IR disposées radialement, espacées de 45°.
- Permettre à un ESP32 de piloter les LED par un signal logique 3,3 V.
- Alimenter la puissance des LED par une alimentation externe 5 V.
- Disposer d'une connectique simple entre l'ESP32 et la carte émettrice.
- Être intégrée dans un boîtier imprimé en 3D qui maintient les LED orientées horizontalement.

## 3. Architecture matérielle retenue

| Fonction | Choix |
|---|---|
| Microcontrôleur de commande | ESP32 exécutant ESPHome (hors de cette carte) |
| Nombre de LED | 8 |
| LED IR | Vishay TSAL6200 |
| Résistance série LED | 22 Ω, 1 W, une par LED |
| Transistor de puissance | Infineon IRLML6344TRPBF, MOSFET N, SOT-23 |
| Résistance série de grille | 100 Ω |
| Résistance de rappel grille | 10 kΩ entre Gate et GND |
| Condensateur céramique | KEMET C320C104K5R5TA, 100 nF, 50 V, X7R |
| Condensateur électrolytique | Panasonic EEU-FC1A102, 1000 µF, 10 V |
| Alimentation | 5 V externe |
| Connecteur | JST-XH 3 broches |
| Format de PCB envisagé | Circulaire, Ø60 mm |
| Fixation | Trou central M3 |

Les références indiquées correspondent aux composants sélectionnés pour le prototype. Les références de résistances 22 Ω, 100 Ω et 10 kΩ seront précisées avec leur fabricant et leur empreinte lors de la nomenclature finale.

## 4. Détail des composants et documentation

### 4.1. LED infrarouges — Vishay TSAL6200

**Quantité : 8**

- LED infrarouge émettant autour de 940 nm.
- Boîtier traversant de 5 mm.
- Angle de demi-intensité annoncé : environ ±17°.
- Les huit LED sont disposées sur le pourtour du PCB, avec un espacement angulaire de 45°.
- Chaque LED est orientée radialement vers l'extérieur et horizontalement.
- Les pattes seront pliées à 90° ; le boîtier imprimé en 3D devra reprendre les efforts mécaniques afin de ne pas solliciter les soudures.

Documentation :
- [Fiche technique Vishay TSAL6200 (PDF)](https://www.vishay.com/docs/81010/tsal6200.pdf)

### 4.2. Résistances de limitation des LED — 22 Ω / 1 W

**Quantité : 8 — une résistance en série avec chaque LED**

Chaque branche est câblée indépendamment :

`+5V → résistance 22 Ω → anode LED ; cathode LED → drain du MOSFET`

La résistance limite le courant de chaque LED. Une résistance dédiée par LED permet de limiter les écarts de courant entre les composants.

Estimation indicative à 5 V, avec une tension directe LED de 1,35 V :

`I ≈ (5 − 1,35) / 22 ≈ 166 mA`

La dissipation instantanée dans la résistance pendant une impulsion de courant vaut approximativement :

`P ≈ I² × R ≈ 0,61 W`

Le choix d'un modèle 1 W apporte une marge en puissance moyenne, mais la validation doit aussi prendre en compte les impulsions répétitives : rapport cyclique de la porteuse IR, durée des trames, température ambiante et courbe de surcharge impulsionnelle de la résistance choisie.

**Point de validation important :** la fiche TSAL6200 spécifie des limites de courant impulsionnel sous des conditions de durée et de rapport cyclique précises. Le courant calculé ci-dessus est une estimation, pas une autorisation générale de fonctionner à 166 mA dans n'importe quelle modulation. Il faudra vérifier les conditions exactes de la fiche technique avec la porteuse et les trames réellement émises par ESPHome, puis mesurer le courant sur le prototype.

### 4.3. MOSFET de commande — Infineon IRLML6344TRPBF

**Quantité : 1**

MOSFET canal N en boîtier SOT-23, utilisé en commutation côté masse (*low-side*).

- La source est reliée à GND.
- Le drain est relié au retour commun des huit cathodes de LED.
- La grille reçoit le signal IR provenant du GPIO ESP32, au travers d'une résistance série.
- Le composant est spécifié par Infineon avec une résistance à l'état passant maximale de 37 mΩ sous VGS = 2,5 V (conditions de la fiche technique), ce qui le rend adapté à une commande logique 3,3 V sous réserve de vérifier la dissipation dans le montage réel.
- Tension drain-source nominale maximale : 30 V.
- Boîtier : SOT-23.

Documentation :
- [Fiche technique Infineon IRLML6344 (PDF)](https://www.infineon.com/assets/row/public/documents/24/49/infineon-irlml6344-datasheet-en.pdf)
- [Page produit Infineon IRLML6344](https://www.infineon.com/part/IRLML6344)
- [Référence commandée chez Conrad](https://www.conrad.fr/fr/p/infineon-technologies-irlml6344trpbf-mosfet-1-canal-n-1-3-w-sot-23-161191.html)

Le brochage exact Gate/Source/Drain doit être repris de la vue de boîtier de la fiche technique et vérifié dans l'empreinte KiCad avant routage.

### 4.4. Résistances de commande du MOSFET

**R_GATE — 100 Ω, quantité : 1**

Résistance en série entre le GPIO ESP32 et la grille du MOSFET. Elle limite le courant transitoire de charge/décharge de la capacité de grille et amortit les fronts.

**R_PD — 10 kΩ, quantité : 1**

Résistance entre Gate et GND. Elle maintient le MOSFET bloqué lorsque le GPIO est en haute impédance, notamment pendant le démarrage ou le redémarrage de l'ESP32.

Les puissances nominales et les références fabricants de ces deux résistances seront précisées dans la nomenclature finale. Des résistances CMS sont envisageables pour faciliter l'intégration sur le PCB.

### 4.5. Condensateur céramique — KEMET C320C104K5R5TA

**Quantité : 1**

- Capacité : 100 nF.
- Tension nominale : 50 V.
- Diélectrique : X7R.
- Tolérance : ±10 %.
- Montage traversant radial.
- Utilisé pour le découplage haute fréquence de l'alimentation 5 V, au plus près de l'étage de commutation.

Documentation :
- [Fiche technique KEMET C320C104K5R5TA](https://search.kemet.com/download/specsheet/C320C104K5R5TA7317)
- [Référence Conrad](https://www.conrad.fr/fr/p/condensateur-ceramique-sortie-radiale-kemet-c320c104k5r5ta-100-nf-50-v-10-x7r-1-pc-s-1420280.html)

### 4.6. Condensateur électrolytique — Panasonic EEU-FC1A102

**Quantité : 1**

- Capacité : 1000 µF.
- Tension nominale : 10 V.
- Série Panasonic FC.
- Montage radial polarisé.
- Dimensions indiquées par Panasonic : Ø10 mm × 16 mm, pas de 5 mm.
- Placé entre +5 V et GND pour fournir une réserve locale d'énergie et limiter les creux de tension lors des impulsions de courant des LED.

Respecter impérativement la polarité : borne positive sur +5 V et borne négative sur GND.

Documentation :
- [Fiche produit Panasonic EEU-FC1A102](https://industrial.panasonic.com/ww/products/pt/aluminum-cap-lead/models/EEUFC1A102)
- [Fiche produit Panasonic Amérique du Nord](https://na.industrial.panasonic.com/products/capacitors/aluminum-electrolytic-capacitors/aluminum-electrolytic-capacitors-radial-lead-type/series/83482/model/84590)
- [Référence Conrad](https://www.conrad.fr/fr/p/panasonic-eeu-fc1a102-condensateur-electrolytique-sortie-radiale-5-mm-1000-f-10-v-20-o-10-mm-1-pc-s-1475960.html)

## 5. Fonctionnement électrique

### 5.1. Alimentation

La carte reçoit une alimentation 5 V externe. Le courant des LED est fourni directement par cette alimentation et ne doit pas traverser les pistes d'alimentation de la carte ESP32.

La masse de l'alimentation LED et celle de l'ESP32 doivent être communes pour que le signal de commande ait une référence électrique correcte.

### 5.2. Commutation

Le MOSFET IRLML6344 commande simultanément le retour des huit branches LED. Lorsque le signal IR est actif, le MOSFET devient passant et le courant circule dans les huit branches. La modulation de la porteuse et les données IR sont produites par ESPHome sur le GPIO de commande.

### 5.3. Connecteur JST-XH

| Broche | Signal | Fonction |
|---:|---|---|
| 1 | GND | Masse commune ESP32 / alimentation |
| 2 | +5V | Alimentation de puissance des LED |
| 3 | IR | Signal logique 3,3 V issu du GPIO ESP32 |

Le câble côté ESP32 sera réalisé avec un connecteur JST-XH femelle et trois fils terminés par des broches Dupont femelles, selon le brochage retenu.

## 6. Estimation électrique préliminaire

Avec huit branches et une estimation de 166 mA par LED :

- courant de crête total théorique : environ 1,33 A ;
- puissance instantanée fournie aux branches : de l'ordre de 6,7 W sous 5 V ;
- dissipation instantanée dans chaque résistance de 22 Ω : environ 0,61 W.

Ces valeurs sont des estimations au niveau des impulsions, pas la consommation moyenne de la carte. La consommation moyenne dépend du rapport cyclique de la porteuse et du contenu des trames infrarouges.

Le connecteur, les pistes d'alimentation, les vias, le MOSFET et l'alimentation externe devront être dimensionnés en tenant compte du courant simultané des huit branches. Le courant réel sera mesuré à l'oscilloscope ou avec un dispositif de mesure adapté pendant les essais.

## 7. Connectique et intégration mécanique

- PCB circulaire de diamètre 60 mm.
- Huit LED disposées à 45° les unes des autres.
- LED orientées horizontalement vers l'extérieur.
- Pattes traversantes pliées à 90°.
- Boîtier imprimé en 3D avec logements de maintien des LED.
- Trou central de fixation M3.
- Connecteur JST-XH accessible après assemblage.

Le support mécanique doit éviter que les LED puissent pivoter ou que les efforts sur le boîtier soient transmis directement aux soudures.

## 8. Arborescence du module

- `carte-driver-ir/kicad/` : fichiers du projet électronique KiCad.
- `esphome/` : configuration ESPHome du module ESP32 de la salle.
- `boitier-3d/` : conception du boîtier, supports et fichiers d'impression 3D.

## 9. Vérifications avant fabrication

- [ ] Confirmer la limite de courant impulsionnel répétitif TSAL6200 pour la porteuse et le rapport cyclique réellement utilisés.
- [ ] Confirmer que les résistances 22 Ω / 1 W supportent les impulsions et la dissipation moyenne dans le boîtier.
- [ ] Mesurer le courant de chaque branche et le courant total.
- [ ] Vérifier le courant et l'échauffement du MOSFET IRLML6344.
- [ ] Vérifier le brochage exact du MOSFET et son empreinte SOT-23.
- [ ] Vérifier l'anode et la cathode des TSAL6200 et leur empreinte.
- [ ] Vérifier la polarité et l'empreinte du condensateur électrolytique.
- [ ] Dimensionner les pistes et le connecteur pour le courant de crête.
- [ ] Vérifier les niveaux logiques et la forme du signal GPIO à 3,3 V.
- [ ] Tester la portée et la couverture réelle dans une salle.
- [ ] Vérifier la température des composants lors de séquences d'émission répétées.

## 10. État du projet

Le choix des principaux composants est documenté. Les fichiers KiCad sont en cours de construction ; le schéma natif, la netlist, les empreintes et le PCB devront être vérifiés avant toute fabrication.

Cette carte est un prototype de développement et ne doit pas être considérée comme validée tant que les essais électriques, thermiques et de couverture IR ne sont pas terminés.
