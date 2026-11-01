# nosiba-data-Progetto-Finale-_Fine_Modulo_Python
# ToysGroup · Esercitazione finale Python for Data Science

## Scenario
ToysGroup distribuisce giocattoli tramite una rete di rivenditori in diverse aree geografiche europee. I dati sono esportati in quattro file di formato diverso (CSV, Excel e JSON), da leggere, pulire, analizzare e modellare in Python.

## Contenuto del repository
|         File                                   |              Contenuto                                            |

| `Finale_Esercitazione_Python.ipynb`            | il notebook con tutti i task, eseguibile dall'inizio senza errori |
| `ESERCIZIO_M4-4-2_traccia_finale-products.csv` | anagrafica dei prodotti (ProductID, ProductName, Category, ListPrice) |
| `ESERCIZIO_M4-4-2_traccia_finale-regions.xlsx` | anagrafica delle regioni (RegionID, State, SalesRegion) |
| `ESERCIZIO_M4-4-2_traccia_finale-sales.json`   | le vendite, una riga per transazione |
| `ESERCIZIO_M4-4-2_traccia_finale-resellers.csv`| i rivenditori, con la colonna Churn |

## I task e i risultati principali

### Task 1 · Python di base sui dati forniti
Con liste, dizionari, set, cicli e funzioni Python (Pandas solo per leggere i file): 12 prodotti, 2000 vendite, 9 paesi in 3 macro-regioni e 3 categorie. Ogni prodotto ha una fascia di prezzo (4 Bassa, 4 Media, 4 Alta). La funzione `fatturato_categoria()` salta i valori mancanti e calcola il fatturato per categoria: Outdoor 331.998,40 €, Giocattoli 174.832,00 €, Giochi da tavolo 53.245,10 €. Con un ciclo while servono 56 transazioni per superare 10.000 € di fatturato cumulato.

### Task 2 · Lettura e ispezione dei dati
I quattro file sono letti in DataFrame e descritti con `.shape`, `.info()`, `.head()` e `.tail()`. In sales, Quantity e SalesAmount hanno 100 valori mancanti ciascuna e SalesDate è letta come testo. Con `.describe()`: SalesAmount va da 12,50 € a 2.457 €, con media 294,78 €. Con NumPy è calcolato il prezzo unitario di ogni vendita con un'operazione vettoriale; con `.loc[]` e una maschera booleana sono selezionati i 3 prodotti Outdoor sopra i 30 €, con `.iloc[]` una selezione di 10 righe e 3 colonne.

### Task 3a · Dati mancanti, filtri e ordinamento
I valori mancanti sono il 5% delle righe in Quantity e SalesAmount. Quantity è imputata con la mediana (5 pezzi), le 100 righe senza SalesAmount sono eliminate (restano 1900 vendite). Con `.query()` sono estratte le 162 vendite sopra il 90° percentile (756 €), ordinate per data crescente e importo decrescente.

### Task 3b · Aggregazioni e combinazione dei DataFrame
Sales è unita a products e regions con `merge()` (how="left"), mantenendo 1900 righe. Con `groupby()` e `agg()`: la categoria con il fatturato più alto è Outdoor, la macro-regione Centro Europa. Le vendite 2025 (640) e degli altri anni (1260) sono separate con un filtro e ricomposte con `pd.concat()` (1900 righe).

### Task 3c · Serie temporali di vendita
SalesDate è convertita in datetime64; con `.dt` sono estratti anno, mese e giorno della settimana. La serie mensile ha 36 righe (gennaio 2023 - dicembre 2025); il mese con il fatturato massimo è maggio 2025 (29.368,30 €). Le vendite con Recente = True (entro 180 giorni dall'ultima data) sono 315, il 16,58% del totale.

### Task 4 · Visualizzazione e storytelling
Tre grafici, scelti in base al tipo di dato, ognuno con un titolo che dice il risultato: un grafico a linee del fatturato mensile con il picco di maggio 2025 evidenziato, un boxplot di SalesAmount per categoria (Outdoor ha le vendite più alte e più variabili) e una heatmap delle correlazioni dei rivenditori (il churn è legato ai giorni dall'ultimo ordine, r = 0,43, non a sconti e reclami).

### Task 5 · Correlazione e regressione lineare
Su una tabella mensile, la coppia più correlata è fatturato e quantità (r = 0,87). Modello semplice (fatturato sull'indice del mese): ogni mese il fatturato previsto cresce di circa 303,58 €; R² 0,477 sul train e -1,879 sul test. Modello multiplo (indice del mese, quantità, rivenditori attivi): R² 0,836 sul train e -0,415 sul test. Con solo 36 mesi e 8 nel test nessun modello è affidabile per prevedere; le previsioni per gennaio-marzo 2026 (circa 21.835-22.442 €) vanno prese con cautela.

### Task 6a · Classificazione del churn dei rivenditori
Regressione logistica con StandardScaler in una pipeline, split stratificato (240 train, 60 test). Con soglia 0,5 il recall è 0,286, con soglia 0,3 sale a 0,714 con la stessa accuracy (0,767, uguale alla baseline): è scelta la soglia 0,3, perché perdere un rivenditore costa più di un falso allarme. AUC 0,761. Accuracy e recall simili su train e test: buon adattamento.

### Task 6b · Segmentazione dei rivenditori con K-Means
Con fatturato per rivenditore, ordini e giorni dall'ultimo ordine standardizzati, il metodo del gomito porta a k = 3. I segmenti sono Attivi, Dormienti e Top clienti; il segmento con il churn più alto è Dormienti (39%), contro circa il 10% degli altri due.

### Task 7 · Debugging, refactoring e versioning
Quattro frammenti di codice corretti con l'aiuto di un assistente AI, riportando per ciascuno errore originale, causa, prompt e versione corretta: media con valori mancanti (Caso 1), data leakage dello scaler (Caso 2), filtro con `and` e categoria inesistente (Caso 3), refactoring di una funzione con nomi espliciti e docstring (Caso 4).

## Istruzioni di esecuzione
1. Scaricare il repository e tenere il notebook e i quattro file di dati nella stessa cartella.
2. Aprire il notebook con Jupyter (Anaconda).
3. Librerie usate: pandas, numpy, matplotlib, seaborn, scikit-learn.
4. Eseguire tutte le celle dall'inizio con **Kernel → Restart Kernel and Run All Cells**.
