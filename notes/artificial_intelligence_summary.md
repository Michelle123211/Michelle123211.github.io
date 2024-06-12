# Umělá inteligence pro počítačové hry

## 1. Architektura autonomního agenta

**(Inteligentní) agent** - Je součástí *prostředí*, senzory jej *vnímá*, ovlivňuje jej *akcemi*, má nějaké *cíle*, které jsou delegované (někdo požádá agenta, aby něco udělal), je schopný pracovat samostatně.

**Objekt** to dělá *zdarma*, voláme *metodu*. **Agent** pro peníze nebo protože *chce*, posíláme *požadavek*.

**Architektura agenta** - Např. *Table-driven*, *Simple-reflex*, *Model-based reflex* (senzorický model, přechodový model), *Simple problem-solving* (plánování), *Knowledge-based* (báze znalostí, odvozovací procedury).

**Software agent** - *Ztělesněný*, *autonomní* (dostává jen hlavní cíl, jedná sám a nezávisle), *inteligentní* (reaktivní (vnímá změny, reaguje na ně), proaktivní (aktivně plní cíle, nečeká, sám je zdrojem událostí), sociální (komunikace pro dosažení společného cíle)).

**Intelligent Virtual Agent** (*IVA*) - Typ SW agentam další požadavky: *plně a pohyblivě ztělesněný* (animace, pohyb a interakce se světem), *komplexní virtuální prostředí*, *bounded rationality* (omezené prostředky).
Vstup ze *senzorů* zpracuje (vytvoří nová fakta), updatuje *paměť* (stav+fakta), zahájí *action-selection*, tj. *reasoning* (uvažování o světě, odvození faktů) a *decision-making* (produkuje akci, dle znalostí, cíle a předpřipravených plánů).
*Believable branch* (uvěřitelnost, věrohodnost) vs *plausible branch* (přijatelnost).

**Vlastnosti prostředí** - *Plně/částečně pozorovatelné*, *Epizodické/sekvenční*, *Statické/dynamické/semidynamické*, *Single/Multi-agent*, *Deterministické/stochastické/strategické*, *Diskrétní/spojité*, *Známé/neznámé*, *Turn-based/Real-time*, *Noiseless/noisy*

**Agent** $Ag: R_E \rightarrow A$, mapuje běh končící stavem prostředí (kompletní historie) na akci z konečné množiny (deterministicky).
**Prostředí** $Env = (E,e_0,T)$ - Konečný počet diskrétních stavů, přechodová funkce $T: R_A \rightarrow 2^E$ (kompletní historie, nedeterministické prostředí).

**Funkce užitku** $u: E \rightarrow \mathbb{R}$ - kvalita stavu vzhledem k cíli agenta, objektivní míra úspěchu.
**Užitek běhu** $u: R \rightarrow \mathbb{R}$ - agregujeme užitky stavů běhu (zprůměrování, minimum, maximum).
**Optimální agent** $Ag_{opt}= \text{argmax}_{Ag \in A} \sum_{r \in R(Ag,Env)} u(r) \cdot P(r|Ag,Env)$ maximalizuje *očekávaný užitek*.

**Intentional stance** - Pro porozumění systému vytvoříme *metafory*, pak dokážeme predikovat chování. Třeba přiřazujeme vlastnosti jako beliefs, desires a intentions neživým věcem.

**Cognitive architecture** - Nejjednodušší je *sensors-reflexes-effectors*, můžeme přidat *current situation* společně s *reasoning*, případně mnoho dalších podmodulů (*long-term memory*, *emotional reactions*, *language*, *scripts*).

**NPC mind** - V cyklu, *Sensors* $\rightarrow$ *Perception* $\rightarrow$ *Reasoning* $\rightarrow$ *Decision* (cíle, podcíle, akce) $\rightarrow$ *Behavior* (předpřipravená) $\rightarrow$ *Actions* (navigation, locomotion, animation). Pracuje se s *Memory/State*.

**Výpočetní model** - *Stav prostředí* $E$ (množina faktů), *perception* $P$ (z funkce $p: E \rightarrow P$), *memory/state* $S$, *action* $A$ (z action-selection $f: P \times S \rightarrow A \times S$), *action execution* $s: A^n \times E \rightarrow E$ (simulátor provede akce všech agentů).

### Percepce

**Percepce** $see: E \rightarrow P$ (ze stavu prostředí na vjem)

### Mechanismus výběru akcí

**Action selection** $f: P \times S \rightarrow A \times S$ (dle vjemů a aktuálního vnitřního stavu, volí z předdefinovaných akcí). Sestává z *reasoning* a *decision-making*.
**Action-selection context** je $c \in P \times S$​

**Action-selection mechanism** (*ASM*) pracuje s reaktivními plány pro implementaci action-selection funkce.

### Paměť

**Pure reactive agent** $Ag: E \rightarrow A$ - nezohledňuje kompletní historii prostředí, nic si nepamatuje.
**Agent with a state** - má vnitřní stav (paměť, klidně databáze znalostí), $see: E \rightarrow P$, $next: S \times P \rightarrow S$, $action: S \rightarrow A$, kde $S$  je množina všech vnitřních stavů.

**Perceptual Aliasing Problem** - Bez paměti, v různých stavech stejný vjem, nedokážeme rozeznat.

### Psychologické inspirace

**Kognitivní modelování** - simulování lidského myšlení, využívá se *psychologie* (z vnějšku) a *neurověda* (nahlížíme do mozku).

**Zpětnovazební učení** - inspirováno psychologií (behaviorismem).

**Emoce** jsou ohodnocující úsudky, motivují a navádí, mají adaptivní funkci, pomáhají doplnit chybějící informace (intuice). Zvyšují uvěřitelnost agentů (skutečná simulace, mockup). Emoční model ALMA. Detekce emocí (analýza sentimentu, výrazy obličeje).

**Practical reasoning** je inspirován lidskými procesy, např. **BDI architektura** (*intentional stance*).

## 2. Metody pro řízení agentů

**Simple reflex agent** - Rozhodování jen dle aktuálního vjemu/stavu (může mít stav, ale ne celou historii) a pravidel.

**Goal-driven agent** (*model-based*) - Snaží se předvídat stav světa po provedení akce, simuluje ostatní agenty, má vnitřní stav, sleduje cíl.

**Abstraktní architektury** pro návrh agenta - *reaktivní vs plánování*. *Hybridní* je nejúspěšnější.

### Symbolické a konekcionistické reaktivní plánování

**Reaktivní plánování** - Kolekce technik pro volbu akce (jen decision-making, ne reasoning). Jen právě jedna následující akce dle aktuálního kontextu (vjem+paměť), *předdefinované reaktivní plány* (chování), žádné prohledávání. Simple reflex agent.

**Symbolická reprezentace** prostředí a chování (např. logickými formulemi) a manipulace (dedukce, odvozování, dokazování). Plánování založené na *dokazování vět*.

**Theoretical reasoning** - Matematické uvažování, ovlivňuje jen, co si myslíme. Např. deductive reasoning agent.
**Practical reasoning** - Vede k akcím, od striktního plánování (dokazování) víc k reaktivitě (polidštění). Nejprve *deliberation* (výběr cíle), pak *mean-ends reasoning* (plánujeme, jak dosáhnout cíle). Např. BDI.

**Deductive reasoning agent** - Symbolická reprezentace, dokazování vět dedukcí pro volbu akce, databáze znalostí jako vnitřní stav (množina formulí, fakty, nekompletní). *Deliberation* pomocí odvozovacích pravidel (dedukce), chce odvodit $DB \vdash_P Do(a)$, nebo alespoň $DB\,\, !\vdash_P \neg Do(a)$. Nepraktické pro skutečný svět.

**BDI model** - *Beliefs* $B$ (znalosti, vnitřní stav; ne nutně pravdivé), *desires* $D$ (možné cíle, motivace; závislé na $B$), *intentions* $I$ (zvolený cíl). *Deliberation*: aktualizace beliefs $BRF: 2^{Bel} \times Per \rightarrow 2^{Bel}$, vygenerování desires $options: 2^{Bel} \times 2^{Int} \rightarrow 2^{Des}$ (aktuální cíl to ovlivňuje), výběr intentions $filter: 2^{Bel} \times 2^{Des} \times 2^{Int} \rightarrow 2^{Int}$. *Means-end reasoning*: plánování $plan: 2^{Bel} \times 2^{Int} \times 2^{Ac} \rightarrow Plan$ (STRIPS, logický přístup, akce jsou trojice $[P_a,D_a,A_a]$, plánovací problém je $[B_0, O, G]$ (počáteční beliefs, deskriptory akcí, cílový stav), chceme plán ($(a_1, ..., a_n)$, $B_i = (B_{i-1} \setminus D_{a_i}) \cup A_{a_i}$), který je *přípustný* ($B_{i-1} \models P_{a_i}$) a *vedoucí k cíli* ($B_n \models G$)). Můžeme mít *knihovnu plánů*. Lze zahrnout *reconsidering* (změna desires a intentions) a *replanning*.
**Procedural reasoning system** - Praktická implementace BDI. *Interpreter* updatuje beliefs, desires, intentions, využívá *plánovací komponentu* s knihovnou malých plánů (goal (stav po provedení), body (nemusí být jen lineární posloupnost), context). Desires jsou všechny konzistentní plány, intention se volí dle *užitku*. Pracuje se *zásobníkem* intentions.

**Reaktivní přístup** - *Interaction* (s prostředím), *embodiment* (tělo, nejen abstraktní matematický konstrukt), *emergence* (interakce jednoduchých chování).
**Reactive agent** - *Behavioral* (jednoduchá chování, kombinují se, třeba i vyvíjejí), *situated* (embodied v prostředí), *reactive* (reaguje na prostředí, bez komplexní deliberation).
**Subsymbolická reprezentace** - Jednodušší reprezentace, např. if-then, stavové automaty, neuronové sítě.

**Subsumption architektura** - Reaktivní. Množina jednoduchých chování/pravidel, každé je goal-oriented (zodpovědné za nějaký cíl), jednoduchá struktura (condition part, action part). Chování fungují paralelně, soupeří, mohou mít priority (zvolí se aplikovatelné pravidlo s nejvyšší prioritou).

**Agent network architecture** - Reaktivní, ale připomíná přístup neuronové sítě. Graf modulů, každý má akci, preconditions a postconditions (dle toho propojení), activation threshold. Zvolí se akce s nejvyšší aktivací. Moc komplexní, nepraktické.

**Connectionist architecture** - Používá se neuronová síť, na základě vstupu vrátí akci. Může být zafixovaná (feed-forward, forma reaktivního plánování).
**Tyrrell's free-flow architecture** - Hierarchická konekcionistická síť. Vstupní vrstva, paměť, chování (rozdělená na podchování, akce). Aktivace se propaguje skrz vážená spojení. Inhibition (potlačení ostatních chování). Priority pomocí penalizace (temporal, uncertainty). Dělá kompromis mezi chováními.
**Creatures** - Perception vrstva, concept vrstva (naučené situace), decision vrstva (neuron pro každou akci, přiřazený skript). Váhy se trénují zpětnovazebním učením. Žádná paměť.
**Black & White** - Desires, opinions+beliefs. Pro každé chování je perceptron, vstupy jsou nějak aktivované, přenásobí se váhami, sečtou, $\sum w_i x_i$. Pokud je výsledná intenzita nad threshold, z desire se stane intention. pětnovazební učení k úpravě vah $w_i \leftarrow w_i + \Delta w_i = w_i + (\eta \cdot (intended\_value - actual\_value)\cdot x_i)$.

### Hybridní přístupy

**Hybridní architektura** - Reaktivní vrstvy (pro okamžité reakce bez složitých výpočtů) a plánovací vrstvy (na symbolické úrovni, vytvářejí plány).
*Horizontální hybridní architektura* - Vrstvy paralelně připojené k senzorům a efektorům, mediator funkce posbírá informace a zvolí akci. Např. Touring Machines.
*Vertikální hybridní architektura* - Vrstvy jsou pod sebou, sériově mezi senzory a efektory. One-pass nebo two-pass. Např. InteRRaP, Stanley.

Dvě soupeřící teorie o tom, co je vědomí a mysl:
**Global Workspace Theory** - Top-down přístup. Vědomí se spustí, když ho potřebujeme. Mysl jako multiagentní systém, procesy mohou tvořit koalice, ty soutěží o naši pozornost. Vědomí je světelný paprsek, svítí na nejaktivnější koalici tasků (na to se soustředíme).
**Integrated Information Theory** - Bottom-up přístup. Vědomí je emergentní vlastnost spolupráce mnoha jednoduchých neuronů. Úroveň vědomí systému závisí na komplexnosti.

**IDA** (Intelligent distribution agent) - Komplikovaná architektura, inspirovaná Global Workspace Theory. Kousky kódu, běží, mohou tvořit koalice. Pak perceptual vrstva (stará se o vstup), conceptual vrstva (předzpracování vjemů do konceptů vyšší úrovně, předprogramované plány), pak goal vrstva (deliberation, spouští koalice). Různé druhy myslí, mechanismus pro *modelování emocí*, *neuronová síť*. Použito v praxi pro přiřazování námořníků v americkém námořnictvu.

### If-then pravidla

**Simple reactive planning** (*SRP*) - If-then pravidla jako ASM, akce na základě precondition. Více firing pravidel - parallel (všechna), prioritized (jen první, nejčastější) nebo floating priority (priority na základě kontextu, pak první).

**Simple hierarchical reactive planning** (*SHRP*) - Hierarchie if-then pravidel (podchování jako efekt pravidla). Chování tak *rozdělujeme hierarchicky* na top-level cíle, podcíle, tasky, atomické akce.

**Etologický pohled** - Čtyři kategorie chování: *appetitive* (příprava), *taxis* (přesun), *consumatory* (naplňování cíle), *clean* (závěrečný úklid). Seřadíme, nejprve cílová podmínka, pak clean, pak appetitive (prerekvizita), taxis, consumatory. Odolné vůči počátečnímu stavu prostředí a náhlým změnám.

Priority *zjednodušují podmínky* (neplatí kontexty nad tím). Také mohou pravidla výš kdykoliv *přerušit* ta níž. $N$ prioritizovaných pravidel modeluje $\frac{N \cdot (N-1)}{2}$ přerušení/přepnutí.

**Perceptual Aliasing Problem** (*PAP*) - Action-selection čistě na vjemech ($f: P \rightarrow A$), nevyužíváme paměť (nebo ne dobře). Pak nerozlišíme různé kontexty, vypadají stejně.
V **SHRP** se manipulace se stavem provádí špatně. ve FSM nebo BT je to implicitně zabudované.

Reaktivní techniky lze **kombinovat**, třeba na jedné úrovni mít SHRP, jako podchování FSM. Všechny architektury reaktivního plánování mohou být **přeloženy** do if-then pravidel (ekvivalentní v expresivitě).

### Sekvenční konečný automat

**Konečný stavový automat** (*FSM*) - ASM, udržování stavu je implicitně zabudované (aktuální stav je část agentovy paměti $S$). Každý stav odpovídá jednomu stavu mysli a je spojen s akcí. Specifikujeme podmínky přechodu.

**Hierarchical FSM** (*hFSM*) - Uvnitř stavu je více podstavů nebo pravidel, hierarchie konečných stavových automatů.

**Sequential FSM** (*sFSM*) - Každý stav má $T$ uspořádaný seznam přechodů (trojice $(c, s, a)$, podmínka přechodu, následující stav, `OnTransition` aktivita) a trojici aktivit (`OnEnter`, `OnInternal`, `OnExit`).

**Hierarchical sequential FSM** (*hsFSM*) - V `OnInternal` aktivitě můžeme třeba řešit zanořený sFSM. Používají se pro IVA. Výpočetně ekvivalentní sFSM.

Snadné popsání **sekvencí** akcí a **alternativ** k akcím (v if-then bychom potřebovali paměť). Také **cyklení** chování.
Musíme ale explicitně specifikovat každý přechod (může být až úplný graf).

### Skriptování

**Skriptování** - Chování/plány přímo v nějakém programovacím jazyce. Nadmnožina SHRP (základní typy, aritmetické výrazy, přiřazení, cykly, rekurze, metody, třídy apod.).

Lze snadno kombinovat **prioritizované** i **paralelní** vyvolávání akcí.
Špatně se reprezentují sekvence a alternativy (blokující akce znemožní reaktivitu, je třeba pamatovat si stav metod v globálních proměnných, ale hodně kódu).

**Behavior oriented design** (*BOD*) - Metodologie návrhu, chování rozložíme top-down způsobem na menší s jasným cílem (funkce senzorů, podchování/akce), ty implementujeme a testujeme separátně, pak kombinujeme.

### Stromy chování

**Stromy chování** (*BT*) - ASM, chování popsané stromem, začíná se v kořeni, pak depth-first, shora dolů, zleva doprava. Vnitřní uzly (*sekvence*, *alternativy*), listy (*kontroly*, *akce*). Každý uzel hlásí rodiči stav (*success*, *failure*, *running*), propaguje se nakonec až do kořene. Nemusíme definovat všechny přechody mezi podstromy. Jsou *modulární*, protože všechny uzly mají stejnou sémantiku.

**Pasivní BT** - Příště se pokračuje, kde se skončilo (perzistentně uložený pointer na aktuální uzel, stav sekvencí a alternativ), podobné FSM. Používá *asynchronous action execution model* (ready nebo running, uzel nelze přerušit).

```pseudocode
function BT.Evaluate()
	return BT.curr.Evaluate() // z aktuálního uzlu, jako FSM
	
function BTNode.Evaluate()
	child-to-run or result = this.Visit() // výsledek nebo potomek ke spuštění
	if (result) // akce nebo kontrola
		if (result is RUNNING) BT.curr = this // akce běží, budeme pokračovat odsud
		else this.Reset() // reset, aby byla akce použitelná příště
		return result
	else // potomek ke spuštění
		result = child-to-run.Evaluate() // rekurzivní sestup
		if (result is SUCCESS or FAILURE)
			note and restart function // interpretujeme dle typu uzlu, pak restart
		return result // propagujeme RUNNING rodiči
```

**Aktivní BT** - Umožňuje aktivní přepínání mezi podstromy (podobně jako SHRP nebo scripting), pokaždé se začne od kořene. Používá *terminable action execution model* (navíc terminate signál). Kromě sekvencí a alternativ lze *switch* (aktivní selektor, hrany do potomků s podmínkami).

```pseudocode
function BT.Evaluate()
	return BT.root.Evaluate() // vždy z kořene
	
function BTNode.Evaluate()
	child-to-run or result = this.Visit() // výsledek nebo potomek ke spuštění
	if (result) // akce nebo kontrola
		if (result is RUNNING) // akce běží, ale neoznačíme jako aktuální
		else this.Reset() // reset, aby byla akce použitelná příště
		return result
	else // potomek ke spuštění
		if (this.running is set and this.running != child-to-run)
			this.running.Terminate() // ukončíme, pokud přepínáme
		result = child-to-run.Evaluate() // rekurzivní sestup
		if (result is RUNNING)
			this.running = child-to-run // poznamenáme, který potomek běží
			return result
		else // potomek uspěl, nebo selhal
			unset this.running
			note and restart function // interpretujeme dle typu uzlu, pak restart
```

