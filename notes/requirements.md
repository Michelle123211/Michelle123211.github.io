---
layout: post
title: Témata magisterských SZZ
subtitle: Vývoj počítačových her
---


## 1. Vývoj počítačových her

Programování počítačových her; problematika herních mechanik, herní návrhové vzory, skriptování her.

Architektura herních engine; vrstvy architektur, výpočetní modely, entity-component system, správa paměti, příklady konkrétních instancí architektur.

Herní design; kdo je herní designér, osy herního designu, herní žánry, specifika herních platforem, game design dokument (vlastnosti, struktura, UML diagramy pro popis herních mechanismů, herní prostor, postavy, specifikace dialogů), historie herního trhu.

Vývojový cyklus počítačové hry; fáze vývojového cyklu, herní design řízený daty, správa dat, testování počítačových her, vývojářské role, herní analytiky, vodopádový model a agilní metodiky návrhu her, obchodní modely komercializace her.

Narativita a hry; rozdíl mezi games of emergence a games of progression, chtěná a nechtěná emergence, environmentální storytelling, procedurální rétorika, ludonarativní disonance.

**Doporučené předměty**:

| **kód**                                                      | **Předmět**                   | **Kredity** | **ZS**   | **LS**   |
| ------------------------------------------------------------ | ----------------------------- | ----------- | -------- | -------- |
| [`NCGD001`](http://is.cuni.cz/studium/garantlink.php?glogin=false&gmodul=predmety&gscript=redir.php&redir=predmet&kod=NCGD001) | Vývoj počítačových her 1      | 6           | —        | 2/2 Z+Zk |
| [`NCGD003`](http://is.cuni.cz/studium/garantlink.php?glogin=false&gmodul=predmety&gscript=redir.php&redir=predmet&kod=NCGD003) | Programování herních mechanik | 4           | 1/2 Z+Zk | —        |
| [`NCGD004`](http://is.cuni.cz/studium/garantlink.php?glogin=false&gmodul=predmety&gscript=redir.php&redir=predmet&kod=NCGD004) | Úvod do herního designu       | 3           | 1/1 Z+Zk | —        |

## 2. Počítačová grafika pro hry

Homogenní souřadnice, afinní a projektivní transformace v rovině a v prostoru, kvaterniony.

Spline funkce, interpolace kubickými spliny, Bézierovy křivky, Catmull-Rom spliny, B-spliny.

Vzorkování a kvantování obrazu, anti-aliasing, textury, změna kontrastu a jasu, kompozice poloprůhledných obrázků.

Reprezentace 3D scén, výpočet viditelnosti, výpočet vržených stínů, měkké stíny, rozptyl světla pod povrchem, modely osvětlení a stínovací algoritmy, rekurzivní sledování paprsku, fyzikální model šíření světla (radiometrie, zobrazovací rovnice), algoritmus sledování cest, předpočítané globální osvětlení, výpočet globálního osvětlení v reálném čase, stínování založené na sférických harmonických funkcích, předpočítaný přenos radiance.

Animace postav, skinning, rigging, morphing.

Architektura grafického akcelerátoru, předávání dat do GPU, textury a GPU buffery, programování GPU - shaderů.

Základy OpenGL, GLSL, CUDA a OpenCL.

Principy komprese rastrové 2D grafiky, standard JPEG, komprese videosignálu.

**Doporučené předměty**:

| **kód**                                                      | **Předmět**                      | **Kredity** | **ZS** | **LS**   |
| ------------------------------------------------------------ | -------------------------------- | ----------- | ------ | -------- |
| [`NPGR033`](http://is.cuni.cz/studium/garantlink.php?glogin=false&gmodul=predmety&gscript=redir.php&redir=predmet&kod=NPGR033) | Počítačová grafika pro vývoj her | 5           | —      | 2/2 Z+Zk |
| [`NSWI072`](http://is.cuni.cz/studium/garantlink.php?glogin=false&gmodul=predmety&gscript=redir.php&redir=predmet&kod=NSWI072) | Algoritmy komprese dat           | 3           | 2/0 Zk | —        |

**Z předchozí akreditace**:

| **kód**                                                      | **Předmět**                | **Kredity** | **ZS**   | **LS**   |
| ------------------------------------------------------------ | -------------------------- | ----------- | -------- | -------- |
| [`NPGR003`](http://is.cuni.cz/studium/garantlink.php?glogin=false&gmodul=predmety&gscript=redir.php&redir=predmet&kod=NPGR003) | Základy počítačové grafiky | 5           | 2/2 Z+Zk | —        |
| [`NPGR004`](http://is.cuni.cz/studium/garantlink.php?glogin=false&gmodul=predmety&gscript=redir.php&redir=predmet&kod=NPGR004) | Fotorealistická grafika    | 5           | —        | 2/2 Z+Zk |
| [`NPGR019`](http://is.cuni.cz/studium/garantlink.php?glogin=false&gmodul=predmety&gscript=redir.php&redir=predmet&kod=NPGR019) | Realtime grafika na GPU    | 5           | —        | 2/2 Z+Zk |

## 3. Umělá inteligence pro počítačové hry

Architektura autonomního agenta; percepce, mechanismus výběru akcí, paměť; psychologické inspirace.

Metody pro řízení agentů; symbolické a konekcionistické reaktivní plánování, hybridní přístupy, prostor rozhodování, if-then pravidla, skriptování, sekvenční konečný automat, stromy chování.

Problém hledání cesty; lokální navigační pravidla (Raynoldsovy steeringy, VO, RVO, Context steering), hledání cesty (A\*, JPS+, goal bounding, RRT, RRT\*, LPA\*, MPAA\*, obousměrné prohledávání), reprezentace prostoru (geometrie, viditelnost).

Komunikace a znalosti v multiagentních systémech, ontologie, řečové akty, FIPA-ACL, protokoly.

Distribuované řešení problémů, kooperace, Nashova ekvilibria, Paretova efektivita, alokace zdrojů, aukce.

Metody pro učení agentů; zpětnovazební učení, základní formy učení zvířat.

Procedurální modelování stavového prostoru (forward model) a jeho prohledávání; A\*, ABCD, MCTS a UCB a další varianty, PGS, PGS-II, prostor skriptů (Kiting, AV, NOK-AV), efektivní implementace.

Klasifikace metod procedurálního generování.

Přístupy pro generování terénu, vizuálních efektů, hudby, předmětů, bludišť a dungeonů.

Šumové funkce (Perlin, Simplex, Worley).

Celulární automaty, L-systémy, grafové a tvarové gramatiky.

Answer set programming. 

Algoritmus kolapsu vlnové funkce. 

Metody smíšené iniciativy.

**Doporučené předměty**:

| **kód**                                                      | **Předmět**                                     | **Kredity** | **ZS**   | **LS**   |
| ------------------------------------------------------------ | ----------------------------------------------- | ----------- | -------- | -------- |
| [`NAIL068`](http://is.cuni.cz/studium/garantlink.php?glogin=false&gmodul=predmety&gscript=redir.php&redir=predmet&kod=NAIL068) | Umělé bytosti                                   | 5           | —        | 2/2 Z+Zk |
| [`NAIL106`](http://is.cuni.cz/studium/garantlink.php?glogin=false&gmodul=predmety&gscript=redir.php&redir=predmet&kod=NAIL106) | Multiagentní systémy                            | 5           | —        | 2/2 Z+Zk |
| [`NAIL122`](http://is.cuni.cz/studium/garantlink.php?glogin=false&gmodul=predmety&gscript=redir.php&redir=predmet&kod=NAIL122) | Umělá inteligence pro počítačové hry            | 3           | 1/1 Z+Zk | —        |
| [`NAIL123`](http://is.cuni.cz/studium/garantlink.php?glogin=false&gmodul=predmety&gscript=redir.php&redir=predmet&kod=NAIL123) | Procedurální generování obsahu počítačových her | 3           | —        | 1/1 Z+Zk |

**Z předchozí akreditace navíc**:

| **kód**                                                      | **Předmět**      | **Kredity** | **ZS** | **LS** |
| ------------------------------------------------------------ | ---------------- | ----------- | ------ | ------ |
| [`NAIL054`](http://is.cuni.cz/studium/garantlink.php?glogin=false&gmodul=predmety&gscript=redir.php&redir=predmet&kod=NAIL054) | Adaptivní agenti | 3           | —      | 0/2 Z  |