---
title: Tmux Cheat Sheet
---
In tmux, la combinazione di tasti predefinita per inviare i comandi (il prefisso) è **`Ctrl + b`**. Significa che dovrai premere `Ctrl + b`, rilasciare i tasti e poi premere il tasto desiderato.

### 1. Gestione Sessioni (da Terminale Shell)

|**Comando**|**Descrizione**|
|---|---|
|`tmux`|Avvia una nuova sessione anonima|
|`tmux new -s nome`|Avvia una nuova sessione con un **nome** specifico|
|`tmux ls`|Mostra la lista di tutte le sessioni attive|
|`tmux a`|Ti ricollega (_attach_) all'ultima sessione attiva|
|`tmux a -t nome`|Ti ricollega a una sessione specifica per nome|
|`tmux kill-session -t nome`|Chiude e distrugge una sessione specifica|

### 2. Gestione Pannelli (Pane) — _Dividere lo schermo_

Tutte le combinazioni richiedono il prefisso **`Ctrl + b`** prima del tasto indicato:

|**Tasto**|**Azione**|
|---|---|
|**`%`**|Divide il pannello corrente **verticalmente** (affiancati)|
|**`"`**|Divide il pannello corrente **orizzontalmente** (sopra/sotto)|
|**`Frecce direzionali`**|Sposta il cursore tra i pannelli|
|**`o`**|Passa al pannello successivo|
|**`x`**|Chiude il pannello corrente (_chiede conferma y/n_)|
|**`z`**|Ingrandisce a tutto schermo il pannello corrente (e viceversa)|
|**`q`**|Mostra brevemente i numeri identificativi di ciascun pannello|
|**`Alt + Frecce`**|Ridimensiona il pannello corrente (_senza dover ripremere Ctrl+b_)|

### 3. Gestione Finestre (Window) — _I "tab" di tmux_

|**Tasto**|**Azione**|
|---|---|
|**`c`**|Crea una nuova finestra (_tab_)|
|**`,`**|Rinomina la finestra corrente|
|**`n`**|Passa alla finestra **successiva** (_next_)|
|**`p`**|Passa alla finestra **precedente** (_previous_)|
|**`0` - `9`**|Passa direttamente alla finestra con quel numero|
|**`w`**|Mostra un menu interattivo con l'elenco di tutte le finestre e pannelli|
|**`&`**|Chiude la finestra corrente (_chiede conferma y/n_)|

### 4. Navigazione, Scroll e Session Control

| **Tasto** | **Azione**                                                                                        |
| --------- | ------------------------------------------------------------------------------------------------- |
| **`d`**   | **Stacca** (_detach_) la sessione corrente (lascia tutto in esecuzione in background)             |
| **`[`**   | Entra in **Scroll Mode** (usa le frecce o `PgUp`/`PgDn` per scorrere i log; premi `q` per uscire) |
| **`:`**   | Apre la riga di comando interna di tmux (es. per digitare `source ~/.tmux.conf`)                  |