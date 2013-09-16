Problem description:
A bakery with a numbering machine at its entrance so each customer is given a unique number. 
Numbers increase by one as customers enter the store. 
A global counter displays the number of the customer that is currently being served.
All other customers must wait in a queue until the baker finishes serving the current customer and the next number is displayed. 
When the customer is done shopping and has disposed of his or her number, the clerk increments the number, allowing the next customer to be served.
That customer must draw another number from the numbering machine in order to shop again.

Assumptions:
1. The "customers" are threads, identified by the letter i, obtained from a global variable.
//It is possible that more than one thread will get the same number when they request it; this cannot be avoided. Therefore,
2. it is assumed that the thread identifier i is also a priority.
// A lower value of i means a higher priority and threads with higher priority will enter the critical section first.

Code: module 1:
 // declaration and initial values of global variables
     Entering: array [1..NUM_THREADS] of bool = {false};
     Number: array [1..NUM_THREADS] of integer = {0};
  1  lock(integer i) {
  2      Entering[i] = true;
  3      Number[i] = 1 + max(Number[1], ..., Number[NUM_THREADS]);
  4      Entering[i] = false;
  5      for (j = 1; j <= NUM_THREADS; j++) {
  6          // Wait until thread j receives its number:
  7          while (Entering[j]) { /* nothing */ }
  8          // Wait until all threads with smaller numbers or with the
 same
  9          // number, but with higher priority, finish their work:
 10          while ((Number[j] != 0) && ((Number[j], j) < (Number[i], i))) { /* nothing */ }
 13      }
 14  }
 15  
 16  unlock(integer i) {
 17      Number[i] = 0;
 18  }
 19
 20  Thread(integer i) {
 21      while (true) {
 22          lock(i);
 23          // The critical section goes here...
 24          unlock(i);
 25          // non-critical section...
 26      }
 27  }

