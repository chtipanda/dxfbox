# 🪵 DwgBox — Générateur Boîte CNC

Générateur paramétrique de boîtes en bois pour CNC 3 axes.
Export DXF compatible **Aspire (Vectric)**, **CamBam**, **Krabscam**.

**→ [Ouvrir l'application](https://chtipanda.github.io/dxfbox/)**
**→ [Lire l'article de présentation](https://chtipanda.github.io/dxfbox/article.html)**

---

## Fonctionnalités

- **4 formes** : rectangulaire, ronde, hexagonale, et **personnalisée** (contour importé depuis un fichier DXF)
- **4 types d'emboîtement** : recouvrement extérieur, recouvrement interne, épaulement externe, épaulement interne
- Calcul automatique du **déport ball-nose** (fond de boîte)
- **Jeu fonctionnel** paramétrable, appliqué sur les pièces qui s'emboîtent
- Détection d'erreurs en temps réel, adaptée à l'emboîtement choisi (mur restant ou jupe trop mince, ball-nose trop grand, couvercle trop bas, cavité vide…)
- Aperçu des deux pièces (vue dessus) et pictogrammes en coupe pour chaque emboîtement
- Export **DXF AC1009** (R12) multi-couches, en lignes, arcs et cercles (ni polylignes ni splines)
- **Notice d'usinage** générée selon les réglages
- **PWA** — fonctionne hors ligne après le premier chargement

## Les 4 emboîtements

| Emboîtement | Base | Couvercle |
|---|---|---|
| **Recouvrement extérieur** | Boîte simple | Plus grand que la base de `gW + jeu` de chaque côté ; une jupe de `gW` d'épaisseur enveloppe la base par l'extérieur (poche de profondeur `gD`) |
| **Recouvrement interne** | Boîte simple | Même contour que la base ; un tenon plein, à `e + jeu` du bord, entre dans la cavité (anneau extérieur usiné sur `gD`) |
| **Épaulement externe** | Feuillure sur le bord extérieur du mur (largeur `gW + jeu`, profondeur `gD`) | Même contour que la base ; une lèvre de `gW` (= l'épaulement) repose sur la feuillure. Le couvercle est jointif : le jeu est sur le côté de la lèvre et au-dessus de la base |
| **Épaulement interne** | Feuillure sur le bord intérieur du mur (à `e − gW` du bord, profondeur `gD`) | Même contour que la base ; un tenon plein, à `e − gW + jeu` du bord, se pose dans la feuillure |

## Paramètres

| Paramètre | Rôle |
|---|---|
| Dimensions L / l / H | Dimensions extérieures de la boîte fermée (en mm) |
| Ratio base / couvercle | Répartition de la hauteur entre les deux pièces |
| Épaisseur paroi `e` | Épaisseur des murs de la base |
| Ø ball-nose | Fraise du fond ; le chemin du fond est déporté de Ø/2 vers l'intérieur |
| Ø fraise contour | Fraise de découpe et de poches ; pour une forme DXF, son rayon fixe l'arrondi minimal des angles |
| Largeur rainure / épaulement `gW` | Largeur de la rainure, ou épaisseur de la lèvre pour l'épaulement externe |
| Profondeur rainure `gD` | Profondeur de la rainure, de la feuillure ou de la poche |
| Jeu fonctionnel | Jeu entre les pièces qui s'emboîtent |

## Forme personnalisée (DXF)

Choisir **Personnalisée (DXF)** dans la liste « Forme », puis importer un fichier.

- **Éléments lus** : polylignes (avec arcs), traits, arcs et cercles. Les traits et arcs sont assemblés en contours s'ils se rejoignent.
- **Contour retenu** : le plus grand contour fermé du fichier. Les autres éléments (traits ouverts, autres contours, splines) sont ignorés.
- **Dimension** : le champ « Largeur de la forme (L) » met le dessin à l'échelle ; la hauteur suit les proportions du dessin. Les unités du fichier importent peu.
- **Angles adaptés à l'usinage** : les angles saillants et rentrants sont arrondis au rayon de la fraise (Ø fraise contour ÷ 2), et les contours intérieurs sont calculés pour qu'une fraise ronde puisse les suivre. Les jeux entre les pièces restent respectés autour des arrondis.
- **Limites** : un seul contour, sans trous ; sur une forme très étroite ou très fine, une paroi trop épaisse donne « Cavité vide ».

## Couches DXF

| Couche | Contenu |
|---|---|
| `CONTOUR_EXT` | Contour extérieur — découpe |
| `CAVITE_INT` | Cavité intérieure — poche |
| `FOND_BALLNOSE` | Chemin du fond déporté de Ø/2 — ball-nose |
| `RAINURE_EXT` | Bord de la feuillure (épaulement interne) |
| `RAINURE_INT` | Bord de la rainure, de la feuillure, de la poche ou du tenon, selon l'emboîtement |

Le détail de l'usinage (ordre des opérations, profondeurs) est donné dans l'onglet **Notice** de l'application.

## Déploiement local

Aucune dépendance. Ouvrir `index.html` dans un navigateur.
Pour le Service Worker (PWA), servir via HTTPS ou `localhost`.

```
# Serveur local rapide
python3 -m http.server 8080
# puis ouvrir http://localhost:8080
```

## Structure du dépôt

```
dxfbox/
├── index.html      Application principale
├── article.html    Article de présentation
├── sw.js           Service Worker (cache hors ligne)
├── manifest.json   Manifest PWA
├── icon-192.svg    Icône 192×192
├── icon-512.svg    Icône 512×512
├── LICENSE         Licence GPL-2.0
└── README.md       Ce fichier
```

## Licence

GPL-2.0 — voir le fichier `LICENSE`.

---

*Par [chtipanda](https://github.com/chtipanda)*
