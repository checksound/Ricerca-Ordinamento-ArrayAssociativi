# Ricerca & Ordinamento & Array associativi

---

Vediamo di analizzare il problema della ricerca di un elemento in un array: la **ricerca sequenziale**.

```java
/**
 * Searches the array A for the integer N. If N is not in the array, then -1 is
 * returned. If N is in the array, then the return value is the first integer i
 * that satisfies A[i] == N.
 */
static int find(int[] A, int N) {
	for (int index = 0; index < A.length; index++) {
		if (A[index] == N)
			return index; // N has been found at this index!
	}
	// If we get this far, then N has not been found
	// anywhere in the array. Return a value of -1.
	return -1;
}

```
---
Nella ricerca sequenziale scorro l'array elemento per elemento, e confronto l'elemento dell'array con quello ricercato, finchè non lo trovo (se presente nell'array).

Se l'array fosse già ordinato, potrei essere molto più veloce nella ricerca, eseguendo la **ricerca binaria**.

---

Se l'array di partenza fosse già ordinato, come detto, potrei eseguire la **ricerca binaria**, molto più veloce della sequanziale:

```java
/**
 * Searches the array A for the integer N. Precondition: A must be sorted into
 * increasing order. Postcondition: If N is in the array, then the return value,
 * i, satisfies A[i] == N. If N is not in the array, then the return value is
 * -1.
 */
static int binarySearch(int[] A, int N) {
	int lowestPossibleLoc = 0;
	int highestPossibleLoc = A.length - 1;
	while (highestPossibleLoc >= lowestPossibleLoc) {
		int middle = (lowestPossibleLoc + highestPossibleLoc) / 2;
		if (A[middle] == N) {
			// N has been found at this index!
			return middle;
		} else if (A[middle] > N) {
			// eliminate locations >= middle
			highestPossibleLoc = middle - 1;
		} else {
			// eliminate locations <= middle
			lowestPossibleLoc = middle + 1;
		}
	}
	// At this point, highestPossibleLoc < LowestPossibleLoc,
	// which means that N is known to be not in the array. Return
	// a -1 to indicate that N could not be found in the array.
	return -1;
}

```

---

Come ordinare gli elementi della nostra struttura, ad esempio array di tipi primitivi od oggetti o elementi di un'`ArrayList`?

Vediamo all'inizio semplici algoritmi per farlo, poi le classi che Java mette a disposizione per eseguire gli ordinamenti. 

---
@snap[north-west]
### Insertion sort
@snapend

