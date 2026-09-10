# Sito Gruppo Scout AGESCI Pino Torinese 1

Sito pubblico su GitHub Pages con i contenuti modificabili da un pannello
protetto, senza toccare il codice.

- **Sito:** https://pinotorinese1.github.io/
- **Pannello:** https://pinotorinese1.github.io/admin.html
- **Firebase:** progetto `sito-pino1`, Realtime Database in `europe-west1`

---

## Come funziona

```
GitHub Pages  ──►  index.html      (pagine pubbliche, HTML statico)
                        │
                        │  fetch REST
                        ▼
                Realtime Database
                        │
     /contenuti   capi, iscrizioni, avvisi   lettura: tutti
     /foto        immagini compresse         lettura: tutti
     /richieste   pre-iscrizioni (minori)    lettura: solo capi
                        ▲
                        │  scrittura solo con login
                        │
                   admin.html     (pannello capi)
```

I tre rami hanno regole diverse. `/richieste` in particolare è **invertito**:
chiunque può creare una richiesta dal modulo pubblico, ma solo un capo
autenticato può leggerla. È il modello della cassetta delle lettere.

I **testi descrittivi** delle branche stanno dentro `index.html`: cambiano
una volta ogni cinque anni e così restano indicizzabili dai motori di
ricerca.

Su Firebase stanno solo i **dati che cambiano davvero**: elenco capi,
iscrizioni, avvisi.

Le pagine pubbliche leggono con una `fetch()` sull'API REST, non con
l'SDK Firebase. Questo evita di aprire un WebSocket (che consumerebbe una
delle 100 connessioni simultanee del piano gratuito) e risparmia circa
200 KB di download a ogni visita. Il pannello usa invece l'SDK completo,
perché lì serve la sincronizzazione in tempo reale.

Se il database non risponde entro **5 secondi**, il sito mostra i
contenuti di riserva scritti in `pt-config.js` invece di lasciare sezioni
vuote. Nel caso peggiore i dati sono vecchi, ma il sito resta in piedi.

---

## File

| File | Cosa fa |
|---|---|
| `index.html` | Tutte le pagine pubbliche (home, gruppo, branche, iscrizioni, contatti) |
| `admin.html` | Pannello di gestione: capi, iscrizioni, avvisi |
| `pt-config.js` | Configurazione Firebase, elenco branche, contenuti di riserva |
| `firebase-rules.json` | Regole di sicurezza del database (copia di servizio) |
| `img/` | Foto delle sezioni. Quelle dei capi stanno nel database, non qui |

I tre file `.html` e `.js` devono stare **nella stessa cartella**, altrimenti
l'import del modulo fallisce.

---

## Uso quotidiano

Apri il pannello, accedi con la tua e-mail, modifica. Le pagine pubbliche
si aggiornano al primo ricaricamento, senza pubblicare niente su GitHub.

**Aggiungere un capo** → scheda Capi → *Aggiungi capo*. Nome, ruolo, branca.
Per la foto usa *Scegli una foto…*: viene rimpicciolita e compressa nel
browser prima del salvataggio, quindi puoi caricare tranquillamente uno
scatto dal telefono. Senza foto compaiono le iniziali su sfondo verde,
che è una resa pulita e uniforme.

**Ordine** → frecce ▲▼ dentro la stessa branca. Il capo titolare (bordo
arancione) compare comunque per primo sul sito, anche se in elenco sta
più in basso.

**Chiudere le iscrizioni** → scheda Iscrizioni, togli la spunta. La pagina
pubblica cambia da sola: sparisce il link al modulo e compaiono i contatti
per essere avvisati alla riapertura.

**Avvisi** → compaiono in evidenza sulla home. Quando uno scade conviene
**spegnerlo** invece di cancellarlo: l'anno dopo spesso serve di nuovo con
due parole cambiate.

---

## Aggiungere un nuovo capo al pannello

Console Firebase → **Authentication** → **Users** → **Aggiungi utente**.

Un account per persona, mai uno condiviso: quando qualcuno lascia la CoCa
si disattiva il suo e basta, senza cambiare la password a tutti.

---

## Modificare il codice

I file si caricano dall'interfaccia web di GitHub, senza terminale:
repository → **Add file** → **Upload files** → trascina → **Commit changes**.

Dopo il commit passano uno o due minuti prima che il sito si aggiorni.
Se non vedi le modifiche, ricarica con `Ctrl+F5` per saltare la cache.

---

## Manutenzione

**Ogni settembre:** aggiornare l'anno scout nella scheda Iscrizioni e
rivedere l'elenco capi dopo i passaggi di branca.

**Backup annuale:** Console Firebase → Realtime Database → menu ⋮ →
*Esporta JSON*. Conviene tenere il file nel repository, così anche in caso
di disastro i contenuti non si perdono.

**Costi:** il piano gratuito Spark dà 1 GB di dati e 10 GB/mese di
download sul Realtime Database. I contenuti del sito pesano circa 10 KB,
quindi questi limiti non si toccano nemmeno da lontano. Non serve inserire
nessun metodo di pagamento.

---

## Se qualcosa non va

**Il login non passa** → Console Firebase → Authentication → Settings →
*Domini autorizzati*: deve esserci `pinotorinese1.github.io`.

**Il pannello dice `permission_denied`** → le regole del database non sono
state pubblicate. Realtime Database → scheda Regole → incolla
`firebase-rules.json` → Pubblica.

**Il sito mostra dati vecchi** → è il fallback che è entrato in funzione.
Apri la console del browser (`F12`) e cerca un messaggio che inizia con
`[PT]`: ti dice il motivo.

**Errore sul caricamento del modulo** → `index.html`, `admin.html` e
`pt-config.js` non sono nella stessa cartella del repository.

---

## Nota sulla chiave API

L'`apiKey` dentro `pt-config.js` **non è un segreto** ed è normale che sia
visibile in un repository pubblico: sta nel codice sorgente di qualsiasi
sito che usa Firebase. La sicurezza dei dati è garantita dalle regole del
database, che permettono a chiunque di leggere ma solo agli utenti
autenticati di scrivere.