Máme posloupnosti úspěchů (sekvence) nebo posloupnosti neúspěchů (alternativy), ale ne uzly, které reagují různě a úspěch a selhání.

**Decorators** - Např. "No Fail", potlačí selhání v celém svém podstromu (pošle dál úspěch).

Lze přidat *parallel node* - paralelně provede všechny své potomky (ve skutečnosti sekvenčně ve stejném framu).

### Prostor rozhodování

**Decision space** (*DS*) - Společná abstrakce všech předchozích technik reaktivního plánování (if-then pravidla, FSM, BT, skriptování). Umožňuje jak přepínání chování a prioritizaci chování od SHRP, tak sekvence a alternativy od FSM. Dokáže realizovat libovolný výpočet deterministického Turingova stroje.

**Decision point** - Sjednocuje seznam if-then a stav sFSM. Podporuje dvě interpretace spojení, výsledkem je buď spuštění jiného (reference, z if-then, předá execution potomkovi) nebo nahrazení jiným (z sFSM). `SELECT` funkce volí potomka pro spuštění (jako u uzlů v BT).

<img src="./img/decision_space_2.png" alt="Generic Decision Point" style="zoom:50%;" />

Lze modelovat: *if-then-else*, *switch-case*, *for*, *while*, *sFSM stav*. Vše ze Scriptingu a také FSM. Navíc je můžeme libovolně *kombinovat dohromady*, tak dokážeme modelovat i *paralelismus* (parallel node jako v BT, v sekvenci referencuje všechny potomky) nebo třeba *rekurzi* v rámci jednoho plánu.

**Switchable action execution model** - Kromě terminate signálu také *switch* (asynchronní, umožní cleanup), přepne do Transition stavu (pro transition behavior, klidně přes několik framů).

Máme **zakořeněný graf** decision points, začíná se pokaždé v kořeni. Hledáme terminální list (jako prohledávání, ale řídí ho select funkce).

**Implicit Decision Space** (*IDS*) je pětice $(D, O, Options(d), Outcome(d, o), Cost(d, o))$, množina bodů rozhodnutí (obsahují select funkci), množina všech možností rozhodnutí, $Options(d): D \rightarrow O^*$ (enumeruje možnosti daného bodu), $Outcome(d, o): D \times O \rightarrow D$ (výsledek rozhodnutí), $Cost(d, o): D \times O \rightarrow \mathbb{R}$ (cena reasoningu, zvažování rozhodnutí).

### (Srovnání)

| Decision-making patterns                       | SHRP | hsFSM | Scripting |      BT      |  DS  |
| ---------------------------------------------- | :--: | :---: | :-------: | :----------: | :--: |
| *Priority rules / Switching / Active selector* |  X   |       |     X     |      X       |  X   |
| *Sequences*                                    |      |   X   |           |      X       |  X   |
| *Alternatives / Passive selector*              |      |   X   |           |      x       |  X   |
| *Mixed rules evaluation*                       |      |       |     X     |              |  X   |
| *Parallel actions*                             |      |       |     X     |      x       |  X   |
| *Behavior looping*                             |      |   X   |           |      X       |  X   |
| *Flexible latching (vstupní + výstupní)*       |      |       |           | Can be added |  X   |
| *Transition behaviors*                         |      |       |           |              |  X   |

*Scriptiing* a *FSM* se navzájem doplňují (co je těžké v jednom, lze ve druhém snadno).
*DS* zobecňuje ostatní, takže dokáže vyjádřit všechny patterny v tabulce.

## 3. Problém hledání cesty

**Navigace**: *Action-Selection* (cíle a lokace), *pathfinding* (seznam bodů), *path following* (steering), *stuck detectors* (přeplánování).

**Path-finding** - Nalezení dobré (nejkratší, nejlevnější, nejbezpečnější, nejuvěřitelnější) cesty (seznam buněk, bodů, křivek) mezi počátečním a cílovým bodem (1:1, 1:N, N:1, N:M) v dané abstrakci (ne/úplná, navgraph, navmesh, tiles) prostředí (statické/dynamické, ne/úplné, ne/známé).

Dvě komponenty: *abstrakce prostředí* a *prohledávací algoritmus*.

**Problémy**: Velká skupina po stejné cestě, lepší jsou alternativy. Vedoucí jde po cestě, ostatní následují, mohou se zaseknout. Jednotky mají collision radius, pathfinding neřeší dynamické prvky. Nemusí jít udržet formace.

**Požadavky**: Časová a prostorová složitost, složitost implementace. Délka předpočítání. Optimalita cesty. First move delay. Udržování skupiny pohromadě. Ne/překrývání jednotek. Řešení dynamických překážek.

**Zjednodušení**: Svět rozdělit do mřížky, v každé max. 1 jednotka (kolize se řeší prohozením). Neřešit udržení skupiny. Při selhání prostě zastavit. Ignorovat collision radius nebo použít jen velmi malý (+ steering).

### Lokální navigační pravidla

**Lokální navigační pravidla** - Řízení low-level pohybu autonomních agentů. Máme množinu chování/pravidel, v každém updatu každé vrátí steering force (vektor), ty zkombinujeme (vybrat jen jednu, nebo průměr) na finální velocity, $v_t = \frac{w_0 \cdot v_{t-1} + \sum_{i \in I} (w_i \cdot s_i)}{w_0 + \sum_{i \in I} w_i}$. Levné, produkují smooth paths, ale někdy je těžké je parametrizovat. Emergentní skupinové chování.

Lze kombinovat s path-planning.

**Výhody**: Jednoduché, předvídatelné, reaktivní, efektivní, hladký pohyb, lze kombinovat.

**Nevýhody**: Skupina se může rozdělit, není globální lookahead, může se zaseknout, je těžké je parametrizovat a debugovat.

#### Reynoldsovy steeringy

**Movement algorithm**:

```pseudocode
force = steering.calculate(args) // sečteme aktivní steering rules
accel = force / mass
accel = clampLen(accel, maxAccel)
velocity = velocity + accel * timeDelta
velocity = clampLen(velocity, maxVelocity)
position += velocity * timeDelta
speed = velocity.length
look-direction = velocity.normalized
```

*Jednotlivci a dvojice*: **Seek** (ke statickému cíli), **Flee** (od statického cíle), **Arrival** (Seek se zpomalovací zónou), **Pursue** (k dynamickému cíli, předpovídá lokaci, pak Seek), **Evade** (od dynamického cíle, předpovídá lokaci, pak Flee), **Wander** (náhodný offset), **Obstacle Avoidance** (od největší hrozby v prostoru před agentem, fixní paprsky), **Containment** (budoucí pozice uvnitř oblasti), **Path Following** (budoucí pozice se promítá na cestu, může se trochu vzdálit), **Wall Following** (ke zdi, pak podél ní v určité vzdálenosti), **Flow Field Following** (dle velocity z budoucí pozice ve flow field)

*Kombinovaná chování a skupiny*: **Flocking** (lokální hejno, Separation + Cohesion + Alignment), **Leader Following** (Arrival kousek za vůdce + Separation, před vůdcem do strany), **Unaligned Collision Avoidance** (vyhnout se místu potenciální kolize s ostatními, lze zrychlit/zpomalit), **Crowd Path Following** (Path Following + Separation), **Queueing** (Seek ke dveřím + Avoid od zdí + Separation + brždění, když je vepředu pomalejší)

#### VO

Pro vyhýbání se dynamickým překážkám (musíme znát pozici, velocity a zhruba tvar), předpokládáme, že se pohybují s konstantní velocity.

**Velocity Obstacle** (*VO*) je množina všech velocities, které v určitém bodě v čase povedou ke kolizi.
$VO_{AB}(\mathbf{v}_B) = \{\mathbf{v}_A | \lambda (\mathbf{p}_A, \mathbf{v}_A - \mathbf{v}_B) \cap B \oplus -A \neq \empty \}$ - Paprsek z $A$ ve směru přibližování, průsečík s Minkowského součtem ($A \oplus B = \{ \mathbf{a} + \mathbf{b} | a \in \mathbf{a}, \mathbf{b} \in B \}$​) neprázdný.
Geometricky lze reprezentovat jako kužel s vrcholem v $\mathbf{p}_A + \mathbf{v}_B$.

V každém plánovacím cyklu se přepočítá $VO_{AB}(\mathbf{v}_B)$ (po všechny pohybující se překážky) a zvolí se $\mathbf{v}_A \notin VO_{AB}(\mathbf{v}_B)$, pokud možno co nejvíc k cílové pozici.

**Symetrie**: $\mathbf{v}_A \in VO_{AB}(\mathbf{v}_B) \Leftrightarrow \mathbf{v}_B \in VO_{BA}(\mathbf{v}_A)$. Pokud agent A vnímá kolizi, vnímá ji i agent B.
**Invariance translace**: $\mathbf{v}_A \in VO_{AB}(\mathbf{v}_B) \Leftrightarrow \mathbf{v}_A + \mathbf{u} \in VO_{BA}(\mathbf{v}_B + \mathbf{u})$​​. Pokud oba agenti změní velocities stejným způsobem, nepomůže to vyřešit kolizi.

Může **oscilovat**, problém řeší RVO.

#### RVO

Předpokládáme podobnou navigační strategii agentů, každý řeší polovinu konfliktu.

**Reciprocal Velocity Obstacle** (*RVO*) je $RVO_{AB}(\mathbf{v}_B, \mathbf{v}_A) = \{ \mathbf{v'}_A | 2 \cdot \mathbf{v'}_A - \mathbf{v}_A \in VO_{AB}(\mathbf{v}_B) \}$. Obsahuje velocities, které jsou průměrem aktuální velocity $\mathbf{v}_A$ a velocity uvnitř velocity obstacle $VO_{AB}(\mathbf{v}_B)$ (máme $\mathbf{v}_A' = \frac{\mathbf{v}_A + \mathbf{v}}{2}$, kde $\mathbf{v} \in VO$, tj. průměr aktuální a z VO, tedy $2 \cdot \mathbf{v}_A' - \mathbf{v}_A = \mathbf{v}$, proto $2 \cdot \mathbf{v}_A' - \mathbf{v}_A \in VO$).
Geometricky lze interpretovat jako $VO_{AB}(\mathbf{v}_B)$ posunutý tak, že jeho vrchol leží ve $\frac{\mathbf{v}_A + \mathbf{v}_B}{2}$.

Jako novou velocity zvolíme tu vedoucí do vrcholu $RVO$ (mimo $RVO$, ale co nejblíž aktuální). Pro více agentů vezmeme sjednocení RVOs.

Máme následující *garance*:
**Collision-free**: Pokud oba agenti zvolí velocities mimo RVOs, pak jsou oba na trase bez kolizí.
$\mathbf{v'}_A \notin RVO_{AB}(\mathbf{v}_B, \mathbf{v}_A) \and \mathbf{v'}_B \notin RVO_{BA}(\mathbf{v}_A, \mathbf{v}_B) \Rightarrow \mathbf{v'}_A \notin VO_{AB}(\mathbf{v'}_A) \and \mathbf{v'}_B \notin VO_{BA}(\mathbf{v'}_A)$
**Same side**: Pokud oba agenti zvolí velocities mimo RVOs, které jsou nejbližší jejich aktuální velocity (tj. zvolí velocity z vrcholu), automaticky zvolí tu samou stranu, na kterou uhnou (např. každý na svou pravou stranu, když jdou proti sobě).
$\mathbf{v}_A + \mathbf{u} \notin RVO_{AB}(\mathbf{v}_B, \mathbf{v}_A) \Leftrightarrow \mathbf{v}_B - \mathbf{u} \notin RVO_{BA}(\mathbf{v}_A, \mathbf{v}_B)$, kde pomocí $\mathbf{v}_A + \mathbf{u}$ značíme nejbližší velocity pro $A$ mimo RVO, podobně pro $B$.
**Oscillation-free**: Pokud agenti zvolí nejbližší velocity mimo RVO, nebude docházet k oscilaci.
$\mathbf{v}_A \in RVO_{AB}(\mathbf{v}_B, \mathbf{v}_A) \Leftrightarrow \mathbf{v}_A \in RVO_{AB}(\mathbf{v}_B - \mathbf{u}, \mathbf{v}_A + \mathbf{u})$. Stará velocity $\mathbf{v}_A$ je v novém RVO, takže se k ní agent $A$ nevrátí.

#### Context steering

**Context steering** - Máme malá bezestavová oddělená chování, která vrací jen kontext, ve kterém by dělala rozhodnutí, ale samotný rozhodovací krok se přeskočí. Kontexty se pak zkombinují ve finální rozhodnutí.

**Context map** - Pole skalárních hodnot, sloty reprezentují směry, hodnoty intenzitu. Každé chování vrací *danger map* a *interest map*. Můžeme zapsat falloff k nule kolem směru.

**Kombinace** - Porovnáme sloty mezi mapami všech chování, vezmeme maximum. Pak v danger map najít minimum, vymaskovat vyšší, aplikovat na interest map, vynulují se vymaskované. Zvolíme slot s nejvyšší hodnotou (směr dle slotu, rychlost dle intenzity) nebo průsečík gradientů z okolních (spojitý rozsah) nebo vážený průměr.

**Vylepšení**: Blur context mapy. Blend poslední context map s aktuální. Level of detail (rozlišení context map, nižší pro entity dál od hráče). Vícevláknově nebo v compute shaderu.

### Hledání cesty

**Náhodná procházka** - Žádné plánování, jen směrem k cíli. Při kolizi zvolíme jiný směr dle nějaké strategie: *random walk* (zcela náhodně), *wall following* (podél zdi, dokud nelze pokračovat k cíli), *robust wall following* (podél zdi, dokud nepřejdeme přes úsečku mezi prvním bodem nárazu a cílem, pak k cíli).

**Prohledávání grafů** - Libovolné grafy. Začneme v počátečním vrcholu, nově objevené přidáváme do open-listu, dle nějaké strategie z něj vybíráme další, prozkoumané vrcholy dáváme do closed-listu. Algoritmy se často liší jen v implementaci open-listu, strategie výběru vrcholu a closed-listu.

```pseudocode
make open-list
push start into open-list
while open-list not empty // search step (= 1 iteration)
	extract node from open-list according to strategy // node extraction/selection
	if node is target // node scanning
		return path to node
	else
		expand node by checking its direct neighbours possibly adding them to open-list // node expansion
		move expanded node to closed-list
return no-path
```

**Prohledávání do šířky** (*BFS*) - Fronta jako open-list, první ve frontě jako strategie. Neuvažuje terén, nepracuje s cenami, prohledává velké množství vrcholů.

**Dijkstrův algoritmus** - Prioritní fronta jako open-list, řazení dle délky cesty z počátku ($f(x, s) = g(s, x)$), první ve frontě jako strategie. Zohledňuje cenu, umí pracovat s různým terénem, prohledává ve všech směrech (nepracuje s cílovým vrcholem) a velké množství vrcholů.

**Best-first search** - Informované prohledávání s heuristikou. Prioritní fronta jako open-list, řazení dle odhadované ceny do cíle ($f(x, t) = h(x, t)$), první ve frontě jako strategie. Pracuje s cílovým vrcholem, vyžaduje heuristickou funkci $h: node \rightarrow \mathbb{R}$ pro odhady. Expanduje agresivně směrem k cíli, negarantuje optimální cestu, neuvažuje terén.

#### A\*

**A\*** - Prioritní fronta jako open-list, řazení dle součtu ceny od počátku a odhadu do cíle ($f(x, s, t) = g(s, x) + h(x, t)$), první vrchol ve frontě jako strategie. Pracuje s cílovým vrcholem, vyžaduje heuristickou funkci $h: node \rightarrow \mathbb{R}$. Kdykoliv se dotkne vrcholu, přepočítá $f(x)$, $g(x)$ a $h(x)$. Expanduje méně vrcholů než Dijkstra, uvažuje terén, s vhodnou heuristikou garantuje optimální cestu.

**Přípustná heuristika** - Pokud $h(x) \leq$ skutečná cena cesty do cíle (nenadhodnocuje). Důležité pro správnost, garantuje optimální cestu.
**Monotónní/konzistentní heuristika** - Pokud $h(a) \leq cost(a \rightarrow b) + h(b)$ pro každou hranu $(a \rightarrow b)$ (trojúhelníková nerovnost). Důležité pro efektivitu, vrcholy v closed-listu se neotevřou znovu (ceny hran ale musí být nezáporné).

Možné heuristické funkce $h(x, t)$ ($t$ je cílový vrchol):
*Manhattanská vzdálenost*: $h(x, t) = |x_x - t_x| + |x_y - t_y|$
*Maximová vzdálenost*: $h(x, t) = max(|x_x - t_x|, |x_y - t_y|)$
*Euklidovská vzdálenost*: $h(x, t) = \sqrt{(x_x - t_x)^2 + (x_y - t_y)^2}$

**Teleporty** vyžadují speciální heuristiku (produkují singularity). Pokud máme teleporty $r_1(a_1 \rightarrow b_1), ..., r_n(a_n \rightarrow b_n)$, upravená heuristika je $h'(x, t) = \min\{ h(x, t), \min_{i = 1..n}(h(x, r_i.a_i) + h(r_i.b_i, t)) \}$.

A\* lze nastavením funkcí $g$ a $h$ snadno převést na BFS, Dijkstru nebo Best-first search.

**Weighted A\*** - Různá váha u heuristické funkce, tj. $f(x, s, t) = g(s, x) + w \cdot h(x, t)$, lze udělat agresivnější, ale za cenu ztráty optimality řešení.

**Výhody**: Rychlejší než Dijkstra, najde optimální cestu, bere v úvahu terén, jednoduchý na implementaci, lze různě nastavovat parametry.

**Nevýhody**: Pokud cesta neexistuje, musí prohledat celý prostor. Velký first move delay. Neporadí si s dynamickým prostředím. Jen pro jednotlivce.

#### JPS+

**Jump Point Search (JPS)** - V pravidelné mřížce, s jednotnou cenou, 8-way pohybem (diagonálně cena $\sqrt 2$). Použije se A\* s lokálními mikroakcemi (lookahead skokem během expanze), ignorují se tak souběžné cesty (vynechávají se symetrické). Vezmeme vrchol z open-listu, z jeho rodiče určíme směr pohybu, pak skáčeme daným směrem co nejdál, zajímavé vrcholy (s forced neighbor nebo cíl) vložíme do open-listu. Pokud máme forced neighbour, expandujeme i tím směrem.

**Horizontální/vertikální lookahead** - Pro příklad jdeme doprava. Ignorujeme buňku za (rodič), buňky diagonálně vzadu, nad a pod (lepší z rodiče), buňky diagonálně vepředu (preferujeme ze sousedů). Dokud je cesta volná, jdeme o krok dopředu a opakujeme.
**Forced neighbor** - Překážka nad/pod blokuje cestu k buňce diagonálně vepředu. V buňce s forced neighbour přestaneme skákat a přidáme ji do open-listu.
**Překážka** - Pokud při skoku jedním směrem narazíme na překážku, zahodíme celý skok.

