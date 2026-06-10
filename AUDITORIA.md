# Auditoria de jugabilitat i atractiu — Gobo26

*Data: 10 de juny de 2026*

## Resum general

La col·lecció té una base molt sòlida: disseny coherent (capçalera, paleta Slate, Montserrat), PWA instal·lable, 30 apps autònomes sense dependències. La diferència de qualitat entre apps és gran: les més recents (La Botiga, L'Oncle Albert, Enfilall, L'Intrús, Endevina!, Cascada Numèrica) ja tenen so, vibració, nivells, rècords i celebracions; les més antigues o senzilles no tenen res d'això.

## Diagnòstic per app (jocs)

| App | Punts forts | Mancances principals | Prioritat |
|---|---|---|---|
| **Dominó** | Lògica completa, SVG de fitxes | Sense so, sense rècords, sense nivells de dificultat, sense persistència, gairebé sense animacions | 🔴 Alta |
| **Sudoku** | 3 nivells, desfer, notes, pistes | **No desa la partida** (es perd en tancar), sense so, sense millors temps | 🔴 Alta |
| **3 en Ratlla** | Nivells d'IA, persistència | Sense celebracions ni vibració, rècords mínims | 🟡 Mitjana |
| **Dites Inacabades** | Bon disseny sèpia, nivells | **Sense localStorage**: ratxes i progrés es perden | 🔴 Alta |
| **Paraula Secreta** | Estadístiques, 3 nivells, teclat | Sense so ni vibració ni celebració final | 🟡 Mitjana |
| **Pinta Paraules** | So i vibració | Sense nivells de dificultat ni progressió | 🟡 Mitjana |
| **Anagrames** | So, rècords | Un sol nivell, poca progressió | 🟡 Mitjana |
| **Mestre Joier** | Nivells, persistència | Sense vibració ni celebracions | 🟢 Baixa |
| Amagades, Endevina!, Intrús, Cascada, Repte Numèric, Col·lect, Enfilall, Mercatus, Maons, La Botiga, Oncle Albert, Morse | Complets (so + nivells + rècords) | Retocs menors | 🟢 Baixa |

## Diagnòstic per app (aventures i eines)

| App | Observacions |
|---|---|
| **Aventures (Retrat, Coral, Ràdio)** | Tenen àudio i persistència. Millora possible: més feedback en triar (transicions), recompte de finals descoberts visible com a col·lecció. |
| **Cançoner SDM** | Estàtic (198 línies). Milloraria amb cercador i índex clicable. |
| **Gimnàs a Casa** | Té rècords i celebracions; falta so de temporitzador. |
| **Tarot, Herbes** | Contingut ric; polit visual menor. |
| **Tensió Arterial, Historial Mèdic, Suma de Temps** | Eines funcionals; fora de l'àmbit "jugabilitat". |

## Mancances transversals (afecten moltes apps)

1. **Partida desada a mig fer** — Sudoku i Dominó perden la partida en tancar la pestanya.
2. **Rècords i ratxes desiguals** — alguns jocs en tenen, altres no; cap els celebra de manera destacada.
3. **So inconsistent** — 9 apps en tenen de complet, la resta poc o gens.
4. **Sense "Repte del dia"** — el motiu més potent perquè la gent torni cada dia no existeix enlloc.
5. **Portada (index.html)** — no mostra res personal (últim joc jugat, rècords); és un llistat estàtic.

## Pla proposat (per fases)

### Fase 1 — Els tres clàssics que coixegen 🔴
- **Dominó**: so, nivells de dificultat de la màquina, rècords i ratxes, persistència de partida, animacions de col·locació de fitxes, celebració de victòria.
- **Sudoku**: desar partida automàticament, millors temps per nivell, so suau, celebració en completar.
- **Dites Inacabades**: persistència de ratxes i progrés, pantalla de resultats més festiva.

### Fase 2 — Polir els jocs de paraules 🟡
- **Paraula Secreta**: so, vibració, celebració amb confeti, compartir resultat.
- **Pinta Paraules**: nivells de dificultat i progressió.
- **Anagrames**: més nivells, ratxa diària.
- **3 en Ratlla**: celebracions, vibració.

### Fase 3 — Capa comuna de "tornar-hi cada dia"
- **Repte del dia**: una paraula/partida diària idèntica per a tothom en 2-3 jocs estrella.
- Harmonitzar so + vibració + confeti a tots els jocs que en falten.

### Fase 4 — Portada viva
- index.html amb "continua jugant" (últim joc), rècords personals visibles i insígnies per joc completat.

## Estimació d'esforç

Cada app de la Fase 1 és una sessió de treball moderada (els fitxers tenen 600-1100 línies i es modifiquen sense reescriure'ls de zero). Les fases 2 i 3 són canvis més petits i repetitius. La Fase 4 és independent i es pot fer en qualsevol moment.