@snap[west]
Supponiamo di avere una lista ordinata e di voler aggiungere un elemento alla lista. Se si vuole che la nuova lista sia ancora ordinata, allora il nuovo elemento deve essere inserito nel posto giusto (rispetto all'ordinamento), cioè con gli elementi più piccoli messi prima e quelli più grandi messi dopo. Questo vuol dire muovere gli elementi più grandi avanti di un elemento per fare posto al nuovo elemento.
@snapend

---

![](assets/img/insertion_sort.png)

---
@snap[north-west]
### Insertion sort
@snapend

```java
/*
 * Precondition: itemsInArray is the number of items that are stored in A. These
 * items must be in increasing order (A[0] <= A[1] <= ... <= A[itemsInArray-1]).
 * The array size is at least one greater than itemsInArray. Postcondition: The
 * number of items has increased by one, newItem has been added to the array,
 * and all the items in the array are still in increasing order. Note: To
 * complete the process of inserting an item in the array, the variable that
 * counts the number of items in the array must be incremented, after calling
 * this subroutine.
 */
static void insert(int[] A, int itemsInArray, int newItem) {
	int loc = itemsInArray - 1; // Start at the end of the array.
	/*
	 * Move items bigger than newItem up one space; Stop when a smaller item is
	 * encountered or when the beginning of the array (loc == 0) is reached.
	 */
	while (loc >= 0 && A[loc] > newItem) {
		A[loc + 1] = A[loc]; // Bump item from A[loc] up to loc+1.
		loc = loc - 1; // Go on to next location.
	}
	A[loc + 1] = newItem; // Put newItem in last vacated space.
}
```

---

Concettualmente questo può essere esteso, per ordinare un array, prendendo ogni elemento di un array non ordinato, uno alla volta, per inserirlo di nuovo dentro l'array in modo ordinato. 

```
static void insertionSort(int[] A) {
    // Sort the array A into increasing order.
    int itemsSorted; // Number of items that have been sorted so far.
    for (itemsSorted = 1; itemsSorted < A.length; itemsSorted++) {
        // Assume that items A[0], A[1], ... A[itemsSorted-1]
        // have already been sorted. Insert A[itemsSorted]
        // into the sorted part of the list.
        int temp = A[itemsSorted]; // The item to be inserted.
        int loc = itemsSorted - 1; // Start at end of list.
        while (loc >= 0 && A[loc] > temp) {
            A[loc + 1] = A[loc]; // Bump item from A[loc] up to loc+1.
            loc = loc - 1; // Go on to next location.
        }
        A[loc + 1] = temp; // Put temp in last vacated space.
    }
}

```
---
@snap[north-west]
### Selection sort
@snapend

@snap[west]
Un altro semplice algoritmo per ordinare consiste nel trovare il più grande elemento della lista e spostarlo alla fine - dove deve stare se si vuole ordinare in modo crescente. Una volta che l'ememento più grande è nell'ultima posizione, si può riapplicare l'idea ai rimanenti. Cioè travare il prossimo elemento più grande, e spostarlo nella posizione precedente l'ultima, e così via. Questo algoritmo si chiama **selection sort**.
@snapend

---

@snap[north-west]
### Selection sort
@snapend

```java
static void selectionSort(int[] A) {
    // Sort A into increasing order, using selection sort
    for (int lastPlace = A.length - 1; lastPlace > 0; lastPlace--) {
        // Find the largest item among A[0], A[1], ...,
        // A[lastPlace], and move it into position lastPlace
        // by swapping it with the number that is currently
        // in position lastPlace.
        int maxLoc = 0; // Location of largest item seen so far.
        for (int j = 1; j <= lastPlace; j++) {
            if (A[j] > A[maxLoc]) {
                // Since A[j] is bigger than the maximum we’ve seen
                // so far, j is the new location of the maximum value
                // we’ve seen so far.
                maxLoc = j;
            }
        }
        int temp = A[maxLoc]; // Swap largest item with A[lastPlace].
        A[maxLoc] = A[lastPlace];
        A[lastPlace] = temp;
    } // end of for loop
}

```
---

@snap[north-west]
### Unsorting
@snapend

@snap[west]
Un argomento legato all'ordinamento, è quello di mettere un elemento in modo casuale in un array o unsorting. Il tipico caso è quello di mischiare un mazzo di carte. Un buon algoritmo per mischiare (**shuffling**) è simile al selection sort, eccetto che invece di spostare l'elemento maggiore alla fine della lista, un elemento è selezionato a caso e messo alla fine della lista. In questo esempio sotto viene mischiato in array di `int`.
@snapend

---

@snap[north-west]
### Unsorting
@snapend

```java
/**
 * Postcondition: The items in A have been rearranged into a random order.
 */
static void shuffle(int[] A) {
	for (int lastPlace = A.length - 1; lastPlace > 0; lastPlace--) {
		// Choose a random location from among 0,1,...,lastPlace.
		int randLoc = (int) (Math.random() * (lastPlace + 1));
		// Swap items in locations randLoc and lastPlace.
		int temp = A[randLoc];
		A[randLoc] = A[lastPlace];
		A[lastPlace] = temp;
	}
}

```
---

@snap[west]
Un altro esempio di utilizzo dell'algoritmo di **selection sort** per ordinare un mazzo di carte (utilizzando la classe ArrayList al posto dell'array). In questo caso la carte sono contenute in una lista di tipo `ArrayList<Card>`. Un oggetto di tipo `Card` contiene due metodi d'instanza `getSuit()` e `getValue()`. 
@snapend

---
@snap[north-west]
### Esempio gioco carte
@snapend

```java
/**
 * Sorts the cards in the hand so that cards of the same suit are grouped
 * together, and within a suit the cards are sorted by value. Note that aces are
 * considered to have the lowest value, 1.
 */
public void sortBySuit() {
	ArrayList<Card> newHand = new ArrayList<Card>();
	while (hand.size() > 0) {
		int pos = 0; // Position of minimal card.
		Card c = hand.get(0); // Minimal card.
		for (int i = 1; i < hand.size(); i++) {
			Card c1 = hand.get(i);
			if (c1.getSuit() < c.getSuit() 
				|| (c1.getSuit() == c.getSuit() && c1.getValue() < c.getValue())) {
				pos = i; // Update the minimal card and location.
				c = c1;
			}
		}
		hand.remove(pos); // Remove card from original hand.
		newHand.add(c); // Add card to the new hand.
	}
	hand = newHand;
}
```
Metodo della classe [Hand](https://github.com/checksound/EsempioGiocoCarte/blob/master/src/Hand.java). Codice [Card](https://github.com/checksound/EsempioGiocoCarte/blob/master/src/Card.java).

---?gist=MassimoCappellano/a90c74299036b6b43029b2cd73328113&lang=Java&title=Mazzo carte

@snap[south span-100]
@[8-20, zoom-25](Classe Hand, carte in mano al giocatore)
@[83-106, zoom-20](metodo per ordinare per seme: asso, due, tre.. di cuori, poi asso, due, tre.. di quadri,...)
@[107-130, zoom-20](metodo per ordinare per valori: tutti gli assi, poi tutti i due, tutti i tre....)
@snapend

---

In questo algoritmo di sorting, viene creata una nuova lista di carte: le carte sono prese dalla vecchia lista in ordine crescente, e messe nella nuova lista. Alla fine alla variabile `hand`  (che conteneva la vecchia lista di carte non ordinate) viene assegnata la nuova lista con le carte ordinate.

---?gist=https://gist.github.com/MassimoCappellano/2577352d45284a989df09b5c45deb223&lang=Java&title=Carta da gioco

---

Le classi [java.utils.Arrays](https://docs.oracle.com/javase/7/docs/api/java/util/Arrays.html) e [java.utils.Collections](https://docs.oracle.com/javase/7/docs/api/java/util/Collections.html) hanno molti metodi statici per eseguire l'ordinamento rispettivamente su array e liste.