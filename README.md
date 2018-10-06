
# Tutorial I/O

* Responsabil: [Mihai Burdușelu](mailto:michelcatalin@gmail.com)
* Data publicării: 01.10.2017
* Data ultimei modificări: 28.09.2017

## Obiective
Scopul acestui tutorial este de a vă familiariza cu API-ul pus la dispoziție pentru citirea și scrierea în fișier. În cadrul laboratoarelor și temelor puteți importa acest API sau puteți sa folosiți o implementare proprie.

## Introducere
Acest API se folosește de [java.io.FileReader](http://docs.oracle.com/javase/8/docs/api/?java/io/FileReader.html) pentru a crea un flux de intrare de caractere.

[BufferedReader](https://docs.oracle.com/javase/8/docs/api/java/io/BufferedReader.html) este responsabil cu preluarea datelor de la un flux primitiv `FileReader` și procesarea acestora pentru a le oferi într-o altă formă. Patternul *Decorator* este aplicat aici, intrucat `BufferedReader` este un wrapper peste `FileReader`.
De asemenea, [java.io.FileWriter](https://docs.oracle.com/javase/8/docs/api/java/io/FileWriter.html) este responsabil pentru crearea unui flux de ieșire de caractere, în vreme ce [BufferedWriter](https://docs.oracle.com/javase/8/docs/api/java/io/BufferedWriter.html) este un wrapper peste acesta.

`BufferedReader` are calitatea de a `buffera` fluxul de date. Astfel, această metodă de citire este mai eficientă decât folosirea directă a unui `FileReader`.

## Importarea API-ului
Acest API este livrat deja compilat sub formă de fișier JAR.
Importarea unui astfel de proiect este explicata în acest [tutorial](http://elf.cs.pub.ro/poo/laboratoare/importare-fisiere-jar).

## Citirea dintr-un fișier text
FileReader este clasa care manipulează operațiile de citire dintr-un fișier. Numele fișierului este pasat drept parametru către constructor.
```
Deschiderea fișierului se face în momentul instanțierii obiectului.
Dacă fișierul nu se află pe disc, API-ul va arunca FileNotFoundException.
```

### Instanțierea unui obiect al clasei FileReader
```java
String filename = "input.txt";
FileReader fileReader = new FileReader(filename);
```

### Citirea tipurilor de date primitive din Java
FileReader conține metode de citire a tipurilor de date primitive din Java: `nextChar`, `nextBool`,  `nextInt`, `nextLong`, `nextFloat` și `nextDouble`.
Exemplu de folosire a metodelor **next**
```
4
3.14 true
```
```java
int integerNumber;
float floatingNumber;
boolean boolVariable;

integerNumber = fileReader.nextInt();
floatingNumber = fileReader.nextFloat();
boolVariable = fileReader.nextBool();

System.out.println(String.format("%d %f %b", integerNumber, floatingNumber, boolVariable));
```
În urma rulării codului de mai sus, se va afișa `4 3.14 true`.

### Citirea stringurilor delimitate de spații
De asemenea, API-ul oferă și metoda `nextWord` care are drept scop citirea unui string pană la întâlnirea primului spațiu.
Exemplu de folosire a metodei **nextWord**
```
apples pears
```
```java
String firstWord, secondWord;
    
firstWord = fileReader.nextWord();
secondWord = fileReader.nextWord();
    
System.out.println(String.format("%s, %s", firstWord, secondWord));
```
În urma rulării codului de mai sus, se va afișa `apples, pears`.

```
**Atenție!** Odată cu apelarea metodelor next* se modifică automat și poziția pointerului din fișier.
Astfel, poziția la care ne aflam la un moment dat într-un fișier înaintează cu apelurile metodelor next*.
```

### Închiderea fișierului
După terminarea operației de citire din fișier, fișierul de citire trebuie închis folosind metoda `close()`.

## Scrierea în fișier
FileWriter este clasa care manipulează operațiile de scriere într-un fișier. Numele fișierului este pasat drept parametru către constructor.
```
Deschiderea fișierului se face în momentul instanțierii obiectului.
Dacă fișierul nu se afla pe disc, acesta este creat automat.
```

### Instanțierea unui obiect al clasei FileReader
```java
String filename = "output.txt";
FileWriter fileWriter = new FileWriter(filename);
```

### Scrierea principalelor tipuri de date din java
FileWriter conține metode de **write**, complementare cu metodele **next** din FileReader.
Metoda `writeNewLine()` trece pe o linie nouă în fișier.

Exemplu de folosire a metodelor **write**
```java
int number = 3;
float pi = 3.14;
String word = "apples";
    
fileWriter.writeInt(number);
fileWriter.writeNewLine();
fileWriter.writeWord(word);
fileWriter.writeFloat(pi);
```
În urma rulării codului de mai sus, fișierul de output va fi următorul:
```
3
apples3.14
```

```
**Atentie!** API-ul nu insereaza spatii intre doua apeluri successive de metode write*.
```

### Închiderea fișierului
După terminarea operației de scriere în fișier, fișierul de scriere trebuie închis folosind metoda `close()`.

## Abstractizare pentru manipularea ambelor fluxuri de date
În cazul în care programul are nevoie atât de citire dintr-un fișier denumit input, cât și de scriere într-altul denumit output, API-ul oferă o abstractizare numită `FileSystem`.
Cele 2 fișiere (input și output) sunt date drept parametri către constructor.

### Instanțierea unui obiect al clasei FileSystem
```java
String input= "input.txt", output = "output.txt";
FileSystem fileSystem = new FileSystem(input, output);
```

### Citirea, respectiv scrierea principalelor tipuri de date din java
Această abstractizare reunește metodele de tip next* pentru citire, respectiv write* pentru scriere ale claselor FileReader și FileWriter.
Exemplu de folosire a metodelor **next** și **write**
```java
int number = fileSystem.nextInt();

fileSystem.writeInt(number);
```
În urma rulării codului de mai sus, se va citi un întreg din fișierul de input și va fi scris în fișierul de output.

### Închiderea fișierelor
După terminarea operațiilor de citire, respectiv scriere, ambele fișiere trebuie închise folosind metoda **close()**.

## Alte modalități de a manipula fluxurile intrare-ieșire
  * [RandomAccessFile](https://docs.oracle.com/javase/7/docs/api/java/io/RandomAccessFile.html)


## Resurse
  * [Basic I/O](https://docs.oracle.com/javase/tutorial/essential/io/)
  * [FileIO](http://elf.cs.pub.ro/poo/_media/laboratoare/tutorial-io/fileio.zip)
  * [FileIO JAR](http://elf.cs.pub.ro/poo/_media/laboratoare/tutorial-io/fileio_jar.zip)
