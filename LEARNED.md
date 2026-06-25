# LEARNED — M2 TOOL V2

## Errori da non ripetere
- NON fare redesign completo in una sessione sola — dividere per moduli
- NON toccare logica JS esistente, solo HTML e CSS Tailwind
- NON usare scrollbar CSS custom (::-webkit-scrollbar) — su alcuni browser/OS diventa invisibile, meglio scrollbar nativa
- NON mettere bottoni dentro la toolbar dell'editor contenteditable — il blur ruba il click su mobile
- NON usare text-decoration:line-through su elementi contenteditable — il browser lo eredita sul testo digitato
- NON chiamare colorize/DOM rebuild su ogni input event — causa cursor jump. Solo su paste e blur (con delay)
- Per iniettare testo nell'editor da automazione/test browser: NON settare `editor-preview.innerText` (crea `.el-text` sbagliati → Elabora legge testo rotto → 0 saldature). Svuotare il ce (`innerHTML=''`) e settare `textarea#input-lista.value` (lo stesso flusso del pulsante "Esempio")

## Cose da ricordare
- "GAMBO" è un termine ferroviario (parte della rotaia), non un operatore — va in RESERVED e ignorato nelle righe figlie
- Il blur dell'editor contenteditable ruba il click ai bottoni vicini — servono setTimeout per dare priorità al click
- Parole di 2-5 lettere non riservate vengono parsate come sigla operatore — aggiungere a RESERVED se sono termini tecnici
- Il parser cerca `^km\s+(DX|SX)`. Se l'operatore scrive il modificatore PRIMA del lato (es. `11+718 ALL DX 4212 S2` invece di `11+718 DX ALL 4212 S2`), il regex NON matcha e la saldatura viene ignorata. Fix: pre-processing che normalizza `^km\s+(ALL|SCIN)\s+(DX|SX)` → `km DX/SX ALL/SCIN` prima del parse principale (zero modifiche alla logica parser)
- PDF download Android: usare `navigator.canShare` come unico discriminante è sbagliato — Chrome Android moderno espone Share API ma il menu "Condividi" confonde l'operatore. Discriminare con `isIOS = /iPhone|iPad|iPod/.test(navigator.userAgent) && !window.MSStream` e usare Share API solo su iOS, download diretto via `<a download>` su Android/desktop
- "Palo", "Suola", "Fungo", parole tecniche brevi: aggiungere a RESERVED del parser per non interromperle come "sigla operatore" durante il blocco figli del difetto

## Allineamento parser a M2 Diagnostica (25/06/2026)
- Il parser di M2 Tool e quello di M2 Diagnostica (`m2-diagnostica/frontend/js/parser.js`) sono LO STESSO motore. Per allineare M2 Tool si copiano i blocchi di Diagnostica (collaudati in campo) dentro `parse()` — solo additivi, niente riscritture. Risultato: parità 100% lato compilazione difetti/NO DAC (vedi STATUS 25/06)
- Tecnica di test (suite riutilizzabile in `_test-align/`, NON pushare): estrarre `parse()` da `index.html` (testo fra `const CODICI_DIFETTO` e `// ── ELABORA`) ed eseguirlo in Node con `new Function(code + '\nreturn parse;')()`. Stessa cosa per Diagnostica (fra `const CODICI_DIFETTO` e `// Classificazione righe`). Differential test = rete di sicurezza contro le regressioni. Validare la sintassi dell'intero `<script>` con `new Function` su tutto il blocco
- Come "Elabora" legge il testo: dal contenteditable `#editor-preview` via gli elementi `.el-text`; se non ce ne sono, usa `textarea#input-lista`
- `TR 45 DB 48.2` imposta sonda+guadagno ma NON fondo_scala (identico a Diagnostica). Per il fondo scala serve `Sonda 45` o un angolo "standalone" tipo `45 - 42 dB`
- Angolo "nudo" su una riga (es. la sola `70`) è IGNORATO (regex `^\d{1,3}$` in isIgnorable lo intercetta prima) → non imposta sonda. Limite condiviso con Diagnostica, non un bug
- La Sede Tecnica viene salvata in minuscolo (es. "lembo esterno") come in Diagnostica
- localStorage M2 Tool: `m2_firme` = firme salvate (MAI cancellare in test), `m2_last_input` = ultima lista incollata, `m2d.headlamp` = impostazione
