---
title: Pandas
---
In Node.js, quando fai una query a PostgreSQL (ad esempio tramite `pg` o un ORM come Prisma), ottieni un array di oggetti/dizionari (`[{ id: 1, name: 'Alice' }, ...]`). Puoi fare `map`, `filter` e `reduce`, ma stai processando i dati **un elemento alla volta** (row-by-row) con la CPU che cicla su ogni riga.

**Pandas** adotta un approccio completamente diverso: gestisce i dati sotto forma di **DataFrame** (una tabella tipo Excel o SQL) organizzata per **colonne** (vettori/array), e non per singoli oggetti riga.

### Le 3 differenze fondamentali rispetto all'approccio Node.js

1. **Esecuzione vettorializzata (in C/Cython)**
    
    - **Node / Oggetti:** Per sommare il 10% di tasse a una colonna `prezzo`, fai un ciclo `.map(row => row.prezzo * 1.1)`.
        
    - **Pandas:** Scrivi semplicemente `df['prezzo'] * 1.1`. Sotto il cofano non c'è un ciclo Python: l'operazione viene eseguita istantaneamente in C su un blocco contiguo di memoria. Su 1 milione di righe, la differenza è di **100x o più in velocità**.
        
2. **Aggregazioni e Manipulation stile SQL** Pandas ha primitive nativamente pronte per l'analisi dati avanzata:
    
    - **Group By & Aggregazioni:** `df.groupby('categoria')['prezzo'].mean()`
        
    - **Merge / Join:** `df1.merge(df2, on='user_id', how='left')`
        
    - **Gestione valori nulli:** `df.fillna(0)` o `df.dropna()` In Node.js dovresti implementare tutte queste logiche a mano accumulando oggetti dentro dei reducers.
        
3. **I/O e Conversione nativa** Passare da un file CSV, Parquet o una tabella PostgreSQL a un DataFrame richiede una sola riga di codice:
    
    - `df = pd.read_sql("SELECT * FROM users", engine)`
        
    - `df.to_parquet("output.parquet")`
        

### Sintesi Visiva

|Operazione|Approccio Node.js / Python base|Approccio Pandas|
|---|---|---|
|**Struttura**|Array di Oggetti / Dizionari|`DataFrame` (Matrice a colonne)|
|**Filtro**|`data.filter(r => r.age > 30)`|`df[df['age'] > 30]`|
|**Nuovo campo**|`data.map(r => ({ ...r, total: r.qty * r.price }))`|`df['total'] = df['qty'] * df['price']`|
|**Raggruppamento**|Reducer manuale complesso|`df.groupby('status').sum()`|
|**Performance**|Lenta su grandi volumi (ciclo su riga)|Altissima (operazioni vettoriali in C)|

In breve: se devi solo spostare qualche decina di record da PostgreSQL a un'API JSON, gli oggetti/dizionari classici vanno benissimo. Ma se devi analizzare, ripulire, trasformare o aggregare migliaia o milioni di righe, Pandas trasforma Python in un motore di calcolo analitico ad altissime prestazioni.

Ok, ho capito in linee generali, ma ho bisogno di qualche info in più :

quando faccio df['age'] mi riferisco all indice della colonna ?

Cosa succede se non esiste 'age' ?

Non ho capito come df[ df['age'] > 30] funziona!

Dentre le [] ci vorrebbe un '' e non una condizione no ?

Mi sono perso qualcosa ?

È totalmente normale che la sintassi `df[df['age'] > 30]` mandi in confusione la prima volta, soprattutto arrivando da JS/Node dove le parentesi quadre servono quasi solo per accedere agli oggetti tramite chiave stringa o agli array tramite indice numerico.

Vediamo esattamente cosa succede sotto il cofano.

### 1. Cosa fa `df['age']`?

Non si riferisce all'indice numerico della colonna, ma al **nome della colonna** stesso.

- Un `DataFrame` in Pandas è una tabella.
    
