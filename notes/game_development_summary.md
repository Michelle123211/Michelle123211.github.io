# Vývoj počítačových her

## 1. Programování počítačových her

**Game design** (z pohledu autorů) vs **Gameplay** (z pohledu hráče) vs **Game mechanics** (stavební bloky designu)

### Problematika herních mechanik

**Herní mechaniky** - rozhraní mezi hráčem a hrou; reakce na vstup hráče; dělají hru zábavnou

**Gameplay experience** - zkušenost/zážitek; vyvolává emoce; hráč je součástí hry

**Gameplay programmer** - od pro interakce, které dělají hru zábavnou; musí rozumět tomu, co dělá hru dobrou/špatnou; ví, co je technicky možné; zajišťuje dobrou playability; systém, který převádí vstup od hráče do herních mechanik

**Playability** - jak snadno/dobře se hra hraje; jak dlouho je možné hru hrát; jak moc je možné hru hrát; podobná pro všechny hráče podobné kategorie, objektivnější
*Sada vlastností popisujících player experience*: satisfaction, learning, efficiency, immersion, motivation, emotion, socialization

**Game feel** - abstraktní, ale hráči ihned poznají, pokud to chybí; zábavné interakce, responzivní

**Juiciness** - abstraktní; dobrý pocit z interakcí; *maximální výstup na minimální vstup hráče*; obvykle to vidíme nebo slyšíme (pohyb, smršťování a protahování, camera shake, zvuky, particles, slow motion); může být to samé jako playability

**Tweening** - juiciness; přecházíme mezi počátečním a cílovým bodem nějakou funkcí; změna pozice, scale, průhlednosti, barvy; parametrická rovnice (parametr čas); $\text{Tween}(A, B, t) = A + P(t) \cdot (B-A)$;
*Manipulace*: Square $x^2$, Flip $1 - x$, Exponentiate $x^N$, Scale(Function, t) $t \cdot \text{Function}(t)$, ReverseScale(Function, t) $(1 - t) \cdot \text{Function}(t)$; lze skládat a zanořovat (neměníme tak vůbec funkci, jen $t$ zvenčí)
*Funkce*: Linear $t$, Quadratic $t^2$, SmoothStartN $t^N$, SmoothStop $1-(1-t)^N$, SmoothStepN $\text{Lerp}(\text{SmoothStart(N-1)}, \text{SmoothStop(N-1), t})$
*Blending*: $\text{Mix}(a, b, \text{weightB}) = (1 - \text{weightB}) \cdot a + \text{weightB} \cdot b$, $\text{Crossfade}(a, b, t) = a + t \cdot (b - a) = (1-t) \cdot a + t \cdot b$ (jako Mix, ale samotné $t$ je váha)

### Herní návrhové vzory

*Základní*:

- **Command** - Příkaz jako objekt třídy `Command` s metodou `Execute()`. Mapování tlačítek, akce pro postavy, undo/redo (metoda `Undo()`).
- **Flyweight** - Oddělíme *intrinsic state* (instance to mají společné) od *extrinsic state* (unikátní pro instanci). Instanced rendering, tile-based mapa, dekorace ve hrách.
- **Observer** - Třída `Observer` s metodou `OnNotify()`. Třída `Subject` se seznamem observerů (veřejné API pro přidání/odebrání). Stačí jen delegát (reference na metodu/funkci). Achievement system, GUI.
- **Prototype** - Prototypová instance, metoda `Clone()`. Spawner, data modelling prototypes.
- **Singleton** - Jen jedna instance, globálně přístupná. Statická metoda pro instanci (lze lazy), privátní konstruktor. Problém s couplingem, více vlákny a lazy inicializací v nepředvídatelný okamžik. File system API, audio player, logger.
- **State** - Změna chování objektu změnou stavu. Stavový automat, stav zapouzdřený ve třídě `State` s virtuálními metodami `Enter()`, `Exit()` a dalšími (např. `Update()`). Stavy postavy (skákání, krčení, chůze), AI, vstup hráče, navigace v menu.
- **Manager-Controller-Script** - Manager (obvykle singleton), controller (herní logika pro konkrétní případ), script (jednotlivé featury, parametrizovatelný).

*Sequencing*:

