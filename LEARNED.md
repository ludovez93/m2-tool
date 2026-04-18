# LEARNED — M2 TOOL V2

## Errori da non ripetere
- NON fare redesign completo in una sessione sola — dividere per moduli
- NON toccare logica JS esistente, solo HTML e CSS Tailwind
- NON usare scrollbar CSS custom (::-webkit-scrollbar) — su alcuni browser/OS diventa invisibile, meglio scrollbar nativa
- NON mettere bottoni dentro la toolbar dell'editor contenteditable — il blur ruba il click su mobile
- NON usare text-decoration:line-through su elementi contenteditable — il browser lo eredita sul testo digitato
- NON chiamare colorize/DOM rebuild su ogni input event — causa cursor jump. Solo su paste e blur (con delay)

## Cose da ricordare
- "GAMBO" è un termine ferroviario (parte della rotaia), non un operatore — va in RESERVED e ignorato nelle righe figlie
- Il blur dell'editor contenteditable ruba il click ai bottoni vicini — servono setTimeout per dare priorità al click
- Parole di 2-5 lettere non riservate vengono parsate come sigla operatore — aggiungere a RESERVED se sono termini tecnici
- Il parser cerca `^km\s+(DX|SX)`. Se l'operatore scrive il modificatore PRIMA del lato (es. `11+718 ALL DX 4212 S2` invece di `11+718 DX ALL 4212 S2`), il regex NON matcha e la saldatura viene ignorata. Fix: pre-processing che normalizza `^km\s+(ALL|SCIN)\s+(DX|SX)` → `km DX/SX ALL/SCIN` prima del parse principale (zero modifiche alla logica parser)
- PDF download Android: usare `navigator.canShare` come unico discriminante è sbagliato — Chrome Android moderno espone Share API ma il menu "Condividi" confonde l'operatore. Discriminare con `isIOS = /iPhone|iPad|iPod/.test(navigator.userAgent) && !window.MSStream` e usare Share API solo su iOS, download diretto via `<a download>` su Android/desktop
- "Palo", "Suola", "Fungo", parole tecniche brevi: aggiungere a RESERVED del parser per non interromperle come "sigla operatore" durante il blocco figli del difetto