- Ogni colonna ha un nome stringa e si chiama **`Series`** (un vettore/array unidimensionale).
    
- Quando scrivi `df['age']`, stai estraendo l'intera colonna "age" come singola `Series`.
    

**Cosa succede se 'age' non esiste?** Lancia un errore immediato: `KeyError: 'age'`. Esattamente come quando cerchi di accedere a una chiave inesistente in un dizionario Python.

### 2. Perché si scrive `df[df['age'] > 30]` invece di una stringa?

Il "trucco" qui è che l'operazione dentro le parentesi viene **valutata per prima** e restituisce un risultato molto particolare.

#### Passaggio 1: La condizione `df['age'] > 30`

Quando scrivi `df['age'] > 30`, Pandas **non** restituisce `True` o `False` per l'intera tabella. Confronta ogni singolo elemento della colonna `age` e restituisce una nuova `Series` di valori booleani (`True`/`False`), chiamata **Maschera Booleana**:

Python

```
# Supponiamo di avere questa colonna age:
# 0: 20
# 1: 35
# 2: 42

df['age'] > 30

# Risultato interno:
# 0: False
# 1: True
# 2: True
```

#### Passaggio 2: L'indicizzazione `df[...]`

Le parentesi quadre esterne di Pandas accettano **anche un array di booleani** della stessa lunghezza delle righe della tabella.

Quando gli passi quella maschera di `True` e `False`, Pandas tiene solo le righe in cui il valore corrisponde a `True` e scarta quelle a `False`.

Scomponendolo in due righe diventa chiarissimo:

Python

```
# 1. Crei la maschera di True/False
mask = df['age'] > 30 

# 2. Passi la maschera al DataFrame
risultato = df[mask] 
```

### Parallelo con Node.js / JS

Per fare un confronto, in JavaScript questo equivale a creare prima un array di booleani e poi usarlo per filtrare l'array principale:

JavaScript

```
const age = [20, 35, 42];

// 1. La maschera
const mask = age.map(a => a > 30); // [false, true, true]

// 2. Il filtro
const risultato = data.filter((_, index) => mask[index]);
```

In Pandas non serve scrivere il ciclo `map` o `filter`: l'operatore `>` lavora già in modo vettoriale su tutti gli elementi contemporaneamente.

C'é solo un ultima cosa che non capisco:

Come fa mask = df['age'] > 30 a essere diverso dal "solito" ?

Cioé perché python si comporta in modo diverso quando incontra la condizione > 30 rispetto a un semplice var['blablabla'] > 30 ?

Il motivo è che in Python il comportamento degli operatori come `>`, `<`, `+`, `-`, `==` **non è scolpito nel marmo**, ma dipende interamente dall'oggetto su cui vengono usati.

In Python, la sintassi `a > b` viene tradotta dal linguaggio in una chiamata a un metodo interno dell'oggetto `a`, chiamato **Magic Method** o **Dunder Method** (da _Double Underscore_): `a.__gt__(b)` (_gt_ sta per _greater than_).

### Come funziona la differenza

1. **In Python base (es. un intero):**
    
    Python
    
    ```
    x = 25
    x > 30  # Python chiama: x.__gt__(30) -> restituisce un singolo bool: False
    ```
    
2. **In Pandas (`Series`):** Gli sviluppatori di Pandas hanno **sovrascritto** il metodo `__gt__` della classe `Series`. Quando scrivi `df['age'] > 30`, Python esegue internamente:
    
    Python
    
    ```
    df['age'].__gt__(30)
    ```
    
    Invece di restituire un solo `True` o `False`, la funzione scritta da Pandas prende quel `30`, lo confronta con tutti gli elementi del vettore C sottostante in una sola operazione e **restituisce una nuova `Series` di booleani**.
    

### Un'implementazione semplificata di come fa Pandas

Se dovessi scrivere una classe personalizzata per ricreare questo comportamento in Python, usando il _method overloading_, la scriveresti così:

Python