**Diagonální lookahead** - Pro příklad jdeme nahoru doprava. Ignorujeme buňku za (rodič), buňky vlevo a dole (lepší z rodiče), buňky diagonálně vlevo nahoře a vpravo dole (lepší přes sousedy vlevo a dole). Uvažujeme jen sousedy nahoře, vpravo a diagonálně v původním směru pohybu.
**Forced neighbor** - Překážka nalevo/dole blokuje cestu k buňce diagonálně vlevo-nahoře/vpravo-dole.
**Lookahead** - Začneme sousedy nahoře a vpravo (jen horizontální/vertikální lookahead). Pokud nenajdeme zajímavý vrchol (tj. jen narazíme do překážky), popojdeme krok diagonálně a opakujeme. Pokud narazíme na zajímavý vrchol (s forced neighbor), přidáme aktuální do open-listu.

**JPS+** předpočítává místa skoků předem pro statická prostředí. Pro každou buňku a každý směr uloží číslo ($+N$ jako vzdálenost do dalšího místa skoku, $-N$ jako vzdálenost do překážky). Nemusí se dělat lookahead dynamicky během expanze.

**Primary jump spots** - mají forced neighbor, poznačíme pro jaký směr je místo skoku vytvořeno.
**Straight jump spots** - z nich lze jít do primary jen horizontálně/vertikálně, poznamenáme směr a vzdálenost.
**Diagonal jump spots** - z nich lze diagonálně do primary nebo straight, který pokračuje souvisejícím směrem (dle symetrických cest).
**Target jump spots** - až za runtime, pokud jdeme diagonálně, směr pohybu je směrem k cíli a vzdálenost k cíli v řádcích a sloupcích je menší nebo rovna vzdálenosti diagonálně k překážce nebo místu skoku (je šance skočit rovnou do cíle).

Nakonec převedeme šipky/směry na čísla podle vzdálenosti buněk v daném směru. Pokud není šipka, je tam překážka. Při **prohledávání** uvažujeme pouze směry s přiřazeným kladným číslem nebo target jump spots (zkoušíme diagonálně). Kontrolujeme jen směry dle směru pohybu.

#### Goal bounding

**Goal bounding** - Pro statická prostředí (libovolné abstrakce se známou topologií) předpočítá, kterých oblastí lze dosáhnout optimálně skrz určité hrany. Tím ořezává prohledávaný prostor (neexpandovat hrany, které nemohou ležet na optimální cestě k cíli, tj. neotevírat vrcholy vedoucí mimo cíl).

Pro každou orientovanou hranu uložíme **bounding box** s vrcholy, které jsou optimálně dosažitelné, pokud začneme pohybem přes tuto hranu.

Lze zaintegrovat do obecné šablony prohledávání grafů. V expanzi jen navíc kontrola, zda leží cíl v bounding boxu zkoumané hrany.

**Předvýpočet** se dobře paralelizuje. Postupně pro každý vrchol spustíme Dijkstrův algoritmus ve flood fill módu (bez cíle), expandované vrcholy označujeme hranou ze startu (první hranou cesty). Všechny označené stejnou hranou jsou optimálně dosažitelné skrz danou hranu. Pak pro každou hranu spočteme bounding box množiny vrcholů.

#### Obousměrné prohledávání

**Obousměrné prohledávání** - Dopředné z počátku do cíle, zpětné z cíle do počátku (potřebuje funkci předchůdce). Musíme správně definovat ukončující podmínku, abychom nepřišli o optimalitu. Výslednou cestu sestavíme z částečných. Pokud prohledávání mezi sebou nemusí komunikovat, lze paralelně (ne prokládat sekvenčně).

```pseudocode
F = init forward search
R = init reversed search
while F and R has non-empty open-list
	do search F step
	if F and R met // ukončovací podmínka
		return path
	do search R step
	if F and R met // ukončovací podmínka
		return path
return no-path
```

**Obousměrný Dijkstra** - Nestačí protnutí closed-listů. Jakmile se setkají, začneme do $\mu$ ukládat délku nejlepší nalezené cesty. Ukončovací kritérium je pak $top_f + top_r \geq \mu$ (součet délek cest do vrcholů na vršcích open-listů).

**Bidirectional A\* (BA\*)** - Použijeme pozměněné heuristické funkce (z původních feasible heuristik) $p_f(x) = \frac{1}{2}(h_f(x) - h_r(x)) + \frac{h_r(target)}{2}$ a $p_r(x) = \frac{1}{2}(h_r(x) - h_f(x)) + \frac{h_f(start)}{2}$, aby byla suma $p_f(x) + p_r(x)$ konstantní. Ukončovací podmínka je pak $top_f + top_r \geq \mu + p_r(target)$.
**Pozorování 1**: Ze zadaného ohodnoceného grafu $G$ vytvořme modifikovaný $G'$ tak, že v něm použijeme (redukovanou) cenu hran zadefinovanou $cost'(a \rightarrow b) = cost(a \rightarrow b) - h(a) + h(b)$ (upravíme ceny hran v $G$). Pokud je $h(x)$ přípustná a konzistentní, pak je *A\* nad $G$ roven Dijkstrovu algoritmu nad $G'$* (změnou cen ho donutíme chovat se jako A\*).
**Pozorování 2**: Pro daný ohodnocený graf $G$ a graf $G'$ s redukovanou cenou hran platí $cost'(x_1 \rightarrow x_n) = cost(x_1 \rightarrow x_n) - h(x_1) + h(x_n)$. (Počítáme s cenami celých cest, nejen hran.)
**Pozorování 3**: Pro obousměrné prohledávání potřebujeme dvě heuristické funkce $h_f(x)$, $h_r(x)$. Redukované ceny ekvivalentních Dijkstrových prohledávání jsou $cost_f(x_1 \rightarrow x_2) = cost(x_1 \rightarrow x_2) - h_f(x_1) + h_f(x_2)$ a $cost_r(x_1 \rightarrow x_2) = cost(x_1 \rightarrow x_2) - h_r(x_2) + h_r(x_1)$. Tyto hodnoty musí být konzistentní ($cost_f(x_1 \rightarrow x_2) = cost_r(x_1 \rightarrow x_2)$), z toho plyne $h_f(x_1) + h_r(x_1) = h_f(x_2) + h_r(x_2)$. Jelikož to musí platit pro každou hranu $(x_1 \rightarrow x_2)$, pak musí být $h_f(x) + h_r(x)$ konstantní.
**Pozorování 4**: Dvě libovolné konzistentní heuristické funkce $h_f(x)$ a $h_r(x)$ neprodukují konzistentní redukované ceny hran grafu. Definujeme tedy pro prohledávání následující heuristiky: $p_f(x) = \frac{1}{2}(h_f(x) - h_r(x))$ a $p_r(x) = \frac{1}{2}(h_r(x) - h_f(x)) = -p_f(x)$. Ty už by stačily, ale pro jednodušší důkaz optimality použijeme: $p_f(x) = \frac{1}{2}(h_f(x) - h_r(x)) + \frac{h_r(target)}{2}$ a $p_r(x) = \frac{1}{2}(h_r(x) - h_f(x)) + \frac{h_f(start)}{2}$.
**Věta**: Máme A\* prohledávání s přípustnou a konzistentní heuristickou funkcí $h(x)$. Obousměrné A\* prohledávání používající $p_f(x)$ a $p_r(x)$ jako heuristické funkce a ukončovací kritérium $top_f + top_r \geq \mu + p_r(target)$ vždy najde optimální cestu.
**Nevýhody**: Vyhodnocení $p_f(x)$ a $p_r(x)$ může být drahé. Ukončovací kritérium $top_f + top_r \geq \mu + p_r(target)$ čeká na prozkoumání až $p_r(target)$ délky vrcholů.

**New Bidirectional A\* (NBA\*)** - Řeší oba nedostatky BA\*, je stále optimální. Prohledávání sdílí hodnoty délek cest do doposud expandovaných vrcholů a sdílejí množiny zatím neuzavřených vrcholů. Můžeme tak mít optimálnější ukončovací podmínku s původními heuristickými funkcemi (popsáno více v následující variantě PNBA\*).

**Parallel NBA\* (PNBA\*)** - Umožňuje paralelní spouštění obou prohledávání jen s velmi malou kritickou sekcí. Zpětné prohledávání běží na grafu $G'$ s otočenými hranami. Prohledávání sdílejí pool vrcholů $M$, odebírají z něj během expanze (na začátku jsou tam všechny). Také sdílejí nejlepší doposud nalezené řešení $L$ (na začátku $\infty$). Vidí si navzájem nejnižší hodnotu v open-listu $F_p$ ($F_1$ pro dopředné, $F_2$​ pro zpětné, na začátku hodnota heuristiky pro počáteční vrchol).
**Prořezávací podmínka**: Expandujeme vrchol, pouze pokud je aktuálně odhadnutá cena cesty menší než nejlepší známá a hypotetická nejlepší cesta vedoucí skrz tento vrchol, kterou ještě můžeme najít, může být lepší než aktuálně nejlepší známé řešení.
**Ukončovací podmínka**: Jednoduše dokud jsou nějaké vrcholy v open-listu.

*Algoritmus pro dopředné prohledávání* (pro zpětné jen prohozené dolní indexy):

```pseudocode
while not finished:
	x := open-list_1.pop() // horní vrchol z open listu, selekce dle A* strategie
	if x in M: // neexpandovat zbytečně vrchol, který už byl expandovaný druhým prohledáváním
		if f_1(x) < L and g_1(x) + (F_2 - h_2(x)) < L: // prořezávací/pruning podmínka
			for all edges (x, y): // téměř standardní A* expanze
				if y in M // ignorujeme již expandované sousedy
				   and g_1(y) > g_1(x) + c_1(x, y): // a našli jsme lepší řešení
					g_1(y) := g_1(x) + c_1(x, y) // standardní A* update souseda y
					f_1(y) := g_1(y) + h_1(y) // standardní A* update souseda y
					open-list_1.insert_or_update(y)
					if g_1(y) + g_2(y) < L // lepší cesta
						lock // test-lock-test mechanismus pro kritickou sekci, výkonnější
							if g_1(y) + g_2(y) < L
								L := g_1(y) + g_2(y) // update sdíleného nejlepšího řešení
		M.remove(x) // vrchol x prozkoumán a expandován/zahozen, už nepotřebujeme zpracovat
	if open-list_1 not empty // update odhadované ceny cesty vedoucí skrz horní prvek open-listu
		F_1 := f_1(top of open-list_1)
	else
		finished := true // žádné další vrcholy
```

#### (Hierarchické prohledávání)

**Partial-Refinement A\* (PRA\*)** - Vytvoří se hierarchie abstrakcí prostředí (kontrakce klik a sirotků v grafu), pak se hledá cesta (pomocí A\*) v nejvyšší úrovní a postupně níž (tam se využije goal bounding kontrola). Důležité jsou vhodné datové struktury pro korespondence mezi vrstvami. Můžeme prokládat plánování a provádění pohybu. Je to rychlé, dokáže brát v úvahu terén, snižuje first move delay. Předvýpočet může být náročný.

#### LPA\*

Algoritmy pro dynamické prohledávání, když prostředí neznáme předem, nebo je neúplné nebo příliš abstraktní. Prozkoumáváme ve směru cíle, při nárazu do překážky hledáme novou cestu.

**Dynamic A\* (D\*)** - Neznámé prostředí, nelze garantovat nejkratší cestu (ale lze garantovat nejkratší ze všech známých). Nejprve optimistická mapa prostředí, pak se skenuje a přepočítává.

**Focussed Dynamic A\* (FD\*)** - Rychlejší varianta D\*, zaměřuje se pomocí heuristiky, expanduje méně uzlů.

