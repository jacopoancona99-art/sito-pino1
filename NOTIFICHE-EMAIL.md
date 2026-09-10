# Notifiche via e-mail

Un solo script gestisce due cose:

- le **pre-iscrizioni**, che arrivano alla casella del gruppo
- i **messaggi dal modulo contatti**, inoltrati al gruppo e alla branca
  competente in base all'argomento scelto

I moduli funzionano anche senza questo passaggio: tutto si vede comunque
nel pannello, con il pallino rosso sulle schede "Richieste" e "Messaggi".
Le notifiche servono a non doverlo controllare ogni giorno.

**Gli indirizzi delle branche vivono solo dentro questo script.** Non
sono nel codice del sito apposta: nel sorgente di una pagina pubblica
verrebbero raccolti dai robot dello spam in poco tempo.

> **Perché non usiamo Firebase per mandare le mail:** servirebbero le
> Cloud Functions, che richiedono il piano Blaze e quindi l'inserimento
> di una carta di credito. Google Apps Script fa la stessa cosa
> gratuitamente, usando l'account Google che avete già.

---

## Come è pensata

La mail è **una comodità, non il registro delle iscrizioni**. L'ordine è:

1. la richiesta viene salvata su Firebase
2. *poi* si prova a mandare la mail

Se la mail non parte — script rotto, quota finita, servizio giù — la
famiglia riceve comunque la conferma e la richiesta è al sicuro nel
database. Nessun dato va perso per colpa di una notifica.

---

## Configurazione (dieci minuti)

### 1. Crea lo script

