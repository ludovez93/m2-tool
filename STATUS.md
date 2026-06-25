# STATUS — M2 TOOL V2

## Ultimo aggiornamento
25 giugno 2026

## Stato attuale
- V2 live e funzionante
- **25/06: parser ALLINEATO AL 100% a M2 Diagnostica.** Modificata SOLO la funzione `parse()` in `index.html` (scheda/Excel/TSV/grafica intatte). Portati additivamente TUTTI i blocchi di Diagnostica: RESERVED esteso, classifica `(?:^|\s)([SAB][12]?)`, "N" da sola = NO DAC, sede estesa (suola/gambo/fungo/lembo/piano sup-inf inline+figlia+angolo), profondità (`profondità|prof|prod`), ampiezza (`hs|altezza`), Sonda/Fondo scala/angolo standalone, note anti-rumore + picchetto/ponte, punzone su riga separata + slash + ditta-prima, nota CC ristretta, **+ rami strutturali**: deviatoi (`DEV N -` e formato `nr km`), doppia fila `SX DX`, binario "interconnessione", skip "non controllabile/non smerigliata", nrSald `N16`, GR→nota "Regolazione". Backup `index.html.bak-pre-parser-align-2026-06-25`. **Test (suite in `_test-align/`, NON pushare): confronto TOTALE su tutti i campi 41/41 identici a Diagnostica, stress 69/69, harness 24/24 + non-regressione 7/7 + completezza 29/29. Sintassi OK.** Non portati (assenti in M2 Tool by design, non confrontati): `raw_text`, `lav_tipo`, strumento DX/SX, operatore inline, contesto lavorazioni/regolazioni. **PUSHATO: commit `7e6ade0` su `main` (solo `index.html` + `STATUS.md`; esclusi `_test-align/` e `*.bak`) → LIVE su GitHub Pages.** **PROVA DEL VIVO SUPERATA (25/06, browser PC):** incollata lista test → 4 saldature (2 conformi/1 difetto/1 NO DAC), scheda difetto 4212 con sede=lembo esterno, sonda=FTA45-2, guadagno=48.2, W=12, prof=3, S2; anteprima modulo I.1 OK. Prova poi rimossa (tolto solo `m2_last_input` da localStorage, firma `m2_firme` intatta).
- **19/06: nuovo logo aziendale «m2 Performing Rails» nelle schede difetto (scheda I.1).** Sostituito lo sfondo base64 della scheda (2 occorrenze in `index.html`: anteprima editor + PDF) → il cerchio azzurro «m2 railgroup» è stato rimpiazzato col nuovo logo, dimensione = stesso ingombro del vecchio (h≈85px), centrato; RFI / «Scheda Nr.» / «DPR MO SE 01 1 0» / riga I.1 intatti. Stessa identica immagine usata anche in M2 Diagnostica (sfondo md5-identico tra i due progetti). Solo ASSET, nessuna logica toccata. Backup `index.html.bak-pre-logo-2026-06-19`. Commit `3af6d07` push su `main` → GitHub Pages.
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
- (Opzionale) prova su iPhone in campo con una lista-notte vera, per conferma finale sul dispositivo reale.
- Allineamento parser COMPLETO e live: niente di pendente lato compilazione difetti/NO DAC.
