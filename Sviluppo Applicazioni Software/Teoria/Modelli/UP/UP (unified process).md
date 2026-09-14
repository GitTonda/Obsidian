Processo di [[Sviluppo iterativo ed evolutivo]] in cui le iterazioni iniziali sono guidate dal **rischio**, dalle esigenze del **cliente** e dall'**architettura**.

#### Caratteristiche Fondamentali:
- **Iterazioni brevi e time-boxed** (fissate nel tempo, tipicamente 2-6 settimane)
- Raffinamento evolutivo continuo di piani, requisiti e codice
- Gruppi auto-organizzati coordinati da riunioni ([[Scrum]])
- Sviluppo guidato dai test e refactoring continuo ([[XP (extreme programing)]], [[Codice e Test - TDD]])
- [[Agile Modeling]]

---

### La Matrice Bidimensionale di UP (Fasi vs Discipline) ⭐
UP organizza il ciclo di vita secondo due dimensioni ortogonali:
1. **Dimensione Temporale (Orizzontale)**: 4 Fasi sequenziali divise in **Iterazioni**.
2. **Dimensione Logica (Verticale)**: 9 Discipline (attività tecniche e manageriali).

![[UP stages.png]]

> [!IMPORTANT]
> **Trabocchetto d'Esame**: Le *Fasi* (Ideazione, Elaborazione, Costruzione, Transizione) sono separate temporalmente in sequenza. Le *Discipline* (Requisiti, Progettazione, Test...), invece, **NON sono separate a cascata**, ma si svolgono **contemporaneamente e si intrecciano in ogni singola iterazione**, variando solo l'intensità dello sforzo!

---

### Le 4 Fasi Temporali e le Loro Milestone:
1. **[[Ideazione]] (Inception)**:
   - Visione iniziale, studio di fattibilità economica e dei rischi principali.
   - Circa 10% dei requisiti/casi d'uso esaminati in dettaglio.
   - *Milestone*: **Obiettivi del Ciclo di Vita** (*Lifecycle Objectives*).
2. **[[Elaborazione]] (Elaboration)**:
   - Serie iniziale di iterazioni per sviluppare il nucleo dell'architettura ([[Architettura Logica]]).
   - Risoluzione della maggior parte dei rischi critici.
   - *Milestone*: **Architettura del Ciclo di Vita** (*Lifecycle Architecture*).
3. **Costruzione (Construction)**:
   - Implementazione iterativa e parallela di tutte le funzionalità rimanenti a basso rischio.
   - *Milestone*: **Capacità Operativa Iniziale** (*Initial Operational Capability*).
4. **Transizione (Transition)**:
   - Beta test, formazione utenti, correzione bug residui e rilascio in produzione.
   - *Milestone*: **Rilascio del Prodotto** (*Product Release*).

---

### Discipline ed Elaborati Principali di UP:
In ogni iterazione si eseguono attività appartenenti alle seguenti discipline:
- **Modellazione del Business**
- **[[Requisiti & Casi d'uso]]**: cattura requisiti funzionali (Casi d'Uso) e non funzionali (Specifiche Supplementari).
- **Progettazione e Architettura** ([[OOD A - UML e Pattern]]):
  - [[Architettura Logica]]: organizzazione in package e layer; separazione Modello-Vista.
  - [[Modello di Dominio]]: vocabolario concettuale del problema.
  - [[Diagramma di Sequenza di Sistema (SSD)]]: operazioni di sistema.
  - [[Contratti delle Operazioni]]: pre- e post-condizioni.
  - [[Modellazione dinamica e statica con UML]]: diagrammi di sequenza e Design Class Diagram (DCD).
  - Assegnazione delle responsabilità tramite i [[Pattern GRASP]] (vedi [[Esempio di Progettazione con GRASP]]).
  - Soluzioni architetturali avanzate tramite i [[Pattern GoF]].
- **Implementazione e Test**:
  - [[Codice e Test - TDD]]: mappatura al codice, Test-Driven Development e cicli di refactoring.
- **Rilascio, Gestione delle Configurazioni, Gestione Progetto, Infrastruttura**.

---

### Modello FURPS+ per i Requisiti:
- **F - Functional**: requisiti funzionali (feature, sicurezza, casi d'uso).
- **U - Usability**: usabilità, ergonomia, interfaccia umana.
- **R - Reliability**: affidabilità, tolleranza ai guasti, recuperabilità.
- **P - Performance**: prestazioni, tempi di risposta, throughput.
- **S - Supportability**: sostenibilità, manutenibilità, configurabilità.
- **+ (Plus)**: vincoli ulteriori (design, implementativi, interfaccia, fisici).

---
#### Correlati:
- [[Processi per lo sviluppo Software]]
- [[Guida Rapida ed Esercizi Esame SAS]]