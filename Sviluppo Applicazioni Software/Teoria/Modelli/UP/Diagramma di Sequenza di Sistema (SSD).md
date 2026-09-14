Elaborato della disciplina dei requisiti (sviluppato in [[Elaborazione]]) che illustra gli eventi di input e output relativi ai sistemi in discussione.

Mostra gli *eventi di sistema* generati dagli attori e il loro ordine temporale:
- Espressi tramite notazione [[OOD A - UML e Pattern|UML]]
- Modello a scatola nera (*black box*)
- Un SSD per ogni [[Requisiti & Casi d'uso|caso d'uso]] (scenario principale e alternativi)
- Costituisce l'input fondamentale per i [[Contratti delle Operazioni]]

Un SSD mostra:
- L'attore primario del caso d'uso
- Il sistema in discussione
- I passi che rappresentano le interazioni tra il sistema e l'attore

#### Eventi di Sistema:
Un attore genera **eventi di sistema** che richiedono al sistema l'esecuzione di **operazioni di sistema**.

3 tipi di evento:
- Eventi esterni
- Eventi temporali
- Guasti o eccezioni

---
#### Correlati:
- [[UP (unified process)]]
- [[Elaborazione]]
- Derivato da: [[Requisiti & Casi d'uso]]
- Input per: [[Contratti delle Operazioni]]
- Realizzato mediante: [[Modellazione dinamica e statica con UML]] (le operazioni di sistema diventano i messaggi iniziali ricevuti dal [[Pattern GRASP#5. Controller|Controller GRASP]])
- Esempio pratico: [[Esempio di Progettazione con GRASP]]
