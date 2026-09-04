#### Requisiti
è una capacità o condizione a cui il sistema deve essere conforme
###### Caratteristiche Requisito:
- breve descrizione
- stato (proposto, approvato, validato, incorporato)
- costi di implementazione
- priorità
- rischio associato all'implementazione
###### Flusso delle attività in UP:
- lista dei requisiti potenziali
- capire il contesto del sistema
- catturare i requisiti funzionali
	 comportamentali che definiscono il funzionamento del sistema (feature); catturati con i **Casi d'Uso**
- catturare i requisiti non funzionali
     proprietà del sistema nel complesso (sicurezza, ottimizzazione, ...); catturati nelle **Specifiche Supplementari**

#### Contesto del Sistema
- ###### Modellazione del dominio:
	 i concetti importanti sono oggetti di dominio in relazione tra di loro
- ###### Modellazione del business:
	 è super insieme del modello di dominio, descrive i processi di business

In UP si usano i **Casi d'Uso**, che rappresentano una maniera per utilizzare il sistema. (descrizione testuale)

#### Casi d'Uso
Sono descrizioni testuali di scenario di uso del sistema software
Un **Attore** è qualcosa o qualcuno dotato di comportamento:
- Primario
	 raggiunge gli obbiettivi utilizzando i servizi del sistema
- Supporto
	 offre un servizio al sistema
- Fuori Scena
	 ha un interesse nel comportamento del caso d'uso
Uno **Scenario** è una sequenza di azioni ed interazioni tra attori

- ###### Formato Breve:
	 riepilogo di un solo paragrafo per il solo scenario di successo
- ###### Formato informale:
	 più paragrafi. vari scenari, più dettagli del breve ma non tutti
- ###### Formato dettagliato:
	 tutti i passi e variazioni in dettaglio + precondizioni e garanzie di successo

#### Come scrivere un caso d'uso
- ###### Portata:
	 i confini del sistema di progettazione
- ###### Livello:
	 *livello di obbiettivo utente*, oppure *livello di sottofunzione*
- ###### Attore finale, Attore primario:
	 l'attore finale è colui che vuole raggiungere l'obbiettivo, mentre l'attore primario è colui che usa il sistema (spesso coincidono)
- ###### Parti interessate:
	 chi ha interessi nel raggiungimento dell'obbietivo
- ###### Pre-condizioni:
	 condizioni che devono per forza essere vere prima di iniziare uno scenario
- ###### Post-condizioni:
	 condizioni che saranno vere alla fine dello scenario
- ###### Estenzione:
	 formata da due parti:
	 - condizione: causa dell'eccezione
	 - gestione: come gestirla

#### Verifica casi d'uso
- ###### Il test del capo:
	 *"cosa avete fatto tutto il giorno?"* se il capo è felice, allora va bene
- ###### Il test EBP:
	 un processo di business elementare è un attività che *aggiunge un valore*
- ###### Il test della dimensione:
	 il formato dettagliato richede 3-10 pagine

