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
