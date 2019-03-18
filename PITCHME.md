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

Se l'array fosse già ordinato, potrei errere molto più veloce nella ricerca, eseguendo la **ricerca binaria**.
---

Se l'array di partenza fosse già ordinato, come detto, posso eseguire la **ricerca binaria**:

```java


```