Vai su [script.google.com](https://script.google.com) → **Nuovo progetto**.

Rinominalo `Notifiche iscrizioni Pino 1` (in alto a sinistra).

Cancella tutto quello che c'è nell'editor e incolla:

```javascript
/**
 * Notifiche via e-mail — Gruppo Scout AGESCI Pino Torinese 1
 *
 * Gestisce due cose:
 *   • pre-iscrizioni  → sempre alla casella del gruppo
 *   • messaggi dal modulo contatti → gruppo + branca competente
 *
 * ⚠ GLI INDIRIZZI DELLE BRANCHE STANNO SOLO QUI.
 *   Non vanno messi nel codice del sito: sarebbero visibili nel
 *   sorgente della pagina e i raccoglitori di indirizzi per lo
 *   spam li troverebbero in poco tempo. Questo script gira sui
 *   server Google e non è leggibile dall'esterno.
 */

// Casella principale: riceve tutto.
const GRUPPO = 'pinotorinese1@piemonte.agesci.it';

// ✏️ DA COMPILARE con gli indirizzi reali delle branche.
//    Lascia la stringa vuota per far arrivare tutto solo al gruppo.
const BRANCHE = {
  branco:  '',   // es. 'branco.pino1@gmail.com'
  reparto: '',
  clan:    '',
  coca:    ''
};

// Quale branca riceve, oltre al gruppo, in base all'oggetto scelto.
const INOLTRO = {
  informazioni:   [],
  iscrizioni:     [],
  ospitalita:     [],
  branco:         ['branco'],
  reparto:        ['reparto'],
  clan:           ['clan'],
  coca:           ['coca'],
  collaborazione: ['coca'],
  altro:          []
};


function doPost(e) {
  try {
    const d = JSON.parse(e.postData.contents);
    if (d.tipo === 'contatto') inoltraMessaggio(d);
    else                       notificaIscrizione(d);
  } catch (err) {
    // Non rilanciamo: il dato è già salvato su Firebase e un
    // errore qui non deve avere conseguenze visibili.
    console.error('Notifica fallita: ' + err);
  }
  return ok();
}


/* ─── Messaggi dal modulo contatti ─── */
function inoltraMessaggio(d) {
  if (!d.nome || !d.email || !d.testo) return;

  // Gruppo + eventuali branche, senza doppioni e senza caselle vuote.
  const destinatari = [GRUPPO]
    .concat((INOLTRO[d.chiave] || []).map(b => BRANCHE[b]))
    .filter((v, i, a) => v && a.indexOf(v) === i)
    .join(',');

  const corpo =
    'Nuovo messaggio dal modulo contatti del sito.\n\n' +
    '── ARGOMENTO ──\n' + (d.oggetto || 'Non indicato') + '\n\n' +
    '── DA ──\n' +
    'Nome:     ' + d.nome + '\n' +
    'E-mail:   ' + d.email + '\n' +
    'Telefono: ' + (d.telefono || 'non indicato') + '\n\n' +
    '── MESSAGGIO ──\n' + d.testo + '\n\n' +
    '───────────────────────────\n' +
    'Rispondi pure a questa mail: la risposta va direttamente a chi ha scritto.\n' +
    'Copia di sicurezza nel pannello:\n' +
    'https://pinotorinese1.github.io/admin.html';

  MailApp.sendEmail({
    to:      destinatari,
    subject: '[Pino 1] ' + (d.oggetto || 'Messaggio') + ' — ' + d.nome,
    body:    corpo,
    replyTo: d.email        // rispondere alla mail scrive a chi ha contattato
  });
}


/* ─── Pre-iscrizioni ─── */
function notificaIscrizione(d) {
  if (!d.nomeRagazzo || !d.cognomeRagazzo || !d.email) return;

  const nome = d.nomeRagazzo + ' ' + d.cognomeRagazzo;
  const anni = calcolaEta(d.dataNascita);

  const corpo =
    'È arrivata una nuova richiesta di pre-iscrizione.\n\n' +
    '── RAGAZZO/A ──\n' +
    'Nome:            ' + nome + '\n' +
    'Data di nascita: ' + formattaData(d.dataNascita) + ' (' + anni + ' anni)\n' +
    'Branca:          ' + brancaPerEta(anni) + '\n\n' +
    '── CONTATTI ──\n' +
    'Genitore:        ' + d.nomeGenitore + '\n' +
    'E-mail:          ' + d.email + '\n' +
    'Telefono:        ' + (d.telefono || 'non indicato') + '\n\n' +
    (d.note ? '── NOTE DELLA FAMIGLIA ──\n' + d.note + '\n\n' : '') +
    '───────────────────────────\n' +
    'Apri il pannello per gestirla:\n' +
    'https://pinotorinese1.github.io/admin.html\n\n' +
    'Questa mail contiene dati personali di un minore: non inoltrarla\n' +
    'fuori dalla Comunità Capi.';

  MailApp.sendEmail({
    to:      GRUPPO,
    subject: '[Pino 1] Pre-iscrizione: ' + nome + ' (' + anni + ' anni)',
    body:    corpo,
    replyTo: d.email
  });
}


function ok() {
  return ContentService
    .createTextOutput(JSON.stringify({ ok: true }))
    .setMimeType(ContentService.MimeType.JSON);
}

function calcolaEta(iso) {
  if (!iso) return '?';
  const n = new Date(iso), o = new Date();
  let a = o.getFullYear() - n.getFullYear();
  const m = o.getMonth() - n.getMonth();
  if (m < 0 || (m === 0 && o.getDate() < n.getDate())) a--;
  return a;
}

function formattaData(iso) {
  if (!iso) return '?';
  const p = iso.split('-');
  return p[2] + '/' + p[1] + '/' + p[0];
}

function brancaPerEta(a) {
  if (typeof a !== 'number') return '?';
  if (a < 7)   return 'troppo piccolo per il Branco';
  if (a <= 11) return 'Branco Mirfak';
  if (a <= 16) return 'Reparto Everest';
  if (a <= 17) return 'Noviziato Silmaril';
  if (a <= 21) return 'Clan Zebrù';
  return 'fuori età, da valutare';
}
```

**Compila la tabella `BRANCHE`** con gli indirizzi reali. Se una casella
non esiste ancora, lascia la stringa vuota: quel messaggio arriverà solo
al gruppo, senza errori.

Il campo `replyTo` fa sì che rispondendo alla notifica si scriva
direttamente a chi ha contattato, senza copiare l'indirizzo a mano.

Salva con l'icona del dischetto.

### 2. Pubblica

Pulsante blu **Esegui il deployment** (in alto a destra) →
**Nuovo deployment**.

- Icona ingranaggio accanto a *Seleziona tipo* → **Applicazione web**
- **Descrizione:** `notifiche iscrizioni`
- **Esegui come:** *Me stesso*
- **Chi ha accesso:** **Chiunque** ← indispensabile, il modulo chiama
  senza essere autenticato
- **Esegui il deployment**

Google chiede l'autorizzazione a inviare mail per conto tuo. Accetta.
Alla schermata "Google non ha verificato questa app" clicca su
**Avanzate** → **Apri progetto (non sicuro)**: è un tuo script, l'avviso
compare per tutti i progetti personali.

Alla fine copia l'**URL dell'app web**, quello che finisce con `/exec`.

### 3. Collega il modulo

Apri `index.html`, cerca questa riga (è verso il fondo, nello script):

```javascript
const URL_NOTIFICA = '';
```

Incolla dentro l'indirizzo:

```javascript
const URL_NOTIFICA = 'https://script.google.com/macros/s/AKfy.../exec';
```

Ricarica il file su GitHub. Fatto.

---

## Prova

Compila il modulo sul sito con dati finti. Dovresti ricevere la mail entro
un minuto, e vedere la richiesta comparire nel pannello.

Ricordati di **cancellare la richiesta di prova** dal pannello.

---

## Se le mail non arrivano

Il modulo continua a funzionare: la richiesta è nel pannello. Per capire
il perché, vai su [script.google.com](https://script.google.com) → il tuo
progetto → **Esecuzioni** nel menu di sinistra: lì vedi ogni chiamata
ricevuta e l'eventuale errore.

Le cause tipiche:

- **"Chi ha accesso" non è impostato su Chiunque** → lo script rifiuta la
  chiamata prima ancora di eseguirla
- **`URL_NOTIFICA` vuoto o sbagliato** in `index.html`
- **Hai modificato lo script senza ripubblicare** → Esegui il deployment →
  *Gestisci deployment* → matita → *Nuova versione*. Questo è il passaggio
  che sfugge sempre: modificare il codice non basta, va rifatto il
  deployment
- **Quota giornaliera esaurita** → 100 mail al giorno con un account Gmail
  normale. Se succede, o sono arrivate cento iscrizioni in un giorno
  oppure qualcuno sta abusando del modulo

---

## Se cambia chi riceve le mail

Modifica la costante `DESTINATARI` in cima allo script, salva, e
**ripubblica** (Esegui il deployment → Gestisci deployment → matita →
Nuova versione). Senza il nuovo deployment continua a girare la versione
vecchia.

---

## Nota sulla privacy

Queste mail contengono nome, età e contatti di un minore. Vale la pena
che arrivino a un indirizzo del gruppo e non a caselle personali, e che
chi le riceve sappia di non doverle inoltrare fuori dalla CoCa.

L'informativa pubblicata sul sito dichiara che le richieste non accolte
vengono cancellate entro tre mesi dalla chiusura delle iscrizioni: la
cancellazione va fatta **anche nelle caselle di posta**, non solo nel
pannello.
