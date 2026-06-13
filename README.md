# Sistemi SuperEnalotto

**Sito online:** https://procolo75.github.io/sistemi-superenalotto/

Generatore di sistemi a griglia per il SuperEnalotto. Tre matrici disponibili — 6×6, 7×7 e 8×8 — con supporto a numeri fissi, numeri ripetuti, SuperStar e modalità ridotta o integrale. Funziona interamente nel browser, senza installazioni né dipendenze esterne.

---

## Struttura del progetto

```
sistemi-superenalotto/
├── index.html          # Pagina principale — elenco dei sistemi disponibili
├── sistema_6x6.html    # Sistema Cruciverba 6×6
├── sistema_7x7.html    # Sistema Cruciverba 7×7
└── sistema_8x8.html    # Sistema Cruciverba 8×8
```

Ogni file è una pagina HTML autocontenuta: niente framework, niente server, niente build step. Basta aprire il file nel browser.

---

## Utilizzo

### Avvio locale

Apri `index.html` direttamente nel browser oppure servi la cartella con un server locale:

```bash
# Python 3
python3 -m http.server 8080
# poi vai su http://localhost:8080
```

Non è strettamente necessario un server — i file funzionano anche aperti direttamente dal filesystem — tranne che per la generazione PDF, che richiede il caricamento di una libreria esterna (`html2pdf.js` via CDN).

---

## I tre sistemi

### 6×6 Cruciverba

- **Griglia:** 6 righe × 6 colonne = 36 celle
- **Gruppi:** 6 righe + 6 colonne + 2 diagonali = **14 sestine dirette**
- **Modalità:** solo integrale — ogni gruppo è già una sestina da giocare
- **Costo:** €14 (€21 con SuperStar)

Il sistema più semplice: ogni riga, colonna e diagonale è esattamente una sestina pronta da giocare, senza bisogno di riduzione.

---

### 7×7 Cruciverba

- **Griglia:** 7 righe × 7 colonne = 49 celle
- **Gruppi:** 7 righe + 7 colonne + 2 diagonali = **16 settine**
- **Modalità disponibili:**

| Modalità | Giocate | Costo base | Con SuperStar |
|---|---|---|---|
| Ridotto n−1 | 16 sestine (1 per settina) | €16 | €24 |
| Integrale | 16 × C(7,6) = 112 sestine | €112 | €168 |

**Ridotto n−1:** da ogni settina viene escluso 1 numero (quello in posizione predefinita o casuale), ottenendo una sestina per gruppo.

**Integrale:** da ogni settina vengono estratte tutte le C(7,6) = 7 sestine possibili.

---

### 8×8 Cruciverba

- **Griglia:** 8 righe × 8 colonne = 64 celle
- **Gruppi:** 8 righe + 8 colonne + 2 diagonali = **18 ottine**
- **Modalità disponibili:**

| Modalità | Giocate | Costo base | Con SuperStar |
|---|---|---|---|
| Ridotto n−2 | 18 sestine (6 per ottina) | €18 | €27 |
| Ridotto n−1 | 18 × C(7,6) = 126 sestine | €126 | €189 |
| Integrale | 18 × C(8,6) = 504 sestine | €504 | €756 |

**Ridotto n−2:** da ogni ottina vengono esclusi 2 numeri → 1 sestina per gruppo.  
**Ridotto n−1:** da ogni ottina viene escluso 1 numero → C(7,6) = 7 sestine per gruppo.  
**Integrale:** tutte le C(8,6) = 28 sestine per ogni ottina.

---

## Opzioni di configurazione

### Numeri ripetuti

Ogni sistema permette di inserire nella griglia da **0 a 10 numeri ripetuti** (numeri che compaiono più di una volta). Questo aumenta la concentrazione su certi valori, ma riduce la copertura totale del range 1–90.

La posizione dei numeri ripetuti nella griglia può essere:
- **Casuale** — disposti in celle casuali
- **Centrata** — concentrata al centro della griglia
- **Bordi** — distribuita lungo i bordi

### Numeri fissi

È possibile specificare fino a N numeri che devono comparire obbligatoriamente nella griglia. I numeri fissi vengono posizionati prima del resto della griglia.

### SuperStar

La SuperStar è opzionale. Quando attivata, si può scegliere tra:
- **Numero unico** per tutte le giocate — casuale o definito manualmente
- **Numero per giocata** — casuale indipendente per ciascuna giocata, oppure definiti manualmente

---

## Vincoli e garanzie

Il generatore rispetta sempre queste regole:

1. **All'interno di ogni gruppo** (riga, colonna, diagonale) nessun numero è ripetuto.
2. **Tra le giocate generate**, nessuna combinazione è identica a un'altra (l'ordine non conta: `{1,2,3}` e `{3,1,2}` sono la stessa combinazione).

Se dopo 300 tentativi non è possibile generare un sistema senza combinazioni duplicate (può accadere con molti numeri ripetuti), viene mostrato un errore.

---

## Tabella numeri esclusi (sistemi ridotti)

Per tutti i sistemi ridotti (7×7 ridotto n−1, 8×8 ridotto n−1 e n−2), dopo la generazione appare una tabella che mostra — per ogni riga, colonna e diagonale — quali numeri sono stati esclusi e quante altre giocate del sistema li contengono ancora. Questo permette di valutare l'esposizione del sistema a ciascun numero escluso.

---

## Esportazione PDF

Ogni sistema include un pulsante **Salva PDF** che appare dopo la generazione. Il PDF viene generato lato client tramite la libreria [html2pdf.js](https://github.com/eKoopmans/html2pdf.js) (caricata da CDN solo al momento del click). Il PDF include la griglia, tutte le giocate e il riepilogo di configurazione con data e ora di generazione.

---

## Avvertenze legali

Il gioco è vietato ai minori di 18 anni.  
Il gioco può causare dipendenza patologica.  
Giocare responsabilmente.