```
class MiaColonna:

    def __init__(self, valori):
        self.valori = valori

    # Sovrascriviamo il comportamento dell'operatore >
    def __gt__(self, limite):
        # Invece di un solo bool, restituiamo una lista di bool!
        return [val > limite for val in self.valori]


# Uso:
colonna = MiaColonna([20, 35, 42])

# Sembra una normale condizione Python, ma invoca colonna.__gt__(30)
mask = colonna > 30

print(mask)
# Output: [False, True, True]
```

È lo stesso identico principio con cui funziona l'overloading degli operatori in C++ o C#. In JavaScript questo non si può fare (non puoi ridefinire cosa fa `>` su un oggetto custom), ed è per questo che arrivando da JS la sintassi `df['age'] > 30` sembra una "magia" del linguaggio, mentre in realtà è semplicemente programmazione a oggetti!

Ok, immagino ci siano parecchie funzioni che si posso usare in pandas..

apply etc...

potresti dirmi i principali ?

In Pandas la superficie dell'API è enorme, ma nella pratica quotidiana l'80% del lavoro di _data manipulation_ si fa usando una ventina di metodi principali.

Per orientarti meglio, ecco i metodi fondamentali divisi per categoria di utilizzo.

### 1. Esplorazione e Ispezione Rapida

I primi comandi da lanciare non appena si carica una tabella per capire cosa contiene.

- **`df.head(n)` / `df.tail(n)`**: Mostra le prime (o ultime) n righe del DataFrame.
    
- **`df.info()`**: Mostra la struttura generale, il tipo di dato di ogni colonna (int, float, object/stringa) e quanti valori non-nulli ci sono.
    
- **`df.describe()`**: Calcola in automatico statistiche descrittive (media, deviazione standard, min, max, percentili) per le colonne numeriche.
    
- **`df.shape`**: Proprietà (non un metodo) che restituisce una tupla con la dimensione `(righe, colonne)`.
    
- **`s.value_counts()`**: Usato su una singola colonna, conta la frequenza di ogni valore distinto (utilissimo per le colonne categoriali).
    

### 2. Selezione e Filtraggio Avanzato

Oltre alla sintassi con le parentesi quadre `df[...]`, Pandas mette a disposizione due indicizzatori fondamentali per selezionare dati in modo preciso:

- **`df.loc[...]`** _(Label-based)_: Seleziona dati usando **i nomi** di righe e colonne.
    
    Python
    
    ```
    df.loc[df['age'] > 30, ['name', 'email']]  # Filtra righe e prende solo colonne 'name' ed 'email'
    ```
    
- **`df.iloc[...]`** _(Index-based)_: Seleziona dati usando le **posizioni numeriche** (stile array).
    
    Python
    
    ```
    df.iloc[0:10, 0:3]  # Prime 10 righe, prime 3 colonne
    ```
    

### 3. Trasformazione ed Elaborazione

Come modificare i dati o applicare funzioni personalizzate.

- **`s.apply(fn)`**: Applica una funzione Python riga per riga su una colonna.
    
    - _Nota:_ È flessibile, ma sotto il cofano usa un ciclo Python standard, quindi è più lento delle operazioni native.
        
- **`df.assign(...)`**: Aggiunge o modifica colonne senza mutare il DataFrame originale (in perfetto stile funzionale).
    
- **`s.astype(...)`**: Converte il tipo di dato di una colonna (es. da stringa a intero o datetime).
    
- **`pd.to_datetime(...)`**: Funzione specializzata per parsare stringhe in veri oggetti data/ora.
    

### 4. Pulizia e Gestione Valori Nulli (NaN)

I dataset reali sono quasi sempre imprecisi o incompleti.

- **`df.isna()` / `df.notna()`**: Restituisce una maschera booleana per identificare i valori mancanti (`NaN`).
    
- **`df.dropna()`**: Rimuove interamente le righe o le colonne che contengono valori nulli.
    
