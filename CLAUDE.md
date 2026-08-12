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
| 2026-06-11 | Retirat el bàner superior global `gobo-global-menu` de totes les pàgines (i dels repositoris Taller_de_lletres i Textos26): a mòbil ocupava la part alta i tapava contingut; la navegació entre espais es fa des de la portada |
| 2026-07-01 | **Nova app: Gobo26-MURDOKU.html** — misteri deductiu tipus Murdle. Generador automàtic de casos amb **solució única garantida** (resolutor intern que compta solucions ≤2 sobre permutacions; pistes generades i podades fins a unicitat, validat amb 150 casos). 3 nivells (Fàcil 4×4, Mitjà 5×5, Difícil 6×6), 4 escenaris rotatius (mansió, hotel, creuer, teatre), interacció tocar-per-col·locar amb ressaltat de conflictes fila/columna, 📅 Repte del dia determinista (`mulberry32(seedData)`), rècords i ratxa (`murdoku_records`/`murdoku_diari`), partida desada (`murdoku_partida`), so WebAudio + botó silenci, pista, confeti i modal de resolució. Afegida targeta a index.html (Jocs i Memòria: 19→20 apps) |
| 2026-07-02 | **Nova app: Gobo26-HOROSCOP.html** — Horòscop 100% positiu (vitalitat, esperança, bon rotllo; res de tètric, per petició expressa de l'usuari). 4 pestanyes: *Els signes* (fitxes dels 12 signes: dates, element foc/terra/aire/aigua, personalitat, capacitats, afinitats, gustos, color i com cuidar-se), *Avui* i *La setmana* (horòscop determinista per data+signe amb `mulberry32` — el mateix per a tothom; blocs d'energia, cor, salut física, salut mental, número i color de la sort; setmana partida en inici/mig/cap de setmana), i *Prediccions* (predicció del dia + botó de prediccions a demanda, totes encoratjadores). Cada horòscop porta una **🎵 nota divertida per a cantaires i músics** (els amics de l'usuari són cantaires de corals). Signe triat desat a `horoscop_signe`. Targeta a index.html (Eines i Curiositats: 7→8 apps) |
| 2026-07-02 | **Murdoku: la pista mai revela l'assassí.** El botó 💡 ara és graduat: primer assenyala un sospitós mal col·locat (sense dir on va), després revela l'habitació d'un sospitós que mai és l'assassí, i quan només queda l'assassí diu «dedueix-ho tu, detectiu». El generador tampoc emet cap pista directa amb la columna exacta de l'assassí (només es pot deduir; verificat amb 300 casos + 150 de unicitat). «Comprova» ara diu quants sospitosos són ben col·locats; comptador d'ajudes usades persistit i mostrat al modal de victòria |
| 2026-07-02 | **Maons més jugable**: si cau la pilota principal amb multibola activa, una pilota extra passa a ser la principal (abans es perdien totes i la vida igualment); la bola de foc travessa els maons sense rebotar; la bola normal només trenca un maó per fotograma (abans podia travessar files); la bola lenta 🐢 ara redueix la velocitat de debò (factor 0,6); velocitat constant per nivell amb correcció anti-trajectòria-horitzontal; patrons de maons per nivell (mur/piràmide/escacs/columnes); el HUD mostra fins a 5 cors (la vida extra no es veia); el missatge «Nou rècord» ara surt (el rècord s'actualitzava abans de comparar); botó de silenci 🔊/🔇 (`maonsSo`) |
| 2026-07-02 | **Nova app: Gobo26-INVASORS.html** — Space Invaders clàssic: onades d'invasors (emojis per files, punts 30/20/10) que s'acceleren quan en queden pocs, 4 refugis destructibles cel·la a cel·la, bombes enemigues, OVNI 🛸 de bonificació (50-200 pts), nau amb invulnerabilitat breu després d'impacte, controls ratolí/teclat/tàctil (arrossegar mou i el dit manté foc automàtic), rècord `invasorsRecord`, so WebAudio + silenci (`invasorsSo`), vibració i confeti. Targeta a index.html (Jocs i Memòria: 20→21 apps) |
| 2026-07-02 | **Bonoloto: sorteig ponderat per tendència** a petició de l'usuari — cada número (1-49) pesa segons les aparicions a la taula oficial de La Primitiva del 2025 i el 2026 (arrays `APARICIONS_2025`/`APARICIONS_2026`, consultada el 02-07-2026); tria sense reemplaçament amb `crypto.getRandomValues`. La línia d'informació mostra les aparicions totals de la combinació; avís, modal ❓ i targeta d'index.html expliquen que «segueix la ratxa» sense canviar la probabilitat real. Distribució validada amb 50.000 mostres |
| 2026-07-02 | **Bonoloto: atzar pur** a petició de l'usuari (en dos passos: primer el filtre del rang de dates 1-31, després tota la resta) — retirats tots els filtres anti-popularitat (`esPocPopular`) i la regla de no repetir números entre combinacions; ara cada combinació és `crypto.getRandomValues` directe sobre el bombo de 49. Textos actualitzats (avís, modal ❓, meta description, targeta d'index.html); la línia d'informació manté suma i parells/senars com a curiositat |
| 2026-07-02 | **Nova app: Gobo26-BONOLOTO.html "La Sort"** — generador honest de combinacions per a Bonoloto i La Primitiva (amb reintegrament). Atzar criptogràfic (`crypto.getRandomValues`) + filtres anti-popularitat (màx. 3 números ≤31, sense 3+ consecutius, mescla parells/senars, sense tots múltiples ni tots acabats igual, suma 100–200): no canvia la probabilitat de guanyar (l'avís i el modal ❓ ho diuen clarament) però redueix el risc de compartir el pot. D'1 a 8 combinacions sense repetir números entre elles, cost del butlletí, copiar al porta-retalls, preferències a `loto_joc`/`loto_quantes`. Targeta a index.html (Eines i Curiositats: 8→9 apps) |
| 2026-06-11 | **Aventures interactives revisades a fons** (RETRAT, CORAL, RADIO): coherència narrativa garantida en tots els recorreguts — finals reescrits perquè no narrin fets d'altres branques, personatges presentats abans de ser anomenats, pistes òrfenes resoltes (beina de la Coral, partitura del faristol), salts temporals i lògics corregits (càmera del corredor, hores, qui actua durant l'emissió). Fonts pròpies que mai es carregaven ara sí (Cinzel/Crimson Text al Retrat; Playfair/Lora a Coral i Ràdio) i millores visuals: caplletra a la primera línia, animació d'entrada de l'art, subratllat decoratiu del títol, reflex al passar per les tries i brillantor segons el tipus de final |
| 2026-08-12 | **Repositori Textos26 retirat de GitHub** a petició de l'usuari (menys exposició pública de textos personals): `Gobo26/Textos26` esborrat de GitHub, còpia sencera conservada localment a `~/Textos26`. Tret l'enllaç/targeta de Textos26 del portal (`index.html`: menú, secció Creació —ara "2 espais"—, footer) i de `README.md` |

---

## Notes importants

- El disc USB genera fitxers `._*` de macOS. Estan ignorats via `.gitignore`. Els errors `non-monotonic index` que apareixen amb `git push` són advertències innocues d'aquest disc i no afecten el resultat.
- No hi ha servidor local ni sistema de build. Les apps s'obren directament al navegador com a fitxers locals per fer proves, o a través de GitHub Pages en producció.
