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