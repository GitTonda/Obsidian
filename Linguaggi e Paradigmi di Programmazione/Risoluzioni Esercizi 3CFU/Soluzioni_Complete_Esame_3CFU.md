# Svolgimento Completo Esercizi d'Esame — LPP 3 CFU
**Insegnamento:** Linguaggi e Paradigmi di Programmazione (Modulo 3 CFU — Teoria delle Categorie e Haskell)  
**Docente:** Prof. Luca Roversi — Università degli Studi di Torino  
**Descrizione:** Risoluzione integrale e formale di tutti gli **83 esercizi d'esame** estratti dai 4 fascicoli ufficiali (`Esame-domande-Parte01.pdf`, `Parte02.pdf`, `Parte03.pdf`, `Parte04.pdf`). Le risposte sono redatte nello stile formale, sintetico e rigoroso richiesto in sede d'esame scritto.

---

## Indice Generale

### [Parte 1 — Categorie, Ordini, Monoidi Categoriali e Costruzioni Universali](#parte-1--categorie-ordini-monoidi-categoriali-e-costruzioni-universali)
- **CT0050 Prime Categorie e Ordini** (Esercizi 1 – 10)
  - Pre-ordini, ordini parziali (poset) e ordini totali come categorie; identità e transitività; unicità delle frecce; grafi vs categorie.
- **CT0100 Monoidi Categoriali** (Esercizi 11 – 15)
  - Monoidi come categorie ad 1 oggetto con frecce come sezioni $(x \odot)$: booleani $(\land, \lor, \oplus)$, stringhe $(\Sigma^*, \cdot, \epsilon)$, liste $(List, ++, [])$.
- **CT0150 Categoria Set, Monomorfismi, Epimorfismi, Oggetto Terminale e Prodotto** (Esercizi 16 – 27)
  - Generalizzazione di iniettività e suriettività; unicità a meno di isomorfismo dell'oggetto terminale e del prodotto; analisi dei candidati prodotto in Haskell.
- **CT0200 Costruzioni Universali e Coprodotto** (Esercizi 28 – 35)
  - Proprietà universale del coprodotto; analisi dei candidati in Haskell; isomorfismi fondamentali (`Either a Void ~= a`, distributività, `Either a a ~= (Either () (), a)`).

### [Parte 2 — Funtori e Prove di Functorialità](#parte-2--funtori-e-prove-di-functorialità)
- **CT0300 Funtori Matematici e in Haskell** (Esercizi 1 – 21)
  - Funtore Powerset e Funtore Stella di Kleene $(-)^*$.
  - Definizione di `fmap` e dimostrazioni delle 2 leggi dei funtori (identità e composizione) per `Maybe`, `List`, `Either a`, alberi `BTree n l`, `BTree n`, alberi mutiforcati `T a`, tipo `U a b`.
  - Diagrammi commutativi in $\mathbf{Set}$ partendo da $f: a \to b$ e decorazione con elementi.

### [Parte 3 — Funtori Applicativi e Legge di Composizione](#parte-3--funtori-applicativi-e-legge-di-composizione)
- **CT0380 Funtori Applicativi: Motivazione** (Esercizio 1)
  - Utilità pratica per programmatori: funzioni a più argomenti in contesti, indipendenza computazionale vs monadi, accumulo di errori.
- **CT0390 Funtori Applicativi in Haskell** (Esercizi 2 – 9)
  - Definizioni di `pure` e `(<*>)`, e dimostrazioni dettagliate della Legge di Composizione Applicativa per: `Maybe`, `Either a`, `List`, `Reader r`, `Diagonal`, `Const c`, `ZipList`, `Identity`.

### [Parte 4 — Trasformazioni Naturali e Monadi](#parte-4--trasformazioni-naturali-e-monadi)
- **CT0450 Trasformazioni Naturali in Haskell** (Esercizi 1 – 8)
  - Definizione categoriale e in Haskell; verifiche formali del quadrato di naturalità per: `maybeToList`, `listToMaybe`, `toEitherUnit`, `fromEitherUnit`, `headEither`, `eitherToList`.
- **CT0530 Monadi in Haskell** (Esercizi 9 – 18)
  - Definizioni di `(>>=)` e dimostrazioni della Legge di Associatività del Bind per: `Maybe`, `Either u`, `List`, `Diagonal`, `Const c`, `Identity`.
  - Monadi `Reader r` e `State s`: definizioni di `fmap`, `pure`, `(<*>)`, `(>>=)` con le giustificazioni concettuali ed essenziali dei lucidi di Roversi.

---


# Parte 1 — Categorie, Ordini, Monoidi Categoriali e Costruzioni Universali
**Corso:** Linguaggi e Paradigmi di Programmazione (3 CFU)  
**Docente:** Prof. Luca Roversi  
**Argomenti:** Categorie elementari, Pre-ordini, Poset, Ordini totali, Monoidi categoriali, Categoria $\mathbf{Set}$, Monomorfismi, Epimorfismi, Oggetto terminale, Prodotto cartesiano, Coprodotto.

---

## CT0050: Prime Categorie e Ordini

### Esercizio 1
> **Testo:** Prove that any pre-order is a category.  
> **Hint:** Recall def. of pre-order; show that reflexivity implies the existence of identity using transitivity; show that transitivity is arrow composition.

**Soluzione:**
1. **Definizione di Pre-ordine:** Un pre-ordine è una coppia $(X, \le)$ dove $X$ è un insieme e $\le \subseteq X \times X$ è una relazione binaria:
   - **Riflessiva:** $\forall a \in X.\; a \le a$
   - **Transitiva:** $\forall a, b, c \in X.\; (a \le b \land b \le c) \implies a \le c$
2. **Costruzione della Categoria $\mathbf{C}$:**
   - **Oggetti:** $\mathrm{obj}(\mathbf{C}) = X$.
   - **Morfismi (frecce):** Per ogni coppia di oggetti $a, b \in X$, l'insieme dei morfismi $\mathbf{C}(a, b)$ contiene un unico elemento se $a \le b$, ed è vuoto altrimenti:
     $$\mathbf{C}(a, b) = \begin{cases} \{(a, b)\} & \text{se } a \le b \\ \emptyset & \text{altrimenti} \end{cases}$$
3. **Morfismo Identità:**
   Per la proprietà riflessiva del pre-ordine, per ogni $a \in X$ vale $a \le a$. Dunque esiste sempre il morfismo $\mathrm{id}_a = (a, a) \in \mathbf{C}(a, a)$.
4. **Composizione di Morfismi:**
   Siano $f = (a, b) \in \mathbf{C}(a, b)$ e $g = (b, c) \in \mathbf{C}(b, c)$. Ciò implica che $a \le b$ e $b \le c$. Per la transitività della relazione $\le$, ne segue che $a \le c$. Definiamo quindi la composizione:
   $$g \circ f = (a, c) \in \mathbf{C}(a, c)$$
5. **Assiomi di Categoria:**
   - **Unità (Identity laws):** Per ogni freccia $f = (a, b) \in \mathbf{C}(a, b)$:
     $$\mathrm{id}_b \circ f = (b, b) \circ (a, b) = (a, b) = f$$
     $$f \circ \mathrm{id}_a = (a, b) \circ (a, a) = (a, b) = f$$
   - **Associatività:** Date tre frecce componibili $f = (a, b)$, $g = (b, c)$, $h = (c, d)$:
     $$(h \circ g) \circ f = (a, d) = h \circ (g \circ f)$$
   *Nota fondamentale:* Poiché tra due qualsiasi oggetti c'è al massimo un morfismo ($|\mathbf{C}(x, y)| \le 1$), qualsiasi diagramma parallelo commuta banalmente, e gli assiomi di categoria sono automaticamente soddisfatti. $\blacksquare$

---

### Esercizio 2
> **Testo:** Write an example of pre-order and the corresponding commuting diagram to show that the pre-order is a category.

**Soluzione:**
Sia $X = \{a, b, c\}$. Definiamo la relazione di pre-ordine $\le$ come:
$$\le \;=\; \{(a, a), (b, b), (c, c), (a, b), (b, c), (a, c), (b, a)\}$$
*(Nota: questo è un pre-ordine ma non un ordine parziale, perché $a \le b$ e $b \le a$ ma $a \ne b$, cioè $a$ e $b$ sono isomorfi).*

**Diagramma commutativo:**
```mermaid
graph LR
    a((a)) -->|id_a| a
    b((b)) -->|id_b| b
    c((c)) -->|id_c| c
    a -->|(a,b)| b
    b -->|(b,a)| a
    b -->|(b,c)| c
    a -->|(a,c)| c
```
- La composizione $(b, c) \circ (a, b) = (a, c)$ commuta.
- La composizione $(b, a) \circ (a, b) = (a, a) = \mathrm{id}_a$ commuta.
- La composizione $(a, b) \circ (b, a) = (b, b) = \mathrm{id}_b$ commuta.

---

### Esercizio 3
> **Testo:** Prove that any partial-order is a category.

**Soluzione:**
Un ordine parziale (poset) è una coppia $(X, \le)$ tale che $\le$ è:
1. Riflessiva: $\forall a.\; a \le a$
2. Transitiva: $\forall a, b, c.\; a \le b \land b \le c \implies a \le c$
3. Antisimmetrica: $\forall a, b.\; a \le b \land b \le a \implies a = b$

Poiché un ordine parziale soddisfa riflessività e transitività, esso è in particolare un **pre-ordine**.
Per la dimostrazione svolta nell'**Esercizio 1**, ogni pre-ordine definisce una categoria dove:
- Gli oggetti sono gli elementi di $X$.
- I morfismi sono le coppie $(a, b)$ con $a \le b$.
- Le identità sono garantite dalla riflessività.
- La composizione è garantita dalla transitività.
- Associatività e unità valgono banalmente perché $|\mathbf{C}(a, b)| \le 1$.
Dunque ogni ordine parziale è una categoria. (L'antisimmetria aggiunge l'ulteriore proprietà che se $a \cong b$, allora $a = b$, ovvero la categoria è *scheletrica*). $\blacksquare$

---

### Esercizio 4
> **Testo:** Write an example of partial-order and the corresponding commuting diagram to show that the pre-order is a category. Highlight a possible graph that cannot be a sub-graph of the commuting diagram corresponding to the given example.

**Soluzione:**
1. **Esempio:** Sia $X = \{a, b, c\}$ con la relazione:
   $$\le \;=\; \{(a, a), (b, b), (c, c), (a, b), (b, c), (a, c)\}$$
2. **Diagramma commutativo:**
   ```mermaid
   graph LR
       a((a)) -->|(a,b)| b((b))
       b -->|(b,c)| c((c))
       a -->|(a,c)| c
   ```
   *(I cappi di identità su ciascun nodo sono impliciti).*
3. **Grafo che NON può essere un sottografo:**
   Un grafo contenente un ciclo tra nodi distinti, ad esempio:
   ```mermaid
   graph LR
       a((a)) --> b((b))
       b --> a
   ```
   **Motivazione:** In un ordine parziale vale l'antisimmetria ($a \le b \land b \le a \implies a = b$). La presenza contemporanea di una freccia $a \to b$ e di una freccia $b \to a$ con $a \ne b$ violerebbe l'antisimmetria. Inoltre, non possono esistere due frecce parallele distinte tra gli stessi due nodi ($|\mathbf{C}(x, y)| \le 1$).

---

### Esercizio 5
> **Testo:** Prove that any total-order is a category.

**Soluzione:**
Un ordine totale è un ordine parziale $(X, \le)$ che soddisfa la proprietà di **totalità (o tricotomia/connessione)**:
$$\forall a, b \in X.\; a \le b \;\lor\; b \le a$$
Poiché un ordine totale è a tutti gli effetti un ordine parziale (e quindi un pre-ordine), la costruzione della categoria è identica a quella degli Esercizi 1 e 3:
- $\mathrm{obj}(\mathbf{C}) = X$.
- $\mathbf{C}(a, b) = \{(a, b)\}$ se $a \le b$, altrimenti $\emptyset$.
- Riflessività $\implies$ esistenza di $\mathrm{id}_a = (a, a)$.
- Transitività $\implies$ composizione $(b, c) \circ (a, b) = (a, c)$.
- Gli assiomi di categoria valgono banalmente poiché $|\mathbf{C}(a, b)| \le 1$. $\blacksquare$

---

### Esercizio 6
> **Testo:** Write an example of total-order and the corresponding commuting diagram to show that the pre-order is a category. Highlight a possible graph that cannot be a sub-graph of the commuting diagram corresponding to the given example.

**Soluzione:**
1. **Esempio:** L'insieme $X = \{1, 2, 3\}$ con il consueto ordine $\le$:
   $$\le \;=\; \{(1,1), (2,2), (3,3), (1,2), (2,3), (1,3)\}$$
2. **Diagramma commutativo:**
   ```mermaid
   graph LR
       1((1)) --> 2((2))
       2 --> 3((3))
       1 --> 3
   ```
3. **Grafo che NON può essere un sottografo:**
   Un grafo in cui due nodi non sono confrontabili in alcuna direzione, ad esempio:
   ```mermaid
   graph LR
       1((1)) --> 2((2))
       1 --> 3((3))
   ```
   *(senza alcuna freccia tra 2 e 3).*  
   **Motivazione:** In un ordine totale ogni coppia di elementi deve essere confrontabile ($\forall x, y.\; x \le y \lor y \le x$). L'assenza di frecce tra $2$ e $3$ (in entrambe le direzioni) violerebbe l'assioma di totalità (come mostrato nei lucidi Roversi, slide 14 di `CT0050`).

---

### Esercizio 7
> **Testo:** Prove that at most one arrow exists between two objects of the commuting diagram corresponding to a pre-order.

**Soluzione:**
In una categoria derivata da un pre-ordine $(X, \le)$, l'insieme dei morfismi tra due oggetti $a$ e $b$ è definito come:
$$\mathbf{C}(a, b) = \begin{cases} \{(a, b)\} & \text{se } (a, b) \in\; \le \\ \emptyset & \text{se } (a, b) \notin\; \le \end{cases}$$
Poiché $\le$ è un sottoinsieme del prodotto cartesiano $X \times X$ (cioè un insieme matematico di coppie), per ogni coppia ordinata $(a, b)$ vale una e una sola delle due alternative:
- $(a, b) \in\; \le \implies |\mathbf{C}(a, b)| = 1$
- $(a, b) \notin\; \le \implies |\mathbf{C}(a, b)| = 0$
In entrambi i casi, $|\mathbf{C}(a, b)| \le 1$. Pertanto, esiste **al massimo una freccia** tra due oggetti qualsiasi in una data direzione. Avere due frecce parallele distinte $f, g: a \to b$ significherebbe che $\{(a, b), (a, b)\}$ ha cardinalità 2, assurdo per definizione di insieme. $\blacksquare$