- **`df.fillna(valore)`**: Sostituisce i valori nulli con un valore fisso, la media, o la mediana.
    
- **`df.drop_duplicates()`**: Elimina le righe duplicate.
    

### 5. Aggregazione e Combinazione (Stile SQL)

Per raggruppare, unire e sintetizzare i dati.

- **`df.groupby('colonna')`**: Divide il DataFrame in gruppi ed esegue calcoli su ciascuno (es. `.groupby('status')['total'].sum()`).
    
- **`pd.merge(df1, df2, on='id', how='left')`**: L'equivalente esatto della `JOIN` in SQL.
    
- **`pd.concat([df1, df2])`**: Concatena più DataFrame verticalmente (stack delle righe) o orizzontalmente.
    
- **`df.sort_values(by='colonna', ascending=False)`**: Ordina la tabella in base a una o più colonne.
    

### Un piccolo trucco sulle prestazioni: Vectorization vs `apply`

Molti sviluppatori che arrivano da JS tendono a usare `.apply()` per qualsiasi operazione, usandolo come un `.map()`:

Python

```
# Meno efficiente (usa un loop Python):
df['full_name'] = df.apply(lambda row: row['first'] + ' ' + row['last'], axis=1)

# Molto più veloce (vettorializzato nativamente):
df['full_name'] = df['first'] + ' ' + df['last']
```

Ove possibile, le operazioni dirette tra colonne sono sempre da preferire a `.apply()` per sfruttare il motore in C sottostante.

E se volessi semplicemente stampare i valori a terminale ?

In Python e Pandas ci sono diversi modi per stampare i dati a terminale, a seconda di cosa vuoi ottenere: l'intera tabella formattata, i singoli valori in un ciclo, o un output convertito in un formato noto (tipo JSON).

### 1. Il modo classico: `print(df)`

Se passi l'intero DataFrame a `print()`, Pandas formatta la tabella in automatico con righe, colonne e indici:

Python

```
print(df)
```

**Cosa succede se la tabella è enorme?** Per evitare di intasare il terminale con milioni di righe, Pandas tronca l'output mettendo dei puntini di sospensione (`...`). Se vuoi forzare il terminale a stampare **tutto** senza tagli, puoi cambiare temporaneamente le opzioni di configurazione:

Python

```
# Stampa tutte le righe e tutte le colonne
with pd.option_context('display.max_rows', None, 'display.max_columns', None):
    print(df)
```

### 2. Stampare i dati valore per valore (Ciclo / Iterazione)

Se vuoi scorrere i dati come faresti in Node.js con un `forEach`, Pandas ti offre due metodi principali per farlo a terminale:

#### A. Usando `.itertuples()` (Piu veloce e idiomatico)

Restituisce ogni riga sotto forma di una _NamedTuple_ di Python. È il modo più performante per ciclare su un DataFrame:

Python

```
for row in df.itertuples():
    # Accedi ai campi usando la dot notation
    print(f"User: {row.name}, Età: {row.age}")
```

#### B. Usando `.iterrows()`

Restituisce l'indice e una `Series` per ogni riga. Meno performante di `itertuples()`, ma ti permette di usare la sintassi a parentesi quadre:

Python

```
for index, row in df.iterrows():
    print(f"Riga {index} -> Nome: {row['name']}")
```

### 3. Convertire in Dizionari o JSON

Se ti serve stampare i dati nel classico formato ad oggetti JS per fare debug o loggare un output strutturato, puoi usare i metodi di esportazione nativi:

#### Come array di oggetti (stile Node/PostgreSQL)

Python

```
import json

# Converte il DataFrame in una lista di dizionari Python
records = df.to_dict(orient='records')

# Lo stampa formattato come JSON pulito
print(json.dumps(records, indent=2))
```

**Output a terminale:**

JSON

```
[
  {
    "id": 1,
    "name": "Alice",
    "age": 35
  },
  {
    "id": 2,
    "name": "Bob",
    "age": 42
  }
]
```

