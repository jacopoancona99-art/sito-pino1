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

## Foto dei capi (da incollare nel pannello)

Queste **non** stanno nel codice: si inseriscono dal pannello admin, nel
campo "Foto" di ciascun capo. Il nome file è solo un suggerimento per
tenere ordine nella cartella.

Dopo averle caricate in `img/capi/`, nel pannello scrivi il percorso
relativo, per esempio: `img/capi/marta.jpg`

| Chi | Salva come | Indirizzo |
|---|---|---|
| Marta | `img/capi/marta.jpg` | https://static.wixstatic.com/media/90ac93_b4d85ba4128741f186d7fe7885107849~mv2.jpg |
| Francesco R. | `img/capi/francesco-r.jpg` | https://static.wixstatic.com/media/90ac93_186f6523352e465faaa345dc43df6ef6~mv2_d_1277_1920_s_2.jpg |
| Fabrizio | `img/capi/fabrizio.jpg` | https://static.wixstatic.com/media/90ac93_a7b6caadb99b4ab6b563d5cb68be9f79~mv2.jpg |
| Don Mimmo | `img/capi/don-mimmo.jpg` | https://static.wixstatic.com/media/90ac93_8335ac21d3db4724ae369d3aecfdf6e7~mv2_d_1277_1920_s_2.jpg |
| Akela | `img/capi/akela.jpg` | https://static.wixstatic.com/media/90ac93_175916b2ed544c818fe1cf2bd8040d0f~mv2.jpg |
| Bagheera | `img/capi/bagheera.jpg` | https://static.wixstatic.com/media/90ac93_e0d2da161b824b76ae0cd858a44a5bfb~mv2.jpg |
| Babbo Lupo | `img/capi/babbo-lupo.jpg` | https://static.wixstatic.com/media/90ac93_8d79e9ab65cd4a99baa52e383f255308~mv2_d_1920_1277_s_2.jpg |
| Kaa | `img/capi/kaa.jpg` | https://static.wixstatic.com/media/90ac93_dcfda80aca8144e7b20b48b97421708c~mv2.jpg |
| Valentina | `img/capi/valentina.jpg` | https://static.wixstatic.com/media/90ac93_b7d15ee312be420b8a5b3c60dab2c73e~mv2_d_2000_1494_s_2.jpg |
| Jacopo | `img/capi/jacopo.jpg` | https://static.wixstatic.com/media/90ac93_7f85d74487d5495b9f608e2c9e475681~mv2.jpg |
| Andrea | `img/capi/andrea.jpg` | https://static.wixstatic.com/media/90ac93_d1e55d4094114ffb95d3359411c4a2da~mv2.jpg |
| Cecilia | `img/capi/cecilia.jpg` | https://static.wixstatic.com/media/90ac93_3eee37affb9948fcb2bf066b0d737789~mv2.jpg |
| Matteo | `img/capi/matteo.jpg` | https://static.wixstatic.com/media/90ac93_15da60652acf4935831526be5c3d0a40~mv2.jpg |
| Francesco | `img/capi/francesco.jpg` | https://static.wixstatic.com/media/90ac93_5f6dd749684243c39b7dd74dfbe5d061~mv2_d_1277_1920_s_2.jpg |
| Anna | `img/capi/anna.jpg` | https://static.wixstatic.com/media/90ac93_c666b501aff54319a8367f45d5681eb8~mv2_d_1277_1920_s_2.jpg |
| Luca | `img/capi/luca.jpg` | https://static.wixstatic.com/media/90ac93_94dc1724a5e24831b8eb27a4c8fb1e69~mv2_d_2000_1333_s_2.jpg |

**Prima di caricare le foto dei capi, chiedi il consenso.** Sono immagini
di persone identificabili su un sito pubblico: basta un passaggio in CoCa
e va messo a verbale. Chi non se la sente resta con le iniziali, che sul
sito hanno comunque una resa pulita e uniforme.

---

## Formato consigliato

Le schede dei capi sono in proporzione 3:2 orizzontale e ritagliano
partendo dall'alto. Le foto verticali (quelle con `1277_1920` nel nome)
verranno tagliate parecchio: se hai gli originali conviene ritagliarle a
mano tenendo il viso nella metà superiore.
