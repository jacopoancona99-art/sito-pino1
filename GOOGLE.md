# Farsi trovare su Google

Il sito è online ma questo non basta: Google deve sapere che esiste,
capire di cosa parla, e decidere di mostrarlo. Sono tre cose diverse.

Quello che serve dal punto di vista tecnico è già nel codice (titolo,
descrizione, dati strutturati, `sitemap.xml`, `robots.txt`). Restano i
passaggi che vanno fatti a mano.

---

## 1. Dire a Google che il sito esiste

Senza questo passaggio possono passare mesi prima che vi trovi da solo.

1. Vai su [search.google.com/search-console](https://search.google.com/search-console)
   e accedi con l'account Google del gruppo
2. **Aggiungi proprietà** → riquadro **Prefisso URL** (quello di destra)
3. Inserisci `https://pinotorinese1.github.io/`
4. Per la verifica scegli **Tag HTML**: ti dà una riga `<meta name="google-site-verification" ...>`
5. Incollala in `index.html` subito sotto `<meta name="viewport" ...>`
6. Ricarica il file su GitHub, aspetta due minuti, torna su Search
   Console e clicca **Verifica**

Poi, sempre da Search Console:

- **Sitemap** nel menu di sinistra → scrivi `sitemap.xml` → Invia
- **Controllo URL** in alto → incolla `https://pinotorinese1.github.io/`
  → **Richiedi indicizzazione**

Da quel momento passano da pochi giorni a un paio di settimane.

---

## 2. Spegnere il sito Wix

**È il punto più importante di tutti, e quello che quasi nessuno fa.**

Il sito Wix è indicizzato da anni. Google lo conosce, ha accumulato
fiducia. Il sito nuovo parte da zero. Se restano entrambi online, Google
vede due siti che dicono le stesse cose sullo stesso gruppo e continua a
mostrare il vecchio, perché lo conosce meglio.

State quindi facendo concorrenza a voi stessi, e state perdendo.

Le opzioni, dalla migliore alla peggiore:

**Sostituire il contenuto del sito Wix** con una pagina sola che dice
"Ci siamo trasferiti" e un link ben visibile al nuovo indirizzo. Chi
arriva dal vecchio non si perde, e Google capisce dove andare.

**Spegnerlo del tutto.** Netto, ma per un po' chi cerca il gruppo trova
una pagina morta.

**Lasciarli entrambi.** La scelta peggiore: il sito nuovo faticherà per
mesi.

---

## 3. Farsi linkare

Google valuta un sito anche da chi lo cita. Per un gruppo locale valgono
molto più tre link giusti che cento generici.

Chiedete di essere linkati, o aggiornate il link se c'era già quello Wix:

- **AGESCI Zona Torino Est** e **AGESCI Piemonte** — hanno gli elenchi
  dei gruppi, e questo è il link che conta di più
- **La parrocchia** di riferimento
- **Il Comune di Pino Torinese**, che di solito ha una sezione con le
  associazioni del territorio
- **Instagram e Facebook** del gruppo: metteteci il link nella biografia

---

## 4. Aiutare chi cerca a trovarvi

Le persone non cercano "Gruppo Scout AGESCI Pino Torinese 1". Cercano
**"scout pino torinese"**, **"scout per bambini pino torinese"**,
**"iscrizioni scout torino"**.

Quelle parole devono comparire naturalmente nei testi del sito, non
infilate a forza. La pagina di iscrizione, in particolare, dovrebbe dire
chiaramente *cosa* si fa, *per che età*, *dove* e *quando* — perché è
quello che i genitori digitano.

---

## Cosa aspettarsi

**Prime due settimane:** il sito compare cercando `site:pinotorinese1.github.io`.
Vuol dire che Google l'ha visto.

**Primo mese:** compare cercando "gruppo scout pino torinese 1", cioè il
nome esatto.

**Da tre a sei mesi:** comincia a comparire su ricerche più generiche
come "scout pino torinese", ammesso che il sito Wix sia stato spento e
che qualche link sia arrivato.

Nessuno può promettere di più, e chi lo promette vende fumo.

---

## Un limite tecnico da conoscere

Il sito è costruito come **pagina unica**: le sezioni si alternano con
JavaScript senza cambiare indirizzo. Il contenuto è tutto nel codice,
quindi Google lo legge, ma può indicizzare **una sola pagina**.

In pratica: non potrete comparire con un risultato dedicato per "Branco
Mirfak" separato da uno per "Reparto Everest". Per un gruppo scout è un
limite accettabile — chi cerca vuole il gruppo, non la singola branca.

Se un domani diventasse importante, si dividono le sezioni in file
separati (`branco.html`, `reparto.html`…). È un lavoro fattibile ma non
banale, e adesso non ne vale la pena.

---

## Da controllare ogni tanto

Su Search Console, una volta ogni due o tre mesi:

- **Pagine** → dice se il sito è indicizzato e segnala eventuali errori
- **Rendimento** → mostra con quali parole vi trovano davvero. Spesso
  sorprende, ed è l'informazione più utile per capire cosa scrivere.
