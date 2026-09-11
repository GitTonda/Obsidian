# Risoluzione Esercizi Esame — Parte 3
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
