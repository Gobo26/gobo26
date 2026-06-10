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
3. **Header** estàndard: fons `#1e293b`, logo Gobo26 (hexàgon SVG) que enllaça a `https://gobo26.github.io/gobo26/`, títol centrat i acció a la dreta
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
| 2026-03-16 | Nova app: Gobo26-Herbes.html — guia d'herbes mediterrànies amb oracle, saviesa de les àvies, fitxes de 25 plantes amb tradicions i rituals |
| 2026-03-17 | Nova app: Gobo26-DOMINO.html — joc del dominó (jugador vs màquina), lògica completa, interfície Gobo26 estàndard |
| 2026-03-18 | Nova app: Gobo26-Tarot.html — Santuari de Tarot, tirades d'1 i 3 cartes dels Arcans Majors, diari de lectures (localStorage), diccionari de les 22 cartes |
| 2026-06-10 | Auditoria de jugabilitat de les 30 apps (AUDITORIA.md) amb pla de millora per fases |
| 2026-06-10 | Dominó refet (Fase 1): pou per robar fitxes, 3 nivells de dificultat de la màquina, rècords i ratxes, partida desada automàticament (localStorage `domino_*`), so WebAudio + vibració amb botó de silenci, confeti i modal de resultat, inici aleatori |
| 2026-06-10 | Sudoku millorat (Fase 1): partida desada automàticament i recuperada en obrir (localStorage `sudoku_*`), millors temps per nivell amb "nou rècord" al modal, victòria automàtica en completar (sense prémer Comprovar), so WebAudio + botó de silenci, vibració, confirmació abans de descartar partida, cronòmetre pausat quan la pestanya s'amaga |
| 2026-06-10 | Dites Inacabades millorat (Fase 1): persistència total (localStorage `dites_*`) — millor ratxa per nivell, comptadors d'encerts/errades/bromes picades, dites vistes no es repeteixen entre sessions; rècords visibles a l'inici i durant el joc amb celebració de nou rècord; corregit el botó d'ajuda que no existia (modal inaccessible) i el CSS de la insígnia daurada; botó de silenci |
| 2026-06-10 | Fase 2 (4 jocs): **Paraula Secreta** — so+vibració+confeti, animació de gir de cel·les (existia però no s'usava), nivell Fàcil ara té 7 intents (era idèntic al Mitjà). **Pinta Paraules** — botó d'ajuda recuperat (modal inaccessible), sistema d'intents per pantalla arreglat (el temps esgotat tornava a l'inici directament, contradient l'ajuda), modal de fi de partida que mai es mostrava ara s'usa, missatges d'error corregits per mode fàcil. **Anagrames** — progressió de dificultat (grups de 4→5→6 lletres), xip mort del marcador ara mostra el rècord. **3 en Ratlla** — sons diferenciats guanyar/perdre/empat, so de fitxa, vibració. Tots quatre: botó de silenci 🔊/🔇 persistent |
| 2026-06-10 | Fase 3 — Repte del dia: **Paraula Secreta** té el nivell "📅 Repte del dia" (paraula determinista a partir de la data, idèntica per a tothom, una oportunitat al dia, progrés desat a `ps_repte`, es pot reprendre a mig fer). **Sudoku** té el botó "📅 Diari" (generació determinista amb mulberry32 i llavor de la data, 45 forats, ratxa de dies consecutius a `sudoku_diari` mostrada al modal de victòria). Patró reutilitzable: `mulberry32(any*10000+mes*100+dia)` |
| 2026-06-10 | Fase 4 — Portada viva: panell "✦ El teu racó" a index.html que llegeix el localStorage compartit dels jocs (mateix origen GitHub Pages, sense tocar cap app): estat dels reptes del dia (paraula + sudoku), enllaços "continua la partida" si hi ha `domino_partida`/`sudoku_partida` desades, i insígnies de rècords (Dominó, Sudoku, Dites, Paraula Secreta, Pinta Paraules, Anagrames, 3 en Ratlla). Tot es construeix amb JS en carregar; si no hi ha dades només es mostren els 2 reptes |
| 2026-06-10 | Retirat el panell "✦ El teu racó" de index.html (Fase 4) a petició de l'usuari: eliminats el CSS `.raco*`, el bloc HTML i el script que el construïa. Els reptes del dia i els rècords continuen funcionant dins de cada joc |
| 2026-06-10 | **Esborrada** Gobo26-GIMNAS-CASA.html a petició de l'usuari (targetes tretes d'index.html i cat-eines.html; comptador "Eines i Curiositats" passa de 8 a 7 apps) |
| 2026-06-10 | **Tarot renovat a fons**: tirades temàtiques (General/Amor/Feina i diners/Benestar — cada arcà té text per tema), nova tirada "Pregunta de Sí o No" (cada arcà té resposta si/no/matís), l'usuari escull les cartes d'un ventall de 22 cartes de revés (en lloc de repartiment automàtic), barreja amb barra de progrés, carta del dia que es gira amb un toc (desada a `tarot_dia`), so WebAudio + botó de silenci (`tarot_so`), diari que guarda tema i interpretacions senceres amb esborrat individual d'entrades (retrocompatible amb el format antic), diccionari amb els significats per tema |
| 2026-06-10 | **Herbes ampliades de 23 a 43 plantes** (sajolida, hisop, donzell, eucaliptus, te de roca, marialluïsa, valeriana, passiflora, rosella, saüc, roser silvestre, estragó, cibulet, all, safrà, pebre negre, gingebre, cúrcuma, nou moscada, ginebró) + cercador per nom amb normalització d'accents + comptador de resultats + 8 dites noves |
| 2026-06-10 | Arranjaments mòbil. **Paraula Secreta**: el teclat viu fora del `<main>` (que és l'única zona amb scroll) i queda sempre visible a baix; `100dvh` al body, media queries per alçada (`max-height` 780/660px) que compacten cel·les i teclat, footer mogut dins del main. **Dominó**: les fitxes de la mà ara són `<button>` natius (els `<div>` amb onclick no responien al toc en alguns mòbils), `:hover` de les fitxes només sota `@media(hover:hover)` + estat `:active` per al toc, `touch-action:manipulation` als controls, helpers `lsGet/lsSet/lsDel` amb try/catch (si Safari bloqueja localStorage, una excepció a `guardarPartida` deixava el torn penjat a la màquina i cap clic feia res), i els indicadors ←/→ de les fitxes jugables ara es veuen (abans quedaven retallats per `overflow:hidden`) |

---

## Notes importants

- El disc USB genera fitxers `._*` de macOS. Estan ignorats via `.gitignore`. Els errors `non-monotonic index` que apareixen amb `git push` són advertències innocues d'aquest disc i no afecten el resultat.
- No hi ha servidor local ni sistema de build. Les apps s'obren directament al navegador com a fitxers locals per fer proves, o a través de GitHub Pages en producció.
