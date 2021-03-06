# Organizare
Tema a avut ca scop implementarea unui loader de fisiere executabile, sub forma unei biblioteci dinamice. Structura acestui executabil s-a bazat pe formatul ELF pentru Linux. 
Abordarea generala este in felul urmator:

- Initializam loader-ul, unde specificam handler-ul pentru semnalul SIGSEGV, fiindca acesta reprezinta prioritatea in contextul actual. 
- Deschidem fisierul binar executabil, iar apoi il parsam.
- Daca primim orice alt semnal in afara de SIGSEGV, cel care poate aparea in cadrul unui page-fault, apelam handler-ul vechi.
- In cadrul executabilului, pargucem segmentele de memorie ale acestuia.
- In functie de page-fault-urile generate, tratam situatia dupa caz. Cazurile principale sunt: Pagina e deja mapata -> chemam handler vechi, pagina nu e mapata -> o mapam noi cu toate specificatiile necesare. Pe langa asta, accesul nevalid duce la SEGFAULT, iar daca adresa paginii cautate nu s-a gasit, apelam handler vechi.
- Ne asiguram ca fiecare pagina e mapata unde trebuie, in functie de offset, adresa virtuala a fiecarui segment, indexul paginii si dimensiunea fisierului (de sine statator, dar si in memorie).

Consider ca tema e foarte utila, reprezinta esenta sistemelor de operare complexe. 
Legat de implementarea mea, consider ca e cel putin demna. Nu cred ca e neaparat optima deoarece folosesc variable ca sa stochez anumite date sau informatii din structura de exec (strict legat de memorie). Astfel, implementarea handler-ului putea fi facuta fara asa multe variabile, dar codul probabil ar fi aratat inuman sau complet imposibil de citit, asta pe langa dimensiunea uriasa a randurilor de cod.  


## Implementare
Pe linux, intregul enunt al temei este implementat. Nu consider ca am functionalitati extra, dar am fost nevoit sa declar o structura externa ce contine un vector de pagini mapate si numarul acestora. Nu as considera ca e o functionalitate extra, dar mi-a usurat munca.

O observatie, am observat ca desi in cerinta se specifica ca adresele de mapare sa fie FIXE, 'MAP_FIXED', teoretic testele trec si fara a introduce acest flag.

Testele sunt interesante, in principiul prin felul in care sunt legate unul de celalalt. Acest lucru se poate vedea si in executia normala a programelor, cum o problema la nivel de memorie poate avea un efect de Domino.

Cea mai grea parte pentru mine a fost intelegerea diferentei dintre dimensiunea executabilului si dimensiunea executabilului in memorie, la nivel de implementare. Conceptual, totul pare abordabil, dar cand vine vorba de implementare, lucrurile se pot complica. De asemenea, a trebuit sa incerc mai multe abordari pentru "zeroizarea" memoriei din .bss, pana cand a iesit cea buna.


## Cum se complileaza
Primul Makefile va genera o blblioteca dinamica (partajata), ce include codul sursa al loader-ului si al parser-ului de executabil.


Makefile
```bash
CC = gcc
CFLAGS = -fPIC -m32 -Wall
LDFLAGS = -m32

.PHONY: build
build: libso_loader.so

libso_loader.so: loader.o exec_parser.o
	$(CC) $(LDFLAGS) -shared -o $@ $^

exec_parser.o: exec_parser.c exec_parser.h
	$(CC) $(CFLAGS) -o $@ -c $<

loader.o: loader.c
	$(CC) $(CFLAGS) -o $@ -c $<

.PHONY: clean
clean:
	-rm -f exec_parser.o loader.o libso_loader.so
```
Al doilea Makefile va genera executabilele pentru testarea loader-ului.


## Bibliografie
Sursa primara de inspiratie: Codul din Lab6, linux, prot.c. In esenta de acolo am imprumutat bucata pentru initializarea loader-ului, dar si bucata de mmap si mprotect, care era extraordinar de asemanatoare. Pe langa asta, am apelat la paginile oficiale linux de documentatie pentru apelurile de sisteme ce tin de semnale.
