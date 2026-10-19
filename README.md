# Émulateur KIM Uno (KIM-1 en navigateur)

Un émulateur du microordinateur **KIM-1** (processeur 6502) qui tourne directement dans le navigateur, avec une interface inspirée du **KIM Uno**, la version miniature du KIM-1 : afficheurs 7 segments rouges, circuit imprimé noir et touches tactiles.

## Contexte

J'ai créé cet émulateur pour les besoins d'une vidéo. Je voulais une interface plus réaliste qu'une simple grille de boutons, tout en restant simple et lisible à l'écran :

- des touches avec un petit retour visuel (le capuchon s'enfonce et devient **rouge** quand on appuie), bien visible en vidéo ;
- les deux derniers chiffres de l'afficheur (les données) en **vert**, pour les distinguer de l'adresse ;
- l'utilisation du **clavier du PC** pour taper les adresses et les codes.

## Origine du projet

Cet émulateur est basé sur l'émulateur en ligne de Maksim Korzh :
<https://maksimkorzh.github.io/KIM-1/>

Le cœur de l'émulation (processeur 6502, mémoire, ROM du KIM-1, assembleur) vient de ce projet. Je n'ai modifié que l'interface de la version mobile (`mobile.html`), devenue `kim-uno.html`. Tout le mérite de l'émulation revient à son auteur.

L'affichage 7 segments utilise la bibliothèque *LED Segment Display Simulator* de Gavin Lyons, écrite pour p5.js, et déjà présente dans le projet d'origine.

## Utilisation

1. Place `index.html` à la racine du projet, à côté des dossiers `js/` et `css/` de l'émulateur d'origine. Ce fichier charge les mêmes scripts que `mobile.html`.
2. Ouvre `index.html` dans un navigateur.

Les deux boutons sous le clavier, `IMPORT PAPER TAPE` et `EXPORT PAPER TAPE`, servent à charger et sauvegarder des programmes (voir plus bas). `FULLSCREEN` passe en plein écran et affiche le moniteur série.

## Raccourcis clavier

Chaque touche du PC déclenche exactement la même action que la touche à l'écran.

| Touche du PC | Touche du KIM Uno |
|---|---|
| `0` à `9`, `A` à `F` | chiffres hexadécimaux |
| `Espace` | **AD** (adresse) |
| `,` | **DA** (donnée) |
| `Entrée` ou `+` | **+** (adresse suivante) |
| `G` | **GO** |
| `S` | **ST** (stop) |
| `R` | **RS** (reset) |
| `P` | **PC** |

Remarques :
- Une seule touche est prise en compte à la fois, et la répétition automatique des touches est ignorée.
- Les raccourcis comme `Ctrl+C` ou `Ctrl+V` ne sont pas interceptés.
- Quand le curseur est dans la zone de texte de la bande papier, le clavier sert à écrire du texte normalement.
- **SST** est un interrupteur : il active le mode pas à pas.

## Charger un programme (bande papier)

Colle le programme dans la zone de texte sous les boutons, puis clique sur `IMPORT PAPER TAPE`. Deux formats sont acceptés, et ils peuvent être mélangés.

**Format simple.** `@` suivi de l'adresse (hexa), puis les octets. Les espaces sont facultatifs. Une ligne qui ne contient que des octets continue à l'adresse suivante.

```
@0200 A9 42 85 F9 20 1F 1F 4C 00 02
```

Ce programme affiche `42` sur les deux chiffres de droite. Pour le lancer : `AD`, `0 2 0 0`, puis `GO`.

**Format standard MOS Technology.** C'est le format de la bande papier du vrai KIM-1 :

```
;LLAAAADDDD...DDDDCCCC
```

- `LL` : nombre d'octets de la ligne, en hexadécimal (par exemple `18` = 24 octets) ;
- `AAAA` : adresse de départ ;
- `DD...` : les octets du programme ;
- `CCCC` : somme de contrôle (facultative à l'import).

Une ligne `;00...` marque la fin de la bande. Elle est facultative.

## Sauvegarder un programme

Clique sur `EXPORT PAPER TAPE`, saisis l'adresse de début puis l'adresse de fin **en hexadécimal avec le préfixe `0x`** (par exemple `0x0200`). Sans `0x`, le nombre est lu en décimal. La bande papier au format standard apparaît dans la zone de texte.

## Licence

Licences du projet d'origine de Maksim Korzh, ainsi que celles de p5.js, jQuery et Bootstrap.
