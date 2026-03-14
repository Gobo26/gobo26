# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Projecte

**Gobo26** és una col·lecció de jocs i eines web en català, sense dependències externes, desplegada a GitHub Pages. Tot el codi de cada aplicació viu en un únic fitxer `.html` que inclou HTML, CSS i JavaScript junts.

## Desplegament

Qualsevol `git push` a la branca `main` dispara automàticament el workflow `.github/workflows/deploy.yml`, que publica el contingut a GitHub Pages via GitHub Actions. No cal cap pas de build ni compilació.

```bash
git add <fitxers>
git commit -m "descripció del canvi"
git push
```

## Arquitectura

### Estructura de fitxers

- `index.html` — pàgina principal amb el llistat de totes les apps
- `cat-*.html` — pàgines de categoria (`cat-paraules.html`, `cat-numeros.html`, `cat-memoria.html`, `cat-eines.html`, `cat-aventures.html`)
- `Gobo26-NOM.html` — cada aplicació/joc (un fitxer autònom per app)
- `manifest.json` — manifest PWA (l'app és instal·lable com a PWA)
- `icon-192.png`, `icon-512.png` — icones de la PWA

### Convencions de cada fitxer d'aplicació

Cada `Gobo26-*.html` segueix aquest patró:

1. **CSS amb variables CSS** a `:root` per a colors del disseny (paleta Slate de Tailwind: `--s800: #1e293b`, `--blue: #2563eb`, etc.)
2. **Font**: `Montserrat` (Google Fonts), pesos 400/600/700/800/900
3. **Header** estàndard: fons `#1e293b`, logo Gobo26 (hexàgon SVG) que enllaça a `https://ja.cat/appgobo`, títol centrat i acció a la dreta
4. **JavaScript vanilla** sense frameworks ni imports. Es barreja `const`/`let` amb `var` (acceptat)
5. **Persistència**: `localStorage` amb claus prefixades per app (ex: `3r_partides`, `intrus_punts`)
6. **Sense peticions de xarxa en temps d'execució**: les dades (llistes de paraules, preguntes, etc.) estan incrustades directament al JavaScript del fitxer

### Disseny visual consistent

- Fons de pàgina: `#f1f5f9`
- Targetes/panells: `background: white`, `border-radius` entre 10-20px, ombres suaus
- Color principal d'acció: `#2563eb` (blau)
- Estats: verd `#16a34a` (encert), vermell `#dc2626` (error), groc/ambre per alertes
- Animacions: `popIn` i `slideUp` definides localment a cada fitxer

## Registre de canvis importants

Quan es facin canvis significatius al projecte (nova app, canvi d'arquitectura, nova convenció visual, modificació del sistema de desplegament, etc.), afegeix una entrada aquí:

| Data | Canvi |
|------|-------|
| 2026-03-14 | Configuració inicial: git, SSH, GitHub Actions deploy a GitHub Pages |

---

## Notes importants

- El disc USB genera fitxers `._*` de macOS. Estan ignorats via `.gitignore`. Els errors `non-monotonic index` que apareixen amb `git push` són advertències innocues d'aquest disc i no afecten el resultat.
- No hi ha servidor local ni sistema de build. Les apps s'obren directament al navegador com a fitxers locals per fer proves, o a través de GitHub Pages en producció.
