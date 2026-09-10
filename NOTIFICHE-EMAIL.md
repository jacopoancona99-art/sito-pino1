# Notifiche via e-mail delle pre-iscrizioni

Il modulo funziona anche senza questo passaggio: le richieste si vedono
comunque nel pannello, con il pallino rosso sulla scheda "Richieste" che
conta quelle non ancora lette.

Questa guida aggiunge una **notifica via mail** quando arriva una nuova
richiesta, così non serve controllare il pannello ogni giorno.

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
 * Notifica via e-mail delle nuove pre-iscrizioni.
 * Gruppo Scout AGESCI Pino Torinese 1
 */

// Chi riceve la notifica. Più indirizzi separati da virgola.
const DESTINATARI = 'pinotorinese1@piemonte.agesci.it';

function doPost(e) {
  try {
    const d = JSON.parse(e.postData.contents);

    // Rifiuta chiamate che non hanno la forma attesa.
    if (!d.nomeRagazzo || !d.cognomeRagazzo || !d.email) {
      return ok();
    }

    const nome = `${d.nomeRagazzo} ${d.cognomeRagazzo}`;
    const anni = calcolaEta(d.dataNascita);

    const corpo =
      'È arrivata una nuova richiesta di pre-iscrizione.\n\n' +
      '── RAGAZZO/A ──\n' +
      `Nome:            ${nome}\n` +
      `Data di nascita: ${formattaData(d.dataNascita)} (${anni} anni)\n` +
      `Branca:          ${brancaPerEta(anni)}\n\n` +
      '── CONTATTI ──\n' +
      `Genitore:        ${d.nomeGenitore}\n` +
      `E-mail:          ${d.email}\n` +
      `Telefono:        ${d.telefono || 'non indicato'}\n\n` +
      (d.note ? `── NOTE DELLA FAMIGLIA ──\n${d.note}\n\n` : '') +
      '───────────────────────────\n' +
      'Apri il pannello per gestirla:\n' +
      'https://pinotorinese1.github.io/admin.html\n\n' +
      'Questa mail contiene dati personali di un minore: non inoltrarla\n' +
      'fuori dalla Comunità Capi.';

    MailApp.sendEmail({
      to:      DESTINATARI,
      subject: `[Pino 1] Pre-iscrizione: ${nome} (${anni} anni)`,
      body:    corpo,
      replyTo: d.email
    });

  } catch (err) {
    // Non rilanciamo: la richiesta è già salvata su Firebase,
    // un errore qui non deve avere conseguenze visibili.
    console.error('Notifica fallita: ' + err);
  }

  return ok();
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
  return `${p[2]}/${p[1]}/${p[0]}`;
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
