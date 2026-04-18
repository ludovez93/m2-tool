# STATUS — M2 TOOL V2

## Ultimo aggiornamento
18 aprile 2026

## Stato attuale
- V2 live e funzionante
- Sessione 18/04: 2 fix critici dopo segnalazioni dal collega CA in campo
  1. Parser ora accetta formato `km ALL/Scin DX/SX` (modificatore prima del lato) — pre-processing 2 righe in `parse()`
  2. PDF download su Android: forzato download diretto invece di Share API (che apriva menu "Condividi" confondendo l'operatore). Discriminato con `isIOS` UA detect

## Sessione precedente (2 aprile 2026)
- Fix: GAMBO nel parser, cursore editor, scrollbar desktop, blur/elabora, strikethrough, bottone Elabora sopra editor

## Cosa fatto oggi
- Fix: GAMBO aggiunto a RESERVED e ignorato nelle righe figlie (non finisce in note TSV)
- Fix: rimosso scheduleRender() da input handler (cursore saltava una riga sopra durante digitazione)
- Fix: rimosso text-decoration:line-through da .el-text.skip (testo barrato quando si scriveva)
- Fix: rimossa scrollbar custom CSS, ora usa scrollbar nativa browser (visibile su desktop)
- Fix: blur colorize ritardato 200ms per evitare che il primo tap su Elabora venisse perso
- Aggiunto bottone "Elabora lista" sopra l'editor per accesso rapido

## Prossimo step
- Da definire
