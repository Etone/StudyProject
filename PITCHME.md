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
    * Integer Parsing
* Bibliotheken
    * Qt
    * boost

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
  queryString += "'" + condition + "'";

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
Aufruf mit Query.exe a; SELECT * FROM table
