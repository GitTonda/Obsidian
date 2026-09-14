I pattern **GRASP** (*General Responsibility Assignment Software Patterns*), formalizzati da Craig Larman, descrivono i principi fondamentali per l'assegnazione delle responsabilità a classi e oggetti nella progettazione orientata agli oggetti (OOD).

---

### Progettazione Guidata dalle Responsabilità (RDD - Responsibility-Driven Design)
L'analisi e progettazione OO considerano un sistema software come una **comunità di oggetti che collaborano** scambiandosi messaggi per soddisfare compiti complessi.

#### Due Tipi di Responsabilità:
1. **Responsabilità di Fare (*Doing*):**
   - Fare qualcosa in prima persona (es. eseguire un calcolo, creare un oggetto).
   - Iniziare un'azione in altri oggetti.
   - Controllare e coordinare attività tra oggetti.
2. **Responsabilità di Conoscere (*Knowing*):**
   - Conoscere i propri dati privati incapsulati.
   - Conoscere gli oggetti correlati.
   - Conoscere cose che è possibile calcolare o ricavare.

> [!NOTE]
> I passi di RDD:
> 1. Identificazione delle responsabilità a partire dai requisiti.
> 2. Assegnazione di queste responsabilità alle classi appropriate.
> 3. Indagine di come soddisfare tali responsabilità mediante la collaborazione (messaggi).

---

### I 9 Pattern GRASP Fondamentali

#### 1. Information Expert (Esperto dell'Informazione)
- **Problema**: Qual è il principio generale più basilare per assegnare le responsabilità agli oggetti?
- **Soluzione**: Assegnare la responsabilità alla classe che **possiede le informazioni necessarie** per soddisfarla.
- **Esempio (NextGen POS)**:
  - Chi deve calcolare il totale di una vendita?
  - `Sale` possiede l'elenco di tutti i `SalesLineItem`, quindi `Sale` è l'Information Expert per la somma totale.
  - Chi deve calcolare il subtotale di una riga? `SalesLineItem` conosce la `quantity` e l'associazione a `ProductDescription`, quindi calcola il proprio subtotale.
  - Chi conosce il prezzo unitario? `ProductDescription` ha l'attributo `price`.
- **Vantaggi**: Mantiene l'incapsulamento delle informazioni, favorisce un basso accoppiamento e un'alta coesione.

---

#### 2. Creator (Creatore)
- **Problema**: Chi deve avere la responsabilità di creare una nuova istanza di una classe `A`?
- **Soluzione**: Assegnare alla classe `B` la responsabilità di creare un'istanza di `A` se una o più delle seguenti condizioni sono vere (preferibilmente più di una):
  - `B` "contiene" o compone in modo aggregato oggetti `A`.
  - `B` registra o tiene traccia di istanze di `A`.
  - `B` usa strettamente oggetti `A`.
  - `B` possiede i dati di inizializzazione per creare `A` (cioè `B` è un *Information Expert* rispetto alla creazione di `A`).
- **Esempio**: `Sale` crea `SalesLineItem`, perché `Sale` contiene e aggrega le righe di vendita.
- **Vantaggi**: Supporta il basso accoppiamento evitando di introdurre classi esterne per la creazione.

---

#### 3. Low Coupling (Basso Accoppiamento)
- **Problema**: Come supportare una bassa dipendenza tra classi, un elevato riuso e un ridotto impatto dei cambiamenti?
- **Soluzione**: Assegnare le responsabilità in modo tale che l'accoppiamento (il numero di dipendenze tra classi) rimanga **basso**.
- **Caratteristica Chiave**: È un **principio valutativo** da tenere a mente durante tutte le decisioni di progetto; non dice direttamente a chi assegnare un metodo, ma permette di scegliere la soluzione migliore tra alternative concorrenti.
- **Vantaggi**: Classi isolate dai cambiamenti delle altre classi, maggiore facilità di comprensione e manutenzione, massimo riutilizzo.

---

#### 4. High Cohesion (Alta Coesione)
- **Problema**: Come mantenere le classi focalizzate, comprensibili, gestibili e contemporaneamente sostenere *Low Coupling*?
- **Soluzione**: Assegnare le responsabilità in modo tale che la coesione rimanga **alta**. Una classe ha alta coesione se le sue responsabilità sono fortemente correlate tra loro e non svolge un numero eccessivo di compiti disomogenei.
- **Caratteristica Chiave**: Altro **principio valutativo**. Lavora in tandem con Low Coupling.
- **Attenzione alle "God Classes"**: Classi che fanno troppe cose diverse violano High Cohesion, diventano fragili e difficili da mantenere.

