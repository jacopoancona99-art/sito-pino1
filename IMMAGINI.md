# Immagini da recuperare dal vecchio sito

Le immagini stanno ancora sui server di Wix. Finché il sito Wix esiste
funzionano, ma il giorno che lo chiudete spariscono anche da qui.
Conviene portarle nel repository una volta per tutte.

## Come fare

1. Crea nel repository una cartella `img` (GitHub → **Add file** →
   **Create new file** → scrivi `img/.gitkeep` → Commit)
2. Apri ciascun indirizzo qui sotto in una scheda del browser
3. Tasto destro → **Salva immagine con nome** → usa **esattamente** il
   nome file indicato nella colonna di sinistra
4. Carica tutto in `img/` (Add file → Upload files, si possono trascinare
   tutte insieme)

**Prima di caricare, ridimensiona.** Alcune di queste sono da 1920 o 2000
pixel di lato: per delle foto che sul sito appaiono a 300 px sono un peso
inutile. Un ridimensionamento a **800 px sul lato lungo** basta e avanza,
e taglia il peso di quattro o cinque volte. Va bene qualsiasi strumento,
anche [squoosh.app](https://squoosh.app) direttamente nel browser.

---

## Foto delle sezioni (usate in `index.html`)

Queste hanno il percorso già scritto nel codice: **i nomi devono
corrispondere esattamente**, altrimenti restano dei riquadri vuoti.

| Salva come | Indirizzo |
|---|---|
| `img/home-natura.jpg` | https://static.wixstatic.com/media/90ac93_fae6462c826f4737b011a58784348e48~mv2_d_1920_1280_s_2.jpg |
| `img/home-coca.jpg` | https://static.wixstatic.com/media/90ac93_5fbd82aa60794e118c9a7696d95dc851~mv2_d_1920_1277_s_2.jpg |
| `img/home-clan.jpg` | https://static.wixstatic.com/media/90ac93_3e33147b8a174cf8a81ed62619f8bd8a~mv2_d_2000_1331_s_2.jpg |
| `img/branco.jpg` | https://static.wixstatic.com/media/90ac93_7ee112d6be1c47e5b8334728a37a757e~mv2.jpg |
| `img/reparto.jpg` | https://static.wixstatic.com/media/90ac93_fdee98a1a0294d748ad2ef2011bb27a0~mv2.jpg |
| `img/clan.jpg` | https://static.wixstatic.com/media/90ac93_3e33147b8a174cf8a81ed62619f8bd8a~mv2_d_2000_1331_s_2.jpg |
| `img/noviziato.jpg` | https://static.wixstatic.com/media/90ac93_38553c6355b140b38d16d8acc4a70aa0~mv2.jpg |

Nota: `home-clan.jpg` e `clan.jpg` sono la stessa foto usata in due punti.
Se preferisci puoi salvarla una volta sola e cambiare uno dei due
riferimenti dentro `index.html`.

---

## Foto dei capi

**Non servono più qui.** Dal pannello admin c'è il pulsante *Scegli una
foto…*: l'immagine viene rimpicciolita e compressa nel browser e salvata
direttamente nel database. Puoi caricare uno scatto dal telefono senza
prepararlo prima.

Se ti servono gli originali dal vecchio sito, sono nello script
`scarica-immagini.ps1`, che li salva in `img/capi/`. Da lì li carichi nel
pannello uno per uno.

**Prima di pubblicare le foto dei capi, chiedi il consenso.** Sono
immagini di persone identificabili su un sito pubblico: basta un
passaggio in CoCa e va messo a verbale. Chi non se la sente resta con le
iniziali.

---

## Formato consigliato

Le schede dei capi sono in proporzione 3:2 orizzontale e ritagliano
partendo dall'alto. Le foto verticali (quelle con `1277_1920` nel nome)
verranno tagliate parecchio: se hai gli originali conviene ritagliarle a
mano tenendo il viso nella metà superiore.
