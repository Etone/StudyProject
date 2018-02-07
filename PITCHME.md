# Vulnerability Detection in Sourcecode
**Securityrelevante Schwachstellen in Sourcecode und Tools zur Erkennung dieser**

---

@title[Motivation]
## Motivation

+++
@title[Netflix Architecture]
![Netflix Architecture](https://image.slidesharecdn.com/archtutorialgluecon-pptx-120524120713-phpapp02/95/netflix-architecture-tutorial-at-gluecon-23-728.jpg?cb=1394002823)

Note:
Netflix Architecture, Systeme werden immer komplexer, unüberschaubar.
Angreifer müssen nur eine Schwachstelle in Systemen finden, um Probleme zu verursachen

+++
@title[Warum Linter]
* Schnelles Beheben von Securityrelevanten Problemen notwendig
* Vorzeitiges erkennen von Problemen dabei notwendig
* **Notwendigkeit für Analyse Tools (Linter)**

---
@title[Security Flaws]
## Security Flaws

+++
@title[Liste untersuchter Flaws]
* Input Validation
    * SQL Injection  
    ![XKCD Exploits of a Mom](https://imgs.xkcd.com/comics/exploits_of_a_mom.png)
    * Integer Parsing
* Bibliotheken
    * Qt
    * boost

Notes:
Untersuchte Flaws SQL Injection und Integer Parsing im eigenen Code
Qt und boost als Beispiel für Security Flaws in Libs
