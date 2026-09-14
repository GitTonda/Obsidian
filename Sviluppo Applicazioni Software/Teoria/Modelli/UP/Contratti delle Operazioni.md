
Elaborato di [[UP (unified process)]] (fase di [[Elaborazione]]) che usa **pre-condizioni** e **post-condizioni** per descrivere nel dettaglio i cambiamenti dello stato del sistema a seguito delle operazioni identificate negli [[Diagramma di Sequenza di Sistema (SSD)]].

###### Post Condizioni:
descrivono i cambiamenti di stato degli oggetti del modello di dominio (oggetti creati, collegamenti formati / rotti e attributi modificati)
Non sono azioni, bensì osservazioni sugli oggetti che risultano al termine dell'operazione.

###### Pre Condizioni:
sono ipotesi significative sullo stato del sistema prima dell'esecuzione dell'operazione.
Utili per indicare gli oggetti rilevanti in quel punto del caso d'uso.

#### Struttura del Contratto:
- **Operazione**: nome dell'operazione e parametri (espressi negli SSD)
- **Riferimenti**: [[Requisiti & Casi d'uso|Casi d'uso]] correlati
- **Pre-condizioni**: assunzioni notevoli sullo stato del sistema prima dell'esecuzione
- **Post-condizioni**: stato degli oggetti del [[Modello di Dominio]] dopo l'esecuzione (istanziamento, modifica attributi, creazione/distruzione associazioni)

##### Come creare un Contratto:
1. Identificare le operazioni di sistema dagli SSD
2. Creare un contratto per le operazioni complesse o i cui effetti sono sottili o non chiare
3. descrivere post condizioni:
	 - creazione o cancellazione di un oggetto
	 - formazione o rottura di collegamento
	 - modifica di attributo

#### ! Nota
Un'operazione di sistema implica una trasformazione. se non ha post condizioni allora si tratta di un **interrogazione**

---
#### Correlati:
- [[UP (unified process)]]
- [[Elaborazione]]
- Modella i cambiamenti del: [[Modello di Dominio]]
- Input dagli eventi di: [[Diagramma di Sequenza di Sistema (SSD)]]
- Guida la progettazione dinamica: [[Modellazione dinamica e statica con UML]]
- Applicazione pratica: [[Esempio di Progettazione con GRASP]]
- [[Guida Rapida ed Esercizi Esame SAS]]

