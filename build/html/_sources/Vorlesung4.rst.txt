Vorlesung4 Montag 15.November 2021
==================================

Intro C++
---------

- Unterschied zu C: Dateiendung .cpp
- Jeder C Code ist auch C++ Code
- Einbinden von C++ Bibliotheken: #include<iostrem>
- Kompilieren mit: g++ -o 00_erstes.bin 00_erstes.cpp
- Erstes C++ Programm:

.. code-block:: C++

	#include<iostream>      // c++ bilbl. iostream
	#include<string.h>      // v bibl. string
	
	int main()
	{
	        char s[15];
	        strcpy(s,"Mustermann");
	                                                                // verwenden einer c-bibl. in c++
	        std::cout << "Erstes c++ Programm: " << s << std::endl; // c++
	        return 0;
	}

- Bibl. iostream ist ein bisschen ein Schreckgespenst
- Es gibt sehr viele Funktionen
- Wegen der Polymorphie wurde sie eingeführt
- cout hat einen Überladungsoperator << TEXT, dann wird das einfach ausgegeben
- cin für Eingaben

.. code-block:: C++

	// Wenn man nicht immer std:: für den Namespace der Standardbibliothek schreiben möchte
	using namespace std;

	int main()
	{
		cout << "Erste Ausgabe" << endl;
		...
	}

- iomanip für Manipulationen:

.. code-block:: C++
	
	#include<iostream>
	#include<iomanip>

	// Wenn man nicht immer std:: für den Namespace der Standardbibliothek schreiben möchte
	using namespace std;
	
	int main()
	{
	        cout << "Erste Ausgabe" << endl;
	        cout << "Zweite Ausgabe\n";
	
		int i;
	        cout << "Geben Sie einen Wert für i an: ";
	        cin >> i; // gibt man Text ein passiert nichts
	
		cout << "Der engegebene Wert für i war: " << i << endl;
	
		// Verwenden von Manipulation

		int i2 = 5;
	        cout << "Hier für den Wert [" << setw(10) << i2 << "] hier nach dem Wert" << endl; // setw setzt für die Variable eine bestimmte Breite
	        cout << "Hier für den Wert [" << setw(5) << setfill('0') << i2 << "] hier nach dem Wert" << endl;
	
		return 0;
	}

- in C++ ist folgendes möglich: for(int i = 0;i < 10; i++)
- setiosflags(ios::left); um alles linksbündig auszugeben

.. code-block:: C++

	#include<iostream>
	#include<iomanip>
	
	using namespace std;
	
	int main()
	{       
	        for(int i=0;i<10;i++)
	        {
	                cout << setw(10) << i << " | " << setw(12) << i*i << " | " << setw(15) << i*i*i << " | " << endl;
	        }
	
	        for(int i=0;i<10;i++)
	        {
	                cout << setiosflags(ios::left) << setw(10) << i << " | " << setw(12) << i*i << " | " << setw(15) << i*i*i << " | " << endl;
	        }
		 
	        return 0;
	}

Klassen in C++
--------------

- statt struct class schreiben, das Handling bleibt gleich (mit Zugriff)
- Bsp

.. code-block:: C++
	
	#include<iostream>
	#include<iomanip>
	 
	using namespace std;

	class student
	{
	        // inline Code
	        private:
	                int mnr = 0;
	        
	        public:
	                void setMnr(int mnr)
	                {
	                        this->mnr = mnr; // das ist unsauber, in neueren Projekten wäre int _mnr = 0; geschrieben worden und this->_mnr
	                }
	        
	                int getMnr() { return this->mnr; }
	        
	        protected:
	};
	
	int main()
	{
	        // Statisch im Speicher
	        student s1;
	        s1.setMnr(44444);
	 
	        cout << "Matrikelnummer ist: " << setw(10) << s1.getMnr() << endl;
	 
	        // Dynamische Speicherverwendung
	        student *s2 = NULL;
	        s2 = new student; // Speicher allokieren
	 
	        s2->setMnr(53443);
	        cout << "Matrikelnummer ist hier: " << setw(10) << s2->getMnr() << endl;
 	
	        delete s2; // Speicher freigeben
	        // Hinweis: jedes malloc brauch ein free, jedes new ein delete!
	 
	        return 0;
	}

Zeiger in C++
-------------

- siehe Bsp Code
- cout hat eine Überladung für strings

Ergänzung Klassen
-----------------

- Destruktor kann manuell aufgerufen werden, sonst automatisch bei Verlassen der Methode o.ä.
- Standardkosntruktor muss angegeben werden, falls man weitere einführen möchte
- Wenn man sauber entwickelt Header Datei, Main Klasse und Code Datei
- Header Datei .hpp
- Bsp chicken
- Kompilieren mit g++ -o chicken.bin prog.cpp chicken.cpp
- ifndef (wird nur included, wenn es nicht irgendwo anders schon included wird, sonst vom define bis zum nächsen endif übersprungen)
- Zum Erstellen Makefile
- linken bedeutet das executable zu erzeugen, das geht nur mit int main()
- g++ -c um nur zu übersetzen aber nicht zu linken
- im make file Abhängigkeiten definieren, damit nicht immer alles übersetzt wird, sondern nur wo sich etwas geändert hat

.. code-block:: Makefile

	
	Variablen
	compiler = /usr/bin/g++
	deleter = rm -f

	# Bloecke
	all: chicken.bin

	chicken.bin: chicken.o prog.o
	        $(compiler) -o chicken.bin chicken.o prog.o
	
	chicken.o: chicken.hpp chicken.cpp
	        $(compiler) -c -o chicken.o chicken.cpp
	
	prog.o: prog.cpp
	        $(compiler) -c -o prog.o prog.cpp
	
	clean:
	        $(deleter) chicken.bin chicken.o prog.o

- chicken.o und prog.o prüfen jeweils ob die angegeben Dateien geändert haben

Vererbung in C++
----------------

- siehe 10_vererbung.cpp
- class car : public vehicle Zugriffsmodifizierer der Basisklasse werden übernommen
- class car : private vehicle Zugriffsmodifizierer der Basisklasse werden alle als private übernommen
- Ableiten der Klasse:

+---------------+---------------+---------------+---------------+
| Ableiten mit:	| public	| protected	| private	|
+---------------+---------------+---------------+---------------+
| public	| public	| protected	| private	|
+---------------+---------------+---------------+---------------+
| protected	| protected	| protected	| private	|
+---------------+---------------+---------------+---------------+
| private	| private	| private	| private	|
+---------------+---------------+---------------+---------------+

- in C++ ist Mehrfachvererbung möglich
- Reihenfolge des Aufrufs der Konstruktoren bei Vererbung: erst Basisklasse, dann abgeleitete Klasse
- Wenn man Variablen "überschreibt", dann werden die nicht wirklich überschrieben. Man kann z.B. über eine get oder set Methode darauf zugreifen











