---

### Esercizio 8
> **Testo:** Prove that at most one arrow exists between two objects of the commuting diagram corresponding to a partial-order.

**Soluzione:**
Ogni ordine parziale è per definizione un pre-ordine. La definizione dell'insieme dei morfismi $\mathbf{C}(a, b)$ si basa unicamente sull'appartenenza della coppia $(a, b)$ alla relazione d'ordine $\le \subseteq X \times X$.
Dunque $|\mathbf{C}(a, b)| \in \{0, 1\}$, esattamente come dimostrato nell'Esercizio 7. $\blacksquare$

---

### Esercizio 9
> **Testo:** Prove that exactly one arrow exists between two objects of the commuting diagram corresponding to a total-order.

**Soluzione:**
Siano $a, b \in X$ due oggetti distinti ($a \ne b$) in un ordine totale $(X, \le)$.
1. Per la proprietà di **totalità**, vale $a \le b \lor b \le a$. Ne segue che tra i due oggetti esiste *almeno* una freccia (o da $a$ a $b$, o da $b$ a $a$).
2. Per la proprietà di **antisimmetria**, se valessero contemporaneamente $a \le b$ e $b \le a$, ne seguirebbe $a = b$, contraddicendo l'ipotesi che $a \ne b$. Dunque non possono esistere frecce in entrambe le direzioni tra nodi distinti.
3. Inoltre, per l'Esercizio 7, non possono esistere frecce parallele nella stessa direzione.
Pertanto, per ogni coppia non ordinata di elementi distinti $\{a, b\}$, esiste **esattamente una freccia** che li collega (orientata da $a$ a $b$ se $a \le b$, oppure da $b$ a $a$ se $b \le a$). $\blacksquare$

---

### Esercizio 10
> **Testo:** Justify why we cannot say that every oriented graph is a category.

**Soluzione:**
Un grafo orientato $G = (V, E)$ consiste unicamente di un insieme di vertici $V$ e di archi orientati $E \subseteq V \times V$. Non ogni grafo è una categoria per due motivi fondamentali:
1. **Assenza dei morfismi identità:** In una categoria, per ogni vertice $v \in V$ deve esistere un morfismo identità $\mathrm{id}_v: v \to v$. Un generico grafo orientato può non avere cappi (self-loops) su ciascun vertice.
2. **Assenza della chiusura per composizione:** In una categoria, se esistono frecce $f: u \to v$ e $g: v \to w$, *deve necessariamente esistere* la freccia composta $g \circ f: u \to w$. In un generico grafo orientato possono esistere cammini di lunghezza $\ge 2$ senza che vi sia un arco diretto dal nodo di partenza a quello di arrivo.
*(Per trasformare un grafo in una categoria occorre considerare la sua categoria libera $\mathcal{F}(G)$, aggiungendo le identità e tutti gli archi di composizione).*

---

## CT0100: Monoidi Categoriali

> **Principio Generale (Roversi):** Un monoide algebrico $(M, \odot, e)$ corrisponde a una categoria $\mathbf{M}$ avente:
> - Un unico oggetto: $\mathrm{obj}(\mathbf{M}) = \{\bullet\}$
> - Come frecce, le "sezioni" (applicazioni parziali dell'operazione): $\hom(\bullet, \bullet) = \{(m \odot) \mid m \in M\}$
> - Come freccia identità: la sezione dell'elemento neutro $(e \odot)$
> - Composizione: $(n \odot) \circ (m \odot) = ((n \odot m) \odot)$

### Esercizio 11
> **Testo:** Let $B = \{T, F\}$. Write the Categorical monoid of the monoid $(B, \land, T)$.

**Soluzione:**
- **Oggetti:** $\mathrm{obj}(\mathbf{B}_\land) = \{\bullet\}$
- **Morfismi:** $\hom(\bullet, \bullet) = \{(T \land), (F \land)\}$ dove:
  - $(T \land): \bullet \to \bullet$ è l'identità: $\mathrm{id}_\bullet = (T \land)$
  - $(F \land): \bullet \to \bullet$
- **Composizione:** $(x \land) \circ (y \land) = ((x \land y)\land)$
  - $(T \land) \circ (T \land) = (T \land)$
  - $(T \land) \circ (F \land) = (F \land) \circ (T \land) = (F \land)$
  - $(F \land) \circ (F \land) = (F \land)$

---

### Esercizio 12
> **Testo:** Let $B = \{T, F\}$. Write the Categorical monoid of the monoid $(B, \lor, F)$.

**Soluzione:**
- **Oggetti:** $\mathrm{obj}(\mathbf{B}_\lor) = \{\bullet\}$
- **Morfismi:** $\hom(\bullet, \bullet) = \{(T \lor), (F \lor)\}$
- **Identità:** $\mathrm{id}_\bullet = (F \lor)$ (poiché $F$ è l'elemento neutro di $\lor$)
- **Composizione:** $(x \lor) \circ (y \lor) = ((x \lor y)\lor)$

---

### Esercizio 13
> **Testo:** Let $B = \{T, F\}$. Write the Categorical monoid of the monoid $(B, \oplus, F)$ ("exclusive or").

**Soluzione:**
- **Oggetti:** $\mathrm{obj}(\mathbf{B}_\oplus) = \{\bullet\}$
- **Morfismi:** $\hom(\bullet, \bullet) = \{(T \oplus), (F \oplus)\}$
- **Identità:** $\mathrm{id}_\bullet = (F \oplus)$ (poiché $x \oplus F = x$)
- **Composizione:** $(x \oplus) \circ (y \oplus) = ((x \oplus y)\oplus)$
  - In particolare: $(T \oplus) \circ (T \oplus) = ((T \oplus T)\oplus) = (F \oplus) = \mathrm{id}_\bullet$.

---

### Esercizio 14
> **Testo:** Write the Categorical monoid of the monoid $(\Sigma^*, \cdot, \epsilon)$ (strings).

**Soluzione:**
- **Oggetti:** $\mathrm{obj}(\mathbf{\Sigma^*}) = \{\bullet\}$
- **Morfismi:** $\hom(\bullet, \bullet) = \{(w \cdot) \mid w \in \Sigma^*\}$ dove $(w \cdot)$ indica la concatenazione prefissa della stringa $w$.
- **Identità:** $\mathrm{id}_\bullet = (\epsilon \cdot)$ (stringa vuota).
- **Composizione:** $(u \cdot) \circ (v \cdot) = ((u \cdot v)\cdot)$.

---

### Esercizio 15
> **Testo:** Write the Categorical monoid of the monoid $(\{[a_1, \dots, a_n] \mid n \in \mathbb{N}\}, {+\!\!+}, [\,])$ (lists).

**Soluzione:**
- **Oggetti:** $\mathrm{obj}(\mathbf{List}) = \{\bullet\}$
- **Morfismi:** $\hom(\bullet, \bullet) = \{(l {+\!\!+}) \mid l \in List\}$
- **Identità:** $\mathrm{id}_\bullet = ([\,] {+\!\!+})$ (la lista vuota).
- **Composizione:** $(l_1 {+\!\!+}) \circ (l_2 {+\!\!+}) = ((l_1 {+\!\!+} l_2){+\!\!+})$.

---

## CT0150: Categoria $\mathbf{Set}$, Monomorfismi, Epimorfismi, Oggetto Terminale e Prodotto

### Esercizio 16
> **Testo:** Recall the definition of monomorphism and how it is obtained as a generalization of the notion: "injective function" between two sets.

**Soluzione:**
1. **Definizione di Monomorfismo:** In una categoria $\mathbf{C}$, un morfismo $f: X \to Y$ è detto **monomorfismo** (o freccia mono, o semplificabile a sinistra) se per ogni coppia di morfismi paralleli $g, h: Z \to X$:
   $$f \circ g = f \circ h \implies g = h$$
2. **Generalizzazione da funzione iniettiva:**
   In $\mathbf{Set}$, una funzione $f: X \to Y$ è iniettiva se $\forall x, x' \in X,\; f(x) = f(x') \implies x = x'$.
   Se consideriamo $Z = \{\bullet\}$ (un singoletto), due funzioni $g, h: \{\bullet\} \to X$ corrispondono alla scelta di due elementi $g(\bullet) = x$ e $h(\bullet) = x'$ in $X$.
   L'uguaglianza $f \circ g = f \circ h$ significa $f(g(\bullet)) = f(h(\bullet))$, ossia $f(x) = f(x')$.
   L'implicazione $g = h$ impone che $g(\bullet) = h(\bullet)$, ossia $x = x'$.
   Il monomorfismo generalizza questo concetto a qualunque oggetto di test $Z$ senza dover fare riferimento agli "elementi interni" degli oggetti, operando solo tramite frecce esterne.

---

### Esercizio 17
> **Testo:** Recall the definition of epimorphism and how it is obtained as a generalization of the notion: "surjective function" between two sets.

**Soluzione:**
1. **Definizione di Epimorfismo:** In una categoria $\mathbf{C}$, un morfismo $f: X \to Y$ è detto **epimorfismo** (o freccia epi, o semplificabile a destra) se per ogni coppia di morfismi paralleli $g, h: Y \to Z$:
   $$g \circ f = h \circ f \implies g = h$$
2. **Generalizzazione da funzione suriettiva:**
   In $\mathbf{Set}$, $f: X \to Y$ è suriettiva se l'immagine di $f$ copre tutto il codominio: $\mathrm{Im}(f) = Y$.
   Se $f$ non fosse suriettiva, esisterebbe almeno un elemento $y_0 \in Y \setminus \mathrm{Im}(f)$. Potremmo allora costruire due funzioni $g, h: Y \to \{0, 1\}$ che concordano su tutti gli elementi di $\mathrm{Im}(f)$ ma differiscono su $y_0$ (es. $g(y_0) = 0$ e $h(y_0) = 1$). In tal caso avremmo $g \circ f = h \circ f$ ma $g \ne h$.
   Imporre la cancellabilità a destra ($g \circ f = h \circ f \implies g = h$) garantisce che non ci siano elementi non coperti in $Y$.

---

### Esercizio 18
> **Testo:** Recall the definition of "Terminal object in a generic category" and how it is obtained as a generalization of the notion: "singleton set".

**Soluzione:**
1. **Definizione:** In una categoria $\mathbf{C}$, un oggetto $T \in \mathrm{obj}(\mathbf{C})$ è detto **terminale** se per ogni oggetto $X \in \mathrm{obj}(\mathbf{C})$ esiste **uno e un solo** morfismo $!_X: X \to T$.
2. **Generalizzazione dal singoletto:**
   Nella categoria $\mathbf{Set}$, consideriamo un insieme singoletto $\{\star\}$. Per qualsiasi insieme $X$, quante funzioni esistono da $X$ a $\{\star\}$?
   Esiste una sola funzione: quella costante che mappa ogni elemento $x \in X$ nell'unico elemento $\star$: $f(x) = \star$.
   In Haskell, l'oggetto terminale è il tipo unit `()` con l'unica funzione:
   ```haskell
   unit :: a -> ()
   unit _ = ()
   ```
   La proprietà universale cattura la natura del singoletto tramite l'esistenza e unicità di una freccia entrante da ogni altro oggetto.

---

### Esercizio 19
> **Testo:** Prove that the terminal object of a category $\mathbf{C}$ is unique up to isomorphism.

**Soluzione:**
Siano $T$ e $T'$ due oggetti terminali nella stessa categoria $\mathbf{C}$.
1. Poiché $T'$ è terminale, esiste un'unica freccia $f: T \to T'$.
2. Poiché $T$ è terminale, esiste un'unica freccia $g: T' \to T$.
3. Consideriamo la composizione $g \circ f: T \to T$.
   Poiché $T$ è terminale, deve esistere un'**unica** freccia da $T$ a $T$.
   Ma sappiamo che l'identità $\mathrm{id}_T: T \to T$ è una freccia da $T$ a $T$.
   Per l'unicità della freccia verso l'oggetto terminale $T$, deve valere:
   $$g \circ f = \mathrm{id}_T$$