---

#### 5. Controller (Controllore)
- **Problema**: Qual è il primo oggetto oltre lo strato UI che riceve e coordina un'operazione di sistema?
- **Soluzione**: Assegnare la responsabilità di ricevere e gestire i messaggi delle operazioni di sistema a una classe che rappresenta:
  - Il sistema complessivo, un dispositivo radice o un sottosistema (**Facade Controller**, es. `Register`, `Store`, `Sistema`).
  - Lo scenario del caso d'uso in cui ha luogo l'operazione (**Use Case Controller**, es. `ProcessSaleHandler`, `ProcessSaleSession`).
- **Regole Fondamentali per l'Esame**:
  - Le finestre e i widget della GUI **NON** sono Controller GRASP (violerebbero la [[Architettura Logica#Principio di Separazione Modello-Vista|separazione Modello-Vista]]). La GUI delega al Controller.
  - **Evitare il Fat Controller (Controller Gonfio)**: Un controller soffre di bassa coesione se contiene tutta la logica applicativa del caso d'uso. Il Controller deve solo coordinare e **delegare** il lavoro agli oggetti dello strato di dominio!

---

#### 6. Polymorphism (Polimorfismo)
- **Problema**: Come gestire varianti di comportamento basate sul tipo? Come progettare componenti software facilmente intercambiabili?
- **Soluzione**: Quando comportamenti correlati variano in base al tipo (classe), assegnare la responsabilità del comportamento usando **operazioni polimorfiche** (interfacce o classi astratte) anziché logiche condizionali (`switch` o `if-else`).
- **Vantaggi**: Facilita l'aggiunta di nuovi comportamenti futuri senza dover modificare il codice esistente (Open/Closed Principle).

---

#### 7. Pure Fabrication (Invenzione Pura / Fabbricazione Pura)
- **Problema**: A chi assegnare la responsabilità quando non si vuole violare *High Cohesion* e *Low Coupling*, ma le soluzioni suggerite da *Information Expert* non sono adeguate o sporcherebbero una classe di dominio?
- **Soluzione**: Assegnare un insieme coeso di responsabilità a una classe "artificiale" di comodo che **non rappresenta alcun concetto del dominio del problema** (es. classi di accesso a database, logging, gestione transazioni, adapter).
- **Esempio**: Una classe `Sale` non dovrebbe essere responsabile di salvare se stessa sul database relazionale SQL (violerebbe la coesione). Si inventa una classe pura `PersistentStorage` o `SaleDAO`.
- **Vantaggi**: Preserva la purezza e la coesione del modello di dominio, incapsulando dettagli tecnici nei servizi di supporto.

---

#### 8. Indirection (Indirezione)
- **Problema**: Dove assegnare la responsabilità per evitare l'accoppiamento diretto tra due o più elementi?
- **Soluzione**: Assegnare la responsabilità a un **oggetto intermediario** che faccia da ponte o mediatore tra i componenti.
- **Relazione con altri pattern**: È il principio fondante di moltissimi pattern architetturali e GoF, tra cui [[Pattern GoF#Adapter|Adapter]], Facade, Proxy e Controller.

---

#### 9. Protected Variations (Variazioni Protette)
- **Problema**: Come progettare oggetti, sottosistemi e sistemi in modo tale che le variazioni o l'instabilità in questi elementi non abbiano impatto negativo sugli altri?
- **Soluzione**: Identificare i punti di variazione o instabilità prevista; assegnare le responsabilità per creare un'**interfaccia stabile** attorno ad essi (incapsulare il concetto che varia).
- **Importanza Teorica**: È il principio fondamentale dell'ingegneria del software che unifica:
  - *Information Hiding* (Parnas)
  - *Open/Closed Principle - OCP* (Meyer/Martin)
  - *Liskov Substitution Principle - LSP*
  - Quasi tutti i pattern GoF (es. Adapter, Strategy, State, Factory).

---
#### Correlati:
- [[UP (unified process)]]
- Fase di: [[Elaborazione]]
- Usato nella: [[Modellazione dinamica e statica con UML]]
- Esempio pratico applicato: [[Esempio di Progettazione con GRASP]]
- Realizzato tramite i design pattern: [[Pattern GoF]]
- Rispetto dell'[[Architettura Logica]]