- **Double buffer** - Dvojice bufferů, do jednoho zapisujeme, druhý se vykresluje. Prohazujeme (změna pointeru nebo kopírování dat. Vypadá jako atomická změna. Rendering, fyzika a AI (update stavu objektů).
- **Game loop** - Hra běží konstantně rychle bez ohledu na rozdílný HW. Zpracuje vstup, updatuje herní stav, vykreslí hru. Implementace: fixed time step, variable time step (nedeterministické, nestabilní), fixed update with variable rendering.
- **Update method** - Objekty s komponentou s `Update()` metodou. Game loop + Update method + Component.

*Decoupling* (změna v jednom nevyžaduje změnu ve druhém):

- **Component** - Objekt je jen kontejner na komponenty. Komponenty jsou znovupoužitelné a mohou spolu komunikovat.
- **Event queue** - Oddělení zaslání události/zprávy od zpracování. Fronta nezpracovaných. Oproti Observer pozorujeme přímo objekt zprávy, ne objekt odesílatele. GUI, tutoriál, audio.

*Optimization*:

- **Data locality** - Využívá se CPU caching. Data ve spojité oblasti paměti v pořadí zpracování. Entity-component-system, particle system, AI (rozdělení na hot data a cold data).
- **Dirty flag** - Odvozená data se počítají z primárních drahým výpočtem. Cachujeme hodnoty. Poznačíme, jestli jsou rozsynchronizované, přepočítáváme až když je potřebujeme. Scene graph, textový editor, physics engine (pohybující/nepohybující se objekty).
- **Object pool** - Předem vytvořená kolekce znovupoužitelných objektů (v souvislém kusu paměti), nemusíme pokaždé alokovat/uvolňovat paměť. Particles, audio.
- **Spatial partition** - Datová struktura, organizuje objekty dle pozice. Při změně pozice se updatuje. Efektivní vyhledávání. Mřížka, BSP, k-d trees, BVH, quadtree/octree. Jednotky v RTS, detekce kolizí.

### Skriptování her

Programovací jazyk enginu, skriptovací jazyk (interpretovaný, snadný; až při buildu se zkompiluje do nativního kódu), skript.

## 2. Architektura herních engine

**Herní engine** - Sada nástrojů pro usnadnění vývoje hry. Efektivní práce inženýrů, umělců i designerů. Game-agnostic, multiplatformní.

**Moduly** - např. rendering, physics, audio, animation, scripting, asset management, AI, networking, user interface.

**Library**
**Middleware** (jedna doména enginu, žádné IDE; FMOD, PhysX)
**Framework** (integruje více subsystémů, může mít vlastní execution loop; Phaser, MonoGame)
**Engine** (množina předchozích, vizuální editor, scripting, asset management, IDE; Unity, Unreal Engine, Godot)

**Monolithic** (konkrétní hra/žánr; GameMaker) vs **Modular** (snadno rozšiřitelné; Unity, Unreal Engine)

**Herní smyčka** - *single-threaded* (sada systémů, windows message pump, callback-driven, event-driven), *multi-threaded* (fork-join, per-system multithreading (částečně Unity), job model (false sharing; Unreal Engine))

*Windows message pump* - Program dává vědět, že je naživu. Kontroluje zprávy od OS a odpovídá, pak teprve provede iteraci game loop.

### Vrstvy architektur

**Game-specific subsystems** - *Game-specific rendering*, *Player mechanics*, *Game cameras*, *AI*, Weapons, Power-ups, Vehicles

**Game-agnostic layer** - *Gameplay foundations* (game worlds and object models, scripting system, artificial intelligence foundations, event/messaging system), *Rendering engine* (low-level renderer, scene graph, visual effects, front end), *Profiling and debugging*, *Physics system* (collision system, physics system), *Animation*, *Human interface devices*, *Audio*, *Networking/Multiplayer*

**Resources** (3D model, texture, material, font, skeleton, collision) - *Resource manager*

**Core systems** - *Assertions*, *Memory management*, *Math library*, *Data structures and algorithms*, *Asynchronous file I/O*

**Platform independence layer** - Wrapper nad platform-specific funkcemi

**3rd party SDKs** (obalit SDK middlewaru), **OS**, **Drivers** (abstrakce komunikace), **Hardware** (PC, mobilní platformy, herní konzole)

### Výpočetní modely

**Herní model** - Oddělit technické detaily toho, jak je svět updatován, od způsobu, jakým se definuje a vytváří. Obvykle *game-agnostic*. Abstrahovat herní objekty tak, aby se *snadno definovaly* a mohly být *zpracovány více různými systémy* současně.

**Inheritance-based** - Hierarchie tříd pro herní objekty, dědí od společného GameObject, implementují rozhraní pro různé systémy. Snadno vede na diamantovou dědičnost, která nelze.
**Composition-based** - Objekt složíme z chování implementujících rozhraní. Špatně rozšiřitelné, nepodporuje změny (nové rozhraní se musí injectovat, je třeba měnit kód).
**Component-based** - GameObject se skládá z komponent (data, funkcionalita), za runtime, nejsou zadrátované v kódu. Společné rozhraní IComponent, může být dědičnost i kompozice. Komponenty pro integraci do systému i gameplay feature. Způsoby implementace (postupně se rozšiřují): *rigid composition* (datové položky), *seznam komponent* (list/pole, špatně se získává konkrétní), *podtřídy* (potomek GameObject, komponenty se konfigurují v konstruktoru, přidají do seznamu), *instance* (ne třídy, jen instance GameObject (prázdný seznam komponent), manuálně přidáme komponenty, komponenty i GameObject implementují ICloneable)
(**Entity-component system**)

**Scene graph** - Stromová hierarchie objektů, lze seskupovat objekty. Komponenta `Transform`.

### Entity-component system

Navazuje na component-based, je to ale data-oriented design (oproti object-oriented). Dedikovaná pole komponent přímo v systémech. Nemusí se hodit pro všechny typy gameplay kódu.

**Entity** (herní objekt, ID lze přeložit na index komponenty v systému), **komponenty** (obvykle pouze data), **systémy** (manipuluje a řídí kód).

### Správa paměti

"Per-frame" blok paměti (pro poslání dat do dalšího framu)

**Memory manager** - Alokuje větší množství paměti hned ze začátku, pak si ji sám spravuje. Může použít různé strategie.

**Single sided stack-based** - Velký kus paměti, pointer na konec obsažené části. Při alokaci vrátíme pointer a posuneme o velikost požadovaných dat. Nelze libovolně uvolňovat (došlo by k fragmentaci).
**Double sided stack-based** - Dva pointery. Data pro level zleva, data pro frame zprava. Na konci framu resetujeme pravý pointer.
**Double-frame allocator** - Nejprve alokujeme všechna data levelu (pokud jsou fixní), pak z jedné strany paměť framu $n$, z druhé paměť framu $n-1$. Můžeme předávat data mezi framy.
**Pool allocator** - Definujeme memory budget (počet instancí), alokujeme dle toho paměť. Můžeme získávat a vracet instance. Seznam aktivních a nevyužívaných.

### Příklady konkrétních instancí architektur

**CryEngine** - vrstvy popsané výše
**Unity** - *asset management*, *scene management*, *custom scripting*, *rendering*, *UI*, *physics*, *animation*, *user input*, *game loop*, *multi-platform*, *audio and video*, *networking*, *navigation* a spoustu dalšího

##  3. Herní design

**Herní design** - sada hypotéz; rozhodnutí o mechanikách, obtížnosti, narativu, combatu, skóre, GUI; balancing; level design; začít zážitkem, co mají hráči cítit a prožívat

**Běžné požadavky**: *accessibility* (pick up and play), *instant save/load*, *always possible to win*

**Formální aspekty her** (nejen videoher) - *Active participation of players* (interaktivní), *Goal*, *Rules*, *Procedures/agency* (co můžeme dělat a co se pak stane), *Resources*, *Conflict*, *Outcome*, *Magic circle* (jiný svět, rozhodnutí v něm by neměla mít následky ve skutečném světě)

**Definice hry** - neexistuje přesná definice; dobrovolná aktivita, make-believe, založená na pravidlech, série smysluplných rozhodnutí, výzvy v simulovaném prostředí

**Elementy hry** - *Aestetics*, *Story* (posloupnost událostí, větvení), *Mechanics* (pravidla, procedury, cíle, druhy interakcí), *Technology* (možnosti i omezení)

### Kdo je herní designér

**Herní designer** - komunikuje s ostatními, přichází s nápady, má poslední slovo, vše pospojuje do funkčního celku, dokumentuje, uvědomuje si limity, rozumí hráčům

**Dovednosti** - *Playfulness*, *Communication*, *Ability to compromise* (naslouchat ostatním), *Teamwork*, *Pressure resistance*, *Game literacy*, *Creativity*

### Osy herního designu

**Osy pro návrh RPG**:
**Randomness** vs **Determinism** - Hráči by se neměli cítit tak, že prohráli kvůli špatnému hodu kostkou, ale kvůli volbě, kterou udělali. Determinismus můžeme přidat třeba omezeným intervalem náhodnosti (nelze úplně selhat, ani když máme smůlu).
**Mechanics** vs **Content** - Spoustu obsahu, nebo méně obsahu a lepší zážitek z mechanik.
**Story** vs **Freedom** - Lineární hra s příběhem, nebo open world hra s důrazem na svobodu.
**Options** vs **Approachability** - Spoustu možností postavy (statistiky, dovednosti) dělá hru nedostupnou pro nováčky.

**Challenge** - překážky po cestě a obtížnost, obvykle se zvyšuje
**Skills** - rozsah dovedností hráčova avatara (může se rozšiřovat, zužovat, měnit), mohou být power-upy

### Herní žánry

**Convergent evolution** - dvě cesty, které vedly k vytvoření moderních videoher:
*Action simulation ancestry* - války a souboje $\rightarrow$ sport (imitace války) a strategické hry (simulace taktiky války, např. šachy) $\rightarrow$ early competitive action game prototypes $\rightarrow$ arkádové hry a akční hry (konzolové a počítačové)
*Shared narrative ancestry* - fantasy knihy $\rightarrow$ stolní RPG $\rightarrow$ textové hry a online role-playing MUDs $\rightarrow$ grafické MUDs a MMORPGs

**Action** (shooters, fighting games, hack and slash)
**Adventure** (point-and-click, visual novels, interactive movie)
**Sports** (simulation, arcade, management)
**Simulation** (flighting sims, business/city, vehicle driving, life sims)
**Platformer** (2D traditional, puzzle platformer, run and gun)
**RPG** (action RPG, Japanese RPG, open world RPG)
**First-person shooter** (military FPS, hero FPS, immersive sims)
**Action-adventure** (cinematic action-adventure, action RPG, open world action adventure)
**Fighting** (2D fighters, 3D fighters, platform fighters)
**Real-time strategy** (traditional RTS, MOBA, tower defense)
**Racing** (simulation, arcade, futuristic)
**Shooter** (run and gun, bullet hell, third-person shooter, looter shooter)
**Puzzle** (physics puzzle, matching puzzles, logic puzzles)
**Casual** (match 3, hidden objects, hyper casual)
**Strategy game** (4X, real-time tactics, grand strategy)
**MMORPG** (fantasy MMORPG, sandbox MMORPG, action combat)
**Stealth** (tactical espionage, immersive sims, stealth horror)
**Party** (board & card games, trivia games, activity games)
**Action RPG** (looter shooters, soulslike, isometric)
**Tactical role-playing** (Japanese TRPGs, strategy-JRPG hybrids)
**Survival** (survival sandbox, survival horror, survival simulation)
**Battle royale** (hero shooters, military shooters)

### Specifika herních platforem

**Výhody**: Větší šance na dlouhodobější prodeje, šance nalezení zlaté platformy, levný zdroj příjmů, větší credibility.
**Nevýhody**: Není jednoduché pokrýt všechny, stojí to hodně prostředků, může to poškodit značku, musíme dělat kompromisy, údržba je náročná.
**Zaměřit se na**: *Control scheme*, *Form factor/experience*, *Interface*, *Technické schopnosti*

### Game design dokument

**Game design document** (GDD) - definuje vizi, ještě před vývojem, není to magický dokument, pomáhá efektivně komunikovat, iterativně se mění
Různé formy: *Design bible*, *Design wiki*, *One-page design*

**Funkce** - promyšlení, zapamatování, komunikace, zajištění financí.

**Vlastnosti** - krátký, stručný, cílený, ilustrovaná, jasně formulovaný.

**Struktura** - *shrnutí*, *pilíře návrhu*, *herní mechaniky* (core gameplay, game flow/loop, ovládání, postavy, nepřátelé, AI, multiplayer), *uživatelské rozhraní*, *grafika* (styl, nálada), *zvuky a hudba* (styl, formát, zvukové efekty, hudba), *příběh* (backstory, svět, popis postav, text a dialogy), *vývojové nástroje*, *rozvržení práce*, *analýza trhu* ...

### Historie herního trhu a jeho trendy

**Historie RPG** - *Akalabeth: World of Doom* (1979), *Final Fantasy* (1987)

**Historie herní grafiky** - *Mystery House*

**Historie arkádových her** (Atari, SEGA) - *Periscope* (1966), *Pong* (1972), *Space Invaders* (1978), *Pacman* (1980), *Donkey Kong* (1981), klesal zájem, 90. léta renesance (fighting games, multiplayer), *Street Fighter II* (1991), *Mortal Combat* (1992)

**Historie konzolí** (Atari, Nintendo, SEGA) - *Fairchild Channel F* (1976), *Atari 2600* (1977), *Nintendo Family Computer* (1983), *Nintendo Entertainment System* (1985), *SEGA Genesis* (1989)

**Historie eSports** - *Space Invaders turnaj* (1980), *Nintendo Power Fest* (1990), *Rock the Rock* (1994)

**Historie násilí ve hrách** - *Street Fighter II* (1991), *Mortal Combat* (1992), *Night Trap* (1992), *Doom* (1993)

**Ekonomická historie videoher**: Na začátku hodně silné arkády, pak začaly klesat. Konzole také relativně dost, ale 1983 úpadek. PC hry pak začaly růst, postupně až dodnes. Game Boy zahájil růst handheld konzolí, pak ale postupně nahrazené mobilními telefony (hodně velký nárůst až dodnes). 

**Aktuální trendy herního trhu**:
Největší tržby mobilní hry (a stále roste), pak konzolové, pak PC.
Největší tržby v Asii, pak v Severní Americe, pak v Evropě.
V Severní Americe sice málo hráčů, ale platí hodně.
Na evropském trhu hlavně Francie, Německo, Španělsko a Velká Británie. Nejvíc konzole, pak mobilní telefony.
Na mobilním trhu hlavně nákupy v aplikacích, méně placené aplikace.

**Aktuální trendy**: *Mobilní hry* (casual, free-to-play), *Free-to-play* (volitelné transakce), *Virtuální realita*, *Rozšířená realita* (omezení technologie, mobilní telefony), *Cross-platform play*, *Cloud gaming* (streamování her ze serveru), *Indie hry*, *Esports*, *Live services* (living games, nový obsah, battle pass), *Game streaming* (sledování herních streamů), *Game as a service* (měsíční přístup ke sbírce her)

## 4. Vývojový cyklus počítačové hry

### Fáze vývojového cyklu

Příprava, vývoj, stabilizace

**Plánování** - žánr, styl, cílové publikum, mechaniky, postavy, engine, analýza trhu, platformy, koncept/design, rozpočet, technologie, vydavatel, záložní plány
**Pre-produkce** - rozvržení práce, storyboard, prototyp, proof-of-concept, benchmarky
**Produkce** - modely, animace, zvuky, dialogy, mechaniky, grafika, UI, veškerý obsah
**Debug and testing** - rendering and performance issues, exploits, softlocks, difficulty, scripting and acting errors
**Pre-launch** - marketing, trailery, dema, early access, nachystání infrastruktury, otestování zátěže
**Launch** - zbývající bugy, quality of life vylepšení, finální úpravy
**Post-launch/Maintenance** - bug fixes, DLCs, game-balancing patches

### Herní design řízený daty

Data pomáhají pokládat lepší otázky. Dobrá příprava může nakonec ušetřit spoustu práce. Měli bychom to využít, abychom udělali hru lepší pro hráče.

**Marketing events** (*acquisition flow*, *ROI balancing*), **Internal data events** (události ve hře), **External data sets** (hodnocení v obchodě, shlédnutí na YT, trendy, demografická data), **Soft feedback** (recenze, play-testy)

**Příprava a aplikace dat** - *Data gathering* (co nejvíc dat), *Data evaluation* (dashboard, dotazy), *Data application* (value changes, new content, new features)

### Správa dat (resource management)

Nástroje třetích stran pro vytvoření (plugin), import do herního enginu (komprese, převod formátu).

**Resource manager** - Práce s assety, převod assetů do herního enginu i správa za runtime (nahrání/odnahrání). Nahrávání assetů je pomalé, buď vše předem, nebo asynchronně na pozadí připravit.
**Asset Conditioning Pipeline** (pravidla pro řešení závislostí) - DCC tool $\rightarrow$ Resource Exporter $\rightarrow$ Resource compiler $\rightarrow$ Resource Linker
**Resource database** - Přehled o assetech v editoru a závislostmi mezi nimi. Spravuje veškerá metadata. Můžeme pracovat s více typy zdrojů, vytvářet nové, mazat existující, prohlížet a modifikovat, přesouvat, odkazovat se na jiné, vyhledávat.
**Resource registry** - Registr nahraných zdrojů, dictionary s GUID jako klíčem a pointerem na zdroj v paměti jako hodnotou.

### Testování počítačových her

**Unit testy**

**Interní testování** vs **Uzavřené testování** vs **Otevřené testování**

**Play-testing** - *Playcentric paradigm* (hráč je klíčový), testujeme hru na hráčích
**Introspection** - Sledujeme, snažíme se to zhodnotit. Je to ale subjektivní. Musíme se dívat na hru jako jiní hráči.

**QA** (technické, chyby v kódu, stabilita) vs **GUR** (herní zážitek, hratelnost, skutečný zážitek oproti plánovanému; preparation, execution, analysis, reporting)

**Dotazníky** (flow, engagement, presence, agency, usability/playability) - *GameFlow*, *GEQ*, *Gameplay-Scale*, *GUESS*, *PANAS*, *Player Immersion in Computer Game Narrative*

**GUX metody** - *fyziologické*, *psychologické*, *chování*
**GUR metody** - *A/B testing*, *Telemetry analysis*, *Diary/camera study*, *Ethnographic field study*, *Online survey*, *Interview*, *Focus group*, *Heuristic evaluation*, *Review* (experti posuzují na základě svých zkušeností), *Usability test* (pozorujeme poznamenáváme), *Unmoderated usability test* (automatický test), *RITE test* (pozorujeme, poznamenáváme, vyřešíme, opakujeme), *Initial experience playtest*, *Extended playtest*, *Benchmark playtest* (na konci vývoje, standardizovaný test, self-report), *Market segmentation* (cluster analysis, self-report), *Personas* (fiktivní postava pro typ hráče), *Narrative usability* (prototyp narativu), *Card sort*

### Vývojářské role

**Production** (dává úkoly designu, sbírá výsledky QA) - *Publisher*, *Producer*, *Esports producer*, *Marketing executive*, *Assistant producer*, *Community manager*
**Design** (úkoluje všechny ostatní) - *Lead designer*, *Gameplay designer*, *Script writer*, *Level designer*, *UX/UI designer*
**Art** (vstup od Design, výstup do Animation a Technical art) - *Concept artist*, *3D modelling artist*, *Environment artist*, *Texturing artist*
**Animation** (vstup od Art, výstup do Programming) - *Animator*, *Technical animator*
**Technical art** (vstup od Art, výstup do Programming) - *Technical artist*, *VFX artist*, *Graphics programmer*
**Programming** (vstup od Design, Technical art, Animation, výstup do WA a Audio) - *Engine programmer*, *Gameplay programmer*, *Physics programmer*, *AI programmer*, *Generalist programmer*, *Tools engineer*, *Network programmer*, *VR/AR programmer*
**Audio** (vstup od Programming, výstup do QA) - *Music composer*, *Audio programmer*, *Sound designer*
**QA** (výstup do Production) - *QA tester*, *Build engineer*

### Herní analytiky

**Herní analytiky** - informace o zajímavých událostech, odpovědi na otázky, detekce problémů, porovnání variant

### Vodopádový model a agilní metodiky návrhu her

Rozlišení: Jestli je iterace daná časem, nebo obsahem. Jestli můžeme změnit výsledky předchozí iterace, nebo je jen rozšířit. Jestli můžeme znát obsah všech iterací předem.

**Waterfall** - Předem definované kroky. Požadavky, analýza, design, coding, testing, závěr. Špatně se reaguje na měnící se požadavky. Nefunguje dobře ve vývoji her (je třeba provádět rychlé create-run-play-test iterace).

**Agile** - Rychlejší iterace, continuous delivery. Není třeba plánovat roky dopředu, hodí se pro dlouhodobý rozvoj.
*Scrum* - Backlog, To do, In progress, Testing, Done. Sprint (plánování, review, daily scrum meeting).
*Kanban* - Jednodušší, žádné role, v podstatě žádné iterace jen flow (nemají pevnou délku, obsah se může měnit), maximální počet tasků ve sloupci omezen. Hodí se, když je třeba spoustu změn nebo když se vytváří spoustu věcí stejného druhu (assety - modely, animace, scénář).

### Obchodní modely komercializace her

**Game as a service** - Zaplatíme za měsíční přístup k velké sbírce her.
**Streaming services** - Hry se streamují ze serveru, není třeba drahý HW, stačí rychlé připojení.

**Freemium** - Hra je zdarma, uvnitř jsou placené featury.
**Premium** - Zakoupení fyzické kopie.
**Digital distribution platforms** - Digitální kopie, jen online. Steam, Google Play, Epic Games Launcher, berou si podíl z tržeb.
**Subscription** - Hráči pravidelně platí, aby mohli hrát.
**Microtransactions** - Často free-to-play hry; *in-app purchases*, *DLCs*, *battle/season pass*.
**Advertising**
**Real money competition**

**Publishing deal** - Externi publisher (financování, PR, marketing).
**Distribution deal** - Externí publisher (PR, marketing, lokalizace, ale bez financování).
**Self-publishing** - Vše sami, využijeme třeba Steam, Google Play.

## 5. Narativita a hry

**Dramatické elementy** (dělají zážitek poutavý emocionálně) - *Challenge* (tak akorát obtížná; *Flow*, balanc mezi výzvou a schopnostmi), *Play* (prostor, kde provádíme akce, svoboda, seberealizace), *Premise* (zasazení světa a děje, možnost interpretace, postupně se dozvídáme víc), *Characters*, *Story-telling* (nejistý konec, lineární nebo emergentní, může se větvit, smysluplné volby), *Conflict* (problém, který musíme vyřešit, emocionálně nás pojí k výsledku příběhu, ke hře a k postavě)

### Rozdíl mezi games of emergence a games of progression

Moderní videohry jsou výsledkem evoluce svou základních struktur. Liší se způsobem, jakým hráči představují výzvu.

**Games of emergence** - Málo jednoduchých pravidel, kombinují se. Více způsobů jak dosáhnout cíle. Nemáme nad tím kontrolu. Existují pro ně *návody*.
**Games of progression** - Předdefinovaná posloupnost výzev, strukturované, hodně prostoru pro storytelling. Máme nad tím velkou kontrolu. Existují pro ně *walkthrough*.

#### Chtěná a nechtěná emergence

**Emergence** jako siruace/chování, které nebylo předpovězeno.

**Chtěná emergence** (desirable) - Interakce vedou na zajímavý gameplay.
**Nechtěná emergence** (undesirable) - Zneužití pravidel, narušení struktury.

### Environmentální storytelling

**Environmentální storytelling** - Vyprávění příběhu pomocí prostředí, vizualizuje výsledky nějakých událostí, hráč to nějak interpretuje, přímo se účastní narativu.

### Procedurální rétorika

**Procedurální rétorika** - Hra předává nějakou zprávu nebo myšlenku skrz svůj systém pravidel, interakcí a procesů. Tím se liší od jiných typů médií.

Příklady: *Animal Crossing* (zadlužení), *Bully* (středoškolská politika).

### Ludonarativní disonance

**Ludonarativní disonance** - Gameplay a storytelling spolu dobře nefungují. Mechaniky jsou v rozporu s příběhem. Pak příběh neodpovídá tomu, co děláme. Když si to uvědomíme, naruší to flow.

Příklady: *Uncharted 4* (hodný člověk, zabíjí tisíce), *Spec Ops: The Line* (jsme hrdina, pak si uvědomíme, že ne), deskové hry (budujeme, někteří hráči mohou vychovat novou generaci, někteří ne, nedává to smysl)
