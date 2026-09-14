Passaggio fondamentale durante la fase di [[Elaborazione]] in [[UP (unified process)]]: dai requisiti ("fare la cosa giusta") alla progettazione ad oggetti ("fare la cosa bene").

Si definisce una soluzione software che realizzi gli obiettivi dell'iterazione corrente, guidata dalle responsabilità ([[Pattern GRASP]]).

---

### Artefatti di Input per la Progettazione
Per progettare gli oggetti software, si parte dagli artefatti prodotti durante l'analisi dei requisiti:
- [[Requisiti & Casi d'uso]]: descrivono il comportamento atteso e gli scenari utente.
- [[Modello di Dominio]]: fornisce i concetti e il vocabolario del dominio reale (riduzione del representational gap).
- [[Diagramma di Sequenza di Sistema (SSD)]]: identifica gli eventi di sistema e le operazioni di sistema in ingresso.
- [[Contratti delle Operazioni]]: specificano le pre-condizioni e post-condizioni di ciascuna operazione di sistema.

---

### Modellazione Dinamica vs Modellazione Statica

In UML esistono due prospettive complementari per modellare il software ad oggetti:

| Aspetto | **Modellazione Dinamica** | **Modellazione Statica** |
| :--- | :--- | :--- |
| **Scopo** | Mostra il comportamento nel tempo, lo scambio di messaggi e la collaborazione | Mostra la struttura permanente, classi, attributi, relazioni e visibilità |
| **Diagrammi Principali** | **Diagrammi di Sequenza**, Diagrammi di Comunicazione | **Design Class Diagram (DCD)**, Diagrammi dei Package |
| **Cosa definisce** | I metodi da eseguire e l'ordine delle interazioni | La struttura delle classi e le firme dei metodi |
| **Ruolo nella Progettazione** | **Si fa PRIMA**: qui si decide *chi fa cosa* assegnando le responsabilità con i pattern [[Pattern GRASP]] | **Si fa DOPO**: riflette e sintetizza le decisioni prese nei modelli dinamici |

> [!IMPORTANT]
> **Regola d'oro di Larman**: Il lavoro di progettazione intellettualmente più difficile e importante avviene durante la **modellazione dinamica** (diagrammi di interazione), dove si scelgono gli oggetti e si assegnano loro le responsabilità attraverso i messaggi. Il diagramma statico delle classi di progetto (DCD) è una semplice conseguenza di ciò che è emerso nei diagrammi di interazione!

---

### 1. Diagrammi di Sequenza (Modellazione Dinamica)

I diagrammi di sequenza mostrano gli oggetti che partecipano all'interazione affiancati in orizzontale, e il tempo che scorre verso il basso lungo le **linee di vita** (*lifelines*).

#### Elementi Notazionali Chiave:
1. **Messaggio Trovato (Found Message)**: freccia orizzontale che entra dall'esterno verso il primo oggetto (il [[Pattern GRASP#5. Controller|Controller]]). Rappresenta l'avvio dell'operazione di sistema.
2. **Linee di Vita (Lifeline)**: rettangolo con nome oggetto (`nomeOggetto:Classe` o `:Classe`) e linea tratteggiata verticale.
3. **Barre di Attivazione (Activation Box)**: rettangoli verticali sulla linea di vita che indicano il periodo in cui l'oggetto è attivo nell'esecuzione di un metodo.
4. **Messaggi Sincroni**: freccia con punta piena (`—>`). Il chiamante attende il completamento.
5. **Risposte o Ritorni (Return)**: freccia tratteggiata aperta (`- - >`), spesso omessa se banale.
6. **Creazione di Istanze**: messaggio `<<create>>` diretto verso il rettangolo della nuova istanza (o costruttore della classe).
7. **Distruzione di Oggetti**: contrassegnata da una grande `X` in fondo alla linea di vita.
8. **Self-call**: freccia che parte e ritorna sulla stessa linea di vita per invocare un metodo interno.

#### Frame di Controllo UML 2.x:
- `alt`: rami condizionali mutuamente esclusivi (`if-else`).
- `opt`: esecuzione opzionale (`if` semplice).
- `loop`: iterazione (ripetizione su una collezione di elementi).
- `ref`: riferimento a un altro diagramma di interazione (scomposizione modulare).

---

### 2. Design Class Diagram - DCD (Modellazione Statica)

Il **DCD** è un diagramma delle classi UML utilizzato da una prospettiva software: non rappresenta concetti del mondo reale, ma vere classi software con specifiche di implementazione.

#### Differenza: Modello di Dominio vs DCD
- **[[Modello di Dominio]]** (Concettuale): classi concettuali del mondo reale, attributi informali, **NESSUN metodo**.
- **DCD** (Software di Progetto): classi software reali, tipi di dato precisi, **metodi con firme complete**, navigabilità delle associazioni, visibilità.

#### Elementi di un DCD:
- **Sezioni della Classe**: Nome, Attributi (con tipo e visibilità), Metodi/Operazioni (con parametri e tipo di ritorno).
- **Visibilità**:
  - `+` Pubblica (*public*)
  - `-` Privata (*private*)
  - `#` Protetta (*protected*)
  - `~` Package (*default/package*)
- **Navigabilità**: freccia aperta sull'associazione che indica che un oggetto conosce e può inviare messaggi all'altro (implementata tramite una variabile di istanza/attributo).

---

### Tipi di Visibilità tra Oggetti (Fondamentale)
Perché un oggetto `A` possa inviare un messaggio a un oggetto `B`, `B` deve essere **visibile** ad `A`. Esistono 4 forme di visibilità:
1. **Visibilità per Attributo (Attribute Visibility)**: `B` è un attributo/campo di istanza di `A` (visibilità permanente).
2. **Visibilità per Parametro (Parameter Visibility)**: `B` viene passato come argomento in un metodo di `A`.
3. **Visibilità Locale (Local Visibility)**: `B` viene istanziato o dichiarato come variabile locale all'interno di un metodo di `A`.
4. **Visibilità Globale (Global Visibility)**: `B` è accessibile globalmente (ad esempio tramite una variabile globale, un metodo statico o il pattern [[Pattern GoF#Singleton|Singleton]]).

---
#### Correlati:
- [[UP (unified process)]]
- Fase di: [[Elaborazione]]
- Input da: [[Requisiti & Casi d'uso]], [[Modello di Dominio]], [[Diagramma di Sequenza di Sistema (SSD)]], [[Contratti delle Operazioni]]
- Regole di assegnazione responsabilità: [[Pattern GRASP]]
- Esempio pratico: [[Esempio di Progettazione con GRASP]]
- Struttura dei package: [[Architettura Logica]]
- Pattern architetturali avanzati: [[Pattern GoF]]
- Implementazione e verifica: [[Codice e Test - TDD]]

