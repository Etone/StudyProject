# Vulnerability Detection in Sourcecode
**Securityrelevante Schwachstellen in Sourcecode und Tools zur Erkennung dieser**
---
## Motivation
+++
![Netflix Architecture](https://image.slidesharecdn.com/archtutorialgluecon-pptx-120524120713-phpapp02/95/netflix-architecture-tutorial-at-gluecon-23-728.jpg?cb=1394002823)  

Note:
Netflix Architecture, Systeme werden immer komplexer, unüberschaubar.
Angreifer müssen nur eine Schwachstelle in Systemen finden, um Probleme zu verursachen

+++
* Schnelles Beheben von Securityrelevanten Problemen notwendig
* Vorzeitiges erkennen von Problemen dabei notwendig
* **Notwendigkeit für Analyse Tools (Linter)**
---
## Security Flaws
+++
* Input Validation
    * SQL Injection
* Integer Overflow and Underflow

Note:
Untersuchte Flaws SQL Injection und Integer Parsing im eigenen Code
Qt und boost als Beispiel für Security Flaws in Libs

---
### Input Validation
+++
#### SQL Injection
![XKCD Exploits of a Mom](https://imgs.xkcd.com/comics/exploits_of_a_mom.png)  
Eingaben von User müssen validiert werden
+++
![Heise Wordpress SQL Injection](https://puu.sh/ziCzQ/167a8776e3.png)  
Auch heute noch von Relevanz
+++
```
QSqlDatabase db = QSqlDatabase::addDatabase("QMYSQL");
db.setHostName("localhost");
db.setDatabaseName("dbstupro");
db.setUserName("admin");
db.setPassword("admin");
bool ok = db.open();
```
Verbindung an Datenbank durch Qt
+++
```
public QString queryDataForKey(QString condition)
{
  QSqlQuery query(db);
  QString queryString("SELECT key, value FROM table WHERE key =");

  //this is unsafe and inefficient
  queryString += "'" + condition + "';";

  query.exec(queryString);
  //Buisness Logic, get Result and so on
  return resultString;
}
```
**unsichere** SQL Query mit Qt in C++
+++
```
//Query.cpp
public int main(int argc, char *argv[])
{
  //Do stuff
  Database::queryDataForKey(argv[1]);
  //Do more stuff
}
```
Unsicherer Aufruf der Methode queryDataForKey()  
Aufruf mit Query.exe a' DROP ALL TABLES;
+++
### How to protect
**NEVER TRUST USER INPUT** -OWASP, MSDN, CERT  
Prepared Statements
---
### Integer Probleme
+++
#### Was kann passieren
* **Overflow (unsigned und signed)**
* **Underflow (unsigned und signed)**
* Extension
* **Narrowing**
* Sign Conversion

Note:
Narrowing = int -> short, dadurch verlust von Infos / bei signed Verlust Vorzeichen
+++
#### Overflow und Underflow
+++
##### Overflow
```
unsigned short a = 65000;
unsigned short b =   540;
unsigned short c =     0;

c = a + b;
printf("Result is %hu + %hu = %hu\n", a, b, c);
```
Erwartetes Ergebnis:  
Result is 65000 + 540 = 65540
+++
![Integer Overflow CPP](https://puu.sh/zjeTY/cab43c4b97.png)
+++
**Definiertes Verhalten** bei unsigned Integer Werten
65000 + 540 = 65540 % SHORT_MAX + 1 = 65540 % 65536 = 4  
**Nicht bei signed**

Note:
Signed nicht definiert, meistens 2er Komplement
SHORT_MAX = 0x7fff | + 1 = 0x8000 = -32768
+++
##### Underflow
```
unsigned short us = 0;
short ss = SHRT_MIN; //-32768
us -= 1;
ss -= 1;

printf("%hu %hu", us ss);
```
![Integer Underflow](https://puu.sh/zjfl1/42bf7f07c3.png&size=contain)
+++
Problem bei Methoden, die vermischte Vorzeichen benutzen
* malloc
* memcpy
+++
```
int copySize = atoi(argsv[1]);

if(copySize > MAX_BUF_SZ) //1024
{
  return -1;
}
memcpy(&d, &s, copySize*sizeof(type));
```
Möglichkeit, copySize > MAX_BUF_SZ durch Usereingabe  
**BUFFER OVERFLOW**

Note:
copySize = -2147482047
if check fails, memcpy behandelt wie unsigned -> Buffer Overflow
Schutz durch z.B. Compilerflag (bei Overflow runtime error)
---
## Tools zur statischen Code Analyse
* MS Visual Studio Code Analyzer
* CppCheck
---
### Funktionsweise
+++
* Tokenize Source Code
* Matchen der Tokens
* Abstract Syntax Tree
+++
#### Abstract Syntax Tree
```
x = 1;
y = 2;
3* (x + y);
```
![AST small](https://i.stack.imgur.com/dhd3v.png);

Note:
Tiefensuche, erst Links, dann rechts, dann Knoten, rekursiv.
---
### MS Visual Studio Code Analyzer
* Integrierte Code Analyse in Visual Studio
* Vordefinierte Regelsätze
* Erweiterbar durch neue Regelsätze und eigene Checks
+++
![MSVSCA Aktivieren](https://puu.sh/zjhn1/d83abc557f.png)
+++
![MSVSCA Regeln](https://puu.sh/zjhnQ/8f6757a437.png)
+++
![MSVSCA Fehlerliste](https://puu.sh/zjhpe/08f1dc6187.png)
+++
Pro | Contra
----|--------
Extrem simpel zu nutzen | Nur unter MS Visual Studio
Erweiterbarer Regelsatz | Basisregeln nicht für Security Optionen
Mehrere Regelsätze gleichzeitig auswählbar | Keine Basisregeln für CERT / andere Standards
GUI integriert in Visual Studio | **Kein CLI**
---
### CppCheck
* Eigenständiges OpenSource Tool
* Vordefinierte Rulestes, die erweiterbar sind
* Nutzbar unter allen OS
+++
![CppCheck UI](https://puu.sh/zjhIJ/678a14b4df.png)
+++
Pro | Contra
----|-------
Einfache Installation | **Nicht alleine für Security Probleme gedacht**
Findet Buffer und Integer Overflows | Fehlermeldungen
GUI und CLI | **Keine Analyse für Input Validations**