### 4. Trasformare una singola colonna in lista Python

Se vuoi solo stampare i valori di una specifica colonna come array semplice:

Python

```
# Stampa un array NumPy/lista di tutti i valori della colonna 'email'
print(df['email'].to_list())
# Output: ['alice@example.com', 'bob@example.com']
```

pandas é capace di prendere valori che vengono dal un JSON ?

Come si comporta con i valori "nested" ?

Sì, Pandas è perfettamente in grado di leggere file JSON o stringhe JSON, ed è uno degli usi più comuni quando si consumano API web.

Per leggere un JSON standard (flat, cioè senza annidamenti) basta una sola riga:

Python

```
import pandas as pd

# Da un file locale o da un URL di un'API
df = pd.read_json('dati.json')

# Oppure direttamente da una stringa JSON
df = pd.read_json(json_string)
```

Se il JSON contiene solo chiavi e valori semplici (`{"id": 1, "name": "Alice"}`), Pandas crea direttamente la tabella senza problemi.

### Come si comporta con i valori annidati (nested JSON)?

Se usi semplicemente `pd.read_json()` su un JSON strutturato in modo complesso (con oggetti annidati o array di oggetti), **Pandas NON loappiattisce da solo**. Inserisce semplicemente il sotto-oggetto o l'array JS direttamente dentro la cella come dizionario o lista Python.

#### Il problema del JSON annidato standard

Supponiamo di avere questo JSON da un'API:

JSON

```
[
  {
    "id": 1,
    "user": { "name": "Alice", "role": "admin" },
    "orders": [{ "item": "Laptop", "price": 1000 }, { "item": "Mouse", "price": 50 }]
  }
]
```

Se fai `pd.read_json()`, la colonna `user` conterrà letteralmente dei dizionari `{ 'name': 'Alice', ... }`, rendendo impossibile fare query vettoriali come `df['user.name']`.

### La soluzione: `pd.json_normalize()`

Per gestire e "srotolare" (flatten) la struttura annidata, Pandas offre la funzione dedicata **`pd.json_normalize()`**.

#### 1. Appiattire oggetti annidati semplici

Se devi solo convertire la struttura a punti (`user.name`, `user.role`), gli passi la lista di oggetti/dizionari:

Python

```
import json
import pandas as pd

data = [
    {"id": 1, "user": {"name": "Alice", "role": "admin"}},
    {"id": 2, "user": {"name": "Bob", "role": "dev"}}
]

# json_normalize trasforma le chiavi annidate in colonne
df = pd.json_normalize(data)

print(df)
```

**Risultato a terminale:**

|id|user.name|user.role|
|---|---|---|
|1|Alice|admin|
|2|Bob|dev|

Le chiavi interne diventano automaticamente nomi di colonna separati dal punto (`.`).

#### 2. Appiattire array annidati (es. `orders`)

Se il JSON contiene un array di sotto-elementi e vuoi creare **una riga separata per ogni elemento dell'array** mantenendo le informazioni dell'oggetto padre, usi il parametro `record_path`:

Python

```
data = [
    {
        "id": 1,
        "name": "Alice",
        "orders": [
            {"item": "Laptop", "price": 1000},
            {"item": "Mouse", "price": 50}
        ]
    },
    {
        "id": 2,
        "name": "Bob",
        "orders": [
            {"item": "Monitor", "price": 300}
        ]
    }
]

# Esplode 'orders' mantenendo le info del padre ('id', 'name')
df = pd.json_normalize(
    data, 
    record_path=['orders'], 
    meta=['id', 'name']
)

print(df)
```

**Risultato a terminale:**

|item|price|id|name|
|---|---|---|---|
|Laptop|1000|1|Alice|
|Mouse|50|1|Alice|
|Monitor|300|2|Bob|

In questo modo Alice viene duplicata su due righe (una per ogni ordine), esattamente come faresti con una `JOIN` SQL tra una tabella `users` e una tabella `orders`.