4. Analogamente, consideriamo la composizione $f \circ g: T' \to T'$.
   Per l'unicità della freccia da $T'$ verso il terminale $T'$, deve valere:
   $$f \circ g = \mathrm{id}_{T'}$$
5. Poiché $g \circ f = \mathrm{id}_T$ e $f \circ g = \mathrm{id}_{T'}$, $f$ e $g$ sono isomorfismi inversi l'uno dell'altro. Dunque $T \cong T'$. $\blacksquare$

---

### Esercizio 20
> **Testo:** Let $\{a, b, c\}$ be a set with three elements. Is it terminal in the category $\mathbf{Set}$? Justify by means of an example.

**Soluzione:**
**No, non è terminale.**
*Giustificazione con controesempio:*
Sia $X = \{1\}$ un insieme con un solo elemento.
Il numero di funzioni da $X$ a $\{a, b, c\}$ è pari alla cardinalità $|\{a, b, c\}|^{|X|} = 3^1 = 3$.
Tali funzioni sono:
- $f_1(1) = a$
- $f_2(1) = b$
- $f_3(1) = c$
Poiché esistono 3 funzioni distinte da $\{1\}$ a $\{a, b, c\}$, la freccia non è unica (dev'esserne esattamente una). Dunque $\{a, b, c\}$ non è terminale.

---

### Esercizio 21
> **Testo:** Recall the definition of "Product between $a$ and $b$" as given in Category Theory. Then, justify why $(a, \backslash x \to x, \backslash x \to y::b)$ cannot be taken as Product between $a$ and $b$.

**Soluzione:**
1. **Definizione di Prodotto:** In una categoria $\mathbf{C}$, il prodotto di due oggetti $A$ e $B$ è una tripla $(P, \pi_1, \pi_2)$ con $\pi_1: P \to A$ e $\pi_2: P \to B$, tale che per ogni altro oggetto $C$ dotato di due frecce $f: C \to A$ e $g: C \to B$, esiste un'**unica freccia mediatrice** $m: C \to P$ tale che:
   $$\pi_1 \circ m = f \quad \text{e} \quad \pi_2 \circ m = g$$
2. **Perché $(a, \backslash x \to x, \backslash x \to y::b)$ NON è un prodotto:**
   Qui il candidato è $P = a$, con proiezioni $p_1 = \lambda x \to x$ e $p_2 = \lambda x \to y$ (funzione costante che restituisce un elemento prefissato $y \in b$).
   Prendiamo come candidato test la vera coppia $C = (a, b)$ con le proiezioni standard $f = \mathrm{fst}$ e $g = \mathrm{snd}$.
   Dovrebbe esistere una funzione $m: (a, b) \to a$ tale che:
   - $p_1(m(x, z)) = x \implies m(x, z) = x$
   - $p_2(m(x, z)) = z$
   Tuttavia, $p_2(m(x, z)) = p_2(x) = y$. Quindi dovrebbe valere $y = z$ per ogni possibile elemento $z \in b$.
   Se il tipo $b$ ha almeno due elementi distinti $z_1 \ne z_2$, la funzione costante restituisce sempre $y$ e non può uguagliare un arbitrario $z \in b$.
   La freccia mediatrice non può esistere (fallisce l'esistenza della fattorizzazione).

---

### Esercizio 22
> **Testo:** Recall the definition of "Product between $a$ and $b$". Then, justify why $((a,a,b), \(x,\_,\_) \to x, \(\_,\_,z) \to z)$ cannot be taken as Product between $a$ and $b$.

**Soluzione:**
Il candidato proposto è $P = (a, a, b)$ con proiezioni $p_1(x, \_, \_) = x$ e $p_2(\_, \_, z) = z$.
Prendiamo come oggetto di test la coppia $C = (a, b)$ con $f = \mathrm{fst}$ e $g = \mathrm{snd}$.
Affinché sia un prodotto, deve esistere un'**unica** freccia mediatrice $m: (a, b) \to (a, a, b)$ tale che:
$$p_1 \circ m = \mathrm{fst} \quad \text{e} \quad p_2 \circ m = \mathrm{snd}$$
La funzione $m$ riceve $(x, z)$ e deve produrre una terna $(t_1, t_2, t_3)$ tale che $t_1 = x$ e $t_3 = z$.
Tuttavia, la seconda coordinata $t_2 \in a$ non è vincolata da alcuna proiezione! Possiamo definire:
- $m_1(x, z) = (x, x, z)$
- $m_2(x, z) = (x, n, z)$ per un qualsiasi elemento prefissato $n \in a$.
Entrambe le funzioni soddisfano:
$p_1(m_1(x, z)) = x = \mathrm{fst}(x, z)$ e $p_2(m_1(x, z)) = z = \mathrm{snd}(x, z)$,
$p_1(m_2(x, z)) = x = \mathrm{fst}(x, z)$ e $p_2(m_2(x, z)) = z = \mathrm{snd}(x, z)$.
Poiché $m_1 \ne m_2$, **la freccia mediatrice non è unica**. Dunque $((a, a, b), p_1, p_2)$ viola l'unicità della proprietà universale.

---

### Esercizio 23
> **Testo:** Let us assume that both $P$ and $Q$ are the Product of $a$ and $b$ in a Category. Prove that $P$ and $Q$ are isomorphic.

**Soluzione:**
Siano $(P, \pi_1^P, \pi_2^P)$ e $(Q, \pi_1^Q, \pi_2^Q)$ due prodotti per gli oggetti $a$ e $b$.
1. Poiché $P$ è un prodotto e $Q$ è dotato delle proiezioni $\pi_1^Q: Q \to a$ e $\pi_2^Q: Q \to b$, per la proprietà universale di $P$ esiste un'unica freccia mediatrice $m: Q \to P$ tale che:
   $$\pi_1^P \circ m = \pi_1^Q \quad \text{e} \quad \pi_2^P \circ m = \pi_2^Q$$
2. Simmetricamente, poiché $Q$ è un prodotto e $P$ ha le frecce $\pi_1^P: P \to a$ e $\pi_2^P: P \to b$, per la proprietà universale di $Q$ esiste un'unica freccia mediatrice $k: P \to Q$ tale che:
   $$\pi_1^Q \circ k = \pi_1^P \quad \text{e} \quad \pi_2^Q \circ k = \pi_2^P$$
3. Consideriamo ora la freccia composta $m \circ k: P \to P$.
   Verifichiamo la composizione con le proiezioni di $P$:
   $$\pi_1^P \circ (m \circ k) = (\pi_1^P \circ m) \circ k = \pi_1^Q \circ k = \pi_1^P$$
   $$\pi_2^P \circ (m \circ k) = (\pi_2^P \circ m) \circ k = \pi_2^Q \circ k = \pi_2^P$$
   Notiamo che anche l'identità $\mathrm{id}_P: P \to P$ soddisfa banalmente $\pi_1^P \circ \mathrm{id}_P = \pi_1^P$ e $\pi_2^P \circ \mathrm{id}_P = \pi_2^P$.
   Ma per la proprietà universale di $P$, la freccia da $P$ in $P$ che fattorizza $\pi_1^P$ e $\pi_2^P$ è **unica**.
   Quindi deve valere:
   $$m \circ k = \mathrm{id}_P$$
4. In modo del tutto analogo, considerando $k \circ m: Q \to Q$, per l'unicità della mediatrice su $Q$ si conclude:
   $$k \circ m = \mathrm{id}_Q$$
5. Avendo trovato $m: Q \to P$ e $k: P \to Q$ tali che $m \circ k = \mathrm{id}_P$ e $k \circ m = \mathrm{id}_Q$, $P$ e $Q$ sono isomorfi: $P \cong Q$. $\blacksquare$

---

### Esercizi 24 & 25
> **Esercizio 24:** Is the following triple:
> `((Bool, Int, Bool), \(x,y,z) -> z, \(x,y,z) -> y)`
> a product between the Haskell types `Bool` and `Int`?

**Risposta:** **NO.**
*Motivazione:* La prima coordinata $x \in \mathrm{Bool}$ non è utilizzata da nessuna delle due proiezioni. Di conseguenza, data una coppia test $(\mathrm{Bool}, \mathrm{Int})$ con proiezioni identità, la freccia mediatrice $m(b, i)$ potrebbe essere scelta come $(b_0, i, b)$ per qualsiasi valore arbitrario $b_0 \in \{\mathrm{True}, \mathrm{False}\}$. Essendoci almeno due scelte distinte per $m$, viene violata l'**unicità della freccia mediatrice**.

---

> **Esercizio 25:** Is the following triple:
> `((Bool, Int, Bool), \(x,y,z) -> x, \(x,y,z) -> y)`
> a product between the Haskell types `Bool` and `Int`?

**Risposta:** **NO.**
*Motivazione:* Identica al caso precedente: la terza coordinata $z \in \mathrm{Bool}$ viene scartata da entrambe le proiezioni. La mediatrice $m(b, i) = (b, i, z_0)$ ammette scelte arbitrarie per $z_0$, violando l'unicità di $m$.

---

### Esercizio 26
> **Testo:** Is the following triple:
> `((Bool, Int), \(x,y) -> x, \(x,y) -> y + 1)`
> a product between the Haskell types `Bool` and `Int`?

**Risposta:** **NO (secondo la formalizzazione del corso).**
*Motivazione:*
1. La seconda proiezione proposta $p_2(x, y) = y + 1$ non è una proiezione canonica (non estrae il dato originale, ma applica una trasformazione).
2. Se consideriamo i tipi di Haskell, `Int` ha limiti finiti (`minBound` e `maxBound`): per $y = \mathrm{maxBound}$, $y + 1$ causa un overflow aritmetico.
3. Se consideriamo la teoria matematica generale su domini numerici come $\mathbb{N}$ (i naturali con lo zero), la funzione $y \mapsto y + 1$ non è suriettiva (lo zero non ha antecedente). Se un candidato test volesse produrre $0$, non esisterebbe alcun intero $y$ tale che $y + 1 = 0$, violando l'esistenza della mediatrice.

---

### Esercizio 27
> **Testo:** Is the following triple:
> `((Bool, Int), \(x,y) -> not x, \(x,y) -> y)`
> a product between the Haskell types `Bool` and `Int`?

**Risposta:**
- **In Teoria delle Categorie astratta:** **SÌ**, è un prodotto valido, poiché $(\mathrm{not}, \mathrm{id})$ è un isomorfismo di categorie (la funzione `not` è biiettiva e involutiva: $\mathrm{not} \circ \mathrm{not} = \mathrm{id}$). Qualsiasi oggetto isomorfo al prodotto con proiezioni composte con isomorfismi soddisfa la proprietà universale (la mediatrice unica è $m(c) = (\mathrm{not}(f(c)), g(c))$).
- **Nel contesto canonico di programmazione:** Si preferisce rispondere che non è il prodotto *canonico* perché la prima proiezione altera i valori di verità scambiandoli.

---

## CT0200: Costruzioni Universali e Coprodotto

> **Principio del Coprodotto $(A + B, \iota_1, \iota_2)$:**
> Per ogni candidato $(C, q_1, q_2)$ con $q_1: A \to C$ e $q_2: B \to C$, deve esistere un'**unica freccia mediatrice** $s: A + B \to C$ tale che:
> $$s \circ \iota_1 = q_1 \quad \text{e} \quad s \circ \iota_2 = q_2$$
> In Haskell il coprodotto è `Either a b` con $\iota_1 = \mathrm{Left}$ e $\iota_2 = \mathrm{Right}$.

### Esercizio 28
> **Testo:** Is the following triple:
> `(Either (Either Bool Int) Int, \x::Int -> Left (Right x), \z::Bool -> Left (Left z))`
> a Co-Product between `Int` and `Bool`?

**Risposta:** **NO.**
*Motivazione:* Il costruttore esterno `Right :: Int -> Either (Either Bool Int) Int` non viene mai raggiunto da nessuna delle due iniezioni.
Di conseguenza, per definire una funzione mediatrice $s: \mathrm{Either}\ (\mathrm{Either}\ \mathrm{Bool}\ \mathrm{Int})\ \mathrm{Int} \to C$, dobbiamo specificare:
- $s(\mathrm{Left}(\mathrm{Right}\ x)) = q_1(x)$
- $s(\mathrm{Left}(\mathrm{Left}\ z)) = q_2(z)$
- $s(\mathrm{Right}\ w) = \text{valore arbitrario in } C$
Poiché il valore su $\mathrm{Right}\ w$ può essere scelto a piacere senza violare le equazioni di fattorizzazione, **la freccia mediatrice $s$ non è unica**.

---

### Esercizio 29
> **Testo:** Is the following triple:
> `(Either Int Bool, \x::Int -> Left (x + 1), \y::Bool -> Right y)`
> a Co-Product between `Int` and `Bool`?

**Risposta:** **NO.**
*Motivazione:* L'iniezione da `Int` applica la funzione $x \mapsto x + 1$. Come visto per il prodotto, non copre tutti i possibili schemi su domini generali o finiti, e non è la canonica iniezione disgiunta.

---

### Esercizio 30
> **Testo:** Is the following triple:
> `(Either Int Bool, \x::Int -> Left x, \y::Bool -> Right (not y))`
> a Co-Product between `Int` and `Bool`?

**Risposta:**
- **In Teoria delle Categorie:** **SÌ**, è un coprodotto valido. Poiché `not` è una biiezione su `Bool`, per ogni coppia di frecce $q_1: \mathrm{Int} \to C$ e $q_2: \mathrm{Bool} \to C$, la funzione:
  $$s(\mathrm{Left}\ x) = q_1(x), \quad s(\mathrm{Right}\ y) = q_2(\mathrm{not}\ y)$$
  è l'**unica** freccia che fattorizza esattamente $q_1$ e $q_2$:
  $$s(\mathrm{Left}\ x) = q_1(x)$$
  $$s(\mathrm{Right}(\mathrm{not}\ y)) = q_2(\mathrm{not}(\mathrm{not}\ y)) = q_2(y)$$

---

### Esercizio 31
> **Testo:** Is the following triple:
> `(Int, \x::Int -> x, \y::Bool -> if y then 0 else 1)`
> a Co-Product between `Int` and `Bool`?

**Risposta:** **NO.**
*Motivazione:* Le due iniezioni **non hanno immagini disgiunte**.
Infatti per $x = 0 \in \mathrm{Int}$ l'iniezione produce $0$.
Per $y = \mathrm{True} \in \mathrm{Bool}$ l'iniezione produce anch'essa $0$.
Se scegliamo come oggetto di test $C = \mathrm{String}$ con $q_1(x) = \text{"Int "}{+\!\!+}\mathrm{show}\ x$ e $q_2(y) = \text{"Bool "}{+\!\!+}\mathrm{show}\ y$, la mediatrice $s: \mathrm{Int} \to \mathrm{String}$ dovrebbe soddisfare contemporaneamente:
- $s(0) = q_1(0) = \text{"Int 0"}$
- $s(0) = q_2(\mathrm{True}) = \text{"Bool True"}$
Poiché $\text{"Int 0"} \ne \text{"Bool True"}$, **nessuna funzione $s$ può esistere**.

---

### Esercizio 32
> **Testo:** Is the following triple:
> `(Int, \x::Int -> if x<0 then x else x+2, \y::Bool -> if y then 0 else 1)`
> a Co-Product between `Int` and `Bool`?

**Risposta:**
- **In matematica pura su $\mathbb{Z}$ (interi infiniti):** SÌ, le immagini sono disgiunte e ricoprono esattamente $\mathbb{Z}$ ($\mathrm{Bool}$ mappa in $\{0, 1\}$, $\mathrm{Int}$ mappa in $\mathbb{Z} \setminus \{0, 1\}$), quindi esiste una biiezione con $\mathbb{Z} + \mathrm{Bool}$.
- **In Haskell pratico:** **NO**, perché il tipo `Int` ha dimensione fissa e limitata superiormente da `maxBound`. L'operazione $x + 2$ causa overflow per $x = \mathrm{maxBound}-1$ e $x = \mathrm{maxBound}$, perdendo l'iniettività e rompendo l'isomorfismo.

---

### Esercizio 33
> **Testo:** Justify why `Either a Void` and `a` are isomorphic.

**Soluzione:**
In Haskell il tipo `Void` (dal modulo `Data.Void`) è il tipo vuoto (oggetto iniziale), privo di valori. Esiste una sola funzione da `Void` a qualunque tipo: `absurd :: Void -> a`.
Definiamo le due funzioni inverse:
```haskell
to :: Either a Void -> a
to (Left x)  = x
to (Right v) = absurd v

from :: a -> Either a Void
from x = Left x
```
Verifichiamo le composizioni:
1. $(to \circ from)(x) = to(Left\ x) = x = \mathrm{id}_a(x)$.
2. Su `Left x`: $(from \circ to)(Left\ x) = from(x) = Left\ x$.
   Il ramo `Right v` non può mai verificarsi poiché non esistono termini di tipo `Void`.
Dunque $(from \circ to) = \mathrm{id}_{\mathrm{Either}\ a\ \mathrm{Void}}$, provando che $\mathrm{Either}\ a\ \mathrm{Void} \cong a$. $\blacksquare$

---

### Esercizio 34
> **Testo:** Justify why `(a, Either b c)` and `Either (a, b) (a, c)` are isomorphic.

**Soluzione:**
Questo è l'isomorfismo di distributività del prodotto sul coprodotto: $A \times (B + C) \cong (A \times B) + (A \times C)$.
Definiamo le due funzioni:
```haskell
to :: (a, Either b c) -> Either (a, b) (a, c)
to (x, Left y)  = Left (x, y)
to (x, Right z) = Right (x, z)

from :: Either (a, b) (a, c) -> (a, Either b c)
from (Left (x, y))  = (x, Left y)
from (Right (x, z)) = (x, Right z)
```
Verifica per casi:
- $(from \circ to)(x, Left\ y) = from(Left(x, y)) = (x, Left\ y)$
- $(from \circ to)(x, Right\ z) = from(Right(x, z)) = (x, Right\ z)$
- $(to \circ from)(Left(x, y)) = to(x, Left\ y) = Left(x, y)$
- $(to \circ from)(Right(x, z)) = to(x, Right\ z) = Right(x, z)$
Entrambe le composizioni sono le rispettive identità, perciò i due tipi sono isomorfi. $\blacksquare$

---

### Esercizio 35
> **Testo:** Are `Either a a` and `(Either () (), a)` isomorphic?

**Soluzione:**
**SÌ, sono isomorfi.**
Il tipo `Either () ()` contiene esattamente due valori distinti: `Left ()` e `Right ()` (è isomorfo a `Bool`).
Dunque `(Either () (), a)` rappresenta una coppia formata da un tag binario e da un elemento di tipo `a`, esattamente come `Either a a`.
Definiamo le funzioni:
```haskell
to :: Either a a -> (Either () (), a)
to (Left x)  = (Left (), x)
to (Right x) = (Right (), x)

from :: (Either () (), a) -> Either a a
from (Left (), x)  = Left x
from (Right (), x) = Right x
```
Verifica:
- $(from \circ to)(Left\ x) = from(Left\ (), x) = Left\ x$
- $(from \circ to)(Right\ x) = from(Right\ (), x) = Right\ x$
- $(to \circ from)(Left\ (), x) = to(Left\ x) = (Left\ (), x)$
- $(to \circ from)(Right\ (), x) = to(Right\ x) = (Right\ (), x)$
Dunque `Either a a` $\cong$ `(Either () (), a)`. $\blacksquare$


---

# Parte 2 — Funtori e Prove di Functorialità
**Corso:** Linguaggi e Paradigmi di Programmazione (3 CFU)  
**Docente:** Prof. Luca Roversi  
**Argomenti:** Funtori matematici e in Haskell, Funtore Powerset, Funtore Kleene Star, Leggi dei Funtori (Identità e Composizione) con dimostrazioni per casi e per induzione strutturale (`Maybe`, `List`, `Either`, `BTree`, `T`, `U`), Diagrammi categoriali di funtori in $\mathbf{Set}$.

---

## CT0300: Funtori Algebrici e Matematici

### Esercizio 1 (Funtore Powerset)
> **Testo:** Let $A = \{x, y\}$ and $B = \{p, q\}$ be sets. Let $f = \{(x, p), (y, q)\}$ be a function on sets.
> • Draw the diagram resulting from the application of the “Powerset” functor to $A$, $B$, and $f$.
> • Decorate the diagram with the elements $\mathcal{P}f(\{x\})$ and $\mathcal{P}f(\{x, y\})$.

**Soluzione:**
1. **Definizione del Funtore Powerset $\mathcal{P}: \mathbf{Set} \to \mathbf{Set}$:**
   - Sugli oggetti (insiemi): mappa un insieme $X$ nell'insieme delle sue parti:
     $$\mathcal{P}(X) = \{s \mid s \subseteq X\}$$
   - Sui morfismi (funzioni): data $f: X \to Y$, la funzione $\mathcal{P}f: \mathcal{P}(X) \to \mathcal{P}(Y)$ è definita dall'immagine diretta:
     $$\mathcal{P}f(s) = \{f(u) \mid u \in s\}$$
2. **Applicazione agli insiemi $A$ e $B$:**
   - $A = \{x, y\} \implies \mathcal{P}(A) = \{\emptyset, \{x\}, \{y\}, \{x, y\}\}$
   - $B = \{p, q\} \implies \mathcal{P}(B) = \{\emptyset, \{p\}, \{q\}, \{p, q\}\}$
3. **Calcolo degli elementi decorati:**
   - $\mathcal{P}f(\{x\}) = \{f(x)\} = \{p\} \in \mathcal{P}(B)$
   - $\mathcal{P}f(\{x, y\}) = \{f(x), f(y)\} = \{p, q\} \in \mathcal{P}(B)$
4. **Diagramma commutativo:**
   ```mermaid
   graph TD
       A["A = {x, y}"] -->|"f = {(x,p), (y,q)}"| B["B = {p, q}"]
       PA["P(A) = {∅, {x}, {y}, {x,y}}"] -->|"P(f)"| PB["P(B) = {∅, {p}, {q}, {p,q}}"]
   ```
   *Decorazione degli elementi:*
   $$\{x\} \in \mathcal{P}(A) \xrightarrow{\mathcal{P}f} \{p\} \in \mathcal{P}(B)$$
   $$\{x, y\} \in \mathcal{P}(A) \xrightarrow{\mathcal{P}f} \{p, q\} \in \mathcal{P}(B)$$

---

### Esercizi 2, 3, 4 (Funtore Stella di Kleene $(-)^*$)

> **Principio del Funtore $(-)^*: \mathbf{Alphabet} \to \mathbf{Monoid}$ (Roversi):**
> - Dato un alfabeto (insieme) $\Sigma$, $\Sigma^*$ è il monoide libero generato da $\Sigma$ con la concatenazione $\cdot$ ed elemento neutro $\epsilon$.
> - Data una funzione tra alfabeti $f: \Sigma \to \Gamma^*$, il funtore la estende a un omomorfismo tra monoidi $f^*: \Sigma^* \to \Gamma^*$ definito induttivamente da:
>   $$f^*(\epsilon) = \epsilon, \quad f^*(w \cdot \sigma) = f^*(w) \cdot f(\sigma)$$

#### Esercizio 2
> **Testo:** Let $\Sigma = \{0, 1\}$, $\Gamma = \{aa, b\}$ be two alphabets. Let $f(0) = aa, f(1) = b$.
> • Draw the diagram resulting from the application of the Kleene Star functor to $\Sigma$, $\Gamma$, and $f$.
> • Can $aabaa$ be an element of $\Gamma^*$ generated by the Kleene Star functor $(-)^*$? Justify the answer.

**Soluzione:**
1. **Diagramma:**
   ```mermaid
   graph TD
       Sigma["Σ = {0, 1}"] -->|"f"| Gamma["Γ = {aa, b}"]
       SigmaStar["Σ*"] -->|"f*"| GammaStar["Γ*"]
   ```
2. **Analisi della stringa $aabaa$:**
   Scomponiamo la stringa $aabaa$ rispetto ai simboli generabili tramite $f$:
   $$aabaa = (aa) \cdot (b) \cdot (aa) = f(0) \cdot f(1) \cdot f(0) = f^*(0 \cdot 1 \cdot 0) = f^*(010)$$
   **Risposta:** **SÌ.** La stringa $010$ appartiene a $\Sigma^*$, e la sua immagine tramite l'estensione funtoriale $f^*$ è esattamente $f^*(010) = aabaa \in \Gamma^*$.

---

#### Esercizio 3
> **Testo:** Let $\Sigma = \{x, y, z\}$, $\Gamma = \{p, qq, r\}$. Let $f(x) = p, f(y) = qq, f(z) = r$.
> • Draw the diagram.
> • Is $pqqrqqp$ an element of $\Gamma^*$ obtained via the Kleene Star construction? Explain your reasoning.

**Soluzione:**
1. **Diagramma:** analogo, con $\Sigma \xrightarrow{f} \Gamma$ e $\Sigma^* \xrightarrow{f^*} \Gamma^*$.
2. **Analisi della stringa $pqqrqqp$:**
   Scomponiamo:
   $$pqqrqqp = (p) \cdot (qq) \cdot (r) \cdot (qq) \cdot (p) = f(x) \cdot f(y) \cdot f(z) \cdot f(y) \cdot f(x) = f^*(xyzyx)$$
   Poiché la stringa $xyzyx$ appartiene al dominio $\Sigma^*$, la stringa $pqqrqqp$ appartiene a $\Gamma^*$ ed è generata esattamente dall'applicazione del funtore $f^*$ a $xyzyx$.
   **Risposta:** **SÌ.**

---

#### Esercizio 4
> **Testo:** Let $\Sigma = \{u, v\}$, $\Gamma = \{mm, n\}$. Let $f(u) = mm, f(v) = n$.
> • Draw the diagram.
> • Determine whether $mmnmmn$ belongs to $\Gamma^*$ under the Kleene Star functor $(-)^*$. Provide justification.

**Soluzione:**
Scomponiamo $mmnmmn$:
$$mmnmmn = (mm) \cdot (n) \cdot (mm) \cdot (n) = f(u) \cdot f(v) \cdot f(u) \cdot f(v) = f^*(uvuv)$$
Poiché la stringa sorgente $uvuv \in \Sigma^*$, $f^*(uvuv) = mmnmmn \in \Gamma^*$.
**Risposta:** **SÌ.**

---

## CT0300: Dimostrazioni delle Leggi dei Funtori in Haskell

> **Le due leggi dei funtori:**
> 1. **Identità:** `fmap id = id` $\iff \forall x.\; \text{fmap id } x = x$
> 2. **Composizione:** `fmap (g . f) = fmap g . fmap f` $\iff \forall x.\; \text{fmap (g . f) } x = \text{fmap g (fmap f } x)$

### Esercizio 5 (Funtore `Maybe a`)
> **Testo:** Recall the definition of the data-type `Maybe a`. Prove that it is a functor by:
> 1. defining a function `fmap`;
> 2. proving that `fmap` enjoys the identity law;
> 3. proving that `fmap` enjoys the composition law.

**Soluzione:**
Definizione del tipo:
```haskell
data Maybe a = Nothing | Just a
```
1. **Definizione di `fmap`:**
   ```haskell
   instance Functor Maybe where
     fmap :: (a -> b) -> Maybe a -> Maybe b
     fmap _ Nothing  = Nothing
     fmap f (Just x) = Just (f x)
   ```
2. **Dimostrazione della Legge di Identità (`fmap id = id`):**
   Dimostriamo per casi sui due costruttori di `Maybe`:
   - **Caso `Nothing`:**
     $$\text{fmap id Nothing} = \text{Nothing} \quad (\text{per def. di fmap})$$
     $$= \text{id Nothing} \quad (\text{per def. di id})$$
   - **Caso `Just x`:**
     $$\text{fmap id (Just } x) = \text{Just (id } x) \quad (\text{per def. di fmap})$$
     $$= \text{Just } x \quad (\text{poiché id } x = x)$$
     $$= \text{id (Just } x)$$
   La legge vale per tutti i costruttori. $\blacksquare$

3. **Dimostrazione della Legge di Composizione (`fmap (g . f) = fmap g . fmap f`):**
   - **Caso `Nothing`:**
     $$\text{LHS} = \text{fmap } (g \circ f)\; \text{Nothing} = \text{Nothing}$$
     $$\text{RHS} = (\text{fmap } g \circ \text{fmap } f)\; \text{Nothing} = \text{fmap } g\; (\text{fmap } f\; \text{Nothing}) = \text{fmap } g\; \text{Nothing} = \text{Nothing}$$
     $\text{LHS} = \text{RHS}$.
   - **Caso `Just x`:**
     $$\text{LHS} = \text{fmap } (g \circ f)\; (\text{Just } x) = \text{Just } ((g \circ f)\; x) = \text{Just } (g(f(x)))$$
     $$\text{RHS} = \text{fmap } g\; (\text{fmap } f\; (\text{Just } x)) = \text{fmap } g\; (\text{Just } (f(x))) = \text{Just } (g(f(x)))$$
     $\text{LHS} = \text{RHS}$. $\blacksquare$

---

### Esercizio 6 (Funtore `List a`)
> **Testo:** Define a data-type `List a` that recursively formalizes the corresponding concept. Then prove that it is a functor (fmap, identity, composition).

**Soluzione:**
Definizione ricorsiva del tipo:
```haskell
data List a = Nil | Cons a (List a)
```
1. **Definizione di `fmap`:**
   ```haskell
   instance Functor List where
     fmap :: (a -> b) -> List a -> List b
     fmap _ Nil         = Nil
     fmap f (Cons x xs) = Cons (f x) (fmap f xs)
   ```
2. **Dimostrazione della Legge di Identità per induzione strutturale su `List a`:**
   - **Caso Base (`Nil`):**
     $$\text{fmap id Nil} = \text{Nil} = \text{id Nil}$$
   - **Passo Induttivo (`Cons x xs`):**
     *Ipotesi Induttiva (I.H.):* assumiamo che $\text{fmap id } xs = xs$.
     $$\text{fmap id (Cons } x\; xs) = \text{Cons (id } x)\; (\text{fmap id } xs) \quad (\text{def. fmap})$$
     $$= \text{Cons } x\; (\text{fmap id } xs) \quad (\text{def. id})$$
     $$= \text{Cons } x\; xs \quad (\text{applicando I.H.})$$
     $$= \text{id (Cons } x\; xs)$$
   La legge vale per ogni lista finita. $\blacksquare$

3. **Dimostrazione della Legge di Composizione per induzione strutturale:**
   - **Caso Base (`Nil`):**
     $$\text{fmap } (g \circ f)\; \text{Nil} = \text{Nil} = \text{fmap } g\; \text{Nil} = \text{fmap } g\; (\text{fmap } f\; \text{Nil})$$
   - **Passo Induttivo (`Cons x xs`):**
     *Ipotesi Induttiva (I.H.):* $\text{fmap } (g \circ f)\; xs = \text{fmap } g\; (\text{fmap } f\; xs)$.
     $$\text{LHS} = \text{fmap } (g \circ f)\; (\text{Cons } x\; xs) = \text{Cons } ((g \circ f)\; x)\; (\text{fmap } (g \circ f)\; xs)$$
     $$= \text{Cons } (g(f(x)))\; (\text{fmap } g\; (\text{fmap } f\; xs)) \quad (\text{per def. } \circ \text{ e I.H.})$$
     Valutiamo RHS:
     $$\text{fmap } f\; (\text{Cons } x\; xs) = \text{Cons } (f(x))\; (\text{fmap } f\; xs)$$
     $$\text{RHS} = \text{fmap } g\; (\text{Cons } (f(x))\; (\text{fmap } f\; xs)) = \text{Cons } (g(f(x)))\; (\text{fmap } g\; (\text{fmap } f\; xs))$$
     $\text{LHS} = \text{RHS}$. $\blacksquare$

---

### Esercizio 7 (Funtore `Either a b`)
> **Testo:** Recall the definition of `Either a b` and prove that `Either a` is a functor.

**Soluzione:**
```haskell
data Either a b = Left a | Right b
```
*(Nota: `Either` ha kind `* -> * -> *`. Fissando il primo argomento di tipo `a`, `Either a` ha kind `* -> *` ed è funtore sul secondo tipo).*

1. **Definizione di `fmap`:**
   ```haskell
   instance Functor (Either a) where
     fmap :: (b -> c) -> Either a b -> Either a c
     fmap _ (Left x)  = Left x
     fmap f (Right y) = Right (f y)
   ```
2. **Legge di Identità:**
   - `Left x`: $\text{fmap id (Left } x) = \text{Left } x = \text{id (Left } x)$
   - `Right y`: $\text{fmap id (Right } y) = \text{Right (id } y) = \text{Right } y = \text{id (Right } y)$
3. **Legge di Composizione:**
   - `Left x`:
     $$\text{LHS} = \text{fmap } (g \circ f)\; (\text{Left } x) = \text{Left } x$$
     $$\text{RHS} = \text{fmap } g\; (\text{fmap } f\; (\text{Left } x)) = \text{fmap } g\; (\text{Left } x) = \text{Left } x$$
   - `Right y`:
     $$\text{LHS} = \text{fmap } (g \circ f)\; (\text{Right } y) = \text{Right } ((g \circ f)\; y) = \text{Right } (g(f(y)))$$
     $$\text{RHS} = \text{fmap } g\; (\text{fmap } f\; (\text{Right } y)) = \text{fmap } g\; (\text{Right } (f(y))) = \text{Right } (g(f(y)))$$
   In entrambi i casi $\text{LHS} = \text{RHS}$. $\blacksquare$

---

### Esercizi 8 & 9 (Albero con valori sulle foglie: `BTree n l`)
```haskell
data BTree n l where
  Leaf :: l -> BTree n l
  Node :: BTree n l -> BTree n l -> BTree n l
```
*(Il tipo contiene valori di tipo `l` sulle foglie).*

#### Esercizio 8 (Definizione di `fmap` e Legge di Identità):
1. **Definizione di `fmap`:**
   ```haskell
   instance Functor (BTree n) where
     fmap :: (l -> m) -> BTree n l -> BTree n m
     fmap f (Leaf x)     = Leaf (f x)
     fmap f (Node t1 t2) = Node (fmap f t1) (fmap f t2)
   ```
2. **Dimostrazione Legge di Identità per induzione strutturale:**
   - **Caso Base (`Leaf x`):**
     $$\text{fmap id (Leaf } x) = \text{Leaf (id } x) = \text{Leaf } x = \text{id (Leaf } x)$$
   - **Passo Induttivo (`Node t1 t2`):**
     *I.H.:* $\text{fmap id } t_1 = t_1$ e $\text{fmap id } t_2 = t_2$.
     $$\text{fmap id (Node } t_1\; t_2) = \text{Node } (\text{fmap id } t_1)\; (\text{fmap id } t_2) = \text{Node } t_1\; t_2 = \text{id (Node } t_1\; t_2) \quad \blacksquare$$

#### Esercizio 9 (Dimostrazione Legge di Composizione):
- **Caso Base (`Leaf x`):**
  $$\text{fmap } (g \circ f)\; (\text{Leaf } x) = \text{Leaf } ((g \circ f)\; x) = \text{Leaf } (g(f(x)))$$
  $$\text{fmap } g\; (\text{fmap } f\; (\text{Leaf } x)) = \text{fmap } g\; (\text{Leaf } (f(x))) = \text{Leaf } (g(f(x)))$$
- **Passo Induttivo (`Node t1 t2`):**
  *I.H.:* vale la legge per i sottoalberi $t_1$ e $t_2$.
  $$\text{fmap } (g \circ f)\; (\text{Node } t_1\; t_2) = \text{Node } (\text{fmap } (g \circ f)\; t_1)\; (\text{fmap } (g \circ f)\; t_2)$$
  $$= \text{Node } (\text{fmap } g\; (\text{fmap } f\; t_1))\; (\text{fmap } g\; (\text{fmap } f\; t_2)) \quad (\text{per I.H.})$$
  $$= \text{fmap } g\; (\text{Node } (\text{fmap } f\; t_1)\; (\text{fmap } f\; t_2)) = \text{fmap } g\; (\text{fmap } f\; (\text{Node } t_1\; t_2)) \quad \blacksquare$$

---

### Esercizi 10 & 11 (Albero con valori sui nodi interni: `BTree n`)
```haskell
data BTree n where
  Leaf :: BTree n
  Node :: n -> BTree n -> BTree n -> BTree n
```

1. **Definizione di `fmap`:**
   ```haskell
   instance Functor BTree where
     fmap :: (a -> b) -> BTree a -> BTree b
     fmap _ Leaf           = Leaf
     fmap f (Node x t1 t2) = Node (f x) (fmap f t1) (fmap f t2)
   ```
2. **Esercizio 10 (Legge di Identità):**
   - Caso `Leaf`: $\text{fmap id Leaf} = \text{Leaf}$.
   - Caso `Node x t1 t2`: per I.H. sui sottoalberi:
     $$\text{fmap id (Node } x\; t_1\; t_2) = \text{Node (id } x)\; (\text{fmap id } t_1)\; (\text{fmap id } t_2) = \text{Node } x\; t_1\; t_2 \quad \blacksquare$$
3. **Esercizio 11 (Legge di Composizione):**
   - Caso `Leaf`: banale $\text{Leaf} = \text{Leaf}$.
   - Caso `Node x t1 t2`:
     $$\text{fmap } (g \circ f)\; (\text{Node } x\; t_1\; t_2) = \text{Node } (g(f(x)))\; (\text{fmap } (g \circ f)\; t_1)\; (\text{fmap } (g \circ f)\; t_2)$$
     $$= \text{Node } (g(f(x)))\; (\text{fmap } g\; (\text{fmap } f\; t_1))\; (\text{fmap } g\; (\text{fmap } f\; t_2))$$
     $$= \text{fmap } g\; (\text{Node } (f(x))\; (\text{fmap } f\; t_1)\; (\text{fmap } f\; t_2))$$
     $$= \text{fmap } g\; (\text{fmap } f\; (\text{Node } x\; t_1\; t_2)) \quad \blacksquare$$

---

### Esercizi 12 & 13 (Tipo ad albero multiforcato `T a`)
```haskell
data T a where
  TL :: a -> T a
  TN :: [T a] -> T a
```

1. **Definizione di `fmap`:**
   ```haskell
   instance Functor T where
     fmap :: (a -> b) -> T a -> T b
     fmap f (TL x)  = TL (f x)
     fmap f (TN ts) = TN (map (fmap f) ts)
   ```
2. **Esercizio 12 (Legge di Identità):**
   - Caso `TL x`: $\text{fmap id (TL } x) = \text{TL (id } x) = \text{TL } x$.
   - Caso `TN ts`: per induzione strutturale e per la proprietà della funzione `map id = id` sulle liste:
     $$\text{fmap id (TN } ts) = \text{TN } (\text{map } (\text{fmap id})\; ts) \overset{\text{I.H.}}{=} \text{TN } (\text{map id } ts) = \text{TN } ts \quad \blacksquare$$
3. **Esercizio 13 (Legge di Composizione):**
   - Caso `TL x`: $\text{fmap } (g \circ f)\; (\text{TL } x) = \text{TL } (g(f(x))) = \text{fmap } g\; (\text{fmap } f\; (\text{TL } x))$.
   - Caso `TN ts`:
     $$\text{fmap } (g \circ f)\; (\text{TN } ts) = \text{TN } (\text{map } (\text{fmap } (g \circ f))\; ts)$$
     $$\overset{\text{I.H.}}{=} \text{TN } (\text{map } (\text{fmap } g \circ \text{fmap } f)\; ts)$$
     $$= \text{TN } (\text{map } (\text{fmap } g)\; (\text{map } (\text{fmap } f)\; ts)) \quad (\text{functorialità di list})$$
     $$= \text{fmap } g\; (\text{TN } (\text{map } (\text{fmap } f)\; ts)) = \text{fmap } g\; (\text{fmap } f\; (\text{TN } ts)) \quad \blacksquare$$

---

### Esercizi 14 & 15 (Tipo `U a b`)
```haskell
data U a b where
  UL :: Either a b -> U a b
  UN :: (U a b, U a b) -> U a b
```
*(Funtore rispetto al parametro $b$: tipo `U a`).*

1. **Definizione di `fmap`:**
   ```haskell
   instance Functor (U a) where
     fmap :: (b -> c) -> U a b -> U a c
     fmap f (UL e)          = UL (fmap f e)  -- usa fmap di Either a
     fmap f (UN (u1, u2))   = UN (fmap f u1, fmap f u2)
   ```
2. **Esercizio 14 (Identità):**
   - Caso `UL e`: $\text{fmap id (UL } e) = \text{UL (fmap id } e) = \text{UL } e$ (per identità di `Either`).
   - Caso `UN (u1, u2)`: per I.H. su $u_1, u_2$:
     $$\text{fmap id (UN } (u_1, u_2)) = \text{UN } (\text{fmap id } u_1, \text{fmap id } u_2) = \text{UN } (u_1, u_2) \quad \blacksquare$$
3. **Esercizio 15 (Composizione):**
   - Caso `UL e`: $\text{fmap } (g \circ f)\; (\text{UL } e) = \text{UL } (\text{fmap } (g \circ f)\; e) = \text{UL } (\text{fmap } g\; (\text{fmap } f\; e)) = \text{fmap } g\; (\text{fmap } f\; (\text{UL } e))$.
   - Caso `UN (u1, u2)`:
     $$\text{fmap } (g \circ f)\; (\text{UN } (u_1, u_2)) = \text{UN } (\text{fmap } (g \circ f)\; u_1, \text{fmap } (g \circ f)\; u_2)$$
     $$= \text{UN } (\text{fmap } g\; (\text{fmap } f\; u_1), \text{fmap } g\; (\text{fmap } f\; u_2)) = \text{fmap } g\; (\text{fmap } f\; (\text{UN } (u_1, u_2))) \quad \blacksquare$$

---

## CT0300: Diagrammi Categoriali in $\mathbf{Set}$ per Funtori (Esercizi 16 - 21)

> **Struttura Standard della Risposta (Richiesta per tutti gli esercizi 16–21):**
> 1. Richiamare la definizione del tipo di dato.
> 2. Disegnare il diagramma commutativo generico generato dal funtore $F$:
>    ```
>        a  ---- f ---->  b
>        |                |
>        F                F
>        v                v
>       F a -- fmap f --> F b
>    ```
> 3. Decorare gli oggetti con istanze concrete:
>    - Scegliere due insiemi finiti semplici: $a = \{1, 2\}$, $b = \{p, q\}$.
>    - Fissare una funzione $f$: $f(1) = p, f(2) = q$.
> 4. Definire il grafo della funzione $f$:
>    $$\mathrm{graph}(f) = \{(1, p), (2, q)\}$$
> 5. Descrivere a parole come opera `fmap f` senza darne la definizione formale di codice:
>    *"Prende una struttura contenente valori in $a$ e la trasforma preservando l'intera forma/scheletro strutturale (nodi, puntatori, rami), sostituendo ogni elemento interno $x \in a$ con il corrispondente valore $f(x) \in b$."*

### Tabella riassuntiva per gli Esercizi 16–21:

| Es. | Tipo | Istanze concrete in $F a$ ($a=\{1, 2\}$) | Istanze trasformate in $F b$ tramite `fmap f` |
|---|---|---|---|
| **16** | `Maybe a` | `Nothing`, `Just 1` | `Nothing`, `Just p` |
| **17** | `List a` | `Nil`, `Cons 1 (Cons 2 Nil)` | `Nil`, `Cons p (Cons q Nil)` |
| **18** | `Either c a` | `Left c0`, `Right 1` | `Left c0`, `Right p` |
| **19** | `T a` | `TL 1`, `TN [TL 1, TL 2]` | `TL p`, `TN [TL p, TL q]` |
| **20** | `BTree n` (valori sui nodi) | `Node 1 Leaf Leaf` | `Node p Leaf Leaf` |
| **21** | `BTree n l` (valori sulle foglie) | `Node (Leaf 1) (Leaf 2)` | `Node (Leaf p) (Leaf q)` |


---

# Parte 3 — Funtori Applicativi e Legge di Composizione
**Corso:** Linguaggi e Paradigmi di Programmazione (3 CFU)  
**Docente:** Prof. Luca Roversi  
**Argomenti:** Funtori Applicativi, Motivazione d'uso ed esempi pratici, Definizioni di `pure` e `(<*>)`, Dimostrazioni rigorose della Legge di Composizione Applicativa (`Maybe`, `Either`, `List`, `Reader`, `Diagonal`, `Const`, `ZipList`, `Identity`).

---

## CT0380: Funtori Applicativi — Concetto e Utilità

### Esercizio 1
> **Testo:** Justify why applicative functors are useful for programmers, using an example.

**Soluzione:**
1. **Il Limite dei Funtori Semplici (`Functor`):**
   La typeclass `Functor` fornisce solo l'operatore:
   $$\text{fmap} :: (a \to b) \to f\, a \to f\, b$$
   Se abbiamo una funzione a più argomenti, ad esempio:
   $$\text{add} :: \text{Int} \to \text{Int} \to \text{Int}$$
   e due valori incapsulati in un contesto computazionale, ad esempio due opzioni `mx, my :: Maybe Int`:
   $$\text{fmap add } mx :: \text{Maybe } (\text{Int} \to \text{Int})$$
   Il risultato è una *funzione incapsulata nel contesto* `Maybe`. Con il solo `fmap`, non è possibile applicare questa funzione al secondo argomento `my :: Maybe Int`, perché `fmap` richiede che la funzione sia "pura" (esterna al contesto).

2. **La Soluzione Applicativa (`Applicative`):**
   I funtori applicativi introducono l'operatore "tie-fighter":
   $$(<*>) :: f\, (a \to b) \to f\, a \to f\, b$$
   unitamente a $\text{pure} :: a \to f\, a$. In questo modo possiamo applicare funzioni con un numero arbitrario di argomenti all'interno del contesto:
   ```haskell
   pure add <*> mx <*> my  -- di tipo Maybe Int
   ```
   Se `mx = Just 3` e `my = Just 4`, il risultato è `Just 7`. Se uno dei due è `Nothing`, il risultato collassa a `Nothing`.

3. **Vantaggi Rispetto alle Monadi (`Monad`):**
   - **Indipendenza dei calcoli:** Nella monade $(>\!\!>=) :: m\, a \to (a \to m\, b) \to m\, b$, il secondo calcolo dipende dinamicamente dal *valore* prodotto dal primo (dipendenza sequenziale). Negli applicativi gli argomenti sono reciprocamente indipendenti.
   - **Analisi Statica e Parallelismo:** Poiché la struttura delle computazioni non dipende dai valori a runtime, un compilatore o runtime può eseguire le sotto-espressioni in parallelo.
   - **Validazione e Accumulo di Errori:** Se si validano i campi di una form web, una monade fallisce e si ferma al primo errore (`short-circuiting`). Un applicativo (es. `Validation`) può eseguire tutte le validazioni indipendenti e accumulare *tutti* gli errori rilevati in una lista o monoide.

---

## CT0390: Funtori Applicativi in Haskell e Prove di Composizione

> ### La Legge di Composizione Applicativa:
> $$\text{pure } (.) \mathbin{\langle*\rangle} u \mathbin{\langle*\rangle} v \mathbin{\langle*\rangle} w = u \mathbin{\langle*\rangle} (v \mathbin{\langle*\rangle} w)$$
>
> **Tipizzazione dei componenti:**
> - $(.) :: (b \to c) \to (a \to b) \to a \to c$
> - $\text{pure }(.) :: f\, ((b \to c) \to (a \to b) \to a \to c)$
> - $u :: f\, (b \to c)$
> - $v :: f\, (a \to b)$
> - $w :: f\, a$
> - Risultato: $f\, c$

---

### Esercizio 2 (`Maybe a`)
> **Testo:** Recall the definition of the data-type `Maybe a`. Assume that you already proved that it is a functor.
> 1. Define the functions `pure` and `(<*>)`.
> 2. Prove that `(<*>)` enjoys the applicative composition law.

**Soluzione:**
1. **Definizione dell'istanza `Applicative`:**
   ```haskell
   instance Applicative Maybe where
     pure :: a -> Maybe a
     pure x = Just x

     (<*>) :: Maybe (a -> b) -> Maybe a -> Maybe b
     Nothing <*> _       = Nothing
     _       <*> Nothing = Nothing
     Just f  <*> Just x  = Just (f x)
   ```
2. **Dimostrazione della Legge di Composizione:**
   $$\text{pure }(.) \mathbin{\langle*\rangle} u \mathbin{\langle*\rangle} v \mathbin{\langle*\rangle} w = u \mathbin{\langle*\rangle} (v \mathbin{\langle*\rangle} w)$$
   Poiché $\text{pure }(.) = \text{Just }(.) $:
   - **Caso 1: $u = \text{Nothing}$**
     - $\text{LHS} = ((\text{Just }(.) \mathbin{\langle*\rangle} \text{Nothing}) \mathbin{\langle*\rangle} v) \mathbin{\langle*\rangle} w = (\text{Nothing} \mathbin{\langle*\rangle} v) \mathbin{\langle*\rangle} w = \text{Nothing}$
     - $\text{RHS} = \text{Nothing} \mathbin{\langle*\rangle} (v \mathbin{\langle*\rangle} w) = \text{Nothing}$
     $\text{LHS} = \text{RHS} = \text{Nothing}$.
   - **Caso 2: $u = \text{Just } f$ e $v = \text{Nothing}$**
     - $\text{LHS} = ((\text{Just }(.) \mathbin{\langle*\rangle} \text{Just } f) \mathbin{\langle*\rangle} \text{Nothing}) \mathbin{\langle*\rangle} w = (\text{Just }(f \circ) \mathbin{\langle*\rangle} \text{Nothing}) \mathbin{\langle*\rangle} w = \text{Nothing} \mathbin{\langle*\rangle} w = \text{Nothing}$
     - $\text{RHS} = \text{Just } f \mathbin{\langle*\rangle} (\text{Nothing} \mathbin{\langle*\rangle} w) = \text{Just } f \mathbin{\langle*\rangle} \text{Nothing} = \text{Nothing}$
     $\text{LHS} = \text{RHS} = \text{Nothing}$.
   - **Caso 3: $u = \text{Just } f$, $v = \text{Just } g$ e $w = \text{Nothing}$**
     - $\text{LHS} = (\text{Just }(f \circ g)) \mathbin{\langle*\rangle} \text{Nothing} = \text{Nothing}$
     - $\text{RHS} = \text{Just } f \mathbin{\langle*\rangle} (\text{Just } g \mathbin{\langle*\rangle} \text{Nothing}) = \text{Just } f \mathbin{\langle*\rangle} \text{Nothing} = \text{Nothing}$
     $\text{LHS} = \text{RHS} = \text{Nothing}$.
   - **Caso 4: $u = \text{Just } f$, $v = \text{Just } g$, $w = \text{Just } x$ (tutti `Just`)**
     - $\text{LHS} = ((\text{Just }(.) \mathbin{\langle*\rangle} \text{Just } f) \mathbin{\langle*\rangle} \text{Just } g) \mathbin{\langle*\rangle} \text{Just } x$
       $$= (\text{Just }(f \circ) \mathbin{\langle*\rangle} \text{Just } g) \mathbin{\langle*\rangle} \text{Just } x$$
       $$= \text{Just }(f \circ g) \mathbin{\langle*\rangle} \text{Just } x$$
       $$= \text{Just }((f \circ g)(x)) = \text{Just }(f(g(x)))$$
     - $\text{RHS} = \text{Just } f \mathbin{\langle*\rangle} (\text{Just } g \mathbin{\langle*\rangle} \text{Just } x)$
       $$= \text{Just } f \mathbin{\langle*\rangle} \text{Just }(g(x))$$
       $$= \text{Just }(f(g(x)))$$
     $\text{LHS} = \text{RHS}$. $\blacksquare$

---

### Esercizio 3 (`Either a b`)
> **Testo:** Recall the definition of the data-type `Either a b`. Assume you proved it is a functor.
> 1. Define `pure` and `(<*>)`.
> 2. Prove that `(<*>)` enjoys the applicative composition law.

**Soluzione:**
1. **Definizione:**
   ```haskell
   instance Applicative (Either a) where
     pure :: b -> Either a b
     pure x = Right x

     (<*>) :: Either a (b -> c) -> Either a b -> Either a c
     Left err  <*> _         = Left err
     _         <*> Left err  = Left err
     Right f   <*> Right x   = Right (f x)
   ```
2. **Dimostrazione della Legge di Composizione:**
   $\text{pure }(.) = \text{Right }(.) $.
   - **Se $u = \text{Left } e$:**
     - $\text{LHS} = ((\text{Right }(.) \mathbin{\langle*\rangle} \text{Left } e) \mathbin{\langle*\rangle} v) \mathbin{\langle*\rangle} w = (\text{Left } e \mathbin{\langle*\rangle} v) \mathbin{\langle*\rangle} w = \text{Left } e$
     - $\text{RHS} = \text{Left } e \mathbin{\langle*\rangle} (v \mathbin{\langle*\rangle} w) = \text{Left } e$
   - **Se $u = \text{Right } f$ e $v = \text{Left } e$:**
     - $\text{LHS} = (\text{Right }(f \circ) \mathbin{\langle*\rangle} \text{Left } e) \mathbin{\langle*\rangle} w = \text{Left } e \mathbin{\langle*\rangle} w = \text{Left } e$
     - $\text{RHS} = \text{Right } f \mathbin{\langle*\rangle} (\text{Left } e \mathbin{\langle*\rangle} w) = \text{Right } f \mathbin{\langle*\rangle} \text{Left } e = \text{Left } e$
   - **Se $u = \text{Right } f$, $v = \text{Right } g$, $w = \text{Left } e$:**
     - $\text{LHS} = \text{Right }(f \circ g) \mathbin{\langle*\rangle} \text{Left } e = \text{Left } e$
     - $\text{RHS} = \text{Right } f \mathbin{\langle*\rangle} (\text{Right } g \mathbin{\langle*\rangle} \text{Left } e) = \text{Right } f \mathbin{\langle*\rangle} \text{Left } e = \text{Left } e$
   - **Se tutti sono `Right` ($u = \text{Right } f$, $v = \text{Right } g$, $w = \text{Right } x$):**
     - $\text{LHS} = \text{Right }(f \circ g) \mathbin{\langle*\rangle} \text{Right } x = \text{Right }((f \circ g)(x)) = \text{Right }(f(g(x)))$
     - $\text{RHS} = \text{Right } f \mathbin{\langle*\rangle} \text{Right }(g(x)) = \text{Right }(f(g(x)))$
   In ogni caso $\text{LHS} = \text{RHS}$. $\blacksquare$

---

### Esercizio 4 (`List a`)
> **Testo:** Define a data-type `List a` that recursively formalizes the corresponding concept.
> 1. Show that it can be a functor by defining `fmap`.
> 2. Define `pure` and `(<*>)`.
> 3. Recall the corresponding applicative composition law, explicitly writing the types of its components.

**Soluzione:**
```haskell
data List a = Nil | Cons a (List a)
```
1. **`fmap`:**
   ```haskell
   fmap :: (a -> b) -> List a -> List b
   fmap _ Nil         = Nil
   fmap f (Cons x xs) = Cons (f x) (fmap f xs)
   ```
2. **`pure` e `(<*>)`:**
   ```haskell
   append :: List a -> List a -> List a
   append Nil ys         = ys
   append (Cons x xs) ys = Cons x (append xs ys)

   instance Applicative List where
     pure :: a -> List a
     pure x = Cons x Nil

     (<*>) :: List (a -> b) -> List a -> List b
     Nil         <*> _  = Nil
     Cons f fs   <*> xs = append (fmap f xs) (fs <*> xs)
   ```
3. **Legge di Composizione e Tipizzazione:**
   $$\text{pure }(.) \mathbin{\langle*\rangle} u \mathbin{\langle*\rangle} v \mathbin{\langle*\rangle} w = u \mathbin{\langle*\rangle} (v \mathbin{\langle*\rangle} w)$$
   - $(.) :: (b \to c) \to (a \to b) \to a \to c$
   - $\text{pure }(.) :: \text{List } ((b \to c) \to (a \to b) \to a \to c)$
   - $u :: \text{List } (b \to c)$
   - $v :: \text{List } (a \to b)$
   - $w :: \text{List } a$
   - Entrambi i membri hanno tipo $\text{List } c$.

---

### Esercizio 5 (`Reader r a`)
> **Testo:** Consider the following Haskell data-type:
> `newtype Reader r a where Reader :: (r -> a) -> Reader r a`
> 1. Show it can be a functor by defining `fmap`.
> 2. Define `pure` and `(<*>)`.
> 3. Recall the applicative composition law with explicit component types.

**Soluzione:**
1. **`fmap`:**
   ```haskell
   instance Functor (Reader r) where
     fmap :: (a -> b) -> Reader r a -> Reader r b
     fmap f (Reader g) = Reader (f . g)
   ```
2. **`pure` e `(<*>)`:**
   ```haskell
   instance Applicative (Reader r) where
     pure :: a -> Reader r a
     pure x = Reader (\_ -> x)

     (<*>) :: Reader r (a -> b) -> Reader r a -> Reader r b
     Reader rf <*> Reader rx = Reader (\r -> (rf r) (rx r))
   ```
3. **Legge di Composizione e Tipi:**
   $$\text{pure }(.) \mathbin{\langle*\rangle} u \mathbin{\langle*\rangle} v \mathbin{\langle*\rangle} w = u \mathbin{\langle*\rangle} (v \mathbin{\langle*\rangle} w)$$
   - $(.) :: (b \to c) \to (a \to b) \to a \to c$
   - $\text{pure }(.) :: \text{Reader } r\ ((b \to c) \to (a \to b) \to a \to c) = \text{Reader } (\lambda \_ \to (.))$
   - $u :: \text{Reader } r\ (b \to c)$
   - $v :: \text{Reader } r\ (a \to b)$
   - $w :: \text{Reader } r\ a$
   - Risultato: $\text{Reader } r\ c$.

---

### Esercizio 6 (`Diagonal a`)
> **Testo:** Consider the Haskell data-type:
> `newtype Diagonal a = Diagonal {runDiagonal :: (,) a a}`
> 1. Show it can be a functor by defining `fmap`.
> 2. Define `pure` and `(<*>)`.
> 3. Recall the applicative composition law with explicit types.

**Soluzione:**
1. **`fmap`:**
   ```haskell
   instance Functor Diagonal where
     fmap :: (a -> b) -> Diagonal a -> Diagonal b
     fmap f (Diagonal (x, y)) = Diagonal (f x, f y)
   ```
2. **`pure` e `(<*>)`:**
   ```haskell
   instance Applicative Diagonal where
     pure :: a -> Diagonal a
     pure x = Diagonal (x, x)

     (<*>) :: Diagonal (a -> b) -> Diagonal a -> Diagonal b
     Diagonal (f1, f2) <*> Diagonal (x1, x2) = Diagonal (f1 x1, f2 x2)
   ```
3. **Legge di Composizione e Tipi:**
   $$\text{pure }(.) \mathbin{\langle*\rangle} u \mathbin{\langle*\rangle} v \mathbin{\langle*\rangle} w = u \mathbin{\langle*\rangle} (v \mathbin{\langle*\rangle} w)$$
   - $\text{pure }(.) = \text{Diagonal } ((.), (.)) :: \text{Diagonal } ((b \to c) \to (a \to b) \to a \to c)$
   - $u = \text{Diagonal } (f_1, f_2) :: \text{Diagonal } (b \to c)$
   - $v = \text{Diagonal } (g_1, g_2) :: \text{Diagonal } (a \to b)$
   - $w = \text{Diagonal } (x_1, x_2) :: \text{Diagonal } a$
   - Risultato: $\text{Diagonal } (f_1 (g_1 x_1), f_2 (g_2 x_2)) :: \text{Diagonal } c$.

---

### Esercizio 7 (`Const c a`)
> **Testo:** Recall `newtype Const c a = Const { getConst :: c }`. Assume that it is a functor and that `c` is a monoid.
> 1. Define `pure` and `(<*>)`.
> 2. Prove that `(<*>)` enjoys the applicative composition law.

**Soluzione:**
1. **Definizione:**
   ```haskell
   instance Monoid c => Applicative (Const c) where
     pure :: a -> Const c a
     pure _ = Const mempty

     (<*>) :: Const c (a -> b) -> Const c a -> Const c b
     Const c1 <*> Const c2 = Const (c1 <> c2)
   ```
2. **Dimostrazione della Legge di Composizione:**
   Siano $u = \text{Const } c_u$, $v = \text{Const } c_v$, $w = \text{Const } c_w$.
   - $\text{pure }(.) = \text{Const mempty}$.
   - **LHS:**
     $$\text{pure }(.) \mathbin{\langle*\rangle} u = \text{Const mempty} \mathbin{\langle*\rangle} \text{Const } c_u = \text{Const } (\text{mempty} \diamond c_u) = \text{Const } c_u$$
     $$(\text{pure }(.) \mathbin{\langle*\rangle} u) \mathbin{\langle*\rangle} v = \text{Const } c_u \mathbin{\langle*\rangle} \text{Const } c_v = \text{Const } (c_u \diamond c_v)$$
     $$\text{LHS} = ((\text{pure }(.) \mathbin{\langle*\rangle} u) \mathbin{\langle*\rangle} v) \mathbin{\langle*\rangle} w = \text{Const } (c_u \diamond c_v) \mathbin{\langle*\rangle} \text{Const } c_w = \text{Const } ((c_u \diamond c_v) \diamond c_w)$$
   - **RHS:**
     $$v \mathbin{\langle*\rangle} w = \text{Const } c_v \mathbin{\langle*\rangle} \text{Const } c_w = \text{Const } (c_v \diamond c_w)$$
     $$\text{RHS} = u \mathbin{\langle*\rangle} (v \mathbin{\langle*\rangle} w) = \text{Const } c_u \mathbin{\langle*\rangle} \text{Const } (c_v \diamond c_w) = \text{Const } (c_u \diamond (c_v \diamond c_w))$$
   Poiché $c$ è un monoide, l'operazione $\diamond$ è associativa:
   $$(c_u \diamond c_v) \diamond c_w = c_u \diamond (c_v \diamond c_w)$$
   Dunque $\text{LHS} = \text{RHS}$. $\blacksquare$

---

### Esercizio 8 (`ZipList a`)
> **Testo:** Recall `newtype ZipList a = ZipList { getZipList :: [a] }`.
> 1. Define `pure` and `(<*>)` (acting pointwise).
> 2. Prove that `(<*>)` enjoys the applicative composition law.

**Soluzione:**
1. **Definizione:**
   ```haskell
   instance Applicative ZipList where
     pure :: a -> ZipList a
     pure x = ZipList (repeat x)

     (<*>) :: ZipList (a -> b) -> ZipList a -> ZipList b
     ZipList fs <*> ZipList xs = ZipList (zipWith ($) fs xs)
   ```
2. **Dimostrazione della Legge di Composizione:**
   Consideriamo l'$i$-esimo elemento delle liste risultanti (per ogni indice $i \ge 0$):
   - In `pure (.)`, ogni elemento è la funzione di composizione $(.)$.
   - Sia $u_i = (\text{getZipList } u) !! i$, $v_i = (\text{getZipList } v) !! i$, $w_i = (\text{getZipList } w) !! i$.
   - **LHS all'indice $i$:**
     $$((\text{pure }(.) !! i) \mathbin{\$} u_i \mathbin{\$} v_i) \mathbin{\$} w_i = ((.) \; u_i \; v_i) \; w_i = (u_i \circ v_i)(w_i) = u_i(v_i(w_i))$$
   - **RHS all'indice $i$:**
     $$u_i \mathbin{\$} ((v_i \mathbin{\$} w_i)) = u_i(v_i(w_i))$$
   Poiché $\text{LHS} !! i = \text{RHS} !! i$ per ogni indice $i$, le liste infinite/finite generate coincidono termine a termine. Dunque $\text{LHS} = \text{RHS}$. $\blacksquare$

---

### Esercizio 9 (`Identity a`)
> **Testo:** Recall `newtype Identity a = Identity { runIdentity :: a }`.
> 1. Define `pure` and `(<*>)`.
> 2. Prove that `(<*>)` enjoys the applicative composition law.

**Soluzione:**
1. **Definizione:**
   ```haskell
   instance Functor Identity where
     fmap f (Identity x) = Identity (f x)

   instance Applicative Identity where
     pure :: a -> Identity a
     pure x = Identity x

     (<*>) :: Identity (a -> b) -> Identity a -> Identity b
     Identity f <*> Identity x = Identity (f x)
   ```
2. **Dimostrazione della Legge di Composizione:**
   Siano $u = \text{Identity } f$, $v = \text{Identity } g$, $w = \text{Identity } x$.
   - $\text{pure }(.) = \text{Identity }(.) $
   - **LHS:**
     $$\text{pure }(.) \mathbin{\langle*\rangle} \text{Identity } f = \text{Identity } ((.) f) = \text{Identity } (f \circ)$$
     $$\text{Identity } (f \circ) \mathbin{\langle*\rangle} \text{Identity } g = \text{Identity } (f \circ g)$$
     $$\text{LHS} = \text{Identity } (f \circ g) \mathbin{\langle*\rangle} \text{Identity } x = \text{Identity } ((f \circ g)(x)) = \text{Identity } (f(g(x)))$$
   - **RHS:**
     $$v \mathbin{\langle*\rangle} w = \text{Identity } g \mathbin{\langle*\rangle} \text{Identity } x = \text{Identity } (g(x))$$
     $$\text{RHS} = u \mathbin{\langle*\rangle} (v \mathbin{\langle*\rangle} w) = \text{Identity } f \mathbin{\langle*\rangle} \text{Identity } (g(x)) = \text{Identity } (f(g(x)))$$
   $\text{LHS} = \text{RHS}$. $\blacksquare$


---

# Parte 4 — Trasformazioni Naturali e Monadi
**Corso:** Linguaggi e Paradigmi di Programmazione (3 CFU)  
**Docente:** Prof. Luca Roversi  
**Argomenti:** Trasformazioni Naturali in Haskell (Definizione e verifiche del quadrato di naturalità), Monadi (Definizione di `(>>=)` e dimostrazioni della Legge di Associatività del Bind per `Maybe`, `Either`, `List`, `Diagonal`, `Const`, `Identity`), Monade `Reader` e Monade `State` (Definizioni di `fmap`, `pure`, `(<*>)`, `(>>=)` con giustificazioni formali ed essenziali).

---

## CT0450: Trasformazioni Naturali in Haskell

### Esercizio 1
> **Testo:** Recall the definition of natural transformation between two functors $F$ and $G$.

**Soluzione:**
1. **Definizione Categoriale:**
   Siano $\mathbf{C}$ e $\mathbf{D}$ due categorie, e siano $F, G: \mathbf{C} \to \mathbf{D}$ due funtori paralleli.
   Una **trasformazione naturale** $\alpha: F \Rightarrow G$ è una famiglia di morfismi in $\mathbf{D}$:
   $$\{\alpha_X: F(X) \to G(X)\}_{X \in \mathrm{obj}(\mathbf{C})}$$
   indicizzata dagli oggetti di $\mathbf{C}$, tale che per ogni morfismo $f: X \to Y$ in $\mathbf{C}$, il seguente quadrato commuta:

   ```mermaid
   graph TD
       FX["F(X)"] -->|"α_X"| GX["G(X)"]
       FX -->|"F(f)"| FY["F(Y)"]
       GX -->|"G(f)"| GY["G(Y)"]
       FY -->|"α_Y"| GY
   ```

   Cioè:
   $$\alpha_Y \circ F(f) = G(f) \circ \alpha_X$$

2. **In Haskell (nella categoria $\mathbf{Hask}$):**
   Dati due funtori `F` e `G` (istanze di `Functor`), una trasformazione naturale polimorfa `alpha :: forall a. F a -> G a` soddisfa la **condizione di naturalità**:
   $$\text{alpha} \circ \text{fmap}_F(f) = \text{fmap}_G(f) \circ \text{alpha}$$
   ovvero, applicata a un generico elemento $x :: F\, a$:
   $$\text{alpha}(\text{fmap}_F\, f\, x) = \text{fmap}_G\, f\, (\text{alpha}\, x)$$

---

### Esercizio 2 (`maybeToList` su `Just x`)
> **Testo:** Let functors `Maybe` and `[]` be given. Consider:
> ```haskell
> maybeToList :: Maybe a -> [a]
> maybeToList Nothing  = []
> maybeToList (Just x) = [x]
> ```
> Prove the naturality law in the case where the argument is `Just x`.

**Soluzione:**
Dobbiamo verificare che:
$$\text{maybeToList}(\text{fmap}_{Maybe}\, f\, (\text{Just } x)) = \text{fmap}_{[]}\, f\, (\text{maybeToList}(\text{Just } x))$$
- **LHS (ramo sinistro):**
  $$\text{fmap}_{Maybe}\, f\, (\text{Just } x) = \text{Just } (f(x))$$
  $$\text{maybeToList}(\text{Just } (f(x))) = [f(x)]$$
- **RHS (ramo destro):**
  $$\text{maybeToList}(\text{Just } x) = [x]$$
  $$\text{fmap}_{[]}\, f\, [x] = \text{map } f\, [x] = [f(x)]$$
Poiché $\text{LHS} = [f(x)] = \text{RHS}$, la condizione di naturalità è verificata. $\blacksquare$

---

### Esercizio 3 (`listToMaybe` su `h:t`)
> **Testo:** Let functors `[]` and `Maybe` be given. Consider:
> ```haskell
> listToMaybe :: [a] -> Maybe a
> listToMaybe []    = Nothing
> listToMaybe (x:_) = Just x
> ```
> Prove the naturality law in the case where the argument is `h:t`.

**Soluzione:**
Dobbiamo verificare:
$$\text{listToMaybe}(\text{fmap}_{[]}\, f\, (h:t)) = \text{fmap}_{Maybe}\, f\, (\text{listToMaybe}(h:t))$$
- **LHS:**
  $$\text{fmap}_{[]}\, f\, (h:t) = \text{map } f\, (h:t) = f(h) : \text{map } f\, t$$
  $$\text{listToMaybe}(f(h) : \text{map } f\, t) = \text{Just } (f(h))$$
- **RHS:**
  $$\text{listToMaybe}(h:t) = \text{Just } h$$
  $$\text{fmap}_{Maybe}\, f\, (\text{Just } h) = \text{Just } (f(h))$$
$\text{LHS} = \text{RHS} = \text{Just } (f(h))$. $\blacksquare$

---

### Esercizi 4 & 5 (`toEitherUnit`)
```haskell
toEitherUnit :: Maybe a -> Either () a
toEitherUnit Nothing  = Left ()
toEitherUnit (Just x) = Right x
```

#### Esercizio 4 (Argomento `Just x`):
- **LHS:**
  $$\text{toEitherUnit}(\text{fmap}_{Maybe}\, f\, (\text{Just } x)) = \text{toEitherUnit}(\text{Just } (f(x))) = \text{Right } (f(x))$$
- **RHS:**
  $$\text{fmap}_{\text{Either ()}}\, f\, (\text{toEitherUnit}(\text{Just } x)) = \text{fmap}_{\text{Either ()}}\, f\, (\text{Right } x) = \text{Right } (f(x))$$
$\text{LHS} = \text{RHS}$. $\blacksquare$

#### Esercizio 5 (Argomento `Nothing`):
- **LHS:**
  $$\text{toEitherUnit}(\text{fmap}_{Maybe}\, f\, \text{Nothing}) = \text{toEitherUnit}(\text{Nothing}) = \text{Left } ()$$
- **RHS:**
  $$\text{fmap}_{\text{Either ()}}\, f\, (\text{toEitherUnit Nothing}) = \text{fmap}_{\text{Either ()}}\, f\, (\text{Left } ()) = \text{Left } ()$$
$\text{LHS} = \text{RHS} = \text{Left } ()$. $\blacksquare$

---

### Esercizio 6 (`fromEitherUnit` su `Right x`)
```haskell
fromEitherUnit :: Either () a -> Maybe a
fromEitherUnit (Left ()) = Nothing
fromEitherUnit (Right x) = Just x
```
**Dimostrazione su `Right x`:**
- **LHS:**
  $$\text{fromEitherUnit}(\text{fmap}_{\text{Either ()}}\, f\, (\text{Right } x)) = \text{fromEitherUnit}(\text{Right } (f(x))) = \text{Just } (f(x))$$
- **RHS:**
  $$\text{fmap}_{Maybe}\, f\, (\text{fromEitherUnit}(\text{Right } x)) = \text{fmap}_{Maybe}\, f\, (\text{Just } x) = \text{Just } (f(x))$$
$\text{LHS} = \text{RHS}$. $\blacksquare$

---

### Esercizio 7 (`headEither` su `h:t`)
```haskell
headEither :: [a] -> Either () a
headEither []    = Left ()
headEither (x:_) = Right x
```
**Dimostrazione su `h:t`:**
- **LHS:**
  $$\text{headEither}(\text{fmap}_{[]}\, f\, (h:t)) = \text{headEither}(f(h) : \text{map } f\, t) = \text{Right } (f(h))$$
- **RHS:**
  $$\text{fmap}_{\text{Either ()}}\, f\, (\text{headEither}(h:t)) = \text{fmap}_{\text{Either ()}}\, f\, (\text{Right } h) = \text{Right } (f(h))$$
$\text{LHS} = \text{RHS}$. $\blacksquare$

---

### Esercizio 8 (`eitherToList` su `Right x`)
```haskell
eitherToList :: Either () a -> [a]
eitherToList (Left ()) = []
eitherToList (Right x) = [x]
```
**Dimostrazione su `Right x`:**
- **LHS:**
  $$\text{eitherToList}(\text{fmap}_{\text{Either ()}}\, f\, (\text{Right } x)) = \text{eitherToList}(\text{Right } (f(x))) = [f(x)]$$
- **RHS:**
  $$\text{fmap}_{[]}\, f\, (\text{eitherToList}(\text{Right } x)) = \text{fmap}_{[]}\, f\, [x] = \text{map } f\, [x] = [f(x)]$$
$\text{LHS} = \text{RHS}$. $\blacksquare$

---

## CT0530: Monadi in Haskell e Associatività del Bind

> ### Legge di Associatività del Bind:
> $$(m \mathbin{>\!\!>=} f) \mathbin{>\!\!>=} g = m \mathbin{>\!\!>=} (\lambda x \to f\, x \mathbin{>\!\!>=} g)$$
> dove $m :: M\, a$, $f :: a \to M\, b$, $g :: b \to M\, c$.

---

### Esercizio 9 (`Maybe a`)
> **Testo:** Consider the applicative functor `Maybe a`.
> 1. Define the function `(>>=)`.
> 2. Prove the associativity law for `(>>=)`.

**Soluzione:**
1. **Definizione di `(>>=)`:**
   ```haskell
   instance Monad Maybe where
     (>>=) :: Maybe a -> (a -> Maybe b) -> Maybe b
     Nothing  >>= _ = Nothing
     (Just x) >>= f = f x
   ```
2. **Dimostrazione dell'Associatività:**
   - **Caso $m = \text{Nothing}$:**
     - $\text{LHS} = (\text{Nothing} \mathbin{>\!\!>=} f) \mathbin{>\!\!>=} g = \text{Nothing} \mathbin{>\!\!>=} g = \text{Nothing}$
     - $\text{RHS} = \text{Nothing} \mathbin{>\!\!>=} (\lambda x \to f\, x \mathbin{>\!\!>=} g) = \text{Nothing}$
     $\text{LHS} = \text{RHS} = \text{Nothing}$.
   - **Caso $m = \text{Just } x$:**
     - $\text{LHS} = (\text{Just } x \mathbin{>\!\!>=} f) \mathbin{>\!\!>=} g = (f\, x) \mathbin{>\!\!>=} g$
     - $\text{RHS} = \text{Just } x \mathbin{>\!\!>=} (\lambda w \to f\, w \mathbin{>\!\!>=} g) = (\lambda w \to f\, w \mathbin{>\!\!>=} g)\, x = (f\, x) \mathbin{>\!\!>=} g$
     $\text{LHS} = \text{RHS}$. $\blacksquare$

---

### Esercizio 10 (`Either u`)
> **Testo:** Consider the applicative functor `Either u`.
> 1. Define `(>>=)`.
> 2. Prove associativity for `(>>=)`.

**Soluzione:**
1. **Definizione:**
   ```haskell
   instance Monad (Either u) where
     (>>=) :: Either u a -> (a -> Either u b) -> Either u b
     Left err  >>= _ = Left err
     Right x   >>= f = f x
   ```
2. **Dimostrazione dell'Associatività:**
   - **Caso $m = \text{Left } e$:**
     - $\text{LHS} = (\text{Left } e \mathbin{>\!\!>=} f) \mathbin{>\!\!>=} g = \text{Left } e \mathbin{>\!\!>=} g = \text{Left } e$
     - $\text{RHS} = \text{Left } e \mathbin{>\!\!>=} (\lambda x \to f\, x \mathbin{>\!\!>=} g) = \text{Left } e$
     $\text{LHS} = \text{RHS} = \text{Left } e$.
   - **Caso $m = \text{Right } x$:**
     - $\text{LHS} = (\text{Right } x \mathbin{>\!\!>=} f) \mathbin{>\!\!>=} g = (f\, x) \mathbin{>\!\!>=} g$
     - $\text{RHS} = \text{Right } x \mathbin{>\!\!>=} (\lambda w \to f\, w \mathbin{>\!\!>=} g) = (f\, x) \mathbin{>\!\!>=} g$
     $\text{LHS} = \text{RHS}$. $\blacksquare$

---

### Esercizio 11 (`List a`)
> **Testo:** Assume `data List a = Nil | Cons a (List a)`.
> 1. Define `(>>=)`.
> 2. Prove the associativity law for `(>>=)`.

**Soluzione:**
1. **Definizione:**
   ```haskell
   append :: List a -> List a -> List a
   append Nil ys         = ys
   append (Cons x xs) ys = Cons x (append xs ys)

   instance Monad List where
     (>>=) :: List a -> (a -> List b) -> List b
     Nil         >>= _ = Nil
     (Cons x xs) >>= f = append (f x) (xs >>= f)
   ```
2. **Dimostrazione per induzione strutturale su $m$:**
   - **Lemma (Distributività del Bind su `append`):**
     $\text{append } ys\; zs \mathbin{>\!\!>=} g = \text{append } (ys \mathbin{>\!\!>=} g)\; (zs \mathbin{>\!\!>=} g)$.
   - **Caso Base ($m = \text{Nil}$):**
     - $\text{LHS} = (\text{Nil} \mathbin{>\!\!>=} f) \mathbin{>\!\!>=} g = \text{Nil} \mathbin{>\!\!>=} g = \text{Nil}$
     - $\text{RHS} = \text{Nil} \mathbin{>\!\!>=} (\lambda x \to f\, x \mathbin{>\!\!>=} g) = \text{Nil}$
     $\text{LHS} = \text{RHS}$.
   - **Passo Induttivo ($m = \text{Cons } x\; xs$):**
     - $\text{LHS} = ((\text{Cons } x\; xs) \mathbin{>\!\!>=} f) \mathbin{>\!\!>=} g$
       $$= (\text{append } (f\, x)\; (xs \mathbin{>\!\!>=} f)) \mathbin{>\!\!>=} g$$
       $$= \text{append } (f\, x \mathbin{>\!\!>=} g)\; ((xs \mathbin{>\!\!>=} f) \mathbin{>\!\!>=} g) \quad (\text{per il Lemma})$$
       $$= \text{append } (f\, x \mathbin{>\!\!>=} g)\; (xs \mathbin{>\!\!>=} (\lambda w \to f\, w \mathbin{>\!\!>=} g)) \quad (\text{applicando l'ipotesi induttiva su } xs)$$
     - $\text{RHS} = (\text{Cons } x\; xs) \mathbin{>\!\!>=} (\lambda w \to f\, w \mathbin{>\!\!>=} g)$
       $$= \text{append } ((\lambda w \to f\, w \mathbin{>\!\!>=} g)\, x)\; (xs \mathbin{>\!\!>=} (\lambda w \to f\, w \mathbin{>\!\!>=} g))$$
       $$= \text{append } (f\, x \mathbin{>\!\!>=} g)\; (xs \mathbin{>\!\!>=} (\lambda w \to f\, w \mathbin{>\!\!>=} g))$$
     $\text{LHS} = \text{RHS}$. $\blacksquare$

---

### Esercizio 12 (`Diagonal a`)
> **Testo:** `newtype Diagonal a = PutD {getD :: (,) a a}`
> 1. Define `(>>=)`.
> 2. Prove the associativity law for `(>>=)`.

**Soluzione:**
1. **Definizione:**
   ```haskell
   instance Monad Diagonal where
     (>>=) :: Diagonal a -> (a -> Diagonal b) -> Diagonal b
     PutD (x, y) >>= f =
       let (x', _) = getD (f x)
           (_, y') = getD (f y)
       in PutD (x', y')
   ```
2. **Dimostrazione dell'Associatività:**
   Siano $m = \text{PutD } (x, y)$.
   Poniamo $f\, x = \text{PutD } (x_1, x_2)$ e $f\, y = \text{PutD } (y_1, y_2)$.
   - $\text{LHS} = (\text{PutD } (x, y) \mathbin{>\!\!>=} f) \mathbin{>\!\!>=} g = \text{PutD } (x_1, y_2) \mathbin{>\!\!>=} g$.
     Poniamo $g\, x_1 = \text{PutD } (x_1', x_2')$ e $g\, y_2 = \text{PutD } (y_1', y_2')$.
     Allora $\text{LHS} = \text{PutD } (x_1', y_2')$.
   - $\text{RHS} = \text{PutD } (x, y) \mathbin{>\!\!>=} (\lambda w \to f\, w \mathbin{>\!\!>=} g)$.
     Calcoliamo la componente per $x$:
     $f\, x \mathbin{>\!\!>=} g = \text{PutD } (x_1, x_2) \mathbin{>\!\!>=} g = \text{PutD } (x_1', \dots)$ la cui prima componente è $x_1'$.
     Calcoliamo la componente per $y$:
     $f\, y \mathbin{>\!\!>=} g = \text{PutD } (y_1, y_2) \mathbin{>\!\!>=} g = \text{PutD } (\dots, y_2')$ la cui seconda componente è $y_2'$.
     Dunque $\text{RHS} = \text{PutD } (x_1', y_2')$.
   $\text{LHS} = \text{RHS}$. $\blacksquare$

---

### Esercizio 13 (`Const c a`)
> **Testo:** `newtype Const c a = Const { getConst :: c }`
> 1. Define `(>>=)`.
> 2. Prove associativity.

**Soluzione:**
1. **Definizione:**
   ```haskell
   instance Monad (Const c) where
     (>>=) :: Const c a -> (a -> Const c b) -> Const c b
     Const c >>= _ = Const c
   ```
2. **Dimostrazione:**
   Sia $m = \text{Const } c$.
   - $\text{LHS} = (\text{Const } c \mathbin{>\!\!>=} f) \mathbin{>\!\!>=} g = \text{Const } c \mathbin{>\!\!>=} g = \text{Const } c$
   - $\text{RHS} = \text{Const } c \mathbin{>\!\!>=} (\lambda x \to f\, x \mathbin{>\!\!>=} g) = \text{Const } c$
   $\text{LHS} = \text{RHS} = \text{Const } c$. $\blacksquare$

---

### Esercizio 14 (`Identity a`)
> **Testo:** `newtype Identity a = Identity { runIdentity :: a }`
> 1. Define `(>>=)`.
> 2. Prove associativity.

**Soluzione:**
1. **Definizione:**
   ```haskell
   instance Monad Identity where
     (>>=) :: Identity a -> (a -> Identity b) -> Identity b
     Identity x >>= f = f x
   ```
2. **Dimostrazione:**
   Sia $m = \text{Identity } x$.
   - $\text{LHS} = (\text{Identity } x \mathbin{>\!\!>=} f) \mathbin{>\!\!>=} g = (f\, x) \mathbin{>\!\!>=} g$
   - $\text{RHS} = \text{Identity } x \mathbin{>\!\!>=} (\lambda w \to f\, w \mathbin{>\!\!>=} g) = (\lambda w \to f\, w \mathbin{>\!\!>=} g)\, x = (f\, x) \mathbin{>\!\!>=} g$
   $\text{LHS} = \text{RHS}$. $\blacksquare$

---

### Esercizio 15 (`Reader r a`)
> **Testo:** `newtype Reader r a where Reader :: (r -> a) -> Reader r a`
> 1. Define `(>>=)` for `Reader r`.
> 2. Write the essential description that justifies the given definition of `(>>=)`.

**Soluzione:**
1. **Definizione:**
   ```haskell
   instance Monad (Reader r) where
     (>>=) :: Reader r a -> (a -> Reader r b) -> Reader r b
     Reader f >>= g = Reader $ \r ->
       let a = f r
           Reader h = g a
       in h r
   ```
2. **Descrizione Essenziale di Giustificazione:**
   La monade `Reader` modella computazioni che hanno accesso in sola lettura a un ambiente condiviso `r`.
   L'operatore di bind `(>>=)` esegue la sequenzializzazione:
   - **Fase 1:** Riceve l'ambiente iniziale $r$ e lo fornisce alla prima computazione $f$, estraendo il valore intermedio $a = f(r)$.
   - **Fase 2:** Applica la continuazione $g$ al valore $a$, ottenendo una nuova computazione reader $g(a) = \text{Reader } h$.
   - **Fase 3:** Fornisce lo **stesso identico ambiente** $r$ alla seconda computazione $h$, restituendo il valore finale $h(r)$.

---

### Esercizi 16, 17, 18 (`State s a`)
```haskell
newtype State s a = State { runState :: s -> (a, s) }
```

#### Esercizio 16 (Definizione e Giustificazione di `(>>=)`):
1. **Definizione di `(>>=)`:**
   ```haskell
   instance Monad (State s) where
     (>>=) :: State s a -> (a -> State s b) -> State s b
     f >>= g = State $ \s ->
       let (v, s') = runState f s
       in runState (g v) s'
   ```
2. **Descrizione Essenziale di Giustificazione (da Lucidi Roversi `LPP0700StateMonad`):**
   - **Step 1:** Si esegue la prima computazione `f` con lo stato iniziale `s`, ottenendo la coppia `(v, s')` formata dal risultato intermedio `v` e dallo stato aggiornato `s'`.
   - **Step 2:** Si passa il valore `v` alla funzione `g`, producendo una nuova computazione dipendente con stato `g v :: State s b`.
   - **Step 3:** Si esegue questa nuova computazione passando lo stato aggiornato `s'`, ottenendo il risultato finale e lo stato terminale `(b, s'')`.

---

#### Esercizio 17 (Definizione e Giustificazione di `fmap`):
1. **Definizione di `fmap`:**
   ```haskell
   instance Functor (State s) where
     fmap :: (a -> b) -> State s a -> State s b
     fmap h (State f) = State $ \s ->
       let (v, s') = f s
       in (h v, s')
   ```
2. **Descrizione Essenziale di Giustificazione:**
   Il funtore trasforma unicamente il dato restituito preservando l'effetto computazionale:
   - Esegue la computazione di stato `f` con lo stato iniziale `s`, producendo `(v, s')`.
   - Mantiene inalterata la transizione di stato verso `s'`.
   - Applica la funzione pura `h` unicamente al valore prodotto, restituendo `(h v, s')`.

---

#### Esercizio 18 (Definizione e Giustificazione di `pure` e `(<*>)`):
1. **Definizione di `pure` e `(<*>)`:**
   ```haskell
   instance Applicative (State s) where
     pure :: a -> State s a
     pure x = State $ \s -> (x, s)

     (<*>) :: State s (a -> b) -> State s a -> State s b
     State sf <*> State sx = State $ \s ->
       let (f, s')  = sf s
           (x, s'') = sx s'
       in (f x, s'')
   ```
2. **Descrizione Essenziale di Giustificazione:**
   - `pure x`: Inietta un valore puro senza modificare lo stato computazionale (stato in uscita uguale allo stato in ingresso).
   - `(<*>)`: Incanala ("threads") lo stato sequenzialmente attraverso le due computazioni indipendenti:
     1. Applica prima la computazione `sf` allo stato iniziale `s`, ottenendo la funzione `f` e lo stato intermedio `s'`.
     2. Passa lo stato intermedio `s'` alla seconda computazione `sx`, ottenendo il valore argomento `x` e lo stato finale `s''`.
     3. Applica la funzione al valore restituendo `(f x, s'')`.