**Lifelong Planning A\* (LPA\*)** - Inkrementální verze A\*, nezačíná pokaždé znovu, nepřepočítává všechny dříve expandované vrcholy. Heuristická funkce musí být nezáporná a konzistentní. Kromě $g(x)$ (odhad nejlepší cesty do vrcholu) má každý vrchol lookahead hodnotu $rhs(x) = \{\min_{x' \in Pred(x)} (g(x') + c(x', x))\}$ (na základě předchůdců) a dvousložkový klíč $k(x) = \begin{bmatrix} k_1(x) \\ k_2(x) \end{bmatrix} = \begin{bmatrix} \min(g(x), rhs(x)) + h(x, t) \\ \min(g(x), rhs(x)) \end{bmatrix}$ (podle něj se řadí v open-listu, $t$ je cíl). Cestu najdeme sledováním nejprudšího gradientu z cíle $t$ (přes předchůdce $x'$ s nejmenší hodnotou $g(x') + c(x', x)$).
Jakmile pozorujeme změny cen hran, updatujeme a přepočítáme cestu (snažíme se udělat vrcholy lokálně konzistentní, tj. $g(x) = rhs(x)$). Změna $rhs(x)$ může změnit pozici koncového vrcholu hrany v open-listu, nebo ho dokonce přesunout z closed-listu. Další iterace počítá s $g(x)$ a $rhs(x)$ z předchozího běhu.

```pseudocode
procedure Main():
	Initialize(); // g(s)=0; rhs(s)=0; vše ostatní nekonečno; s se vloží do open listu (klíč (0;0))
	forever: // hlavní smyčka robota, nikdy nekončí
		ComputeShortestPath(); // zpracováváme open-list, počítáme g(x), rhs(x), dokud není cesta nalezena
							   // pak zrekonstruujeme od cíle zpět
		Wait for changes in edge costs; // robot se pohybuje, skenuje prostředí, čeká se na nová data ze senzorů
		// máme nová data - změna v grafu
		for all directed edges (u, v) with changed edge costs:
			Update the edge cost c(u, v); // updatujeme cenu hran
			UpdateVertex(v); // první kolo updatů vrcholů - přizpůsobení open-listu
							 // rhs(x) se dívá zpět, proto updatujeme v, potenciálně se vloží zpět do open-listu
```

Takto pak vypadají pomocné metody:

```pseudocode
procedure ComputeShortestPath():
	while (U.TopKey() < CalculateKey(s_goal) || rhs(s_goal) != g(s_goal)):
		u = U.Pop(); // pop z vršku open-listu U
		if (g(u) > rhs(u)): // našli jsme odhad g vyšší než rhs, našla se lepší cesta do u
			g(u) = rhs(u); // updatujeme odhad, to znevaliduje všechny následníky u
			for all s in Succ(u): UpdateVertex(s); // donutíme je přehodnotit jejich rhs
		else: // hlavně pro případ g(u) < rhs(u), tedy že se cesta do u zhoršila
			g(u) = infinity; // změna v grafu zkazila předchozí nejlepší cestu, restartujeme výpočet g
			for all s in (Succ(u) UNION {u}): UpdateVertex(s); // updatujeme u i následníky
			
procedure UpdateVertex(u): // znamená update rhs hodnoty a odebrání/update v open-listu
	if (u != s_start): rhs(u) = min_{s2 in Pred(u)} (g(s2) + c(s2, u));
	if (u in U): U.Remove(u);
	if (g(u) != rhs(u)): U.Insert(u, CalculateKey(u));
```

**D\* Lite** - Založený na LPA\*, ale prohledává z cíle směrem k počátku, udržuje tak v $g$ vzdálenosti do cíle. Pro rekonstrukci cesty se tak pohybuje směrem k cíli, lze explicitně zabudovat akci pro pohyb.

#### MPAA\*

**Repeated A\* (RA\*)** - Najdeme počáteční cestu. Pokud se graf změní, restartujeme celý výpočet (opakovaný A\*, nic se neukládá mezi běhy).

**Adaptive A\* (AA\*)** - Vychází z RA\*, po dokončená běhu A\* pro vrcholy v closed-listu updatuje $h(x)$ skutečnými hodnotami, tj. $h(x) = g(t) + h(t) - g(x)$ (kde $t$ je cíl), tím upřesní heuristiku (a pořád zůstává feasible).

**Multipath Adaptive A\* (MPAA\*)** - Rychlejší než D\* Lite (jen s výjimkou bludišť). Zachovává stav open-listu a closed-listu mezi běhy. Jako AA\* updatuje heuristickou funkci. Při A\* prohledávání ukládá také mapu rodičů (předchůdce na nejlepší cestě z počátku do vrcholu), při výpočtu cesty pak staví mapu následníků (další vrchol na nejlepší cestě do cíle), tu využije v dalších iteracích ke skákání směrem k cíli podél starých cest (v rámci cílové podmínky). Sledují se, dokud jsou $h$ ve stejných krocích jako ceny hran (tj. odhad je pořád přesný, cena cesty se nezměnila).

```pseudocode
function GoalCondition(node):
	while next(n) != null and h(n) == h(next(n)) + c(n, next(n)):
		n := next(n)
	return node == target
	
procedure BuildPath(node): // rekonstruujeme nejlepší nalezenou cestu
	while node != start:
		next(parent(node)) := node // počítáme informaci o rodičích, ukládá se spolu s f, g, h
		node := parent(node)
		
procedure Main():
	observe, initialize f, g, h, initialize next
	while start != target:
		node := A*(start)
		foreach s in closed-list: // předpokládáme feasible heuristiku, upřesníme ji
			h(s) := g(target) + h(target) - g(s)
		BuildPath(node)
		while graph not changed along path[start, node]
			start := path.remove_first()
			move agent to start
```

#### RRT a RRT\*

**Rapidly-exploring random tree (RRT)** - Hledání cesty bez abstrakce prostředí, jen s raycasty. Náhodně vrhá body (v rámci hranic prostředí), najde nejbližší existující bod ve stromě, udělá krok směrem k náhodnému bodu, tak postupně staví strom. Pokud je na cestě překážka, buď zkusí nový bod, nebo může přidat alespoň kus cesty k překážce. Opakuje, dokud není dostatečně blízko cíli. Výsledná cesta není optimální.

**RRT\*** - Vylepšení RRT, vylepšuje a vyhlazuje cestu. Nový bod připojí k existujícímu bodu v okolí s nejkratší cestou od počátku, pak přepojí hrany kolem, pokud tím může vylepšit cenu cesty. Může dávat lepší cesty než RRT, s dostatečným množstvím času by našel nejkratší cestu k cíli.

```pseudocode
G.init(root: Point)
while path to epsilon-area around target not found // dokud nejsme blízko cíle
	point := random // zvolíme náhodný bod v rámci hranic prostředí
	nearest := G.nearest_vertex(point) // nejbližší existující bod ve stromě
	new_vertex = nearest + (point - nearest).normalized * step // krok daným směrem
	if no obstacle nearest->new_vertex // do ničeho jsme nenarazili
		min_cost_vertex = find vertex // bod kolem s minimální cenou cesty od počátku
			| from G.vertices_around(new_vertex)
			| with min path cost from root
			| and no obstacle on vertex->new_vertex
		G.add_edge(min_cost_vertex, new_vertex) // nový vrchol a nová hrana ve stromě
		for vertex in G.vertices_around(new_vertex) // přepojíme hrany kolem, pokud to dá lepší cestu
			if obstacle in vertex->new_vertex
				continue
			if path_cost(root, vertex) > path_cost(root, new_vertex) + |vertex, new_vertex|
				G.remove_edge(parent(vertex), vertex)
				G.add_edge(new_vertex, vertex)
```

### Reprezentace prostoru (spatial awareness)

**Spatial awareness** - Schopnost porozumět, zpracovat a zapamatovat si prostorové vztahy mezi objekty, prostorem nebo námi samými. Např. Jaké předměty jsou viditelné? Jak se dostat k předmětu? Jaké bezpečné cesty vedou do základny soupeře?

Používáme abstrakce pro reprezentaci prostoru, algoritmy pro hledání/sledování cest, anotace objektů, procedurální forward model (jak se objekty chovají), koordinaci s ostatními.

#### Geometrie

**Height map** - Pro velká venkovní prostředí, lze snadno převést na trojúhelníky.
**Constructive Solid Geometry** (CSG)- Primitivní tvary, množinové operace a transformace.
**Pole trojúhelníků** - Seznam vrcholů a indexů. Nemusí být efektivní (např. nalezení nejbližšího).

**Binary space partitioning tree** (BSP tree) - Ukládání prostorových dat (jako trojúhelníků). Rekurzivně dělíme prostor nadrovinami (např. axis-aligned), dokud neplatí nějaká ukončovací podmínka (např. maximálně jeden bod v oblasti). Vznikne strom.

**Raycasting** - Paprsek skrz oblasti (lze z BSP stromu), kontrola kolize s trojúhelníky. Jestli z určitého bodu vidíme na nějaký jiný nebo je daným směrem překážka. Potřebuje informaci o statické geometrii prostředí. Nijak nepomůže s hledáním cesty.
**Spherecasting** - Místo přímky uvažujeme objem.

Pro hledání cesty potřebujeme **abstrakci prostředí** s další informací (propojení, kde se dá pohybovat, jakými akcemi).

**Tile-based** - Obdélníkové buňky. Cestu lze vyhladit (např. funnel algoritmem), nebo to nechat na steeringu.
*Regular tiles* - Pravidelná mřížka, metainformace (např. kde se nedá chodit), pak jako navgraph pro hledání cesty, místo středů můžeme vzít středy hran.
*Quadtree tiles* - Oblast se zdí/překážkou podrozdělíme, rekurzivně až do prázdné oblasti nebo nejmenší buňky. Metainformace (jestli se tam dá chodit).
*Framed Quadtree* - Hranice buněk rozdělíme na velikosti nejmenší buňky (rám vrcholů). Výsledná cesta je blízko ideální, není třeba vyhlazení cesty, ale hodně hran pro prohledávání.

**Weighted graph** - Ohodnocený graf dle ceny pohybu v daném terénu. Cena ve vrcholech (jednodušší), nebo hranách (může se lišit dle směru).  V tile-based abstrakci lze snadno.

**Navigation graph** (*navgraph*) - Graf navigačních bodů, na kterých může postava stát. Umístěné ručně, nebo automatizovaně (extrahovat kontury, triangulace, duální graf). Hrany jsou orientované a znamenají, že můžeme přejít. Lze přidat informace navíc (např. že je tam předmět; že se musí skočit). Libovolné grafové algoritmy, rychlá abstrakce, ale řídká a ztrácí se hodně sémantické informace (zdi, díry, viditelnost). Hraně nebo vrcholu můžeme přidat vzdálenost, do které se dá vzdálit.

**Navigation mesh** - Prostředí složené z konvexních polygonů, pokrývají oblasti, kde mohou postavy stát bez kolizí, sdílejí hrany (tím vzniká graf, lze použít libovolný grafový algoritmus (centroidy polygonu nebo středy hran), pak Funnel algoritmus). Lze spočítat z geometrie (např. Recast algoritmus). Přirozenější pohyb, ale pomalejší pathfinding než s navgraph. Méně primitiv než celá geometrie, levnější raycasty.
*Off-mesh links* - Spojují polygony, které nesdílející hranu, ale dá se mezi nimi navigovat. Často nesou nějakou sémantickou informaci navíc (jak spojení přejít).

**Potential fields** - Reprezentace prostředí pomocí funkce potenciálu, mapuje prostor na reálná čísla (jak snadné/těžké je cestovat v určitém bodě prostoru). Překážky nás odpuzují, cíl nás naopak přitahuje. Cestujeme podél gradientu funkce (samplujeme hodnoty v mřížce, uložíme do flow field, lze předpočítat, ušetříme raycasty, užitečné pro spousty agentů). Lokální technika (steering, generování sil), může se zaseknout v lokálním optimu.

#### Viditelnost

**Raycast** - Dokáže zodpovědět jeden dotaz viditelnosti, ale moc pomalé, když jich chceme víc.

**Visibility matrix** - Boolean matice, předpočítaná viditelnost (pomocí raycastů) mezi zajímavými body. Najdeme nejbližší samplované body pomocí BSP, podíváme se do matice. Můžeme snadno provádět komplikovanější dotazy (binární operace nad řádky matice).

## 4. Komunikace a znalosti v multiagentních systémech

**Multiagentní systém** - Agenti v prostředí, komunikují (posíláním zpráv přes síť).
**Komunikace a spolupráce**: společné slovníky (znalosti, ontologie), standardní zprávy, nad nimi komunikační protokol (pro předvídání komunikace).

### Ontologie

**Ontologie** - Explicitní a formální popis části reality, pro ukládání, odvozování a výměnu znalostí. Definice konceptů + vztahy mezi nimi. Popisují, co jsou třídy, instance, properties (has-a) a relations (is-a).
Dictionary (seznam slov), glossary (významy), thesaurus (vztahy), informal hierarchies (podtřídy, např. Wiki), formal is-a hierarchy, classes with properties, value restrictions, arbitrary logical contraints.
Application ontologies, Domain ontologies, Upper ontologies (popis všeho).

**Báze znalostí** - Strukturální část (ontologie) a fakta o konkrétních věcech.

**Frames** - Frame (třída), slot (property), metoda (přístup k datům).
**XML** - Značky jsou pojmy slovníku (lze definovat vlastní), obsahy jsou objekty, lze využít atributy značek.
**RDF** (Resource Definition Framework) - Následník XML (obvykle psáno pomocí XML syntaxe). Trojice subject-predicate-object, např. FatherOf(Karel, Petr). Predikáty definované uživatelem.
**KIF** (Knowledge Interchange Format) - Reprezentace znalostí v logice prvního řádu.
**DAML+OIL** (*DARPA Agent Markup Language* + *Ontology Interchange Language*) - Předchůdce OWL.

**Deskripční logika** - Rozhodnutelná část logiky prvního řádu. K definici dalších tříd nebo pokládání dotazů. Individua (konstanty, objekty), koncepty (unární predikát, třída), role (binární predikát, vztahy, vlastnosti). Např. John, programmer(John), teacher(John, Mary). Axiomy, ABox pro obecné vlastnosti (koncepty a role mezi nimi), TBox pro jedince (jejich třídy, vztahy). Celá rodina logických systémů (dle pravidel/axiomů).
**AL** (Attributive language) - konstanty, negace pro atomy (konstanty), negace pro koncepty (jen na pravé straně u existujících), průnik konceptů $\sqcap$ (konjunkce), kvantifikátory (s omezeními).
**U** (sjednocení konceptů), **E** (plný existenční kvantifikátor), **C** (neomezená negace konceptů), **H** (hierarchie rolí, is-a), **N** (numerická omezení predikátů)

**Modální logika** - Logika prvního prvního řádu, necessity $\square P$ (nutně pravdivé), possibility $\Diamond P$​ (může být pravdivé), navíc axiomy.
**Kripkeho sémantika** - Více modelů, tzv. possible worlds (různé přiřazení hodnot atomickým formulím, různé realizace funkcí a relací). Pro possibility najít jeden svět, ve kterém platí, pro necessity prozkoumat všechny.
**Terporální logika** - Speciální případ, možné světy jsou snapshoty v různých časech. Pro plánování.

**OWL** (Web Ontology Language) - Syntax v XML nebo RDF. Formální, ale praktický přístup. Několik vrstev.
**OWL 1** (verze 1) - Vrstvy *OWL-Lite* (skoro RDF, mnoho omezení axiomů), *OWL-DL* (deskripční logika, SHOIN (S = ALC, H = hierarchie rolí, O = enumerace tříd, I = inverzní vlastnosti, N = numerická omezení)), *OWL-Full* (kompletní logika prvního řádu)
**OWL 2** (verze 2, víc prakticky orientovaná) - Vrstvy *OWL-EL* (část logiky, polynomiální rozhodování), *OWL-QL* (skoro SQL, query language), *OWL-RL* (speciální forma axiomů, pravidla)

**Protégé** (editor pro návrh ontologií) + **SPARQL** (jazyk pro pokládání dotazů nad knowledge base).

### Řečové akty

Agenti musí komunikovat (k výměně informací, ovlivnění ostatních).

**Speech acts** - Jako standardní zprávy, mají povahu akcí (mají jasný efekt, mění stav světa). Mají performative verb (request, inform, promise, query), definuje druh speech actu, a propositional content (obsah zprávy).
Tři kategorie (v lidské komunikaci): *locutionary act* (co bylo skutečně řečeno), *illocutionary acts* (co tím bylo myšleno), *perlocutionary acts* (co se skutečně stalo, efekt).

**Speaker** (vyjadřuje druh speech actu, přidává obsah), **hearer** (posluchač). Oba si musí rozumět, musí platit preconditions (aby to mluvčí řekl, věří tomu, že to posluchač chce/dokáže, ale není jasné, jestli by to udělal jen tak sám), platí honesty (pokud něco požadujeme, opravdu to chceme).

**Kategorie speech acts**: *Assertives* (informace), *directives* (požadavek), *comissives* (slib), *expressives* (vyjádření mentálního stavu, emoce, např. poděkování), *declarations* (přímo změna stavu).

**Planning theory of speech acts** - Plánovací systém nad speech acts, aplikuje se STRIPS (sémantika pomocí operátorů, tj. precondition-delete-add). Řeč se pak dá plánovat a promýšlet.
**Definice speech actu** - Preconditions Cando a Want (co musí platit, abychom to dokázali/chtěli udělat). V definici se používá believe, want, cando.

Př. **Inform(S,H,F)** - speaker, hearer, informace.
Preconditions:
	Cando: (S believe F), tj. $S$ tomu musí sám věřit, aby byl schopen to udělat
	Want: (S believe (H want F)), tj. $S$ věří tomu, že $H$ to chce
Effect: (H believe (S believe F)), tj. $H$ věří, že $S$ věří $F$


### Protokoly, FIPA-ACL

Máme předdefinované speech acty, sémantiku (preconditions, efekty), ontologie pro obsah zprávy (akce/informace). Vše spojíme.

**Komunikační protokoly** - Určují komunikaci agentů. Co, kdy, komu můžou poslat, jaký je obsah, kódování. Specifikuje, jaký perfomative může být použit kdy. FIPA (Foundation for Intelligent Physical Agents) vydává standardy (protokoly i jazyky). Např. Contract net, Blackboard system.

**Zpráva** - Hlavička (sender, receiver, reply-to, metadata o obsahu (language, encoding, ontology (odkaz na ontologii)), performative, id konverzace) a obsah (ve vnitřním jazyce, např. KIF, Prolog, dle ontologie).

**KQML** (Knowledge Query and Manipulation Language) - Jazyk pro vnější komunikaci (obálku). Specifikuje např. performative (ask-one, ask-all, ...), content, receiver, language (např. Prolog), ontology.
**KIF** (Knowledge Interchange Format) - Vnitřní jazyk, pro propositional content zprávy. Formalismus logiky prvního řádu pro reprezentaci znalostí, dotazování (vztahy, kvantifikátory).

**Agent Communication Languages (ACL)** - Z planning theory na speech acts. Pro hlavičku zprávy. Základní performatives např. inform, query, request, subscribe, propose, refuse, confirm, agree, cancel, failure a další.

**FIPA-ACL** - Zjednodušení KQML, praktická implementace v JADE (Java Agent DEvelopment Framework). Specifikuje strukturu zpráv (pole, např. ontology, language, encoding, performative).
**FIPA-SL** - Jazyk pro obsah zprávy, syntaxe výrazů, které mohou agenti odeslat (logické spojky, kvantifikátory, referenční operátory). Jsou i omezenější, SL0 (bez logických spojek), SL1 (výroková logika), SL2 (predikátová a modální logika, omezená na rozhodnutelnou).

**Příklady komunikačních protokolů FIPA**:
*Request protokol* - Výzva k nějaké akci (REQUEST, REFUSE/AGREE, FAILURE/INFORM-DONE/INFOMR_RESULT).
*Query protokol* - Žádost o odpověď na dotaz.
*Subscribe protokol* - Přihlášení odběru ke zprávám, když se něco stane (účastník souhlasí/odmítá).
*Protokol contract net* - CFP (s akcí pro provedení), PROPOSE/REFUSE, ACCEPT_PROPOSAL/REJECT_PROPOSAL.
Brokering protokol - Žádost, aby jiný agent našel agenty pro provedení akce.
Protokoly pro různé aukce (anglické, holandské).

## 5. Distribuované řešení problémů

Agenti potřebují sdílet informace a úkoly, na to jsou protokoly.

### Kooperace

Agenti potřebují sdílet informace a úkoly, domlouvat se o cílech a informacích, synchronizovat se.

**PPS** (Parallel Problem Solving) - Paralelní řešení, procesory jsou jednoduché a homogenní.
**MAS** - Agenti jsou autonomní, nesdílí společný cíl, řešíme konflikty, smlouvání.

**CDPS** (Cooperative Distributed Problem Solving) - Žádné soutěžení, žádná autonomie (agenti nemohou odmítnout), benevolence (žádné konflikty, společné cíle jsou důležitější než vlastní), jen distribuce/sdílení úkolu. Tak je snazší definovat protokoly, pokud agenti udělají vše, co řekneme.
*Problem decomposition* (problém hierarchicky rekurzivně dělíme), *sub-problem solution* (přiřazujeme agenty k podproblémům, řeší je, sdílí informace, je třeba synchronizovat akce), *solution synthesis* (složení řešení podproblémů na řešení celého problému).

**Task sharing** - Agent kontroluje, jestli dokáže vyřešit úkol, pokud ano, vyřeší ho.
**Result sharing** - Sdílení výsledků, reaktivní (čeká se na všechny) nebo proaktivní (aktivně se ptá agentů na výsledek). Řešíme confidence (můžeme říct více nezávislým agentům a porovnat), completeness (agent má lokální pohled na problém, potřebujeme jich víc), precision (sdílení může zlepšit přesnost, ensemble), timely manner (rdistribuovaný přístup redukuje čas potřebný k vyřešení).

**Contract net (CNET)** - CFP od FIPA-ACL, řeší task sharing smlouváním.
*Recognition* (agent zodpovědný za úkol rozpozná, že potřebuje pomoc), *announcement* (broadcast nabídky smlouvy, popis problému, omezení), *bidding* (ostatní vytvoří nabídku s parametry), *awarding/expediting* (zvolíme vítěze, odměníme ho smlouvou, lze přidat aukce).
FIPA-ACL: cfp, Refuse/Proposal/Not-understood, Accept-proposal/Reject-proposal, Inform-done/Inform-ref/Failure.

**Blackboard system (BBS)** - Task sharing, result sharing, klidně i veškerá komunikace (zprávy). Blackboard je sdílená struktura, jsou na ní oznámení úkolů (hierarchie tasků) a sdílí se tam výsledky. Asynchronní. Agenti (experti) sledují tabuli, pokud umí vyřešit task, vyřeší ho a zapíšou řešení. Arbiter (organizátor) řídí přístup k tabuli. Ontologický popis tasků i schopností agentů. Jednoduché, ale agenti musí často sdílet stejnou architekturu a tabule je bottleneck (lze vytvořit hierarchické úrovně).

### Interakce agentů

**Interakce agentů**: Agenti si mohou pomáhat, ale také překážet, vytvářet společenství nebo nepřátele.
**Nejjednodušší případ**: Dva sobečtí (racionální) agenti, kteří soutěží mezi sebou. Maximalizují svůj očekávaný užitek (konflikt mezi společným a vlastním cílem).

**Rozhodovací proces** - Hra 2 agentů $i$ a $j$. V tazích provádějí akce, tím mění prostředí a to na základě toho řekne výsledek (outcome), $e: A_i \times A_j \rightarrow O$, kde $O = \{ o_1, o_2, ... \}$ je množina možných outcomů (utility/payoff). Obvykle reprezentujeme tabulkou.

**Strategie** - Předepisuje agentovi, jak volit akci. *Pure* (deterministická, jen jedná akce) nebo *mixed* (pravděpodobnostní rozdělení).
**Hra v normální formě** - Každý hráč zvolí svou strategii, pak hrají všichni zároveň. Výsledek závisí na zvolených akcích. Můžeme reprezentovat pomocí reálné matice (řádky/sloupce odpovídají akcím, hodnoty jsou payoff/utility).

**Teorie her** - Jakou strategii by měl agent zvolit na základě definice hry (tabulky). Obvykle bereme *worst case scenario* (oponent je racionální). *Best response* je strategie maximalizující utility hráče pro (zafixované) strategie protivníků.

**Dominantní strategie** - Poskytuje stejný nebo lepší výsledek než libovolná jiná strategie proti všem strategiím oponenta (je bezpečná proti všem možným akcím oponenta).

**Social welfare** (sociální blahobyt) - Celkový výkon systému, součet užitků všech agentů, $\sum O_i$.
**Social fairness** (sociální spravedlnost) - Násobek užitků jednotlivých agentů. Je tak maximalizována, pokud mají všichni agenti stejné (nebo podobné) užitky.

**Tragedy of the commons** - Neregulované, nevlastněné společné zdroje využijí racionální agenti až do zničení (nemohou sami dojít k řešení lepšímu pro všechny). Př. společné kusy země s trávou jako pastviny, rybaření v oceánu, znečištění ovzduší. Je třeba stanovit pravidla, mohou vznikat komunity (starají se o zdroje, spolupráce ve využívání), může se zavést daň (např. aukcí)..

#### Nashova ekvilibria

**Equilibrium** - Stabilní stav systému, žádný z účastníků nic nezíská, pokud změní jen svou strategii a vše ostatní zůstáne stejné.

**Nash equilibrium (NEQ)** - Dvojice strategií $s_1$ a $s_2$, které jsou ty nejlepší proti sobě navzájem (best response). Jiná strategie by snížila outcome.
**Nash theorem** - Každá konečná normal-form game (s konečným počtem hráčů, konečným počtem akcí pro každého hráče, konečným počtem strategií) má alespoň jedno Nashovo equilibrium v mixed strategies.

#### Paretova efektivita

**Multi-objective optimization** - Hledáme řešení optimální pro všechny agenty společně. Obvykle je jich více.

**Pareto dominantní** - Výsledek hry je Pareto dominován jiným, pokud by všichni preferovali ten jiný. (Liší se od Nash dominance.)
**Pareto front** - Řešení optimální pro všechny (ostatní jsou dominována některým z Pareto front).
**Pareto optimální/efektivní strategie** - Strategie je Pareto optimální, pokud neexistuje žádná jiná strategie, která by zlepšila výsledek agenta, aniž by zhoršila výsledek druhého (tzn. neexistuje žádný jiný výsledek, který by všichni preferovali).

#### (Příklady her)

Pomocí specifikace prostředí hry lze podporovat koordinaci nebo soutěž.

**Vězňovo dilema** - Dva vězni mohou spolupracovat C, nebo se zradit D. Velké nutkání zradit druhého. NEQ je DD. Pareto front je CC, DC, CD. Social welfare je CC. Dilema, protože NEQ s dominantními strategiemi DD Pareto dominuje CC (oba si mohou polepšit, ale racionálně toho nedosáhnout).

| i/j           | Cooperate | Defect |
| ------------- | --------- | ------ |
| **Cooperate** | (3, 3)    | (0, 5) |
| **Defect**    | (5, 0)    | (1, 1) |

**Battle of sexes** (Coordination game) - NEQ v pure strategies, Pareto optimal i Social welfare je BB a FF.

| M/F        | Ballet | Fight  |
| ---------- | ------ | ------ |
| **Ballet** | (1, 2) | (0, 0) |
| **Fight**  | (0, 0) | (2, 1) |

**Kámen, nůžky, papír** - NEQ v pure strategies neexistuje, jen v mixed.

|              | Rock    | Paper   | Scissors |
| ------------ | ------- | ------- | -------- |
| **Rock**     | (0, 0)  | (-1, 1) | (1, -1)  |
| **Paper**    | (1, -1) | (0, 0)  | (-1, 1)  |
| **Scissors** | (-1, 1) | (1, -1) | (0, 0)   |

**Game of chicken** (Anti-coordination game) - NEQ v pure strategie je CD a DC, Social welfare je CC.

|             | Chicken | Dare   |
| ----------- | ------- | ------ |
| **Chicken** | (5, 5)  | (1, 6) |
| **Dare**    | (6, 1)  | (0, 0) |

**Iterované vězňovo dilema** - Šance, že budeme hrát znovu, spolupráce je dlouhodobě úspěšnější. Komunikujeme skrz tresty za zrady, např. Tit-for-tat a varianty.

### (Volby)

CFP lze řešit pomocí hlasování, pak není třeba master, vybere se nejlepší kandidát.

**Volby** - Máme $N$ agentů, $M$​ *kandidátů* (možností volby). Agenti mají preference (**ballot**), ty posbíráme, agregujeme, vznikne **social welfare** (celkové preference). Pak zvolíme **social choice** (vítěz voleb) dle *volebního schéma*.
**Preference ballot** - Kompletní preference, tj. kompletní uspořádání kandidátů.

**Majority scheme** - Každý volič uvede jednu volbu, vyhrává kandidát s většinou hlasů (může selhat).
**Plurality scheme** - Jedna volba, vyhrává kandidát s nejvíc hlasy (nemusí být preferovanou volbou většiny).
**Dvoukolový systém** - Plurality s více kandidáty, pak majority se 2 nejlepšími. Senátoři a prezidenti.
**Plurality with elimination** (instant runoff) - Využívají se preference z prvního kola. Plurality, eliminujeme nejhoršího, jeho hlasy redistribuujeme (dáme hlasy druhým volbám). Iterujeme, dokud není většina. Eliminuje taktické hlasování, ale jsou potřeba kompletní preference.

**Condorcet paradox** - Existují situace (s pluralitou), kdy bez ohledu na to, jaký výsledek zvolíme, většina voličů bude nespokojená s výsledkem. Může vést na tactical voting (neupřímné hlasování).
**Condorcet winner** - Porazí všechny protivníky ve všech porovnáních po dvojicích.
**Condorcet method** - Porovná všechny kandidáty po dvojicích pro skutečně férové volby. Je třeba $o(n^2)$ kol, to není praktické.

**Sequential majority elections** - Varianta plurality, kandidáti po dvojicích jako turnaj, vítěz postupuje. Pořadí turnajů ovlivňuje volby, ale vítěz je ten nejlepší.

**Borda count** - Rodina schémat, jsou potřeba úplné preference, kandidáti mají body podle umístění (např. $N-1$ za první místo, $N - N = 0$​ za poslední, nebo exponenciální). Social welfare sečtením bodů, social choice je kandidát s maximem. Pokud existuje Condorcetův vítěz, pak vyhraje.
Různé varianty (poskytují Condorcetova vítěze, pokud existuje): **Black method** (Condorcetův typ plurality, pokud neexistuje Condorcetův vítěz, pak Borda count), **Baldwin method** (iterativní verze Borda count, vyřadíme nejhoršího, přepočítáme, až do většiny), **Nanson method** (iterativní, eliminujeme všechny pod průměrem).

**Slater system** - Z úplných preferenci vytváříme graf, minimalizujeme počet otočených tak, aby byl acyklický. Jen teoretický přístup, NP-úplné.

**Vlastnosti volebních schémat**:
*Pareto property* - Pokud všichni preferují A před B, v social welfare je také A lepší než B. PLatí v majority a Borda, neplatí v sequential majority (záleží na pořadí).
*Condorcet winner* - Pokud existuje, měl by být celkovým vítězem. Platí pro Borda a sequential majority.
*Independence of irrelevant alternatives* (IIA) - Uspořádání dvojice v social welfare závisí pouze relativních uspořádáních dané dvojice v individuálních hlasováních. Neplatí pro majority, sequential majority, ani Borda.
*Universality* - Social welfare závisí na všech hlasech rovnoměrně, výsledek je deterministický.
*Dictatorship* - Jeden z voličů je dictator, jeho hlas je social welfare.
*Non-dictatorship* - Žádný z voličů není diktátor.

**Arrow's theorem** - Pro 3 a více kandidátů neexistuje ideální schéma hlasování, které by splňovalo vlastnosti universality, non-dictatorship, Pareto property a IIA.
**Zjednodušená Arrow's theorem** - Pro volby s víc než 2 kandidáty je dictatorship jediné schéma, které splňuje Pareto property a IIA.

Nelze splnit vše, důležité jsou hlavně Pareto property a IIA (také Condorcet winner).

### Aukce (alokace zdrojů)

**Aukce** - Mechanismus pro férovou alokaci zdrojů mezi agenty. Chceme, aby byli všichni spokojení a zdroj dostal ten, kdo ho nejvíc chce. CFP je druh aukcí, alokuje se práce.
**Auction protocol** - Jak probíhá aukce, jak je vybrán vítěz. Počet kol, způsob sdělení ceny (open-cry, sealed-bid), jaká cena se platí.

**Seller/caller** (prodávající) chce maximalizovat cenu.
**Buyer/bidder** (kupující) chce minimalizovat cenu, nechce jít přes svou utility (zná ji jen on).

**First-price sealed bid** - Obálková metoda, jedno kolo, vyhrává nejvyšší nabídka a přímo ta se platí. Dominantní strategie je nabídnout o trochu míň než utility, ideálně chceme trochu víc než odhadovaná maximální nabídka ostatních. Více soutěživá, nemusí vyhrát ten s nejvyšší utility.
**Vickrey** - Sealed-bid, jedno kolo, vyhrává nejvyšší, platí se druhá nejvyšší. Nutí kupující nabídnout skutečnou utility, to je také dominantní strategie.
**Vickrey-Clarks-Groves** - Obecná verze Vickreyho aukce pro $k$ stejných předmětů, platí se nejvyšší prohrávající nabídka.

**English** - Více kol, open-cry, first-price, postupně se zvyšuje cena (od minimální, o epsilon). Poslední vyhrává, platí nabídnutou cenu. Dominantní strategie je zvyšovat až do utility, pak přestaneme. Odhalí pouze dolní odhad utility. Nepodporuje soutěžení, jeden silný hráč odradí slabší, pak nemá kdo navyšovat cenu.
**Dutch** - Více kol, open-cry, first-price, postupně se snižuje cena. Dominantní strategie je čekat, dokud nebude cena naše utility (nebo trochu míň). Neodhaluje preference ostatních kupujících.
**Quiet** - Verze English, ale na papíře, bez křičení. CFP.
**Amsterdam** - Začneme English aukcí, když už zbývají jen 2 kupující, přejde se na Dutch (třeba s počáteční cenou jako dvojnásobek poslední z English).

**Combinatorial** - Nabídky na víc předmětů současně, chceme optimální volbu podmnožin.

## 6. Metody pro učení agentů

**Unsupervised learning** - Máme data, hledáme strukturu, identifikujeme clustery, vyhodíme reprezentanty.
**Supervised learning** - Učíme se neznámou funkci ze vstupů na výstupy dle training set. Klasifikace, nebo regrese. Např. gradient-based metody, neuronové sítě.
**Reinforcement learning** - Učení z interakcí s prostředím, maximalizace odměny.

### Zpětnovazební učení

**Zpětnovazební učení (RL)** - Agent se nějak chová v prostředí, dostává odměny, na základě nich upraví své chování.

**Markovský rozhodovací proces (MDP)** - Popisuje prostředí a problém, který agent řeší. Je to pětice $(S,A,P,R, \gamma)$, tj. stavy, akce, přechodová funkce (pravděpodobnost $P_a(s, s')$, Markovská podmínka), funkce odměn (okamžitá odměna $R_a(s, s')$) a discount factor.
**Celková odměna** je $\sum_{t = 0}^\infty \gamma^t R_{a_t}(s_t,s_{t+1})$.
**Policy** $\pi: S \times A \rightarrow [0, 1]$, kde $\pi(s, a)$ udává pravděpodobnost provedení akce $a$ ve stavu $s$​ (popisuje chování agenta), je řešením problému.

**Cílem** je najít policy maximalizující celkovou odměnu $E[\sum_{t = 0}^\infty \gamma^t R_{a_t}(s_t,s_{t+1})]$.

**Hodnota stavu** $V^\pi(s) = E[R] = E[\sum_{t=0}^\infty \gamma^t r_{t+1} | s_0 = s]$ (očekávaný užitek stavu, pokud agent následuje policy $\pi$).
**Hodnota akce** $Q^\pi(s,a) = E[\sum_{t=0}^\infty \gamma^t r_{t+1} | s_0 = s, a_0 = a]$ (ve stavu $s$ provede akci $a$, pokračuje dle policy $\pi$).
Platí: $Q^\pi(s_t,a_t) = E_{s_{t+1} \sim P_a} [R_a(s_t, s_{t+1}) + \gamma V^\pi(s_{t+1})]$ a $V^\pi(s) = E_{a \sim \pi(s)} [Q^\pi (s, a)]$.

**Optimální policy** $\pi^*$ maximalizuje očekávanou dlouhodobou odměnu, tj. $V^{\pi^*}(s) = \max_\pi V^\pi(s)$, obdobně $Q^{\pi^*}(s, a) = \max_\pi Q^\pi(s, a)$. Vybírá akci $\pi^*(s) = \arg\max_{a \in A_s} \sum_{s'} P_a(s, s')[R_a(s, s') + \gamma \cdot V(s')]$.
Stačí znát optimální $Q$, pak $\pi^*(s) = \arg\max_a Q(s, a)$.

**Bellmanovy rovnice** - Nutná podmínka optimality $\pi$. Užitky jsou spolu provázané.
$V^\pi(s) = \sum_a \pi(s, a) \sum_{s'} P_a(s, s') [R_a(s, s') + \gamma V^\pi(s')]$
$Q^\pi(s, a) = \sum_{s'} P_a(s, s') [R_a(s, s') + \gamma \sum_{a'} \pi(s', a') Q^\pi(s', a')]$​​
**Bellmanovy rovnice optimality** - Použijí se pro Bellmanův update.
$V^*(s) = \max_{a \in A_s} E[R_a(s, s') + \gamma V^*(s')] = \max_a \sum_{s'} P_a(s, s') \cdot [R_a(s, s') + \gamma V^*(s')]$
$Q^*(s, a) = E_{s' \sim P_a} [R_a(s, s') + \gamma \max_{a'} Q^*(s', a')] = \sum_{s'} P_a(s, s') .[R_a(s, s') + \gamma \max_{a'} Q^*(s', a')]$

**Kompletní informace** - Se znalostí přechodového modelu a funkce odměn jen jednoduchý optimalizační problém Praktické jen pro malé a konečné)
**Value iteration** - Nejprve libovolné užitky stavů, pak iterativní update ze sousedů, dokud se nezafixují, $V_{i+1}(s) \leftarrow \max_{a \in A_s} \sum_{s'} P_a(s, s') \cdot [R_a(s, s') + \gamma V_i(s')]$. Výsledná policy bere nejlepší akci.
**Policy iteration** - Nejprve libovolná strategie, pak střídavě policy evaluation $V_{i+1}(s) = \sum_{a \in A_s} \pi_i(s, a) \sum_{s'} P_a(s, s') [R_a(s, s') + \gamma V_i(s')]$, dokud se neustálí, pak hladový policy improvement $\pi_{i+1}(s) \leftarrow \arg\max_{a \in A_s} \sum_{s'} P_a(s, s') [R_a(s, s') + \gamma V^{\pi_i}(s')]$.

Obvykle přechodový model neznáme, agent se ho může učit. Také se může učit $Q$, pak pro vyhodnocení policy nepotřebuje přechodový model, $\pi(s) = \arg\max_a Q^\pi(s, a)$.

**Pasivní učení** - Agent má pevně danou strategii (podle ní se chová), učí se jen užitky stavů $V^\pi(s)$​. Funkci odměn i přechodovou funkci se učí pozorováním.
**Aktivní učení** - Agent nemá předem danou strategii, kromě užitků stavů se učí i ji (maximalizuje celkový zisk). Musí prozkoumávat prostředí.

----

**Přímý model** - Užitek stavu jako průměr jeho ocenění přes všechny iterace/běhy. Neuvažují se ale závislosti (přes Bellmanovy rovnice), konverguje pomalu.

**Adaptivní dynamické programování** (ADP) - Učí se odměny a přechodovou funkci z pozorování, pak využívá Bellmanovy rovnice, $V^\pi(s) = \sum_{a} \pi(s, a) \sum_{s'} P_a(s, s') [R_a(s, s') + \gamma V^\pi(s')]$. Po každém kroku nastaví odměnu, užitek stavu, přepočítá přechodový model a updatuje všechny užitky (třeba pomocí value iteration).

**Temporal difference** (TD) - Neučí se přechodovou funkci. Po každém kroku upravuje užitek předchozího stavu $V^\pi(s) \leftarrow V^\pi(s) + \alpha [ (R_a(s, s') + \gamma \cdot V^\pi(s')) - V^\pi(s)]$, využívá přímo pozorovaného následníka.

----

**On-policy metody** - Pro pohyb v prostředí se používá ta samá policy, kterou se agent učí.
**Off-policy metody** - Jedna policy pro pohyb (může prozkoumávat), druhá se učí.

**Active ADP** (*greedy agent*) - Provede krok, updatuje přechodový model, funkci odměn, updatuje všechny užitky Bellmanovými rovnicemi $V_{i+1}(s) \leftarrow \max_a \sum_{s'} P_a(s, s') \cdot [R_a(s, s') + \gamma V_i(s')]$ a vybere nejlepší akci. Nenaučí se optimální strategii (první akce vybral zcela náhodně, pak se snaží dostat na již nalezenou cestu).
**Random exploration** - S pravděpodobností $1/t$ náhodná akce (pravděpodobnost klesá s časem), jinak hladově akci maximalizující užitek. Nakonec konverguje k optimální strategii, ale pomalu.
**Epsilon-greedy policy** - Vyvážení explorace a exploitace. S pravděpodobností $(1-\epsilon)$ vybere nejlepší akci a s pravděpodobností $\epsilon$ vybere náhodnou akci. Postupně se snižuje $\epsilon$.
**Explorační funkce** - V updatu použijeme explorační funkci pro balanc mezi hladovostí a zvědavostí. Jako parametry má očekávaný užitek a $N(s, a)$ (tj. počet vyzkoušení dané akce $a$ ve stavu $s$). Nevyzkoušené akce mají větší váhu. Získáme optimistický odhad užitku (zpočátku více táhne do vzdálenějších neprozkoumaných oblastí).

**Active temporal difference** - TD aktivní přístup. Po každém přechodu z $s$ do $s'$ pomocí $a$ provede update, $V_{i+1}(s) \leftarrow V_i(s) + \alpha [ (R_a(s, s') + \gamma \cdot V_i(s')) - V_i(s)]$ (stejně jako pasivní, mění jeden užitek). Má ale stejný problém jako aktivní ADP (akce vybírá dle odhadů, musí i prozkoumávat).

**Monte Carlo metody** - Jako policy iteration, ale pro vyhodnocení strategie se používá Monte Carlo metoda odhadující hodnoty $Q^\pi(s, a)$. Provedeme ve stavu $s$ akci $a$ a potom začneme provádět strategii $\pi$, dokud se nedostaneme do nějakého cíle, odměnu zaznamenáme. Spočítáme pro všechny možné kombinace stavů a akcí, a to několikrát, abychom udělali průměr. Vylepšení strategie pak hladově, $\pi_{i+1}(s) = \arg\max_a Q_i(s, a)$. Může konvergovat pomalu (po jednom běhu update jen jedné dvojice stav-akce). Jen pro epizodické problémy (pokaždé začíná v náhodném stavu).

**Q-learning** - Jako TD, ale místo $V$ se učí přímo funkci $Q$ (v matici), nemusí tak znát model prostředí. V pasivním učení nemá smysl, protože akce je daná strategií. Na začátku samé nuly, pak po každém kroku update $Q(s_t,a_t) \leftarrow Q(s_t,a_t) + \alpha [(R_{a_t}(s_t, s_{t+1}) + \gamma \cdot \max_{a_{t+1}} Q(s_{t+1},a_{t+1})) - Q(s_t, a_t)]$. Jako akci provede tu s největším $Q(s, a)$ (nebo balanc mezi explorací a exploitací pomocí $f(Q(s, a), N(s, a))$). Učí se optimální strategii. Lze jen pro diskrétní prostory a akce (po diskretizaci spojitých může být matice obrovská a pak se učí pomalu nebo vůbec).

**SARSA** (State-action-reward-state-action) - Jako Q-learning, ale 2 kroky zpět, používá skutečně použitou akci. Update $Q(s_t, a_t) \leftarrow Q(s_t, a_t) + \alpha \cdot [R_{a_t}(s_t, s_{t+1}) + \gamma \cdot Q(s_{t+1}, a_{t+1}) - Q(s_t, a_t)]$ až po provedení $a_{t+1}$. Učí se optimální strategii.  Pro hladového agenta totožné s Q-učením, ale při použití explorační funkce odlišné.

----

**MARL** (Multiagent reinforcement learning) - Více agentů, učí se současně. Mohou spolupracovat, nebo soutěžit. Algoritmy často založené na těch pro jednoho agenta, např. Q-learning (můžeme zjednodušit jako $n$​-tici akcí, pak lze skoro přímo, ale výrazně vyšší složitost).

**Self-learning** (self-play) - Agent předpokládá, že ostatní se chovají stejně jako on. Můžeme modelovat ostatní, ale je třeba rozdělit odměnu.

Dva možné přístupy:
**Markov Games approach** - Hledáme optimální strategii pro celý systém (Nashovo equilibrium, Pareto optimalita, social welfare nebo social fairness).
**Deep learning approach** - MADRL, state of art.

**Markov Game** - Zobecnění MDP pro $N$ agentů, každý má svou reward funkci $R_i$ a svůj prostor akcí. Společný prostor akcí je pak součin prostorů jednotlivých agentů. Každý agent má svou policy. Lze formulovat jako hru, reprezentovat tabulkou (odměna pro každého dle kombinace akcí). Value function $V_\pi$ nezávisí jen na policy agenta, ale také na policies ostatních.
**Nashovo equilibrium** - Policies, které jsou nejlepšími odpověďmi na sebe navzájem (žádný agent nemůže zlepšit svůj užitek jednostrannou změnou strategie).
**Pareto optimální** - Všichni agenti se chovají nejlépe (změna strategie jednoho vede na horší celkovou strategii, neexistuje žádná jiná množina strategií, která by dávala větší užitek všem agentům současně).

**Problémy MARL** - Prostředí jsou částečně pozorovatelná. Prostory stavů a akcí mohou být spojité. Agenti updatují policy naráz (prostředí není stacionární z pohledu agenta, odměny pro agenty nejsou nezávislé). Agenti mohou konvergovat k suboptimálnímu řešení, nebo oscilovat mezi více NEQ. Mohou být velké rozdíly v odhadech value funkce.

**Řešení problémů**:
**Value-based** - Základy v teorii her (Nash equilibrium, Pareto optimalita), standardní přístupy (Q-learning).
**Policy-based** - Policy se parametrizuje, hledá se nejlepší sada parametrů pomocí optimalizace (agent modelován pomocí rozhodovací funkce, třeba neuronová síť).

----

**Hluboké zpětnovazební učení** - Využívá neuronové sítě, lze i pro spojité akce a stavy. V policy-based metodách reprezentujeme policy pomocí neuronové sítě (vstupy jsou stavy, výstupy jsou akce, váhy jsou parametry).

**Policy-based learning** - Místo hodnoty $Q(s,a)$ optimalizujeme přímo samotnou policy (mapování stavu na akci). Parametrizujeme ji nějakými parametry $\theta$, tj $\pi(\theta)$, pak hledáme optimální parametry $\theta^*$ (např. gradient descent, sampling kolem $\theta$, vyhodnocení pomocí Monte Carlo simulací) tak, aby policy maximalizovala celkovou odměnu. Můžeme použít neuronové sítě, ale i jiné mechanismy.

**DQN** (Deep Q-learning Network) - Matici $Q$ aproximujeme pomocí hluboké neuronové sítě (na vstupu sítě je stav, výstupů je tolik, kolik je akcí), která pro každý stav vrací ohodnocení všech akcí (jak dobrá je akce v daném stavu, $Q$). Agent provádí akce v prostředí, pamatuje si stavy, akce a získané odměny. Na jejich základě je sestavena trénovací množina a síť je trénována pro minimalizaci rozdílu mezi $Q_{\theta_k}(s,a)$ a $R_a(s,s') + \gamma \cdot \max_{a'} Q_{\theta_k}(s',a')$, kde $\theta$ jsou váhy sítě. Strategie pak bere nejlepší akci dle aktuálního odhadu $Q$.
**Experience replay** - Ukládají se čtveřice $(s, a, r, s')$, při trénování z nich pak síť volí náhodně. Tím se snižuje korelace mezi po sobě jdoucími vstupy sítě, což vylepšuje trénink.
**Network targeting** - Jedna je trénovaná pro odhady, druhá je pevná pro výběr akce (po určitém počtu kroků se nahradí trénovanou).

**Actor-critic** - Actor optimalizuje parametrizovanou policy $\pi(\theta): S \rightarrow A$ (vylepšuje svůj performance). Critic optimalizuje svůj odhad performance actora (učí se prostředí a odhaduje agentův výkon pomocí advantage funkce $A(s, a) = Q(s, a)-V(s)$). Oba se učí současně, dohromady řeší kompletní problém. Použijeme hluboké neuronové sítě (pro $\pi(\theta)$, $Q$ a $V$). Actorů můžeme mít víc.
Advantage můžeme vyjádřit jako $A(s_t, a_t) = R_a(s_t, s_{t+1}) + \gamma V(s_{t+1}) - V(s_t)$, pak nepotřebujeme síť pro $Q$.

**A3C** (Asynchronous Advantage Actor-Critic) - Hraje se více her naráz, kdykoliv je k dispozici nějaká informace, hned se natrénuje do sítě. Nahrazuje použití experience replay, dostává také nekorelované vstupy. Aktualizace vah se potom průměrují přes aktualizace spočítané ve všech nezávislých hrách. Můžeme si to také představit jako více distribuovaných agentů dělajících update.

### Základní formy učení zvířat

**Klasické podmiňování** (Pavlov) - Spojujeme dva podněty, z nichž jeden získává nový význam, reakce zůstává stejná. Z nepodmíněné reakce na nepodmíněný podnět uděláme podmíněnou reakci na jiný podnět.
Nejprve provedeme podmíněný podnět, pak hned nepodmíněný, následuje nepodmíněná reakce (např. světlo, jídlo, slintání). Provádíme vícekrát, pak reakce už při podmíněném podnětu. Nakonec odebereme nepodmíněný.

**Operantní podmiňování** - Spojení určité reakce s jejími důsledky, učení následkům vlastního reagování (metodou pokud omyl). Dáváme odměnu za konkrétní akci, zvyšujeme šanci, že ji provede znovu. Může následovat i trest.

**Habituation** označuje jev, kdy si na podnět už natolik zvykneme, že na něj přestaneme reagovat.
**Sensitization** je opačný jev, kdy podnět zvyšuje reakci, naučíme se na něj být citlivější.

**Imprinting** - V určitém kritickém období života se mláďata dokáží naučit, jak vypadají jejich rodiče (matka). V tu dobu lze nahradit rodiče někým jiným (i člověkem).

*Trénování zvířat*:
**Capturing** - Odměňujeme spontánní pohyb (nějaká přirozená akce).
**Shaping** - Motivujeme k postupně komplexnějšímu chování (není pro zvíře přirozené), rozdělíme na menší části.

*Příklad*: Učíme psa dát pac. Klasické podmiňování na clicker (umožňuje přesnou signalizaci odměňované akce). Shaping pro dotyk s rukou, postupně výš. Pak ještě výš, signalizujeme konec akce. Nakonec klasické podmiňování na verbální povel.

## 7. Procedurální modelování stavového prostoru

**State space** $(S, A, Action(s): S \rightarrow A^*, Result(s,a): S \times A \rightarrow S, Cost(s,a): S \times A \rightarrow \mathbb{R})$:

**Goal-driven agent** (*model-based*) - Má přechodový model, předvídá stav po provedení akce.

**Model hry** - Může hru simulovat, někdy efektivněji než jen jako druhý běh hry.
*Simplified* - zjednodušený pro komplexní hry, aby se dal prohledávat.
*Forward* - plná simulace.
*Learned* - učíme se ho ze vstupů.

### Forward model

**Forward model** - Říká, jak se svět vyvíjí, pak řešíme jen problém hledání cesty (prohledáváme prostor stavů). Abstrahuje stav hry (dynamická část), oddělí od hry. Základní funkce (je důležitá efektivní implementace): `Clone` (klon stavu nebo celého modelu), `Actions` (enumerace dostupných akcí), `Advance` (další stav dle stavu a akce).

### Prohledávání

**Game tree** (*strom hry*) - Popis hry, v kořeni počáteční stav, potomci dle dostupných akcí.
*Branching factor* a *depth* - odhadneme velikost stromu.

*Základní algoritmy prohledávání prostoru stavů turn-based her*:
**Minimax algoritmus** - Minimalizuje maximální odměnu protihráče. Rekurzivně prohledává, často zastaví předčasně a odhadne výsledek (přijdeme o optimalitu).
**Alfa-beta prořezávání** - Odřízneme větve, které dají nejlépe horší výsledky než jiná nejhůř. $\alpha$ je doposud nejlepší volba pro MAX, $\beta$​ je doposud nejlepší volba pro MIN.
*Killer move* - Vylepšení. Tak, který způsobil cutoff v jiné větvi ve stejné hloubce zkusíme jako první.

*Horizon effect* - Omezuje se maximální hloubka, ale za horizontem může být významná změna.

#### A*

**A\*** - Standardní prohledávací algoritmus. Dostane počáteční stav, cílovou podmínku a forward model (ten využije pro expanzi). Udržuje si ceny navštívených a open-list. Postupně konstruuje graf stavů. Pokud došel čas, vrátí cestu k nejslibnějšímu vrcholu. Příště můžeme pokračovat, kde jsme skončili. Obvykle chceme agresivnější (ne přípustnou) heuristiku pro rychlejší prořezání (obětujeme optimalitu cesty), $cost(s) = pathCost(s) + w \cdot h(s)$

#### (Dopředné a zpětné plánování)

**Plánování** - Prohledávání jako plánování. Akce popíšeme jako trojice $a = (\text{precond}, \text{p.eff}, \text{n.eff})$ podmnožin literálů (z nich se sestavuje stav). Přechodová funkce je $\gamma: S \times A \rightarrow \mathcal{P}(s)$, přičemž $\gamma(s,a) = (s - \text{n.eff}(a)) \cup \text{p.eff}(a)$. Akce je aplikovatelná, když $\text{precond}(a) \subseteq s$. Máme počáteční $s_0$ a cílový stav $g$, hledáme posloupnost akcí $g = \gamma(...\gamma(\gamma(s_0,a_1), a_2)...,a_n)$​.
*Předpoklady a zjednodušení*: Konečné, plně pozorovatelné, deterministické, statické, jen jeden cíl, sekvenční plány, žádné délky trvání akcí, offline plánování (ne real-time).

**Forward planning** - Počáteční stav a prázdný plán. Dokud nesplníme cíl, enumerujeme možné akce, zvolíme jednu nedeterministicky (ve skutečnosti prohledávání, třeba BFS nebo IDA), přidáme do plánu.
**Backward planning** - Cíl a prázdný plán. Dokud počáteční stav nesplňuje aktuální cíl, nedeterministicky zvolíme jednu z akcí relevantních pro cíl (přispívá k cíli a efekty nejsou s cílem v konfliktu), vytvoříme regression set $\gamma^{-1}(g,a) = (g - \text{effects(a)}) \cup \text{precond(a)}$, akci přidáme do plánu. Podobné STRIPS.

#### ABCD

Upravíme minmax (alfa-beta), zohlední *simultaneous uzly* (hráči hrají současně), *durative actions* (akce mají délku trvání) a to, že se hráči *nemusí opravdu střídat*.

**Randomized alpha-beta** (*RAB*) - Hra pouze se simultaneous uzly, serializujeme. Na sudých vrstvách randomizujeme hráče, na lichých přepínáme. Jeden hráč může být dvakrát za sebou.

**Alternate alpha-beta** (*ALT*) - Žádné předpoklady, simultaneous uzly kdekoliv v průběhu prohledávání, když na něj narazíme, střídáme, kdo jde první.

**Alpha-beta considering durations** (*ABCD*) - Řeší simultaneous nodes i durative actions. Iterativní prohlubování (na konci evaluation function) pro any-time výsledky. Posuneme se do bodu, kdy může někdo provést akci, určíme hráče na tahu (v simultaneous uzlu dle nějaké policy). V simultaneous uzlu vyvoláme celé podruhé a předáme akce hráče na tahu, tak se řeší delayed action effect (akce prvního ještě neovlivní stav, pak se aplikují současně). Různé nastavení (RAB nebo ALT), různé metriky ohodnocení stavů.
*Move ordering* - Chceme brát tahy od nejlepších k nejhorším. Pokud jsme v předchozích iteracích iterativního prohlubování viděli daný stav ve větší než aktuální hloubce, máme dostatečně přesný odhad, podle toho setřídíme tahy. Nebo můžeme mít skript pro doporučení tahy.

**State evaluation function** pro RTS:
$e(s) = \sum_{u \in U_1} hp(u) - \sum_{u \in U_2} hp(u)$
$LTD(s) = \sum_{u \in U_1} hp(u) \cdot dpf(u) - \sum_{u \in U_2} hp(u) \cdot dpf(u)$
$LTD2(s) = \sum_{u \in U_1} \sqrt{hp(u)} \cdot dpf(u) - \sum_{u \in U_2} \sqrt{hp(u)} \cdot dpf(u)$
*Playouts* (pokud neexistuje dobrá heuristika, využito v MCTS, skript pro volbu akcí)

#### MCTS a UCB a další varianty

**Monte Carlo Tree Search (MCTS)** - Inkrementálně a asymetricky sestavuje strom, dokud nedojde rozpočet. Odhaduje kvalitu akcí $Q(s, a) = \frac{1}{N(s, a)} \sum_{i = 1}^{N(s, a)} SIM_i(s, a)$ dle výsledků simulací (kde se začlo v $s$ provedením $a$​​), z toho pravděpodobnost zvolení akce. Postupně se zpřesňuje, konverguje k minmaxu.
**Fáze iterace**: *Selection* (tree policy, až do listu), *Expansion* (expand policy, jeden nebo více nových vrcholů, náhodně nebo dle heuristiky pro uspořádání akcí), *Simulation* (default policy, kompletní simulace) a *Backpropagation* (výsledek se propaguje po cestě).
Nakonec se vybírá nejlepší akce z kořene (best policy; nejvyšší odměna, nejnavštívenější, ...).

```pseudocode
function MCTS(state) returns an action
	tree ← Node(state)
	while is_time_remaining() do
		leaf ← select(tree, TreePolicy) // explorace/exploitace
		child ← expand(leaf, ExpandPolicy)
		result ← simulate(child.state, DefaultPolicy)
		back_propagate(result, child)
	return best_child(tree, BestPolicy).a
```

**Vlastnosti**: *aheuristic* (není třeba heuristika pro ohodnocení stavů), *anytime*, *asymmetric* (slibnější vrcholy mají přednost).
**Použití**: single-player hry, multi-player hry (pak ve vrcholu vektor odměn, zohledňujeme vždy odpovídající komponentu), real-time hry, hry s nejistotou nebo simultánními tahy, plánování.

----

**Upper-confidence bounds (UCB)** - Balanc explorace a eploitace (confidence value $c$ řídí poměr). One-sided confidence interval, do kterého s vysokou pravděpodobností spadne skutečná očekávaná odměna. Potřebujeme průměrnou sledovanou odměnu akce $Q(a_i)$, počet vyzkoušení akce $n_i$, celkový počet pokusů $n$. Volíme akci maximalizující UCB.
**UCB1**: $UCB(a_i) = Q(a_i) + c \sqrt{\frac{2 \cdot \ln(n)}{n_i}}$

**Upper Confidence for Trees (UCT)** - MCTS s UCB jako tree selection policy. Zajistí, že každého potomka navštívíme alespoň jednou (pro $n_i = 0$ je UCT rovné $\infty$), to se nemusí hodit pro velký branching factor (pak je lepší třeba $\epsilon$-greedy). Když dojdeme do vrcholu, který není plně expandovaný, pak ho expandujeme. Jako počty pokusů bereme počty navštívení rodiče a potomka. Dokáže konvergovat k minimaxovému stromu.

----

**UCT Considering Durations (UCTCD)** - Řeší simultaneous uzly (oba hráči hrají naráz). Do uzlů se ukládají pouze akce (klony stavů se pouze předávají do rekurzivních volání průchodu a nakonec zahodí). U druhého hráče v simultaneous uzlu se aplikují akce společně s předchozím.
Každý uzel má svůj typ: *SOLO* (jen jeden hráč, tahy se aplikují), *FIRST* (simultaneous node, první hráč, akce se pozdrží), *SECOND* (simultaneous node, druhý hráč, aplikují se akce rodiče i potomka).

----

**Modifikace MCTS** mění jednu nebo více fází MCTS, potýkají se především s následujícími problémy:
*Selection* - Vysoký branching factor, přísné časové limity. Pak záleží na pořadí.
*Expansion* - Vysoký branching factor, přísné limity. Pak záleží na pořadí.
*Simulation* - Trvá dlouho dojít do terminálního stavu, nebo je to těžké s náhodnými akcemi.
*Back-propagation* - Výsledky mohou být různorodější, nejen výhra/prohra.

**Kategorie modifikací** - Většina modifikací může být rozdělena do 4 kategorií:
*Action reduction* - Omezení počtu dostupných akcí.
*Early termination* - Ukončení simulační fáze předčasně.
*Tree reduction* - Optimalizace růstu stromu změnou UCT funkce.
*UCT alternatives* - Jiné přístupy k selection fázi MCTS.

----

*Selection enhancements* - Mění tree policy, tedy způsob prozkoumání stromu. Skóre akcí lze vylepšit využitím znalosti domény.

**UCB1-Tuned Tree Policy** - Selection, expansion fáze. Lepší alternativa k UCB1, zohledňuje skutečný pozorovaný rozptyl odměn za akci. $V_i(n_i)$ přidává confidence interval k rozptylu $\sigma_{a_i}$. Omezené $1/4$, protože to je největší možný rozptyl pro odměny z $[0,1]$. Pro jiný rozsah musíme přizpůsobit.
$UCB1_{tuned}(a_i) = Q(a_i) + c \sqrt{\frac{\ln (n)}{n_i} \cdot \min \{\frac{1}{4}, V_i(n_i)\}}$

**First Play Urgency** - Nenavštíveným vrcholům se jako skóre místo nekonečna nastaví fixní hodnota $x$ (dle problému). Umožňuje brzkou exploitaci (slibných tahů nad $x$). Vhodné pro velké branching faktory, nemusíme každou akci zvolit alespoň jednou.

**Decisive Moves** - Selection, simulation fáze. Decisive move vede okamžitě k výhře. Anti-decisive move zabrání protihráči, aby v dalším tahu udělal decisive move. Při volbě akce děláme look-ahead (o 1 vrstvu pro decisive, 2 vrstvy pro anti-decisive). Pokud vidíme decisive move, zvolíme pro expanzi ten. Umožní ukončit simulaci dříve, může být adaptivní (blízko konci hry lookahead přes víc vrstev).

**Move Groups** - Snižuje branching faktor pro akce složené z podakcí (tahy jsou podobné, mají velmi korelované očekávané odměny). Vytvoří se rozhodovací vrstva navíc, v ní jsou akce uspořádané do skupin, UCB1 nejprve zvolí skupinu, z ní se zvolí akce. Podpoří se tak exploitace korelovaných akcí.

**Progressive Bias** - K UCB přidáme doménově specifickou heuristiku (pro řízení prohledávání, odhaduje hodnotu stavu). Použije se heuristic discount factor $\alpha$ (čím víckrát jsme použili akci, tím má heuristika menší vliv), $a_i.s$ je stav, ve kterém aplikujeme $a_i$.
$PB(a_i) = UCB(a_i) + \alpha (a_i, n_i, n) \cdot h(a_i.s)$

----

**Transposition Tables and DAG** - Vylepšení selection a back-propagation. Místo stromu v podstatě DAG (do podobných stavů se můžeme dostat různými posloupnostmi tahů). Máme dictionary navštívených stavů, klíčem je stav, sdílejí hodnoty. Data ukládáme do hran, hodnota ve vrcholu se spočte váženým průměrem předchůdců.
*Selection* - Místo jediné akce zkontrolujeme podstrom do určité hloubky (můžeme mít jen o kus dál lepší odhady), na základě něj spočteme skóre akce (tzv. "look-ahead" UCB value).
*Back-propagation* - Můžeme propagovat po všech cestách zpět do kořene (reversed BFS), ale tak můžeme přijít o optimální řešení. Nebo propagovat jen po cestě, po které jsme šli.

----

**All Moves As First (AMAF)** - Vylepšení back-propagation. Rychle vybuduje informaci pro první úroveň uzlů. Update statistik pro všechny akce zvolené během selekce a simulace, jako by byly první aplikovaná akce (pokud lze provést v kořeni). AMAF score (odhad získaný tímto způsobem) lze ukládat separátně.
**Permutation AMAF** - Updatuje také vrcholy, kterých lze dosáhnout permutací tahů v simulaci, pokud zachovává dosažený stav. Tj. na cestě do jiných listů stromu.
**$\alpha$-AMAF** - Blend standardního UCT skóre pro každý vrchol s AMAF skórem, $\alpha A + (1-\alpha)U$.
**Some-First AMAF** - Historie, která se používá pro update, se zařízne po prvních $m$ tazích.
**Cutoff AMAF** - AMAF algoritmus se používá pro update statistik jen v prvních $k$ simulacích.
**RAVE** (Rapid Action Value Estimation) - Podobné $\alpha$-AMAF, ale použitá hodnota $\alpha$ se v každém vrcholu zvlášť postupně zmenšuje s každou další návštěvou. Místo fixní hodnoty $\alpha$ použijeme $\max\{0, \frac{V - v(n)}{V}\}$ ($v(n)$ je počet návštěv, $V$ je fixní).

----

Můžeme prořezávat, tím eliminovat zřejmě horší možnosti. Mnoho strategií je nezávislých na doméně. Dva typy prořezávání tahů:
*Soft pruning* - Prořezávání tahů, které mohou být později prohledávány a vybírány.
*Hard pruning* - Prořezávání tahů, které nebudou nikdy prohledávány nebo vybírány.

**Progressive Unpruning/Widening** - Heuristický soft pruning (všechny tahy budou eventuálně zváženy s dostatkem času). Vynucuje dřívější exploitaci (podobně jako First Play Urgency). Když počet iterací překročí nějaký threshold, odřízneme $N$ stromů dle evaluation function. Po několika simulacích je vrátíme zpět. Lze i bez heuristiky, ale má to jen malý efekt.

**Absolute and Relative Pruning** - Dvě strategie zachovávající korektnost UCB algoritmu:
*Absolute pruning* - Prořezává všechny akce z pozice kromě té nejnavštívenější, jakmile je jasné, že žádná jiná akce se nestane více navštívenou.
*Relative pruning* - Používá upper bound na počet návštěv, které akce dostala, aby se detekovalo, kdy nejnavštěvovanější volba zůstane nejnavštěvovanější.

**Pruning with Domain Knowledge** - Pokud máme znalost domény, můžeme prořezávat akce vedoucí do slabších pozic.

**Score Bounded MCTS** - Obecné vylepšení pro hry, které mají výsledky s různým skóre. Pro každý vrchol si udržujeme optimistickou $opti(n)$ a pesimistickou $pess(n)$ hodnotu pro odměnu (hranici skóre vrcholu z pohledu maximalizačního hráče), také je propagujeme zpět stromem. Pak lze prořezávat. Optimální skóre v uzlu $n$ značíme $real(n)$, platí $pess(n) \leq real(n) \leq opti(n)$, hranice k němu konvergují.

----

*Simulation enhancements* - Zahrne se znalost domény pro modifikaci default policy (aby se nevybíraly tahy náhodně). Znalost lze posbírat offline, nebo online.

**Rule-based** - Ručně zakódujeme doménově specifickou policy.

**History Heuristics** - Předpoklad, že tah, který je dobrý v jedné pozici, může být dobrý i v jiné. Může vylepšit selekci (history bonus) i simulaci. Ukládáme navíc globální statistiky, $Q(a)$ (průměrná odměna za akci) a $N(a)$ (počet návštěv).
*$\epsilon$-greedy* - V simulaci se s pravděpodobností $\epsilon$ zvolí historicky nejlepší akce (nejlepší UCB), jinak náhodná.
*Roulette sampling* na základě $Q(a)$ nebo konkrétní distribuci.

**Last Good Reply** - Lze použít v selection a simulation. Každý tah ve hře se považuje za odpověď na předchozí tah. Odpověď je úspěšná, pokud hráč nakonec zvítězil. Uloží se poslední úspěšná odpověď (pozdější přepíše předchozí), po dalším výskytu tahu se opět použije.
*LGR-1 policy*: Během simulace hrají hráči uloženou poslední dobrou odpověď, pokud je to legální tah.
*LGR-2 policy*: Ukládají se odpovědi na poslední 2 tahy. Pokud není žádný LGR-2 záznam pro poslední 2 tahy, použije se LGR-1.
*LGRF* (Last Good Reply with Forgetting) - Odpověď se odstraní, pokud během poslední simulace vedla k prohře. LGRF-1, LGRF-2.

----

*Backpropagation enhancements* - Speciální update vrcholu.

**Weighting Simulation Results** - Pozdější simulace bývají přesnější, kratší simulace bývají přesnější. Výsledek simulace se propaguje s váhou $w$ (jako by to bylo $w$ simulací).

**Decaying Reward** - Hodnota odměny se násobí nějakou konstantou $0 < \gamma \leq 1$​ mezi každým vrcholem. Dřívější/rychlejší výhry tak mají větší váhu než pozdější/pomalejší.

----

*MCTS Parallelization* - Simulace v MCTS jsou nezávislé, lze je dělat paralelně. Je však třeba správně kombinovat výsledky a synchronizovat.

**Leaf Parallelization** - Paralelně několik nezávislých playouts (simulací) ze stejného vrcholu po expanzi. Výsledky se agregují (součet, průměr) a propagují zpět. Strom neroste rychleji, ale odhady jsou přesnější.

**Root Parallelization** (*Multi-tree MCTS*) - Staví se víc MCTS prohledávacích stromů naráz (paralelizované v kořeni). V určitém synchronizačním kroku (např. těsně před uděláním skutečného rozhodnutí) se statistiky první úrovně stromu zagregují (váženě dle počtu návštěv, majority voting scheme). Lze i sdílet periodicky.

**Tree Parallelization** - Simultánní MCTS simulace na jednom jediném stromě, celé iterace jsou paralelní, strom se sdílí (přístup pomocí zámků). Lze vrcholu přidat dočasnou "virtual loss", ať ostatní vlákna vybírají spíše jiné.
*Tree Parallelization with Global Mutex* - Globální zámek v kořeni, jen jedno vlákno smí vždy přistoupit ke stromu, ostatní mohou dělat simulace z různých listů.
*Tree Parallelization with Local Mutexes* - Lokální zámky na vnitřních vrcholech, kdykoliv vlákno navštíví vrchol.

#### PGS

**Portfolio Greedy Search (PGS)** - Vhodné pro velký branching factor, kombinuje skripty a playouts. Máme portfolio skriptů, hladově hledáme lepší přiřazení jen jeden skript pro všechny jednotky.
Přiřadíme defaultní skript protihráčovým jednotkám. Pak najdeme skript pro nás jako nejlepší odpověď (všechny jednotky stejný), hladově pomocí playouts. Pro protivníka opět nejlepší odpověď.
Následně vylepšíme naše přiřazení. Postupně pro každou jednotku najdeme nejlepší skript (ohodnotíme pomocí playout nebo state evaluation function). Dokud máme budget, střídavě vylepšujeme přiřazení pro protivníka a nás.

**Convergence problem** - PGS nemusí vždy kovergovat k minmax řešení (což je to ideální), může se zacyklit, tak se vynechají větve obsahující optimální řešení.

#### PGS-II

**Nested Greedy Search (NGS)** - Vylepšuje PGS, ale problém s konvergencí neřeší, pouze odsouvá o vrstvu dál (nemusí najít optimální řešení).
Přiřadíme defaultní skript všem našim i protihráčovým jednotkám. Pak, dokud máme rozpočet, zkoušíme vylepšit naše přiřazení. Pro každou možnou změnu rovnou vylepšujeme i protihráče (zanořený další greedy search).

Nezkoušíme všechna možná přiřazení $|P|^n$, ale děláme to hladově $|P| \cdot n$​ (počet skriptů v portfoliu krát počet jednotek; je to lokální prohledávání, není to perfektní).

#### Prostor skriptů (Kiting, AV, NOK-AV)

**Scripted behaviors** - Místo elementárních akcí používáme skripty (přecházíme z action-space do script-space), tím se omezuje branching factor. Rychlé, ale chybí look-ahead.

*Random* (RND) - Vybírá legální tah s uniformním pravděpodobnostním rozdělením.
*Attack-Closest* (AC) - Zaútočit na nejbližší jednotku (lokální prohledávání pro nalezení nejbližší jednotky), pokud to jde; pokud nabíjí, počkat; pokud není v dosahu, přesunout se na dosah nejbližšímu.
*Attack-Weakest* (AW) - Jako AC, ale útočí na nejslabšího v dosahu.
**Kiting** (Kit) - Zaútočit na nejbližší jednotku (AC), ale pak se vzdálit od nejbližšího během nabíjení místo čekání. Lze kombinovat i s AW.
**Attack-Value** (AV) - Jako AC, ale zaútočit na jednotku s nejvyšším $dpf(u)/hp(u)$​​ (eliminovat velké hrozby s málo zdravím).
*No-OverKill* (NOK) - Zaútočit na jednotku jen, pokud už nedostává smrtelný damage.
*No-OverKill-Closest* (NOK-Closest) - Zaútočit na nejbližší jednotku, pokud už nedostává smrtelný damage.
**No-OverKill-Attack-Value** (NOK-AV) - Zaútočit na jednotku dle AV, pokud už v tomto tahu nedostává smrtelný damage.
*Kiting-AV* (Kit-AV) - Hit and run, cíl se volí dle AV (nejvyšší $dpf(u)/hp(u)$​), ale pak hned utéct pryč.
*Kiting-NOK-AV* - Kiting, ale cíl se zvolí podle NOK-AV.

#### Efektivní implementace

S efektivní implementací forward modelu (tj. jeho funkcí `Clone`, `Actions` a `Advance`) jsme schopni prohledávat více stavů.

## 8. Procedurální generování obsahu

**Procedurální generování obsahu** - Algoritmické vytváření obsahu s omezeným nebo nepřímým vstupem uživatele. Pomocí nějakých pravidel popíšeme, jak má vypadat výsledek. Např. terén, vegetace, silnice, města, textury, efekty, questy, příběhy apod.
**Důvody**: *Komprese dat*, *Ekonomika* (rychlejší, levnější, znovupoužitelné), *Variabilita* (replayability, adaptivity, unpredictability), *Rules enforcement* (generujeme podle podmínek), *Model reality* (simulace), *Fun*.
**Historie**: *Volvelle* (17. století, nová slova), *Musical dice game* (Johann Phillipp Kirnberger (1757), W. A. Mozart (1787)), *Automated Love Letter Generator* (1952, výběr slov do volných bloků ve větách), *Solo Dungeon Adventures* (1975, Gary Gygax, DnD), **Rogue** (1980, dungeon), **Elite** (1984, galaxie, fixní seed), *Civilization* (1991, mapy), *Diablo* (1995, dungeony, loot).
**Moderní příklady**: *Tiger Woods PGA Tour* (2006, SpeedTree pro vegetaci), *Dwarf Fortress* (2006, svět, historie, poezie, ...), *Spore* (2008, svět, animace, hudba), *Minecraft* (2011), *No Man's Sky* (2016, planety, fauna, flora), *Space Engineers* (planety), *Spelunky* (levely), *Borderlands* (zbraně), *Yavalath* a *Pentalath* (deskové hry), *Path of Exile* (hráč vytváří mapy, přidává atributy), *Galactic Arms Race* (dává častěji zbraně podobné oblíbeným), *.kkrieger* (moderní ukázka komprese).
**Těžké problémy**: Puzzles, narratives and other heavily structured content. Mixed-initiative generation. Modelling player experience. Personalized content. Avoiding occasional catastrophic failure. Converting bad content to good content. Measuring quality (emergentní metriky, ne vstupy generátoru).
**Nevýhody**: Omezená kontrola nad výsledkem. Může to být až moc náhodně. Může být moc generické, repetitivní. Parametry nemusí být intuitivní. Top-down přístupům často chybí sémantický kontext.


### Klasifikace metod procedurálního generování

**Dle času**: *Design-time* (generuje se ve studiu, ve hře pak statická verze, úplná kontrola) vs *Runtime* (generuje se na hráčově stroji, riskantní)

*Dle high-level přístupu*:
**Teleological** (bottom-up) - simulace procesů v reálném světe, obvykle design-time
**Ontogenetic** (top-down) - pozorujeme, jak vypadá skutečný svět, snažíme se o podobné výsledky (aproximaci)

*Dle úrovně kontroly*:
**Most direct** - znatelný zásah člověkem, diskrétní, méně variabilní
**Most indirect** - malý zásah, vypadá velmi procedurálně, kontinuální, více různorodé

**Necessary** - Generuje něco potřebného, např. hlavní questy v RPG.
**Optional** - Generuje něco méně potřebného (lze použít více experimentální metody), např. vedlejší questy v RPG.

**Stochastic** - Pokaždé zcela náhodné, např. Rogue.
**Deterministic** - Zafixujeme seed, pokaždé stejné, např. Elite.

*Hlavní přístupy*:
**Simulation-based** - Výchozím a množina operátorů pro změnu světa. Běží po danou dobu. Poskytuje historii, může reagovat na hráče, ale žádná kontrola a pomalé. Např. terén.
**Constructionist** - Spojování předpřipravených bloků, specializované na hru. Velká kontrola, ale těžko se získává rozmanitost. Např. rogue-like místnosti, platformery.
**Grammars** - Formální gramatikou se popíše prostor možných obsahů, pak se parsují a zpracovávají pravidla. Oddělení pravidel od sestavení obsahu, ale náchylné k overgeneration nebo undergeneration. Např. platformer, budovy, vizuální obsah.
**Optimization** - Prohledávání pro nalezení optimální kombinace dle vyhodnocovací funkce, např. evoluční algoritmy. Velmi citlivé na použitou reprezentaci a konkrétní implementaci, může být obtížné definovat dobrou vyhodnocovací funkci. Např. bludiště.
**Constraint-driven** - Splňování omezujících podmínek. Deklarujeme podmínky, constraint solver hledá řešení. Oddělení specifikace od prohledávání, ale je třeba specifikovat všechny podmínky a může to být pomalé. Např. interiér, platformer, answer set programming.

### Přístupy pro generování

#### Terén

**Klasifikace druhů generování terénu**:
*Podle použitého přístupu*: teleological (simulace), ontogenetic (něco, co vypadá jako terén)
*Podle časování*: design time, runtime (musíme tomu opravdu věřit)
*Podle reprezentace*: tilemaps (typ terénu v mřížce), heightmap (výška v mřížce), voxels (materiál ve 3D mřížce), meshes, ...

*Jednoduché přístupy s heightmap*:
**Fault formation** - V mřížce samé 0, určíme předěl, na jedné straně trochu zvýšíme (postupně míň), opakujeme. Nakonec filtr pro vyhlazení.
**Hills-adding** - Přidáváme náhodné kopce (paraboly), dokud nejdou od sebe rozeznat.
**Particle deposition** - Náhodně umístíme particle (jednotka výšky), cestuje dolů, pokud může.
**Mid-point displacement** - Heightmap nižšího rozlišení (2x2, třeba náhodně) zjemňujeme. Nejprve průměry dvojic po okrajích, pak doplnění hodnoty uprostřed, navíc náhodný šum.
**Diamond-square** - Vylepšuje předchozí. Střídáme diamond step (zprůměrujeme z rohů do středu + náhodný šum) a square step (doplníme do mřížky + náhodný šum), šum postupně zmenšujeme.

*Noise přístupy s heightmap* (velmi levné): **Value noise** (náhodné hodnoty daleko od sebe, SmoothStep interpolace, **Perlin/Simplex noise** (pro jeskyně třeba průřez šumem), **Oktávy**, **Ridged noise**, **Domain warping**

*Heightmap přístupy bez šumu*:
**Celulární automaty** - Naplníme mřížku náhodnými hodnotami, pak celulární automat po určitý počet kroků (např. majority rule).
**Simulation** - Simulace kapek deště (někam dopadnou, nesou s sebou dolů terén).
**Geological simulation** - Eroze, tektonické desky, ...
**Agent-based simulation** - Miners nebo mountain shapers, nějak se pohybují, něco dělají.
**AI a ML** - Např. GAN, uživatel načrtne, co kde chce, pak se vygeneruje.
**Grammar-based** - Horu přepíšeme na kopce, těm přidáme stromy, ... Hierarchické.

*3D přístupy*:
**Random walk** - Náhodně volíme směr (třeba pomocí Perlin noise), šum mapujeme na úhel. Např. jeskyně.
**Perlin worms** - Dvě noise funkce, průřez z každé, pak průnik.
**3D noise** - Nastavíme threshold pro různé materiály. Můžeme začít s 2D šumem pro heightmap, pak pomocí 3D šumu jeskyně, převisy.
*3D Worley noise* - Použijeme hrany buněk, může to tvořit jeskyně. Lze přidat domain warping.
**Celulární automaty**
**Agent-based** (miners)

*Biomy*: Rozdělit svět do biomů (**Voronoi diagram** nebo **Whittakerův diagram**), pak generovat různě. Nebo generovat terén, pak rozdělit třeba podle výšky (a odlišit vegetací).

**L-systém** (vegetace, řeky, jeskyně), **Wave Function Collapse** (mapy, ostrovy)

#### Vizuální efekty

**Top-down** - Umělec má vizi, vytvoříme nástroj naplňující ji.
**Bottom-up** - Vybereme techniku (celulární automaty, gramatiky, noise), přidáme filtry a úpravy.

**Noise** - Jen noise a nějaké chytré triky. Hodně se používá domain warping. Třeba různým intervalům přiřadíme barvy.
**Random walk** - Můžeme mít specifická pravidla (třeba hodně rovných čar). Můžeme mít několik náhodných procházek s různými barvami.
**Evoluční algoritmy** - Př. generování Mony Lisy z průhledných polygonů.
**Celulární automaty** - Obrázek jako seed, po několika iteracích se na něm vytvoří zajímavý filtr.
**Fraktály** (gramatiky, L-systémy) - Vegetace, stromy.
**Generative Adversial Networks** - Bere hodně obrázků na vstup, generuje obrázky s podobnými rysy.

**Art toys** - Hra bez cíle, žádný outcome. Více experimentální než hry. Menší kontrola nad výsledkem. Možná implementace: vezmeme libovolný vstup, zvolíme pro něj reprezentaci (pixely, tahy, čáry), aplikujeme transformace, nakonec post-processing.

**Procedural effects** - Typicky spojené s živly (oheň, voda, vítr). Často obarvený šum, oktávy pohybující se různě rychle, fraktály. Mohou být i simulace.

**Procedurálně vytvořené sprity** - Třeba celulární automaty, zrcadlíme, dodáme nějaké prvky.

#### Hudba

Určit **tempo**, vybírat jen tóny ze **stupnice** (major, natural minor, major pentatonic).
**Melodie** - Random walk po stupnici, s pravděpodobnějšími malými kroky. Dovolit i nějaké pauzy.
**Tónina** - Končit na prvním tónu.
**Rytmus** - 2/4, 3/4, 4/4. BDrum kick na první dobu, snare na další doby.
**Akordy** - Obsahující tóny ze stupnice, major, minor, major 7th. Můžeme použít akord dle tónu z melodie, nebo jen jeden tón o oktávu níž.

**Struktura písně** - Intro-Verse-Chorus-Verse-Chorus-Bridge-Chorus. Opakovat vzory melodie, třeba jen s malými obměnami.

**Náhodné procházky** (po stupnici, vyhlazené noisem, opakování částí), **Markovské řetězce** (tón závisí na $n$ předchozích), **Celulární automaty** (některé přirozeně formují repetici, vzory), **Neuronové sítě** (GAN), **L-systems**

#### Předměty

**Předměty** - Loot, předměty v obchodě. Pokaždé nové, zajímavé, ale smysluplné, nepřehnat to s množstvím a rozumně škálovat sílu.

**Základní přístup** - Základní předměty + vylepšení, vybíráme náhodnou kombinaci.
**Large-scale variability** - Zakázat některá vylepšení na některých předmětech, přidat themes a omezení. Pak nejsou tak moc stejné.
**Building-blocks** - Předpřipravené části s rozsahy (síla), pak sestavit.

**Reasonably powered items** - Procentuální zlepšení nebo dle levelu hráče.
**Power scaling** - Předmět by neměl být užitečný jen příliš krátkou dobu, ani příliš dlouhou dobu.
**Vzácnost předmětů** - Změřit drop rate v počtu hodin, přizpůsobit délce hry.
**Pity timer** - Alespoň jednou za nějaký čas dát nějaký lepší předmět.

**Player interaction with loot system** - Crafting, modularity (drahokamy do socketů apod.), předměty nebo efekty na postavu, které zvyšují drop chance nebo raritu.

#### Bludiště

Lze reprezentovat jako **graf**.

**Typy bludišť**: *Dimensions* (2D, 3D, 4D, ...), *topology* (obdélník, koule, toroid, teleporty, ...), *tesellation* (orthogonal, hex, triangular).

**Perfect maze** - Jen jedna cesta mezi libovolnými dvěma body, všechny buňky dosažitelné.
**Braid maze** - Žádné slepé uličky, vždy cykly, každá cesta nás někam dovede.

**Run factor** - Kolik je tam dlouhých rovných cest.
**River factor** - Jak dlouhé jsou slepé uličky.
**Elite factor** - Jak velká část bludiště je pokryta nejkratší cestou od startu do cíle.
*Unicursal maze* - Elite faktor 1.0 (jen jedna cesta celým bludištěm)

**Objectives**: *challenge*, *puzzle*, *estetické*, *zábavné k prozkoumání*

**Řešení bludišť**: *Computer methods* (BFS, DFS, heuristické prohledávání), *Human methods* (DFS, wall-following, filling dead-ends, heuristika).

**Metriky**: Počet cest do cíle, délka cesty ze startu do cíle, run factor, vzory.

**Reprezentace bludiště**: *Direct representation* (bitmapa), *positive representation* (přidávají se překážky), *negative representation* (přidávají se chodby).

**Přístupy** pro generování se pak liší podle reprezentace (*Wall-adding*, *Passage-carving*) a podle typu bludiště (na perfektní můžeme aplikovat post-processing).

**Generování perfektního bludiště**:
*Kruskalův algoritmus* - Začneme s úplným grafem (s náhodnými váhami hran), pak hledáme nejmenší kostru (např. Kruskal, hladový, přidává náhodné hrany, pokud netvoří cykly).
*Backtracking* - Backtracking nebo DFS, náhodným směrem. Vytváří delší slepé uličky.
*Wilson's algorithm* - Z náhodného bodu začneme náhodnou procházku, lze chodit i zpět. (stáhne oblast). Při dotyku již vytvořené cesty se začne znovu.

**Generování braid maze**:
*Post-processing* - Vygenerujeme perfektní bludiště, odstraníme nějaké zdi (náhodné nebo sousedící se slepou uličkou).
*Wall adding* - Přidáváme náhodně jednotlivé zdi, ale jen pokud negenerují slepé uličky, a to maximální možný počet.

**Search-based přístup** - Za design-time. Prohledáváme možné obsahy. Můžeme použít každý výstup, nebo otestovat a zahodit špatné příklady. Může být třeba evaluační funkce.
**Evoluční algoritmy** - Vybereme reprezentaci, způsob změny a způsob ohodnocení. S pravděpodobností dle ohodnocení vybíráme rodiče, křížíme, náhodně mutujeme.

**Answer Set Programming** - Popíšeme omezení, pak samplujeme bludiště rovnoměrně ze všech možných.

#### Dungeony

**Dungeony** - Místnosti, volitelně chodby, nepřátelé.

Můžeme použít některou z následujících **metod**:

**Evoluční algoritmy** (s různými kódováními)
**Building blocks** - Ručně navrhneme místnosti, z nich se prohledáváním vytváří obsah.
**Hierarchical representation** - Rozděl a panuj. Svět rozdělíme na menší části, každou generujeme zvlášť, třeba i jinou metodou, propojíme.
**Template search** - Kombinace dvou předchozích. Hierarchie šablon (dungeon, rozložení místností, obsah místnosti), různé varianty uvnitř. Něco jako gramatika. High-scale (volba šablony) i low-scale (instanciace) variability. Např. Spelunky.
**Shape/graph grammars** - Lze kombinovat. Graph grammar pro vytvoření dungeonu (reprezentace jako graf), pak shape grammar pro umístění do prostoru.
**Cellular automata** - Dungeon vypadající jako jeskyně.
**Answer set programming**
**Wave function collapse** - Je nutný post-processing (osamocené místnosti, více propojení).

**Metody bludiště**:
*Pouze místnosti* - Jen místnosti v ortogonální 2D mřížce, žádné chodby. Vygenerujeme bludiště, pak post-processing nebo templating pro instanciaci místností.
*Začneme bludištěm* - Vygenerujeme bludiště, odstraníme některé slepé uličky, převedeme na vyšší rozlišení, do volného prostoru dáme místnosti.
*Skončíme bludištěm* - Náhodně umístíme místnosti, prostor mezi vyplníme bludištěm (až k místnostem nebo na hranice buněk a pak spojíme). Můžeme odstranit slepé uličky.

**Binary division** - Rekurzivně dělíme na 2 části, různě střídáme osy. Pak buď celá buňka místnost, nebo někde uvnitř. Propojit sourozence ve stromě.

**Agent-based methods**:
*Simple* - Náhodná procházka, s malou pravděpodobností umístí místnost.
*Look-ahead* - Pokud místnost nelze umístit, backtrackujeme.

### Šumové funkce

Levné, snadno se parametrizují, dají se kombinovat.

**White noise** - Zcela náhodné hodnoty, mohou být velké rozdíly mezi sousedními.
**Value noise** - Méně náhodných hodnot v prostoru dál od sebe, pak interpolace (bilineární, SmoothStep, SmootherStep).
**Gradient noise** - Tabulka náhodných gradientů, ne hodnot. Pak interpolujeme tyto gradienty, podle nich vyplňujeme hodnoty. Např. Perlin noise.

*Triky a vylepšení*:
**Octaves** - Kombinace několika noise funkcí s různými frekvencemi a amplitudami (typicky násobíme frekvenci a dělíme amplitudu).
**Domain warping** - Pozice posuneme o další noise, $f(p) = noise(p + noise(p))$.
**Ridged noise** - Vezmeme absolutní hodnotu, pak překlopíme, nakonec přeškálujeme.
**Billow noise** - Jen absolutní hodnota, nepřeklápíme. Př. kameny, mraky.

#### Perlin

Gradient noise. Máme tabulku pseudo-náhodných gradientů (ukazují ve směru kopců), pak interpolujeme. Pro bod najdeme políčko mřížky, spočteme skalární součiny gradientů a vektorů z rohu do bodu, hodnoty interpolujeme (bilineární, SmoothStep, SmootherStep), dostaneme výšku.
Viditelné artefakty po osách.
Použití: heightmap, náhodná procházka, jeskyně.

#### Simplex

Simplex je určen $n$​ body a tvoří ho jejich lineární kombinace.
Vylepšení Perlin noise, simplexy místo mřížky. Pro interpolaci je tak potřeba méně bodů, výkonnější.
Odstraňuje směrové artefakty, je výkonnější.

#### Worley

Také cellular noise.
Máme mřížku náhodných bodů (grid jittering), pro každý pixel spočteme vzdálenost k $n$​ nejbližším, nějak zkombinujeme, např. jen nejbližší, jen druhý nejbližší (zvýrazňuje hrany), nejbližší mínus druhý nejbližší.
Použití: voda, kámen, biologické buňky, 3D noise pro jeskyně.

### Celulární automaty

**Celulární automaty** - Mřížka buněk (třeba i 3D) v nějakém stavu, pak různá pravidla pro změnu stavu dle počtu sousedů v konkrétním stavu.
Např. *B3/S23* - buňka se narodí, pokud má právě 3 sousední naživu, přežije, pokud má 2-3 sousedy naživu, jinak zemře.
Přepisující se systém (něco jako gramatika/L-systém), může vytvářet opakující se vzory.

**Použití**: Terén, jeskyně, procedurálně vytvořené sprity.

**Elementární celulární automaty** - Nejjednodušší třída, jen 1D. Jen dvě možné hodnoty 0/1, následující stav založen na buňkách vlevo, vpravo a na aktuálním stavu.

### L-systémy

**Gramatika** je čtveřice $(V, T, R, S)$, neterminály, terminály, pravidla a počáteční symbol. Aplikujeme pravidla, dokud nemáme jen terminály.

**L-systémy** (*Lindenmayer system*) - Specifická verze bezkontextové gramatiky. Každý symbol odpovídá nějaké akci a vždy se aplikují všechna pravidla naráz. Běh můžeme zastavit po určitém počtu iterací. Postupně vznikají řetězce délky Fibonacciho čísel.
*Použití*: vegetace (stromy, květiny), sítě řek, jeskyně, hudba.

**Binary tree L-system** - Generuje binární stromy, používá něco jako turtle graphic.

```pseudocode
Variables: 0 1	// finální čára, čára
Constants: [ ]	// push + vlevo, pop + vpravo | 45°
Start: 0
Rules:
	0 --> 1[0]0
	1 --> 11
```

**Fractal plant** - Trochu odlišná interpretace, realističtější vegetace.

```pseudocode
Variables: X F		// nic, čára
Constants: + - [ ]	// vpravo, vlevo, push, pop	| 25°
Start: X
Rules:
	X --> F+[[X]-X]-F[-FX]+X]
	F --> FF
```

**Stochastic L-system** - Přidáme do pravidel trochu náhody (úhly, délky čar), náhodně vybíráme z pravidel, náhodně třeba pravidlo nepoužijeme vůbec.

### Grafové a tvarové gramatiky

**Graph grammar** - Gramatika, která běží na grafu (přirozené hezké reprezentace pro určitý obsah, strukturovaná).
*Příklad pravidla*: Uzel označený A s hranou do uzlu B --> oba se přeoznačí na A a přidá se další uzel B a do něj hrany z obou A.
*Problém*: Převedení grafu na obsah. Můžeme začít grafem, pak se ho pokusit umístit do prostoru. Nebo začít prostorem a pak se snažit gramatikou vytvořit graf zapadající do něj. Nebo to vytvářet současně a navzájem ovlivňovat.
*Použití*: Lock and key puzzles.

**Shape grammar** - Zobecnění L-systému, aplikuje pravidla paralelně, ale pracuje s tvary.
*Příklad pravidla*: Vidíme hranu, vytvoříme malý zářez; vidíme základy, vytáhneme je nahoru a postavíme jedno podlaží budovy.
*Použití*: Architektura (budovy, města; City Engine), dungeony a obecně generování něčeho v mřížce (layout mission grafu, např. pravidlo vidíme vrchol říkající, že chceme propojení, umístíme dveře, nebo cestu na konci opět s propojením, nebo křižovatku se 2 dalšími propojeními).

### Answer set programming

Puzzles, strategy maps, dungeons - spoustu podmínek ke splnění.

**Constraint satisfaction problem** (CSP) - Specifikujeme doménu, se kterou pracujeme, a jaká jsou tam omezení. Pak obecný solver (typicky nějaký heuristický search) najde řešení.

**Answer set programming** (ASP) - Podoblast CSP (specifický přístup), populární a užitečné hlavně pro náročné problémy (NP a těžší). Postavené na teoretické logice, generuje model pro náš problém.

**Program** - Množina pravidel ve tvaru implikací $x_0 \leftarrow x_1 \and x_2 \and ... \and x_n$ (head a body). Běžně ve 3 částech, *generate*, *define* (pomocná pravidla) a *test* (integrity constraint, jestli je validní).
**Fakt** - Pravidlo bez těla, $x_0 \leftarrow \top$.
**Integrity constraint** - Pravidlo bez hlavy, $\bot \leftarrow x_1 \and x_2 \and ... \and x_n$ (vždy nepravdivá).
**FullASP** - V hlavě smí být i disjunkce literálů (prázdná je definovaná jako $\bot$)

**Answer set** - V inkluzi minimální (ne nadmnožina jiné) množina literálů, které splňují daný program (jsou jeho modelem). Je to nejjednodušší vysvětlení programu.

**Klasická negace** ($\neg x$, $\neg x \in M$) - Víme, že literál neplatí.
**Defaultní negace** (${\sim}x$, $x \notin M$) - Nevíme, že literál platí.

**Redukt** programu $P$ množinou literálů $M$ ($P/_M$) - Vezmeme pouze aplikovatelná pravidla (defaultní negace nesmí být v $M$), z nich pouze pozitivní literály (negativní hlavy změníme na $\bot$). Dostaneme znovu program, ale odstraníme všechny defaultní negace podle množiny literálů.
Odhadujeme množinu literálů, které budou pro program dobré.
Množina $M$ je answer set programu $P$, pokud je to answer set pro program $P/_M$.

*Příklad*: Máme program se 2 pravidly: $P = \{ q \leftarrow {\sim}p, \,\, p \}$. Můžeme redukovat podle 4 různých množin, dostaneme $P/_{\{\}} = \{q,p\}$, $P/_{\{p\}} = \{p\}$, $P/_{\{q\}} = \{q,p\}$, $P/_{\{p,q\}} = \{p\}$. Jen $\{p\}$ je answer set (minimální model programu). $\{\}$ není modelem programu $\{q,p\}$, stejně tak $\{q\}$ není modelem programu $\{q,p\}$ a $\{p,q\}$ sice je modelem $\{p\}$, ale ne minimálním.

**AnsProlog** - Konkrétní jazyk pro ASP, predikátová logika.

```pseudocode
vertex(0..2).									// 3 vrcholy
color(r;g;b).									// 3 barvy
edge(0, 1). edge(1, 2). edge(2, 0).				// trojúhelník
{colored(V, C) : color(C)} = 1 :- vertex(V).	// každý vrchol má přiřazenou 1 barvu
:- edge(X, Y), colored(X, C), colored(Y, C).	// 2 sousední vrcholy nesmí mít stejnou barvu
```

**Použití**: *Bludiště* (musí být propojené, žádná propojení navíc, jen kostra grafu), *puzzle games*.

### Algoritmus kolapsu vlnové funkce

**Wave Function Collapse** (WFC) - Máme malý vstupní obrázek, chceme podobné, ale větší (lokálně podobné vstupu). Forma CSP.

**Local similarity** ($N$ se volí při startu, často 3): Každý $N\cdot N$ vzor výstupu musí existovat ve vstupu (žádné nové vzory). Pravděpodobnost výskytu $N \cdot N$​ vzoru ve výstupu musí být blízko k četnosti vzoru na vstupu (těžší k dosažení, snažíme se to optimalizovat).

**Overlap verze** - Máme bitmapu, vybereme všechny $N \cdot N$ vzory (překrývají se), sousedit mohou ty, které mají podobnou hranu. Ve výstupu vybíráme nezaplněné pole (s nejmenší entropií vzorů, které tam můžeme umístit, nejvíc omezené), náhodně vybereme vzor (podle vah vzorů ze vstupu), updatujeme, co je kam možné umístit (od sousedních políček propagujeme do dalších, udržuje se path-consistency).

**Tile verze** - Máme rovnou několik tiles, jejich váhy (místo bitmapy) a informaci, které mohou sousedit. Vybereme prostor s nejnižší entropií (nejméně možnostmi), zvolíme náhodně dlaždici podle váhy, propagujeme constraints napříč obrázkem.

**Nevýhody**: Může narazit na slepou uličku (nejsou už žádné možnosti). Můžeme restartovat, nebo backtracking.
**Použití**: Ostrovy, dungeony (s post-processingem), mapy.

### Metody smíšené iniciativy

**Mixed-initiative programming** - Člověk i počítač ovlivňují, co bude výstupem. Lidé poskytují za runtime nějaká omezení, počítač s tím pak pracuje. Iniciativa se střídá mezi člověkem a počítačem, jakýsi dialog.
*Příklad*: Interactive fitness pro ohodnocení obsahu. Počítač poskytuje algoritmus, člověk evaluaci, dobře spolupracují.

**Computer-main methods** - Vše generuje počítač, člověk to jen řídí směrem k preferencím (třeba vybírá z několika příkladů). Př. interactive evaluation.
**Human-main methods** - Člověk dělá velkou část práce, počítač pracuje na detailech. Př. procedural texturing, procedural vegetation (člověk řekne, kde bude les, počítač umístí).

**Nevýhoda**: Přidání interakce je často obtížné. Ještě to není moc prozkoumané.

**Příklady**:
*Generování neuronové sítě* - Snaží se zvýšit rozlišení toho, co kreslíme. Počítač se snaží odhadnout, co kreslíme, dá několik možností. Iteruje se.
*Tanagra* - Generování levelu platformeru. Beat timeline, člověk může rozdělit beat, přidat ho, s každým beatem se přidá překážka. Ovlivňuje rak pacing hry, ale počítač generuje překážky.
*Ropossum* - Generátor levelů pro Cut the Rope. Přidáváme věci do levelu, můžeme něco zafixovat něco, počítač doplní zbytek, něco trochu změní.
*Sketch-based methods*- Načrtneme, kde chceme mít co v terénu (např. hory, jezera, řeky, silnice), pak se podle toho terén vygeneruje.