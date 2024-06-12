# Počítačová grafika pro hry

## 1. Souřadnice, transformace, kvaterniony

Nejprve projdeme *základní operace* s vektory.

**Skalární součin** (dot product): $p \cdot q = \sum_i p_i q_i$ nebo také $p \cdot q = |p| |q| \cos \alpha$, maticově $p \cdot q = p^Tq$

**Projekce vektoru** (*ortogonální projekce*): $p_{proj} = \frac{p \cdot q}{|q|} \cdot \frac{q}{|q|} = \frac{p \cdot q}{q \cdot q} q$
Maticově pomocí $qq^T$ jako $p_{proj} = \frac{1}{|q|^2} (qq^T) p = \frac{1}{|q|^2} \begin{pmatrix} q_x^2 & q_xq_y & q_xq_z \\ q_xq_y & q_y^2 & q_yq_z \\ q_xq_z & q_yq_z & q_z^2 \end{pmatrix} \begin{pmatrix} p_x \\ p_y \\ p_z \end{pmatrix}$

**Vektorový součin** (cross product): $p \times q = \begin{pmatrix} p_yq_z - p_zq_y, p_zq_x - p_xq_z, p_xq_y - p_yq_x \end{pmatrix}$ a $|p \times q| = |p||q| \sin \alpha$
Maticově $p \times q = \begin{pmatrix} 0 & -p_z & p_y \\ p_z & 0 & -p_x \\ -p_y & p_x & 0 \end{pmatrix} \begin{pmatrix} q_x \\ q_y \\ q_z \end{pmatrix}$

**Násobení matic**: Pro $A \cdot B = C$ je $c_{ij} = \sum_{k = 1}^n a_{ik}b_{kj}$ (skalární součin řádku a sloupce)

### Homogenní souřadnice

**Afinní prostor** - Definován množinou bodů a vektorů, jsou uzavřené na základní operace. Je to podprostor projektivního prostoru.
**Projektivní prostor** - Rozšíření afinního prostoru o body v nekonečnu. Pro reprezentaci bodů používáme homogenní souřadnice (pak lze i projektivní transformace vyjádřit maticí).

**Homogenní souřadnice** - Pro popis bodů v projektivním prostoru. Přidá se složka $x$, tzv. váha bodu. Tím prostor nižší dimenze zasadíme do prostoru vyšší dimenze.
Spojení transformací do jedné matice: $T \cdot R \cdot S \cdot \mathbf{v} = M \cdot \mathbf{v}$ (pořadí je důležité)
Rychlá inverze jen s rotací a translací: $M^{-1}(R, \mathbf{t}) = M(R^T, R^T \cdot -\mathbf{t})$​.

*Z kartézských na homogenní*: $[x, y, z] \rightarrow [x, y, z, 1]$
*Z homogenních na kartézské*: $[x, y, z, w] \rightarrow [\frac{x}{w}, \frac{y}{w}, \frac{z}{w}]$ (pro $w \neq 0$)

**Sčítání**: $(x_1, y_1, w_1) + (x_2, y_2, w_2) = (x_1w_2 + x_2w_1, y_1w_2 + y_2w_1, w_1w_2)$
**Násobení skalárem**: $\alpha \cdot (x, y, w) = (\alpha x, \alpha y, w)$

### Afinní a projektivní transformace v rovině a v prostoru

**Afinní transformace**: Zachovávají dimenze (z bodů na body, z přímek na přímky) a paralelizmus (i poměr délek rovnoběžných úseček zůstane stejný). Nemusí nutně zachovávat úhly a vzdálenosti bodů (poměr vzdáleností bodů na stejné úsečce se však zachová). Např. translace, rotace, škálování, zkosení, zrcadlení.
**Projektivní transformace**: Zobrazují body v souřadnicích nižší dimenze na průmětnu (rovina/přímka). Podle typu projekce se zachovávají různé vlastnosti.

**Transformační matice** reprezentuje transformaci, pak jí násobíme vektor souřadnic.
$\begin{pmatrix} x' \\ y' \\ z' \end{pmatrix} =  \begin{pmatrix} t_{11} & t_{12} & t_{13} \\ t_{21} & t_{22} & t_{23} \\ t_{31} & t_{32} & t_{33} \end{pmatrix} \cdot \begin{pmatrix} x \\ y \\ z \end{pmatrix}$

#### Afinní transformace

V homogenních souřadnicích můžeme každou afinní transformaci reprezentovat maticí, můžeme je i skládat.

**Obecná afinní transformace**: $A \cdot p = \left( \begin{array}{ccc|c} &&& \\ & M & & T \\ &&& \\ \hline 0 & 0 & 0 & 1 \end{array} \right) \cdot \begin{pmatrix} p_x \\ p_y \\ p_z \\ p_w \end{pmatrix}$

**Translace**: Ve 2D $T(t_x, t_y) = \begin{pmatrix} 1 & 0 & t_x \\ 0 & 1 & t_y \\ 0 & 0 & 1 \end{pmatrix}$, ve 3D $T(t_x, t_y, t_z) = \begin{pmatrix} 1 & 0 & 0 & t_x \\ 0 & 1 & 0 & t_y \\ 0 & 0 & 1 & t_z \\ 0 & 0 & 0 & 1 \end{pmatrix}$​.

**Rotace**: Ve 2D kolem počátku o úhel $\alpha$, ve 3D kolem kartézských os.
$R_x(\alpha) = \begin{pmatrix} 1 & 0 & 0 & 0 \\ 0 & \cos\alpha & -\sin\alpha & 0 \\ 0 & \sin\alpha & \cos\alpha & 0 \\ 0 & 0 & 0 & 1 \end{pmatrix}$, $R_y(\alpha) = \begin{pmatrix} \cos\alpha & 0& \sin\alpha & 0 \\ 0 & 1 & 0 & 0 \\ -\sin\alpha & 0 & \cos\alpha & 0 \\ 0 & 0 & 0 & 1 \end{pmatrix}$, $R_z(\alpha) = \begin{pmatrix} \cos\alpha & -\sin\alpha & 0 & 0 \\ \sin\alpha & \cos\alpha & 0 & 0 \\ 0 & 0 & 1 & 0 \\ 0 & 0 &0 & 1 \end{pmatrix}$

**Škálování**: Ve 2D $S(s_x, s_y) = \begin{pmatrix} s_x & 0 & 0 \\ 0 & s_y & 0 \\ 0 & 0 & 1 \end{pmatrix}$, ve 3D $S(s_x, s_y, s_z) = \begin{pmatrix} s_x & 0 & 0 & 0 \\ 0 & s_y & 0 & 0 \\ 0 & 0 & s_z & 0 \\ 0 & 0 & 0 & 1 \end{pmatrix}$

**Zkosení**: Ve 2D $Sh(a, b) = \begin{pmatrix} 1 & a & 0 \\ b & 1 & 0 \\ 0 & 0 & 1 \end{pmatrix}$, ve 3D $Sh(a, b, c, d, e, f) = \begin{pmatrix} 1 & a & b & 0 \\ c & 1 & d & 0 \\ e & f & 1 & 0 \\ 0 & 0 & 0 & 1 \end{pmatrix}$

**Složené transformace**: $T_3 \cdot (T_2 \cdot (T_1 \cdot p)) = (T_3 \cdot T_2 \cdot T_1) \cdot p$
*Inverzní transformace* po krocích: z $M = A \cdot B \cdot C$ máme $M^{-1} = C^{-1} \cdot B^{-1} \cdot A^{-1}$.
Pro *ortonormální matice* (např. rotace) pak $R^{-1} = R^T$.

**Transformace tuhého tělesa** - Mění orientaci a pozici, ale zachovávají tvar (skládají se pouze z translací a rotací). Patří sem i převod mezi souřadnicovými systémy.

**Transformace normálových vektorů** - Speciální matice $N = (M^{-1})^T$.

**Převod mezi souřadnicovými systémy** - $Cs(A, s, t, u) = R_z(\gamma) \cdot R_x(\beta) \cdot R_z(\alpha) \cdot T(-A)$
Pokud je počátek v nule a všechny vektory jsou jednotkové, matice $M_{stu \rightarrow xyz}$ má jednoduše ve sloupcích souřadnice vektorů systému $[s, t, u]$ v systému $[x, y, z]$.

#### Projektivní transformace

**Projekce** - Z vyšší dimenze do nižší dimenze, ale v počítačové grafice pořád nějak zachováváme i souřadnici $z$​.
Různé druhy projekce podle zachovaných vlastností a posílání promítacích paprsků.

**Rovnoběžné projekce** - Promítací paprsky jsou rovnoběžné, zachovává se rovnoběžnost.
   **Kolmé projekce** - Promítací paprsky jsou rovnoběžné a zároveň kolmé na průmětnu.
      **Mongeova projekce** - Průmětna kolmá na jednu ze tří os. Půdorys, nárys, bokorys.
      **Axonometrie** - Obecná kolmá projekce, průmětna určena vzdáleností na osách.
         *Isometrie* ($a$, $a$, $a$), *Dimetrie* ($a$, $a$, $b$), *Trimetrie* ($a$, $b$, $c$)
   **Kosoúhlé projekce** - Ne kolmé, může dojít ke zkrácení měřítek některých os.
      **Kabinetní projekce** - Průmětna $xy$, měřítko $z$ na $1/2$, pod úhlem ($30°$ nebo $45°$)
      **Kavalírní projekce** - Stejné měřítko na všech osách, pod úhlem ($30°$ nebo $45°$​​)
**Perspektivní (středové) projekce** - Promítací paprsky tvoří svazek procházející středem projekce, nezachovávají se rovnoběžky.
   **Jednobodová** - Průmětna rovnoběžná se 2 osami, jeden hlavní úběžník.
   **Dvoubodová** - Průmětna rovnoběžná s 1 osou, dva hlavní úběžníky.
   **Tříbodová** - Průmětná má zcela obecnou polohu, tři hlavní úběžníky.

**Projekční matice**: Používá se k přepočtu souřadnic do projekce, např. ze 3D do 2D pro zobrazení objektu na monitoru.

**Obecná kolmá projekce** (*axonometrie*): Pozice pozorovatele $S$ (tj. bod $[0, 0]$ průmětny), normálový vektor průmětny $N$ (opačný ke směru pohledu, osa $z$), směr nahoru $u$ (osa $y$). Pak je vektor doprava $u \times N$ (osa $x$). Kolmá projekce je pak převedení souřadnic $Cs(S, u \times N, u, N)$.

**Kosoúhlá projekce**: Průmětna $xy$, koeficient zkrácení $K$, úhel průmětu osy $z$ je $\alpha$.
$\begin{pmatrix} 1 & 0 & K \cdot \cos\alpha & 0 \\ 0 & 1 & K \cdot \sin\alpha & 0 \\ 0 & 0 & 1 & 0 \\ 0 & 0 & 0 & 1 \end{pmatrix}$

**Obecná středová projekce**: Máme střed projekce $S$, normálový vektor průmětny $N$, vzdálenost průmětny od středu projekce $d$ a svislý vektor $u$. Pak přesuneme střed projekce do počátku, srovnáme $N$ do osy $z$, tedy $Cs(S, u \times N, u, N)$ a pak provedeme perspektivní projekci. $\begin{pmatrix} 1 & 0 & 0 & 0 \\ 0 & 1 & 0 & 0 \\ 0 & 0 & 1 & 0 \\ 0 & 0 & 1/d & 0 \end{pmatrix} \cdot \begin{pmatrix} x \\ y \\ z \\ 1 \end{pmatrix} = \begin{pmatrix} x \\ y \\ z \\ \frac{z}{d} \end{pmatrix}$ $\Rightarrow$ $\begin{pmatrix} x \frac{d}{z} \\ y \frac{d}{z} \\ d \end{pmatrix}$

### Kvaterniony

**Eulerovy úhly** - $E(h, p, r) = R_y(h) \cdot R_x(p) \cdot R_z(r)$, tj. yaw/head, pitch, roll). Špatně se interpoluje a může nastat gimbal lock.
**Gimbal lock** - Otočí se tak, že se dvě osy ztotožní. Přijdeme o jeden DoF.

**Kvaternion**: $q = (\mathbf{v}, w) = ix + jy + kz + w$, kde $\mathbf{v} = (x, y, z) = ix + jy + kz$ a $w \in \mathbb{R}$
Těleso, zobecnění komplexních čísel ve 4D, pro zápis rotace a orientace v prostoru.
Platí $i^2 = j^2 = k^2 = ijk = -1$, tedy také $jk = -kj = i$, $ki = -ik = j$ a $ij = -ji = k$.

**Sčítání**: $(\mathbf{v_1}, w_1) + (\mathbf{v_2}, w_2) = (\mathbf{v_1 + v_2}, w_1 + w_2)$
**Násobení**: $\mathbf{qr} = (\mathbf{v_q}, w_q)(\mathbf{v_r}, w_r) = (\mathbf{v_q \times v_r} + w_r \mathbf{v_q} + w_q \mathbf{v_r}, w_q w_r - \mathbf{v_q \cdot v_r})$, není komutativní
Kvaterniony reprezentují rotace, vynásobením získáme složenou rotaci.
**Násobení skalárem**: $s\mathbf{q} = (0, s)(\mathbf{v}, w) = (s\mathbf{v}, sw)$
**Sdružený kvaternion**: $(\mathbf{v}, w)^* = (\mathbf{-v}, w)$
**Unit**: $id = (\mathbf{0}, 1)$, reprezentuje identitu (je to také jednotkový kvaternion)
**Norma kvaternionu** (squared absolute value): $|\mathbf{q}|^2 = n(\mathbf{q}) = \mathbf{qq^*} = x^2 + y^2 + z^2 + w^2$
Tedy $|\mathbf{q}| = \sqrt{\mathbf{qq^*}}$.
**Inverzní kvaternion**: $\mathbf{q^{-1}} = \frac{\mathbf{q^*}}{n(\mathbf{q})} = \frac{\mathbf{q^*}}{|\mathbf{q}|^2}$ (dělí se po složkách)

**Jednotkový kvaternion** (*unit quaternion*): $n(q) = 1$, tj. $x^2 + y^2 + z^2 + w^2 = 1$.
**Goniometrická podoba**: $\mathbf{q} = (\mathbf{u_q} \sin\theta, \cos\theta)$, tj. rotace o úhel $\theta$ kolem osy $\mathbf{u_q}$, přičemž $\mathbf{q}$ a $-\mathbf{q}$ jsou ta samá rotace (osa rotace otočená o 180°). Identita je nulová rotace.

**Logaritmus**: $\log \mathbf{q} = \theta \mathbf{u_q}$​, protože $\mathbf{q} = \mathbf{u_q} \sin \theta + \cos\theta = \exp(\theta \mathbf{u_q})$
**Mocnina**: $\mathbf{q^t} = (\mathbf{u_q}\sin\theta + \cos\theta)^t = \exp(t \theta \mathbf{u_q}) = \mathbf{u_q} \sin t \theta + \cos t \theta$

**Rotace**: $\mathbf{p' = q p q^{-1} = q p q^*}$, kde $\mathbf{q} = (\mathbf{u}_q \sin \theta, \cos \theta)$ (jednotkový kvaternion), je otočení vektoru/bodu $\mathbf{p}$ (reprezentujeme ho jako kvaternion s nulovou skalární složkou) kolem osy $\mathbf{u}_q$ o úhel $2\theta$.

**LERP**: $\mathbf{q}_t = \frac{(1-t)\mathbf{q}_0 + t \mathbf{q}_1}{|(1-t)\mathbf{q}_0 + t \mathbf{q}_1|}$ (normalizujeme, protože se nezachovává délka vektoru)
**SLERP** (*spherical linear interpolation*): $slerp(\mathbf{q},\mathbf{r},t) = \mathbf{q}(\mathbf{q}^*\mathbf{r})^t$
Musí platit $\mathbf{q} \cdot \mathbf{r} \geq 0$, jinak vezmeme $\mathbf{-q}$. Interpoluje po povrchu koule. Po rozepsání mocniny a sdruženého kvaternionu $slerp(\mathbf{q},\mathbf{r},t) = \frac{\sin(\theta(1-t))}{\sin\theta} \cdot \mathbf{q} + \frac{\sin (\theta t)}{\sin\theta} \cdot \mathbf{r}$, ale $\theta$ musíme spočítat z $\cos\theta = q_x r_x + q_y r_y + q_z r_z + q_w r_w$.

**Rotace mezi dvěma vektory**: Mapování $\mathbf{s}$ na $\mathbf{t}$. Výsledný kvaternion je  $\mathbf{q} = (\mathbf{u} \cdot \sin\frac{\theta}{2}, \cos\frac{\theta}{2})$, kde $\theta$ je úhel mezi $\mathbf{s}$ a $\mathbf{t}$, ale upravíme. Normalizujeme vektory, pak osa rotace je $\mathbf{u} = \frac{\mathbf{s} \times \mathbf{t}}{|\mathbf{s} \times \mathbf{t}|}$, dále využijeme vztahy  $\sin \frac{\theta}{2} = \sqrt{\frac{1 - \cos \theta}{2}}$, $\cos \frac{\theta}{2} = \sqrt{\frac{1 + \cos \theta}{2}}$ a $\sin\theta = 2 \sin\frac{\theta}{2} \cos\frac{\theta}{2}$, nakonec $\cos \theta = \mathbf{s} \cdot \mathbf{t}$ a $\sin \theta = |\mathbf{s} \times \mathbf{t}|$. Dostaneme $\mathbf{q} = (\mathbf{q_v}, q_w) = (\frac{\mathbf{s} \times \mathbf{t}}{\sqrt{2(1+\mathbf{s} \cdot \mathbf{t})}}, \frac{\sqrt{2(1+\mathbf{s} \cdot \mathbf{t})}}{2})$ (není třeba explicitně počítat úhel). Oproti Slerp máme jen směry, nezajímá nás rotace objektu kolem vlastní osy.

**Nevýhoda**: Transformace bodu/vektoru je neefektivní. Obvykle kvaternionem vyjádříme složité transformace a změny orientace (skládání, interpolace), pak převedeme na matici a s tou počítáme.

### (Prostory souřadnic)

Při vykreslování scény se pomocí transformací přesouváme mezi několika prostory souřadnic.

**Object space** - Lokální souřadnice objektu/modelu.
*Model transform* - Dostaneme souřadnice ve world space (umístění do globálního systému).
**World space** - Pozice a orientace objektu ve scéně, relativní ke globálnímu počátku.
*View transform* - Dostaneme souřadnice relativní k pozici a orientaci kamery.
**Camera space** - Prostor relativní k pozici a orientaci kamery/pozorovatele, dívá se ve směru $-z$.
*Projection* - Matice perspektivní projekce do homogenního prostoru. Definuje viditelné frustum podle optiky kamery.
**Clip space** - Clip volume (původně frustum) je box od $(-w, -w, -w)$ do $(w, w, w)$, kde $w$ je čtvrtá souřadnice vrcholu (máme homogenní souřadnice). Vrcholy uvnitř jsou vykresleny, mimo zahozeny.
*Perspective divide* (perspektivní dělení) - Každou komponentu vydělíme $w$, převedeme na kartézské.
**Normalized device space** (NDS) - Krychle od $(-1, -1, -1)$ do $(1, 1, 1)$, hloubka $z$ normalizovaná z původního rozsahu od near do far. Vertex shader by měl vracet data v tomto prostoru.
*Viewport transform* - Lineární transformace, jen převádí souřadnice pixelů.
**Window space** (také viewport) - 2D souřadnice pixelu na obrazovce (také ve framebufferu), ale navíc $z$.

`out.position = projectionMatrix * viewMatrix * modelMatrix * inputModelSpacePosition`
Transformace ve vertex shaderu, do clip space. Můžeme složit do jedné matice, tzv. **MVP**.

**LookAt transformace kamery** - Pozici kamery $e$, chce se dívat na pak bod $p$. Sestavíme ortonormální bázi $\mathbf{f} = norm(\mathbf{e}-\mathbf{p})$,  $\mathbf{r} = norm((0, 1, 0, 0) \times \mathbf{f})$ a $\mathbf{u} = norm(\mathbf{f} \times \mathbf{r})$. Transformace pro přesun kamery do její pozice a orientace:
$\mathbf{TR} = \begin{pmatrix} 1 & 0 & 0 & e_x \\ 0 & 1 & 0 & e_y \\ 0 & 0 & 1 & e_z \\ 0 & 0 & 0 & 1 \end{pmatrix} \begin{pmatrix} r_x & u_x & f_x & 0 \\ r_y & u_y & f_y & 0 \\ r_z & u_z & f_z & 0 \\ 0 & 0 & 0 & 1 \end{pmatrix}$

**View transformace** - Transformuje svět relativně k pozici kamery:
$(\mathbf{TR})^{-1} = \mathbf{R}^T\mathbf{T}^{-1} = \begin{pmatrix} r_x & r_y & r_z & 0 \\ u_x & u_y & u_z & 0 \\ f_x & f_y & f_z & 0 \\ 0 & 0 & 0 & 1 \end{pmatrix} \begin{pmatrix} 1 & 0 & 0 & -e_x \\ 0 & 1 & 0 & -e_y \\ 0 & 0 & 1 & -e_z \\ 0 & 0 & 0 & 1 \end{pmatrix} = \begin{pmatrix} r_x & r_y & r_z & -(r \cdot e_x) \\ u_x & u_y & u_z & -(u \cdot e_y) \\ f_x & f_y & f_z & -(f \cdot e_z) \\ 0 & 0 & 0 & 1 \end{pmatrix}$

**Perspektivní projekce** (v OpenGL) - Středová projekce, průmětna rovnoběžná s $xy$. Převádí souřadnice mezi systémy (z viewing frustum do kvádru se středem v počátku) a škáluje body do $[-1, 1]$ na všech osách. Mapuje hloubku $z$ do NDS nelineárně. Pro symetrické viewing frustum lze zjednodušit.
$\mathbf{P}_{frustum} = \begin{pmatrix} \frac{2n}{r-l} & 0 & \frac{r+l}{r-l} & 0 \\ 0 & \frac{2n}{t-b} & \frac{t+b}{t-b} & 0 \\ 0 & 0 & -\frac{f+n}{f-n} & -\frac{2nf}{f-n} \\ 0 & 0 & -1 & 0 \end{pmatrix}$, $\mathbf{P}_{frustum} = \begin{pmatrix} \frac{n}{r} & 0 & 0 & 0 \\ 0 & \frac{n}{t} & 0 & 0 \\ 0 & 0 & -\frac{f+n}{f-n} & -\frac{2nf}{f-n} \\ 0 & 0 & -1 & 0 \end{pmatrix}$​

**Pespektivně-korektní interpolace** (hyperbolická) - Na obrazovce nelze lineárně interpolovat texturové souřadnice, ani hloubku. Lze ale $u/z$ a $v/z$ (pak $u$ a $v$ získáme vynásobením $z$) a stejně tak $1/z$​ (reciprocal depth, pak vezmeme převrácenou hodnotu).

## 2. Křivky a spliny

**Aproximace** (např. *B-spline*) - Výsledná křivka body nemusí procházet.
**Interpolace** (např. *Catmull-Rom spline*) - Výsledná křivka body prochází.

**Explicitní zápis** jako graf funkce $y = f(x)$, např. $y = \sin x$, $z = x^2 + y^2$.
**Implicitní rovnice** jako pravidlo, např. $x^2 + y^2 = 25$, $0 = x^2 + y^2 + z^2$.
**Parametrická rovnice** jako vektorová funkce $f(t) = [x(t), y(t)]$, z parametru dostaneme vektor souřadnic křivky. Umožňuje křivky/funkce zanořovat a skládat (neměníme funkci, jen parametr zvenčí).
Např. přímka $P(t) = A + t \cdot (B - A)$, kružnice $P(t) = [\cos t, \sin t]$​.

**Normalized transformace** - Speciální případ, kdy $P(0) = 0$ a $P(1) = 1$. Zajímavé je to uprostřed.

**Interpolace** je pak průměrování.
*Lineární interpolace*: $\mathrm{Lerp}(A, B, t) = (1 - t) \cdot A + t \cdot B$
*Nelineární interpolace*: Jdeme z $A$ fo $B$, ale ne přímo. Křivky, spliny, tweening/easing.

**Polynomiální křivky**: $f(t) = a_0 + a_1 t + a_2 t^2 + ... + a_n t^n$, kde $a_i$ je vektor koeficientů. Např. funkci $x(t) = 6t - 9 t^2 + 4 t^3$, $y(t) = 4 t^3 - 3 t^2$ lze zapsat jako $f(t) = \begin{pmatrix} x(t) \\ y(t) \end{pmatrix} = \begin{pmatrix} 6 \\ 0 \end{pmatrix} t + \begin{pmatrix} -9 \\ -3 \end{pmatrix} t^2 + \begin{pmatrix} 4 \\ 4 \end{pmatrix} t^3$.

**Geometrická spojitost** - Vizuální spojitost a spojitost tečen. Zaručuje pouze lineární závislost tečných vektorů, ne i stejnou velikost. $G^0$ je poziční spojitost (napojení křivek ve stejném bodě), $G^1$ je tangenciální spojitost (křivka je $G^0$, tečné vektory jsou lineárně závislé), $G^2$ je křivková spojitost (křivka je $G^1$, druhé derivace jsou stejné).
**Analytická spojitost** (*parametrická*) - Spojitost rychlosti pohybu, totožné tečné vektory. Analytická spojitost je superiorní geometrické. $C^0$ (nemění se skokem poloha), $C^1$ (křivka je $C^0$, nemění se skokem směr), $C^2$ (křivka je $C^1$, nemění se skokem rychlost).

**Free-form křivky**: $P(t) = \sum_{i = 0}^{N -1} w_i(t) B_i$, kde $B_i$ jsou kontrolní body, $w_i$ jsou váhy bodů, $t$ je čas.
**Cauchyova podmínka**: $\sum_{i=0}^{N-1} w_i(t) = 1$. Místo transformace celé křivky stačí transformace bodů a nový výpočet křivky.

### Spline funkce

**Spline funkce** řádu $n$ je po částech polynomiální (max. řádu $n$) funkce, která je v bodech spojení maximálně možně spojitá, tj. $C^{n-1}$​.

**Globální parametr** $u \in [u_0, u_N]$, rozdělíme na $N$ podintervalů, pro každý definujeme polynom $P_i$. Pak $S(u) = P_i(u)$, kde $u_i \leq u \lt u_{i+1}$.
**Lokální parametr** $t \in [0, 1]$ zvlášť pro každý polynom.
**Uniform spline** má podintervaly stejně veliké, pak $t_i = (u - u_i) / (u_{i+1} - u_i)$.

Spíše se ve splinu používají kubické křivky (Bézierovy nebo Hermitovy (místo kontrolních bodů rychlosti)) než kvadratické, lépe se s nimi manipuluje.

### Interpolace kubickými spliny

**Interpolace**: Máme zadaných několik bodů $x_0, x_1, ..., x_n$ a pro ně předepsané hodnoty funkce $y_0, y_1, ..., y_n$. Body proložíme spline.

**Interpolace kubickým splinem**: Spline bude složený z kubických polynomů, budou se napojovat právě v zadaných bodech. Pak $S(x) = P_k(x) = p_{k,0} + p_{k,1}(x - x_k) + p_{k,2} (x - x_k)^2 + p_{k,3} (x - x_k)^3$ pro $x \in [x_k, x_{k+1}]$ a $k = 0, 1, ... n-1$. Pomocí $S(x)$ tedy počítáme hodnotu v bodě $x$.

Klademe několik podmínek, pak lze získat jednoznačné koeficienty $p_{k,i}$. Hodnoty splinu ve spojovacích bodech jsou ty zadané, tj. $S(x_k) = y_k$. Platí $C^0$ spojitost, tj. $P_k(x_{k+1}) = P_{k+1}(x_{k+1})$. Platí $C^1$ spojitost, tj. $P_k'(x_{k+1}) = P_{k+1}'(x_{k+1})$. Stejně tak $C^2$ spojitost, tj. $P_k''(x_{k+1}) = P_{k+1}''(x_{k+1})$.

**Natural cubic spline** má navíc podmínku $P''(x_0) = P''(x_n) = 0$ (druhé derivace v počátečním bodu splinu a v koncovém bodu splinu jsou $0$). 

### (Příprava na další)

Pro kubické polynomy $\mathbf{P}(t) = \mathbf{a}t^3 + \mathbf{b}t^2 + \mathbf{c}t + \mathbf{d}$ řešíme soustavu rovnic.
$\mathbf{P}(t) = \mathbf{T} \mathbf{C} = \begin{pmatrix} t^3 & t^2 & t & 1 \end{pmatrix} \begin{pmatrix} a_x & a_y & a_z \\ b_x & b_y & b_z \\ c_x & c_y & c_z \\ d_x & d_y & d_z \end{pmatrix}$, kde $\mathbf{C}$ je matice koeficientů (konstant).

**Derivaci** spočteme derivací vektoru $\mathbf{T}$, tj. $\mathbf{P}'(t) = \frac{d}{dt}\mathbf{T}(t)\cdot \mathbf{C} = \begin{pmatrix} 3t^2 & 2t & 1 & 0 \end{pmatrix} \cdot \mathbf{C}$.

**Geometrická omezení** - Křivky můžeme definovat jako vážený součet geometrických podmínek (body, tečné vektory), např. $\mathbf{Q}(t) = \sum_{k = 0}^3 B_k(t) \mathbf{g_k}$, kde $B_k$ jsou blending funkce (třeba kubický polynom s $t$). V maticovém vyjádření oddělíme zvlášť, tj. $\mathbf{P}(t) = \mathbf{T}(t) \mathbf{C} = \mathbf{T}(t) \mathbf{M} \mathbf{G}$, např.
$\mathbf{P}(t) = \begin{pmatrix} t^3 & t^2 & t & 1 \end{pmatrix} \cdot \begin{pmatrix} m_{11} & m_{12} & m_{13} & m_{14} \\ m_{21} & m_{22} & m_{23} & m_{24} \\ m_{31} & m_{32} & m_{33} & m_{34} \\ m_{41} & m_{42} & m_{43} & m_{44} \end{pmatrix} \cdot \begin{pmatrix} G_1 \\ G_2 \\ G_3 \\ G_4 \end{pmatrix}$
*Konkrétní případ*: Kdybychom nastavili $m_{41} = 1$, $m_{31} = -1$, $m_{32} = 1$ a zbytek $0$, pak po vynásobení dostaneme $\mathbf{P}(t) = (1-t) G_1 + t G_2$, což je rovnice přímky.

**Fergusonova/Hermitovská křivka**: Nejjednodušší křivka popsaná způsobem výš, popisují se koncové body a tečné vektory (tj. rychlosti v bodech):
$\mathbf{F}(t) = \begin{pmatrix} t^3 & t^2 & t & 1 \end{pmatrix} \cdot \begin{pmatrix} 2 & -2 & 1 & 1 \\ -3 & 3 & -2 & -1 \\ 0 & 0 & 1 & 0 \\ 1 & 0 & 0 & 0 \end{pmatrix} \cdot \begin{pmatrix} P_0 \\ P_1 \\ T_0 \\ T_1 \end{pmatrix}$
Lze je snadno převést na Bézierovy křivky a zpět.

### Bézierovy křivky

**Bézierova křivka** $n$-tého stupně je $\mathbf{f}(t) = \sum_{k=0}^n B_{n, k}(t) \mathbf{P_k}$, kde $B_{n,k}(t) = \begin{pmatrix} n \\ k \end{pmatrix} t^k (1-t)^{n-k}$ je $k$-tý Bernsteinův polynom $n$-tého stupně. Má $n+1$ řídících bodů (krajními prochází) a jako blending funkci pro geometrická omezení používá Bernsteinovy polynomy.
**Bernsteinova báze** pro řád $n$ jsou všechny polynomy $B_{n, k}$ pro $k = 0, 1, ..., n$. Navíc platí $\sum_{k = 0}^n B_{n, k}(t) = 1$ pro $t \in [0, 1]$ a báze je symetrická. Bézierova křivka je pak lineární kombinací Bernsteinových bázových polynomů.

**Lineární Bézierova křivka**: $\mathrm{Lerp}(A, B, t) = (1-t)\cdot A + t \cdot B$.
**Kvadratická Bézierova křivka**: $f(t) = P_0 (1-t)^2 + P_1 2 t (1-t) + P_2 t^2$. Prostřední bod jen ovlivňuje její tvar.
**Kubické Bézierovy křivky**: $\mathbf{f}(t) = \sum_{k = 0}^3 B_{3,k}(t) \mathbf{P_k}$. Často se používají v praxi.
$\mathbf{T} \mathbf{M} \mathbf{G} = \begin{pmatrix} t^3 & t^2 & t & 1 \end{pmatrix} \cdot \begin{pmatrix} -1 & 3 & -3 & 1 \\ 3 & -6 & 3 & 0 \\ -3 & 3 & 0 & 0 \\ 1 & 0 & 0 & 0 \end{pmatrix} \cdot \begin{pmatrix} P_0 \\ P_1 \\ P_2 \\ P_3 \end{pmatrix}$, kde $M$ je bázová matice

**Vlastnosti**:
*Interpolace koncových bodů*: Křivka prochází koncovými body, tedy platí $f(0) = P_0$ a $f(1) = P_n$.
*Tečny v koncových bodech*: První a poslední segment kontrolního mnohoúhelníku je tečnou Bézierovy křivky, tedy $f'(0) = n \cdot (P_1 - P_0)$ a $f'(1) = n \cdot (P_n - P_{n-1})$ (kde $n$ je konstanta).
*Konvexní obal*: Pro $0 \leq t \leq 1$ je křivka celá uvnitř konvexního obalu kontrolního mnohoúhelníku.
*Afinní invariance*: Platí Cauchyho podmínka, Bézierova křivka je tedy invariantní vůči lineární transformaci.
*Globální vliv bodů*: Vliv kontrolního bodu na křivku je globální. Pokud se tedy posune jeden bod, změní se celá křivka. Proto se v praxi nejčastěji používají spliny.
*Bézierovy podkřivky*: Bézierova křivka může být v libovolném bodě rozdělena na dvě (nebo libovolně mnoho) podkřivky, přičemž každá z nich je opět Bézierova křivka.
*Křivky vyšších stupňů*: Každá křivka stupně $n$ je zároveň křivkou stupně $m$ pro libovolné $m > n$​ a křivky vyššího stupně lze převést na libovolný nižší jen posunutím kontrolních bodů.

**Midpoint control** - Jiný způsob kontroly, pro kvadratické Bézierovy křivky určujeme přímo prostřední bod křivky (v čase $t = 0.5$).

### Catmull-Rom spliny

**Kochanek-Bratels křivky**: Vychází z Hermitovských křivek, ale tečné vektory se dopočítají automaticky z parametrů bodů (tension, continuity, bias). Křivka začíná ve druhém a končí v předposledním bodě. Také tzv. TCB křivka.
$L_i = \frac{(1-t)(1-c)(1+b)}{2} \cdot (P_i - P_{i-1}) + \frac{(1-t)(1+c)(1-b)}{2} \cdot (P_{i+1} - P_i)$
$R_i = \frac{(1-t)(1+c)(1+b)}{2} \cdot (P_i - P_{i-1}) + \frac{(1-t)(1-c)(1-b)}{2} \cdot (P_{i+1} - P_i)$

**Catmull-Rom spline** - Speciální případ TCB křivky s $t_i = c_i = b_i = 0$.Rovnice tečného vektoru je pak $T_i = \frac{1}{2} \cdot (P_{i+1} - P_{i-1})$. Také speciální případ kubického Hermitova splinu se speciálními hodnotami zvolenými jako rychlost na začátku a na konci každé části.
$\mathbf{M} \mathbf{G} = \frac{1}{2} \begin{pmatrix} -1 & 3 & -3 & 1 \\ 2 & -5 & 4 & -1 \\ -1 & 0 & 1 & 0 \\ 0 & 2 & 0 & 0 \end{pmatrix} \cdot \begin{pmatrix} P_{i-1} \\ P_i \\ P_{i+1} \\ P_{i+2} \end{pmatrix}$
Také technika/algoritmus pro vytvoření splinu. Máme posloupnost bodů, spočteme tečny. Sestavujeme Hermitův spline. Rychlosti na začátku a na konci jsou $0$, v $N$-tém bodě pak $V_N = \frac{P_{N+1} - P_{N-1}}{2}$.

### B-spline

**B-spline** (basis spline) - Aproximační křivka, kontrolní body určují tvar (pomocí blending funkcí, bází). Dodržuje vlastnost lokálnosti (změna jednoho kontrolního bodu vyvolá jen lokální změnu).

**Coonsův B-spline** - Složen z kubických křivek, sousední křivky vždy sdílí 3 kontrolní body. Změna jednoho bodu vyvolá změnu ve 4 nejbližších segmentech. Je $C^2$ spojitý. Křivka nezačíná v počátečním kontrolním bodu, ale v tzv. antitěžišti (tj. $1/3$ těžnice trojúhelníku $P_0$, $P_1$, $P_2$, která začíná v bodě $P_1$; oproti tomu těžiště je ve $2/3$). Spočítá se $Q(0) = \frac{P_0 + 4 \cdot P_1 + P_2}{6}$. Každý segment je určen 4 body.
$\mathbf{M} \mathbf{G} = \frac{1}{6} \cdot \begin{pmatrix} -1 & 3 & -3 & 1 \\ 3 & -6 & 3 & 0 \\ -3 & 0 & 3 & 0 \\ 1 & 4 & 1 & 0 \end{pmatrix} \cdot \begin{pmatrix} P_{i-1} \\ P_i \\ P_{i+1} \\ P_{i+2} \end{pmatrix}$

### ~~De Casteljau a de Boor algoritmus~~

**Algoritmus de Casteljau** (také *algoritmus de Boor*) - Výpočet bodu Bézierovy křivky v konkrétním čase pomocí opakované lineární interpolace. Nemusí být nutně efektivnější než přímý výpočet ze vzorečku Bézierovy křivky, ale umožňuje přesnější výpočet (bez velkých mocnin malých čísel).
$\beta_i^{(0)} := \beta_i$, pro $i = 0, ..., n$
$\beta_i^{(j)} := \beta_i^{(j-1)}(1 - t_0) + \beta_{i+1}^{(j-1)} t_0$, pro $i = 0, ..., n-j$ a $j = 1, ..., n$

Pomocí toho samého algoritmu můžeme Bézierovu křivku snadno rozdělit na dvě podkřivky stejného stupně v libovolném bodě $t$. Funguje to pro Bézierovy i Hermitovy křivky.

## 3. Obrázky a textury

### ~~2D Fourierova transformace a konvoluce~~

Každý **signál** není reprezentovaný jen jednou frekvencí, ale celým **spektrem frekvencí**, ze kterého se pak *skládá výsledný signál*.

**Fourierova transformace** - Transformace signálu z domény času do domény frekvencí. Pro periodický signál vrátí koeficienty k sin a cos vlnám. Spojitá a diskrétní.
**Diskrétní Fourierova transformace** $X_k = \sum_{n = 0}^{N-1} x_n \cdot e^{-i 2 \pi k n / N}$, kde $k$ je frekvence. Používá se Eulerova formule $e^{i \pi} = -1$ pro vzorkování na kruhu ($e^{ix} = \cos x + i \cdot \sin x$).
**Inverzní transformace** $x_n = \frac{1}{N} \sum_{k = 0}^{N - 1} X_k \cdot e^{i 2 \pi k n / N}$ (rekonstrukce signálu).
**2D Fourierova transformace** $F(x, y) = \frac{1}{MN} \sum_{m=0}^{M-1} \sum_{n=0}^{N-1} f(m, n) \cdot e^{-j 2 \pi (x \frac{m}{M} + y \frac{n}{N})}$
**Inverzní 2D transformace** $f(m, n) = \sum_{x=0}^{M-1} \sum_{y=0}^{N-1} F(x, y) \cdot e^{j 2 \pi (x \frac{m}{M} + y \frac{n}{N})}$

**Fourierova věta**: Pokud je funkce spojitá, dostatečně hladká (derivace vysokých řádů) a periodická, pak může být vyjádřena jako nekonečná řada sinů a kosinů (součet sinů a cosinů, každá komponenta má svou amplitudu a frekvenci).

----

**Konvoluce** je skládání dvou funkcí jako vážený klouzavý průměr. Je komutativní ($f * g = g * f$), *asociativní* ($f * (g * h) = (f * g) * h$), *distributivní* ($f*(g+h) = f*g+f*h$) a asociativní při násobení skalárem ($a(f*g) = (af)*g = f*(ag)$).
**Spojitá konvoluce** $(f * g)(x) := \int_{-\infty}^{\infty} f(t) \cdot g(x-t) \, d t$, kde $g$ je konvoluční jádro (váhová funkce)
**Diskrétní konvoluce** $(f * g)[n] = \sum_{m = -\infty}^{\infty} f[m]g[n-m]$ (vážený klouzavý průměr na posloupnosti)
**2D diskrétní konvoluce** $(f*g)[m, n] = \sum_{i=-\infty}^{^\infty} \sum_{j=-\infty}^{\infty} f[i, j] \cdot g[m-i, n-j]$

### Vzorkování a kvantování obrazu

**Vzorkování** (*sampling*) - Převádíme spojitý definiční obor funkce obrazu na diskrétní. Zaznamenáváme hodnoty v diskrétních krocích (perioda, vzorkovací frekvence).
**Kvantizace** - Převádíme spojitý obor hodnot na diskrétní. Rozsah pokryjeme konečným počtem intervalů, volíme reprezentanty (zaokrouhlíme).

**Obrazová funkce** $f(x, y): R^2 \rightarrow \text{"barva"}$. Spojitá funkce se spojitým oborem hodnot.
**Digitální funkce** mapuje diskrétní prostor na diskrétní.
**Digitalizace obrazu** - Vzorkování a kvantizace (dvojí diskretizace). Přináší artefakty a aliasing.

**Nyquist-Shannon theorem** - Přesná rekonstrukce spojitého, frekvenčně omezeného, analogového signálu z jeho vzorků je možná právě tehdy, pokud byla vzorkovací frekvence alespoň dvojnásobkem maximální frekvence ve vstupním signálu, tedy $F_S \geq 2 F_{MAX}$.
**Nyquistova hodnota** (také *Nyquistův limit*) - Frekvence, kterou potřebujeme pro rekonstrukci celého obrazu (dle Shannonova teorému). Alespoň tak, aby $\Delta x \leq \frac{1}{2F_u}$ a $\Delta y = \frac{1}{2F_v}$.

**Podvzorkování**: Aliasing (ztráta vysoko frekvenční informace), Moiré efekt (falešné nízké frekvence).

### Anti-aliasing

**Aliasing** - Způsobený nedostatečným vzorkováním v diskrétním prostředí. Přicházíme o informace, mohou vznikat falešné informace.
*Prostorový aliasing* (obrázky), *časový aliasing* (video)
**Příklady**: *Jaggies* (zuabté hrany), *crawling* (jaggies se hýbou při pohybu kamery), *pixel popping*, *pixel flickering*, *unconnected lines*, *Moiré efekt* (vzor se rozbíhá)

**Základní prevence**: Zvýšit vzorkovací frekvenci, předzpracovat signál (low-pass), zakrýt artefakty jinými.

**Anti-aliasing** - Intenzita pixelu dle pokrytí jeho plochy.
**Anti-aliasing na GPU** - Rozlišení framebufferu definuje vzorkovací frekvenci, obvykle nižší než vyžadovaná. Řeší se "rozmazáním".

*Zvýšení sample rate*:
**SSAA** (Super-sampling) - Renderujeme ve vyšším rozlišení, downsamplujeme (průměr). Dobré výsledky, ale pomalé, velká spotřeba paměti.
**MSAA** (Multi-sampling) - Depth a stencil s vyšší frekvencí pro určení coverage mask, dle toho barva jednou pro pixel (framebuffer je také větší, ale barva se kopíruje). Multisample buffer. Podpora v HW.
**EQAA/CSAA** - (Enhanced quality, Coverage sample) - Stejný počet color/depth/stencil, ale 4x větší rozlišení coverage sampling (poměrně levný test), nakonec blending dle pokrytí.

*Rozmazat hrany/kontrasty (post-processing)*:
**MLAA** (Morphological) - Pattern matching (Z/U/L) pro nalezení hran/kontrastů, blend barev kolem hran (rozmazání podél gradientu). Rozmazává ale i textury.
**FXAA** (Fast approximate) - Ve screen space detekuje hrany a rozmazává, ale nezmírňuje rozmazání pattern matchingem. Velmi levný, ale hodně rozmazává.
**SMAA** (Enhanced subpixel morphological) - Jako MLAA, ale víc vzorků pro pattern recognition, používá pro detekci i barvy, nejen jas. Dá se kombinovat s MSAA/SSAA. Poměrně levný, tolik nerozmazává.

*Další*:
**TXAA** (Temporal) - Průměruje několik posledních framů, používá downsampling, může pracovat i s hranami. Horší výkon, vše rozmazává.

**Filtrování textur** - Lze také považovat za techniky pro anti-aliasing.

### Textury

**Textury** - 1D, 2D, 3D pole. Modulace veličin na povrchu těles. Jako zdroj nebo render target.
**UV mapping** - Mapování mezi body v textuře a vrcholy objektu (interpolují se do fragmentů).
**Multitexturing** - Více textur pro jeden fragment, kombinujeme.

**Baking** Drahé a komplexní detaily se renderují do povrchové textury.

**Diffuse map** - Albedo, diffuse barva povrchu.
**Heightmap** - Výška bodů nad "zemí".
**Gloss/specular mapping** (glossy reflection) - Jak moc je povrch lesklý na kterém místě.
**Light mapping** - Předpočítané globální osvětlení, jen pro statické scény.
**Shadow mapping** - Předpočítané lokace stínů, jen pro statické scény.
**Ambient occlusion map** - Jak moc je který bod vystaven ambientnímu světlu.
**Bump mapping** - Pouze heightmap, normálu spočteme z rozdílu výšky.
**Normal mapping** - Normálové vektory v tangent space, pro výpočet osvětlení. Nemění se ale silueta.
**Parallax mapping** - Posunutí souřadnic v textuře dle úhlu pohledu a depth/height map.
*Základní varianta* - Jdeme směrem k pozorovateli o výšku v bodě.
*Steep parallax mapping* - Rozsah hloubky se rozdělí na intervaly, jdeme ve směru pohledu po intervalech, do prvního pod.
*Parallax occlusion mapping* - Ne první vrstva po kolizi, ale lineární interpolace mezi vrstvami.
**Displacement mapping** - Posunutí vrcholů dle displacement map až během renderování.
**Environment mapping** - Okolní prostředí, lze i v několika místech nebo za real-time. Z vektoru odrazu vezmeme texel. Mip-maps pro hrubší povrchy. Latitude-longitude, sphere mapping, cube mapping.
**Decals** - Poloprůhledný obtisk aplikovaný na povrch objektu.

**MIP-mapping** - Texture LOD, uložíme v různých rozlišeních (obvykle poloviční v obou směrech). Při použití volíme level dle derivací souřadnic (změna u/v vzhledem ke změně x/y). HW podpora.
**RIP-mapping** - Anisotropické úrovně, i poloviční rozlišení jen v jedné ose.
**Summary tables** - Předpočítané součty levého horního obdélníku. Čtyři čtení textury pro součet libovolné oblasti, pro průměr.

**Texture filtering** - Jak se spočtou vzorky z hodnot okolních texelů (interpolace). Lze adaptivně.
**Nearest-neighbour** - Zvolí se hodnota nejbližšího texelu. Spoustu artefaktů.
**Bilinear interpolation** - Vážený průměr hodnot 4 nejbližších texelů. Akcelerované HW.
**Linear mipmap interpolation** - Lineární interpolace mezi nejbližšími texely dvou nejbližších mipmap.
**Trilinear interpolation** - Bilineární interpolace v rámci dvou nejbližších mipmap, pak lineární.
**Anisotropic filtering** - Promítnutý pixel je deformovaný čtyřúhelník. Dle kratší strany volba mipmap level, podél delší strany multisampling (zprůměrování vzorků). HW podpora, ale drahé.
**Vlastní filtrování** - Můžeme implementovat vlastní, třeba se summary tables.

**3D textura** - Vlastnosti měnící se napříč objemem, vnitřní struktura materiálu, animované textury. Trilineární interpolace.

**(Sparse) Virtual texture** (megatexture) - Všechny textury v jedné velké, na disku, stránkování (rozdělíme na stránky, pool rezidentních, tabulka mapování UV souřadnic). Mipmaps, různé části pak v různých rozlišeních. Quadtree pro hierarchii právě rezidentních stránek, než se nahraje pořadované rozlišení, použijeme HW podpora. nižší. Je třeba chytřejší filtrování (nelze před okraje stránek). Méně paměti, velký bandwidth.
*Feedback analysis* - Samostatný průchod, ve výrazně nižším rozlišení, renderují se souřadnice virtuálních stránek, mipmap level, ID virtuální textury.

**Texture atlas** - Velká textura obsahující ostatní. Pak se méně mění stav HW.

**Forward texture mapping** - Iterace přes texely textury, umisťují se do pixelů frame bufferu. Efektivní cachování, ale zbytečné procházení mnoha texelů.
**Inverse texture mapping** - Iterace přes pixely frame bufferu, získávají se texely. Standard. Nemusí být lineární přístup, jsou třeba texture caching techniky.

### Změna kontrastu a jasu

**Histogram** - Tabulka četností jednotlivých jasových (barevných) hodnot (kolik pixelů má danou hodnotu).
**Transformační (převodní) funkce** $t: \mathbb{R} \rightarrow \mathbb{R}$, obvykle $[0,1] \rightarrow [0,1]$ - Mezi jasy na vstupu a na výstupu, upraví histogram. Např. gamma-korekce, zvýšení kontrastu, změna jasu.

**Změna jasu** - Bílé a černé barvy zachováme, hodnoty uprostřed snížíme/zvýšíme.
**Změna kontrastu** - S-křivky nebo inverzní s-křivky. Bílé a černé barvy zachováme, střední část také, ale stínům a světlům intenzitu snížíme nebo zvýšíme.
**Ekvalizace histogramu** - Automatické rozprostření grafu pro úpravu distribuce jasu.
*Globální* - Kumulovaný histogram, vydělíme celkovým počtem pixelů, máme převodní funkci v $[0,1]$​.
*Lokální* - Analýza jen na (větším) okolí daného pixelu.

----

**Intenzita** - Fyzikální vlastnosti, přímo hustota fotonů.
**Jas** - Subjektivní vnímání člověka (relativní, zhruba 1% rozdíl).

**Gamma korekce** - Exponenciální vztah mezi intenzitou a jasem. Je třeba logaritmická stupnice.
Pro efektivní využití omezeného rozsahu pixelu, pro zobrazení na zařízeních s jinou charakteristikou.

**CRT monitory** $I = K (V + \epsilon)^\gamma$, kde $V$ je přiváděné napětí, $I$ je intenzita světla, nelineární závislost.
*Kontrast* $K$ - Multiplikativní koeficient, určuje rychlost stoupání.
*Jas* $\epsilon$ - Posunutí vstupního argumentu, kde na křivce začneme.
*Postup*: Kompenzace (umocnění na $\frac{1}{\gamma}$, s $\gamma = 2.22$), tak se to uloží. Nejlépe se využije rozsah uložených hodnot a monitor to nakonec zobrazí správně.

**Rendering** - Veškeré výpočty světla se musí provádět v lineárních hodnotách, gamma korekce až na závěr.

### Kompozice poloprůhledných obrázků

**Alpha kanál** $\alpha$ - Procentuální pokrytí plochy pixelu barvou. Často rovnou $[\text{R}\alpha, \text{G}\alpha, \text{B}\alpha, \alpha]$​.

**Skládání** - Skládáme pixely $[\text{A}, \alpha_\text{A}]$ a $[\text{B}, \alpha_\text{B}]$ do výsledné hodnoty $[\text{C}, \alpha_\text{C}]$.
**Model pro skládání pixelů** - Náhodné pokrytí pixelu neprůhlednou barvou s pravděpodobností $\alpha$. Pak nezáleží na skutečném tvaru. Uvažujeme 4 atomické oblasti s jednou barvou (nemíchají se), plochy dle pravděpodobností.
**Binární alfa operace** - Podle různých možností překryvů: *Clear*, *A*, *B*, *A over B*, *B over A*, *A in B*, *B in A*, *A out B*, *B out A*, *A atop B*, *B atop A*, *A xor B*. Lze různě kombinovat, zanořovat.
**Sloučení**: $[\text{F}_\text{A}\text{R}_\text{A} + \text{F}_\text{B}\text{R}_\text{B}, \text{F}_\text{A}\text{G}_\text{A} + \text{F}_\text{B}\text{G}_\text{B}, \text{F}_\text{A}\text{B}_\text{A} + \text{F}_\text{B}\text{B}_\text{B}, \text{F}_\text{A}\alpha_\text{A} + \text{F}_\text{B}\alpha_\text{B}]$

**Další unární operace** (operace pixelu a konstanty):
$\text{darken}(\text{A}, \rho) = [\rho \text{R}_\text{A}, \rho \text{G}_\text{A}, \rho \text{B}_\text{A}, \alpha_\text{A}]$
$\text{fade}(\text{A}, \delta) = [\delta \text{R}_\text{A}, \delta \text{G}_\text{A}, \delta \text{B}_\text{A}, \delta\alpha_\text{A}]$
$\text{opaque}(\text{A}, \omega) = [\text{R}_\text{A}, \text{G}_\text{A}, \text{B}_\text{A}, \omega \alpha_\text{A}]$​

Operace lze různě kombinovat, zanořovat.

## 4. Realtime grafika - vykreslení scén, osvětlení, stínování

**Raytracing** - Pro každý pixel se najde trojúhelník, pak se spočte barva. Geometrie se nedá streamovat. Obvykle offline rendering, může dát fotorealistické výsledky, pomalé.
**Rasterization** - Pro každý trojúhelník se najdou pixely, pak se spočte barva. Geometrie se dá streamovat. Realtime rendering, aproximace světelných efektů, rychlé.

### Reprezentace 3D scén

**Povrchová** - Informace o povrchu/plášti těles (hrany, stěny). Snadno se zobrazuje.
**Objemová** - Informace o vnitřních objemech těles (body uvnitř nich). Spíš jako pomocná datová struktura.

**"Drátový model"** - Pouze vrcholy a hrany těles, nelze použít pro výpočet viditelnosti.
**VHS(T)** - Kompletní topologická informace, seznamy vrcholů, hran (dvojice vrcholů), stěn (posloupnost hran) a třeba i těles (stěny v něm). Špatně se zjišťují informace o lokálnosti.
**"Okřídlená hrana"** ("winged-edge") - Jako VHST (tělesa mají seznam hran), ale pro každou hranu pointery na vrcholy, stěny, sousední hrany v každé stěně a informace o pořadí vrcholů. Musíme mít 2-manifold (každá hrana se dotýká max. 2 stěn).
**Trojúhelníková mesh** - Vertex buffer + index buffer.
**Corner table** - Tabulka vrcholů (a atributů) + tabulka rohů (vnitřní rohy trojúhelníku; vrchol u něj a protější roh sousedního trojúhelníku). Snadno se hledají sousední objekty.

**Výčtová** - Výčet obsazeného prostoru pomocí diskrétní reprezentace (voxely). Např. buněčný model, octree. Vykreslujeme odzadu dopředu jen přivrácené stěny krychlí.
**Constructive Solid Geometry** (*CSG*) - Elementární tělesa, geometrické transformace a množinové operace. CSG strom. Silné, přesné, ale špatně se zobrazuje.

**Scene graph** - Hierarchická reprezentace scény stromem. Snadno najdeme, co je vidět. Např. BVH, spatial partitioning (BSP, quadtree/octree, grid). Mohou být i meta-objekty (occluders, collision geometry).

### Výpočet viditelnosti

**Malířův algoritmus** - Odzadu dopředu, postupně překresluje, lze i poloprůhledné objekty, stěny se ale nesmí prosekávat. Seřadíme podle nejvzdálenějšího bodu, vezmeme první, pak řada testů pro kontrolu pořadí s ostatními ve stejném rozmezí (minimax test, P versus rovina Q, Q versus rovina P, úplný test). Pokud je třeba zkusíme prohodit a opakujeme testy, ale detekujeme cykly (lze řešit rozdělením některé stěny).

**Z-buffer** - Pracujeme s jednotlivými pixely. Kromě frame bufferu také z-buffer, překresluje se podle souřadnice $z$. Není třeba třídit, geometrii lze streamovat, lze paralelizovat, zvládá i nestandardní situace. Neřeší ale jednoduše poloprůhledné objekty (nejprve neprůhledné, pak setříděné průhledné), je třeba každý frame vymazat buffer, pixely se zahazují (a zbytečně vyhodnocují), hrozí z-fighting.

**Raycasting** - Paprsek do každého pixelu, pak nejbližší průsečík (hledá se s pomocnou strukturou).

----

**Hierarchical occlusion culling** - Scénu reprezentujeme v hierarchické stromové struktuře, během renderingu procházíme a kontrolujeme, jestli jsou vidět, případně přeskočíme. Také tzv. frustum & occlusion culling.

**Face culling** - Odfiltrujeme strany na základě jejich orientace vzhledem ke směru pohledu (front face, back face).

### Výpočet vržených stínů

**Raytraced shadows** - Pomocí raytracingu, i měkké stíny. Přesné, ale drahé. Spíše se používají aproximace.

**Light/shadow map** - Statické stíny můžeme předpočítat a uložit do textury.

**Shadow-receiving plane** - Stín vrháme jen na jednu rovinu (např. zem). Renderujeme rovinu s nastavením stencil na 1, pak ostatní objekty s vynulováním stencil. V dalším průchodu promítáme objekty z pohledu světla do dané roviny, vykreslíme jako stín (třeba blending), pokud je ve stencil 1. Stíny jen na dané rovině, nelze self-shadowing.

**Shadow volumes** - Pro objekty vrhající stín vytvoříme shadow volume (na CPU, ve vertex shaderu nebo geometry shaderu), cokoliv je uvnitř, je ve stínu. Nejprve depth pre-pass, pak rendering stěn shadow volume se zápisem pouze do stencil bufferu. Nakonec rendering scény se stencil testem, stencil 0 znamená světlo. Přesné, ale výpočetně náročné. Nelze pro komplexní geometrii.
*Depth pass* - Renderuje boční stěny shadow volume. Update stencil bufferu na depth pass. Nejprve front faces a inkrement, pak back faces a dekrement. Počítá shadow faces před objektem. Problém s kamerou uvnitř shadow volume.
*Depth fail* - Renderuje boční stěny i front/back cap. Update stencil bufferu na depth fail. Nejprve front faces a dekrement, pak back faces a inkrement. Počítá shadow faces za objektem.

**Shadow mapping** - Shadow depth-buffer, renderujeme scénu z pozice zdroje světla do textury (perspektivní/ortografická projekce). Ve druhém průchodu renderujeme z pohledu kamery, trnasformace do prostoru světla, porovnáme $z$, dle toho světlo/stín. Podpora v HW (shadowmap sampler). Nejpoužívanější, ale spoustu problémů (aliasing, omezená přesnost).
*Shadow acne* - Střídá se světlo a stín. Světlo dopadá pod úhlem, ale shadow map se projektuje kolmo ke směru, texely nejsou zarovnané s povrchem. Přidat shadow bias nebo constant slope-scaled bias.
*Peter panning* - Stín nedoléhá k hraně objektu. Světlo proniká skrz tenkou geometrii, nebo moc velký bias. Můžeme pro shadow map renderovat back faces.
*Perspective aliasing* - Jako z-fighting, kvůli omezené přesnosti z-bufferu, bod se sám zastíní.
*Projection aliasing* - Zubaté stíny, kvůli omezenému rozlišení shadow map, vidíme jednotlivé texely.
**Cascaded shadow maps** - Scénu pokryjeme několika shadow maps, pro různé rozsahy, postupně se zvětšují směrem od kamery (viewing frustum rozdělíme na několik částí). Pomáhá to proti perspective aliasingu.
**Sample distribution shadow maps** - Analyzovat hloubku scény (např. v compute shaderu), ať pak můžeme co nejlépe napasovat kaskády na potřebný rozsah pro co nejlepší využití přesnosti shadow map. Při pohybu kamery ale na okrajích dochází k shimmeringu (jednou stín je, pak hned ne), protože se neustále mění rozsah. Také je to nestabilní (najednou se něco pohne těsně před kamerou).

### Měkké stíny

**Víceprůchodové varianty** základních jednoprůchodových metod.
**Post-processing** - Vyhlazovací filtr.

**Percentage closer filtering** - Ve spojení s shadow mapping, potlačuje projection aliasing (zubaté okraje). Vezmeme menší okolí ze shadow map, zkombinujeme výsledky porovnání hloubky, dostaneme poměr stínu (kolik procent vzorků je blíž). Navíc lze využít Poisson sampling pro souřadnice vzorků a náhodně otáčet.

**Receiver plane bias** - Pro zmírnění shadow acne při použití shadow map a percentage closer filtering. Slope-scaled bias nestačí pro kernel pokrývající velké oblasti s velkým sklonem. Maticí Jakobiánu spočteme parciální derivace hloubky v shadow map vzhledem ke změně UV souřadnic v shadow map space, pak aplikujeme odpovídající depth bias pro každý texel.

**Raytracing** - S plošným světlem. Z pixelu pošleme několik paprsků do různých míst na ploše světla. Výsledky kombinujeme. Dostaneme hladký přechod (umbra, penumbra). Je to ale drahé (hodně paprsků).

**Raytraced distance field** - Pro statickou scénu předpočítáme signed distance field (v každém bodě vzdálenost k nejbližší geometrii, mezi nimi interpolujeme lineárně). Na paprsku ke světlu pak můžeme přeskočit prázdný prostor. Když jsme poměrně blízko, provedeme cone tracing, aproximujeme zastíněnou plochu. Ušetří se pár testů.

**Přesné zobrazení** měkkdých stínů - Globální zobrazovací metody, např. distribuovaný raytracing, radiozita, fotonové mapy.

### Rozptyl světla pod povrchem

**Subsurface scattering** - Část světla proletí dovnitř mírně průsvitného objektu, rozptýlí se uvnitř materiálu, pak opustí povrch (potenciálně v odlišném bodě). Př. kůže, vosk, mléko.

**Forward scattering** - Paprsky světla zezadu projdou kompletně na druhou stranu.
**Back scattering** - Světlo se vrátí zase ven směrem k pozorovateli. Dává to materiálům měkčí dojem.

**BSSRDF** (Bidirectional scattering-surface reflectance distribution function) $S(x_i, \omega_i ; x_o, \omega_o)$ udává poměr vycházející radiance k přicházejícímu toku $d L_o(x_o, \omega_o) = S(x_i, \omega_i ; x_o, \omega_o) \, d\Phi_i(x_i, \omega_i)$.
**Zobrazovací rovnice**: $L_o(x_o, \omega_o) = \int_A \int_{2\pi} S(x_i, \omega_i; x_o, \omega_o) L_i(x_i, \omega_i) (n \cdot \omega_i) \, d\omega_i \, dA(x_i)$​
Jdeme přes povrch, v každém bodě jdeme přes všechny směry z polokoule.

**Monte Carlo simulace** - Path-tracing, photon mapping. Dost pomalé.

**Pozorování**: Jemnější efekt, color bleeding, větší utlumení a rozptýlení s větším průnikem, posun barvy na hranici světla a stínu. 

**Diffusion profile** - Zjednodušující předpoklad, že funkce rozptylu závisí pouze na distance-to-exit. Pak obyčejný blur/filter kernel ($R(r)$, aplikujeme konvolucí).
$L_o(x_o, \omega_o) = \int_A R(||x_i - x_o||) \int_\Omega L_i(x_i, \omega_i) (n \cdot \omega_i) \, d\omega_i \, dA(x_i)$

**Wrap lighting** - Světlo dosahuje za roh. Upraví se diffuse člen $\cos\alpha$, vliv světla se rozšíří i na sousední neosvětlené části povrchu (lineární transformace). Lze přidat zbarvení přechodu.

**Absorpce simulovaná pomocí depth map** - Depth map z pohledu zdroje světla. S ní pak změříme, jakou vzdálenost urazí světlo skrz materiál, použijeme pro aproximaci útlumu světla (exponenciální závislost $C_{light} \cdot e^{-\sigma \cdot dist}$). Pouze pro konvexní objekty.

**Texture-space diffusion** - Unwrapujeme mesh objektu (UV souřadnice [0,1] přemapujeme do [-1,1]). Pak spočítáme osvětlení (na základě původních souřadnic). Výsledek rozmažeme a aplikujeme znovu na 3D model (jako light map), nakonec se blenduje s color map. Lze různé barevné kanály rozmazat různě.
**Screen-Space Subsurface Scattering** - Místo textur počítáme rovnou ve screen-space, až na základě hotově vyrenderovaného framu. Využijeme další render target pro výpočet irradiance per pixel. Kvůli zakrytí nám mohou někde chybět informace.

**Pre-integrated Subsurface Scaterring** - Efekt je viditelný na nerovném/zaobleném povrchu a na přechodu světlo/stín). Předpočítáme, jak vypadá falloff světla, mapujeme na povrch kruhu. Při renderování v bodě meshe určíme lokální zakřivení, zvolíme poloměr odpovídající koule, pomocí něj a $N \cdot L$​ (jako u wrap lighting) získáme hodnotu z předintegrované falloff textury.

**Subsurface scattering textures** - Speciální textury, říkají, co je pod povrchem. Pak je nějak smícháme s obvyklými texturami.

### Modely osvětlení a stínovací algoritmy

**Osvětlovací model** - Výpočet barvy dle materiálu, světel, pozorovatele a pozice a normály povrchu. Jak povrch odráží dopadající světlo. Fyzikální (např. BRDF) nebo empirické (např. Phong).
**Stínovací algoritmus** - Interpolační techniky, výpočet barvy ve všech pokrytých pixelech v závislosti na směru dopadajícího světla, aplikace osvětlovacího modelu.

----

**Ideálně difuzní** (Lambertův model odrazu) - $B_D = C \cdot I_L \cdot (L \cdot N) = C \cdot I_L \cdot \cos \alpha$, světlo se odráží do všech směrů konstantně, intenzita závisí jen na úhlu dopadu.
**Ideální odraz** (zrcadlový) - Odražené ($r = 2n (n \cdot l) - l$) a zalomené ($\frac{\sin \beta}{\sin \alpha} = \frac{n_1}{n_2} = n_{12}$), poměr dle Fresnelových zákonů.

**Phong** - $E = E_A + (\sum_{i} E_{i_D} + E_{i_S})$, difuzní složka $E_D = I_i \cdot C_D \cdot k_D \cdot \cos \alpha$ (kde $\cos \alpha = \vec{l} \cdot \vec{n}$), lesklá složka $E_S = I_i \cdot C_S \cdot k_S \cdot \cos^h \beta$ (kde $\cos \beta = \vec{r} \cdot \vec{v}$) a okolní světlo $E_A = C_D \cdot k_A$. Často $k_A + k_D + k_S = 1$. Pro blízké zdroje světla je třeba zohlednit vzdálenost, používá se $E = E_A + \sum_{i} \frac{E_{i_D} + E_{i_S}}{c_0 + c_1 d_i + c_2 d_i^2}$.
**Blinn-Phong** - Zjednodušení, v lesklé složce je $\cos ^{4h} \frac{\beta}{2} = (\vec{h_i} \cdot \vec{n})^{4h}$ místo $\cos^h\beta = (\vec{r_i} \cdot \vec{v})^h$. Předpokládají se světla i pozorovatel v nekonečnu. Half-way vektor $\vec{h_i} = (\vec{l_i} + \vec{v}) / |\vec{l_i} + \vec{v}|$ je všude konstantní, stačí ho spočítat jednou pro každé světlo.

**BRDF** - Všechny modely pro výpočet světla lze reprezentovat pomocí BRDF.

**Direct light** - Místo integrálu ze zobrazovací rovnice počítáme jen sumu (přímé odrazy od zdrojů světla).

----

**Flat shading** (konstantní stínování) - Spočte se normála v těžišti, podle ní barva, použije se všude.
**Gouraud shading** (interpolace barvy) - Normály ve vrcholech, pak barvy, bilineární interpolace.
**Phong shading** (interpolace normály) - Normály ve vrcholech, interpolace pomocí barycentrických souřadnic, barva v každém pixelu zvlášť.
**Bump/normal mapping** - Barva v každém pixelu zvlášť dle normály z textury.

### Rekurzivní sledování paprsku (raytracing)

**Raycasting** - Vygenerujeme paprsek z pozorovatele skrz každý pixel výsledného obrazu. Hledáme nejbližší objekt.

**Raytracing** - Modelujeme propagaci světla ve scéně rekurzivně, v místě nárazu další paprsky ke světlům (shadow ray, určíme stíny) a ve směru odrazu a lomu. Ke světlu odraženému přímo (dle modelu odrazivosti) přičteme výsledek z rekurzivního volání pro odraz (na lesklém povrchu) a lom (na povrchu průhledného materiálu). Koeficienty $k_L$, $k_R$ a $k_T$​​ udávají poměr složek pro konkrétní objekt.
*Omezení hloubky rekurze*: staticky (pevný počet odrazů), dynamicky (dle významu/příspěvku paprsku) nebo kombinace.
*Průsečík paprsku se scénou*: Analyticky, nebo numericky. S akcelerační strukturou (BVH).

**Výhody**: Fyzikálně přesný výpočet odrazů a lomů, snadný výpočet stínů (i měkkých v jiné variantě).
**Nevýhody**: Výpočetně drahé, geometrii nelze streamovat, horší podpora dynamických scén (rebuild akcelerační struktury).
**Nedostatky**: Jen s jedním paprskem na pixel vzniká alias (lze řešit stochastickým vzorkováním), pouze bodové zdroje světla (ostré stíny), neumí kaustiky, matné materiály (color bleeding) ani průsvitnost (jen úplnou průhlednost).

**Urychlovací metody**: Stejný počet paprsků, ale urychlení jednotlivého testu paprsek-těleso (bounding volume) nebo méně jednotlivých testů (BVS, octree). Redukce počtu paprsků (dynamické řízení rekurze). Zobecněné paprsky (nesou více informací).

**Distribuovaný raytracing** - Do obrazové funkce se přidá další proměnná (úhel odrazu/lomu, vlnová délka, čas, ...) a pomocí více paprsků (svazek) se integruje přes různé hodnoty. Monte Carlo aproximace integrálu celkové intenzity světla (vážený průměr, nebo importance sampling).
*Glossy reflection* - Více nedokonale odražených paprsků, převáží se dle BRDF.
*Soft refraction* - Více zalomených paprsků.
*Soft shadows* - Vzorkování na ploše zdroje.
*Motion blur* - Přidáme parametr pro čas, vzorkuje se časový interval otevření simulované závěrky.
*Depth of field* - Vzorkujeme na podstavě kužele, vrchol v rovině ostrosti, zprůměrujeme.
*Rozklad světla* - Při lomu sledujeme paprsky různých vlnových délek.

### Fyzikální model šíření světla

**Physically-based rendering** (PBR) - Obecný koncept, používají se realističtější modely simulující fyzikální fenomény. Můžeme dostat fotorealistické výsledky. Raytracing nebo BRDF.

**Hodnota pixelu** je formálně průměrná radiance $L(x, \omega)$ odražená z povrchů viditelných skrz pixel. Simulujeme přenos světla.

**Polární souřadnice** - Popis souřadnic ve 2D pomocí vzdálenosti a úhlu.
**Sférické souřadnice** $(r, \theta, \varphi)$ - Polární úhel $\theta \in [0, \pi]$, azimut $\varphi \in [0, 2\pi]$.
**Radián** - Středový úhel oblouku o délce poloměru kružnice. Velikost úhlu v radiánech $O/r$.
**Steradián** - Prostorový úhel s plochou $r^2$. Velikost úhlu ve steradiánech $\Omega = \frac{A}{r^2}$.
**Prostorový úhel** - Úhel ve 3D, část prostoru vymezená rotační kuželovou plochou.
**Diferenciální prostorový úhel** - Nekonečně malý prostorový úhel kolem daného směru. Obvykle reprezentován jako směr $\omega$, pak $d\omega$ je velikost a $d\omega = dA \frac{\cos \theta}{r^2}$ (prostorový úhel vyměřený diferenciální plochou $dA$).

#### Radiometrie

**Radiometrie** - Popis světla jako toku, přenos vyzářené energie, měří elektromagnetické záření.

**Foton** - Vlnová délka $\lambda \,\mathrm{[m]}$ (barva), frekvence $\nu = \frac{c}{\lambda} \,\mathrm{[Hz]}$, energie $E = h \frac{c}{\lambda} = h \nu \,\mathrm{[J = kg \, m^2 \, s^{-2}]}$.
**Viditelné spektrum**$\lambda \in (380 \,\mathrm{nm}, 780 \,\mathrm{nm})$ (světlo).

**Zářivá/radiantní energie** $Q = \int_{0}^\infty n(l) \cdot e(l) \,dl \,\mathrm{[J]}$ - Energie fotonů v jednom bodě.
**Zářivý tok/výkon** (radiant flux/power) $\Phi = \frac{d Q}{d t} \,[\mathrm{W = J/s = kg \, m^2 \, s^{-3}}]$ - Energie vyslaná do všech směrů za jednotku času.
**Intenzita záření** (irradiance) $E_L = \frac{d\Phi}{dA} \,[\mathrm{W/m^2}]$ - Energie ze všech směrů a všech zdrojů dopadající na jednotku plochy za jednotku času. Vždy vzhledem k nějakému bodu $x$ na ploše.
*Inverse-square law*: Závisí na vzdálenosti s faktorem $1/r^2$.
*Lambert cosine law*: Závisí na úhlu s faktorem $\cos \theta$ (úhel od normály $\theta$), $E' = \frac{\Phi}{A'} = \frac{\Phi}{A} \cdot \cos \theta$.
**Intenzita vyzařování** (radiozita) $M_L(x) = \frac{d \Phi_o(x)}{dA} \,[\mathrm{W/m^2}]$ - Energie vyzářená (odražená/vysílaná) do volného poloprostoru z daného místa na jednotku plochy a jednotku času.
**Zářivost** (radiant intensity) $I(\omega) = \frac{d \Phi(\omega)}{d \omega} \,\mathrm{[W/sr]}$ - Energie v daném směru (do prostorového úhlu) za jednotku času. Světelné zdroje mají různou zářivost do různých směrů.
Oproti intenzitě záření nezávisí na vzdálenosti, platí $E_L(r) = \frac{I}{r^2}$, tedy $I = \frac{d\Phi}{d\omega_L} = E_L(r)r^2$. Pak pro $r=1$ (na jednotkové kouli) $I = E_L$ (zářivost a intenzita záření se rovnají).
**Zář** (radiance) $L(x, \omega, A) = \frac{dI}{dA \cdot \cos \theta} = \frac{dE}{d\omega} = \frac{d^2 \Phi}{d\omega \, dA \cdot \cos \theta } \,\mathrm{[W / m^2 sr]}$ - Energie světla podél paprsku na plochu (tj. na jednotku kolmo promítnuté plochy a jednotku prostorového úhlu). Nezávisí na vzdálenosti ani úhlu.
Úměrná vzhledem k vnímanému jasu, konstantní podél paprsku ve vakuu (co je vyzářeno, je přijato).
Pomocí záře: $E(x) = \int_\Omega L_i(x, \omega) \cos \theta \, d\omega$ a také $\Phi = \int_A E(x) dA_x = \int_A \int_{H(x)} L(x, \omega) \cos \theta \,d\omega dA_x$

**Světelné spektrum** - Zářivý výkon jednotlivých vlnových délek ve světle. Z něj spočteme barvu.
**Spektrální zář** $L(x, \omega, A, \lambda) = \frac{dL(x, \omega, A)}{d\lambda} \,\mathrm{[W \cdot sr^{-1} \cdot m^{-3}]}$ (obdobně z frekvence).

$Q \,\mathrm{[J]} \overset{dt}{\sim} \Phi \,\mathrm{[W]} \overset{dA}{\sim} E_L \,\mathrm{[W/m^2]} \overset{d\omega}{\sim} L(x, \omega, A)  \,\mathrm{[W/m^2 \cdot sr]} \overset{d\lambda}{\sim} L(x, \omega, A, \lambda)  \,\mathrm{[W/m^3 \cdot sr]}$

**Fotometrie** - Zohledňuje lidské vnímání a pouze viditelné spektrum.
**Jas** (luminosity) popisuje citlivost záře, která se dostane k našim očím, tj. vnímání množství fotonů z daného směru. Nelineární vzhledem k luminance (exponenciální vztah).

#### (BRDF)

**Stínování** - Počítáme odcházející zář $L_o$ podél view paprsku $v$ (proti pozorovateli) dle vlastností materiálů a světelných zdrojů. Počítáme tedy odražené světlo z přicházejícího.

**Interakce světla** - Může být vyzářeno (světelný zdroj), odraženo, absorbováno, rozptýleno nebo zalomeno.

**Odrazivost materiálu** - Vztah mezi odraženou září $L_r$ a příchozí září $L_i$, určuje vzhled objektu.
**BRDF** (Bidirectional reflectance distribution function) - Model odrazivosti, poměr vycházející radiance k přicházející irradianci, $f_r(x, \omega_i \rightarrow \omega_o) = \frac{dL_r(\omega_o)}{dE(\omega_i)} = \frac{d L_r(\omega_o)}{L_i(\omega_i) \,\cos\theta_i \,d\omega_i} \,\mathrm{[sr^{-1}]}$ (jako hustota pravděpodobnosti). Nezáporná, symetrická, neporušuje zákon zachování energie.
Světlo dopadající do kamery je pak $L_o(\omega_o) = f_r(x, \omega_i \rightarrow \omega_o) \cdot E(\omega_i) \cdot \cos\theta_i$
$L_o(\omega_o) = L_{o.d}(\omega_o) + L_{o.s}(\omega_o) = f_d(\omega_i, \omega_o) \cdot E_d \cos \omega_i + f_s(\omega_i, \omega_o) \cdot E_s \cos \theta_i$

*Základní kategorie BRDF*: diffuse (nezávisí na $\omega_o$​), glossy, specular, retro-reflective.

**Isotropické povrchy** - BRDF nezávislé na otočení kolem normály povrchu, závislé jen na absolutní hodnotě rozdílu azimutů směrů.
**Anisotropické povrchy** - BRDF závisí na otočení kolem normály, různá hodnota v různých směrech.

**Conductors** (vodiče) - Jen malá část světla je propuštěna dovnitř a rychle pohlcena, zbytek se odráží.
**Dielectrics** (nevodiče) - Část světla se propouští dovnitř (odrazy, pohlcení, odraz zpět), část se odrazí.
**Semiconductors** - Komplexnější interakce.

**Různé BRDF modely**: Phong (porušuje symetrii a zachování energie), Torrance-Sparrow, Cook-Torrance (teoretické), Ward, Lafortune (experimentální), Lambertův.

#### Zobrazovací rovnice

Z definice BRDF: $dL_r(\omega_o) = f_r(\omega_i \rightarrow \omega_o) \cdot L_i(\omega_i) \cos \theta_i \,d\omega_i$
**Lokální odraz světla** $L_r(x, \omega_o) = \int_{\omega_i \in H(x)} L_i(x, \omega_i) \cdot f_r(x, \omega_i \rightarrow \omega_o) \cdot \cos \theta_i \, d\omega_i$ - Pro celkovou odraženou zář integrujeme příspěvky všech příchozích přes polokouli, převážíme BRDF a úhlem.

**Zobrazovací rovnice** $L_o(x, \omega_o) = L_e(x, \omega_o) + L_r(x, \omega_o)$​ - Energetická bilance světla v bodě. Kompletně popisuje šíření světla ve scéně.
Většina renderovacích algoritmů jsou (přibližná) řešení zobrazovací rovnice, např. local illumination, raytracing, distributed raytracing, pathtracing.

**Rekurze**: $L_o(x, \omega_o) = L_e(x, \omega_o) + \int_{\omega_i \in H(x)} L_o(ray(x, \omega_i), -\omega_i) \cdot f_r(x, \omega_i \rightarrow \omega_o) \cdot \cos\theta_i \, d\omega_i$
Několik odrazů, nekonečná rekurze (jako bychom renderovali z $x$).
**Integrál**: Lze použít Monte Carlo odhad $\left<I\right> = \frac{1}{N} \sum_{k = 1}^N \frac{g(\xi_k)}{p(\xi_k)}$, generujeme náhodné směry, pak $\hat{L}_o = \frac{1}{N} \sum_{k = 1}^N \frac{L_i(x, \omega_{i,k}) f_r(x, \omega_{i,k} \rightarrow \omega_o) \cos \theta_{i,k}}{p(x)}$. Jako funkci rozdělení pravděpodobnost můžeme vzít uniform hemisphere sampling $p(\omega_i) = \frac{1}{2\pi}$ nebo cosine-weighted hemisphere sampling $p(\omega_i) = \frac{\cos \theta_i}{\pi}$​.

**Direct illumination** - Zjednodušení, uvažuje se jen jeden odraz (světlo přímo ze zdrojů).
$L_r(x, \omega_o) = \sum_{l \in L} L_{i_l}(x, \omega_{i_l}) \cdot f(x, \omega_{i_l} \rightarrow \omega_o) \cos \theta_{i_l}$​
**Environment map** - Osvětlení můžeme buď sečíst přes zdroje, nebo vzít z environment map.

----

**Ideal diffuse reflection** (Lambertian) - Dokonale matný povrch, odraz rovnoměrně do všech směrů. BRDF je konstantní, $f_{r, d}(\omega_i \rightarrow \omega_o) = \frac{\rho}{\pi}$, kde $\rho \in [0,1]$ (část, které se odrazí). Odraz je pak $L_r(\omega_o) = \frac{\rho}{\pi} \cdot \int_{\omega_i \in H(x)} L_i(\omega_i) \cos \theta_i d\omega_i = \frac{\rho}{\pi} \cdot E$. Vzhled nezávisí na $\omega_o$.
**Ideal mirror reflection** - Platí $\theta_i = \theta_o$, pak $\omega_o = 2 \mathbf{n} (\mathbf{n} \cdot \omega_i)  - \omega_i$. Rovnice odrazu pak $L_r(x, \omega_i) = L_i(x, \omega_r)$, kde $\theta_i=\theta_r$ a $\varphi_o = (\varphi_i + \pi) \mod 2\pi$.
**Specular reflection** - Odrazí se jen část světla, popisují to Fresnelovy rovnice na základě úhlu. Pak $L_r(x, \omega_i) = R_F(\theta_i) \cdot L_i(x, \omega_r)$, přičemž $R_F(\theta_i)$ je Fresnel reflectance (lze využít Schlickovu aproximaci).
**Ideal refraction** - Dle indexu lomu a Snellova zákona $n_i \sin \theta_i = n_o \sin \theta_o$ spočteme směr zalomeného paprsku, pak $L_o = L_i \frac{n_o^2}{n_i^2}$, z Fresnelových rovnic také $L_t(\theta_i) = (1 - R_F(\theta_i) \cdot L) \cdot \frac{n_1^2}{n_2^2} \cdot L$.
**Glossy reflection** - Světlo se odráží do určitého rozsahu kolem směru ideálního odrazu.
**Microfacet BRDF** - Hrubý povrch složený z různě natočených mikroplošek, světlo se odráží mnoha směry (odraz i lom). Pro jednotlivé plošky se používá specular BRDF, ale dochází k reflection, masking, shadowing. Počítá se statisticky $f = \frac{R_F(\theta_i) G(\omega_i, \omega_r) D(\theta_h)}{4 \cos(\theta_i) \cos(\theta_r)}$, geometrie nějak zahrnutá v $G$, rozdělení pravděpodobností orientace plošek v $D$.

### Algoritmus sledování cest (pathtracing)

**Raytracing** - Paprsky jen ve směru odrazu, nezachytí globální osvětlení z ostatních směrů.

**Pathtracing** - Rekurzivní rozbalování zobrazovací rovnice. Pro $L_i(x, \omega_o)$, musíme spočítat $L_o(ray(x, \omega_i), -\omega_i)$ pro všechny směry $\omega_i$ kolem $x$, rekurzivně v každém bodě po cestě. Raytracing + Monte Carlo odhad integrálu (výsledek poskládáme z několika paprsků náhodným směrem). Zvládá různé efekty globálního osvětlení, ale neefektivní (velké množství paprsků).

**Naivní podoba** - Paprsek skrz pixel (třeba náhodný vzorek na ploše), pak náhodná procházka (importance sampling dle BRDF), vždy jen jeden paprsek dál. Může být omezený počet skoků. Po cestě sčítáme světlo vyzářené ploškami (nebo radianci z mapy prostředí mimo). Musíme ale mít štěstí a trefit se do světelného zdroje.

**Next Event Estimation** - V každém bodě přidáme paprsky ke všem světelným zdrojům (náhodný bod na ploše) nebo jen k jednomu náhodnému (vydělíme pravděpodobností volby).
**Ruská ruleta** - Nestranné ukončení rekurze, jen s určitou pravděpodobností (pomocí BRDF) pokračujeme dál.
**Více paprsků** - Každým směrem vyšleme více paprsků (třeba tisíce), nejen jeden, váženě zprůměrujeme.

**Path tracing** - Od pozorovatele směrem do scény, končí ve světlech. Neefektivní.
**Light tracing** - Od zdrojů světla. Lze spojit s NEE (paprsek do kamery). Jedna náhodná procházka vyplní více pixelů. Neefektivní.
**Bidirectional path tracing** - Paprsky ze světla i z kamery, po určitém počtu odrazů spojíme. Po cestě spojujeme každou dvojici bodů a také NEE. Lepší výsledky, rychleji konverguje.

### Předpočítané globální osvětlení

**Globální osvětlení** (global illumination) = *Direct illumination* (světlo přímo ze zdroje) + *Indirect illumination* (světlo ze všech ploch ve scéně). Počítá přenos světla mezi plochami. Color bleeding, caustics, glossy reflections, refractions.
Např. radiosity, ray-tracing, path-tracing, ambient occlusion, photon mapping, signed distance field, image-based lighting.

**Baked lighting** - Obecný koncept předpočítání informace o osvětlení statických scén a světel a uložení do textur. Např. předpočítaný přenos radiance, ambient occlusion.

**Environment mapping** (image-based lighting) - Do textury uložíme radianci okolního světla (z nekonečna). Pro dokonalé zrcadlo pak stačí jeden vzorek, jinak integrál $L_r(x, \omega_o) = \int_{\omega_i \in H(x)} L_i(x, \omega_i) V(x, \omega_i) f_r(x, \omega_i \rightarrow \omega_o) \cos \theta_i \, d\omega_i$ (Monte Carlo metoda).
**Environment map pre-filtering** - Irradiance indexovaná normálou. Mapu prostředí rozmažeme, pak bereme jen jeden vzorek (aproximace vzorkování více bodů). Různé rozmazání pro různě glossy povrchy.

**Irradiance environment mapping** - Pro matné povrchy $L_r(\omega_o) = f_{r,d} E$. Kdykoliv se EM změní, promítneme ji do SH, získáme koeficienty, přenásobíme $A_l$ (pro převážení $\cos$), získáme koeficienty pro irradiance EM ($E_{l,m} = A_l L_{l,m}$), zabalíme do speciální matice $M$. Z normály spočteme irradianci $E(n) = n^T M n$​.

**Light mapping** - Předpočítání globálního osvětlení do textury, pak se obvykle násobí s diffuse color.
**Radiosity normal mapping** - Spojení light map a normal map. Pro objekty máme radiosity light map (v každém texelu je směrová distribuce přicházejícího světla, v podstatě irradiance EM pro každý texel). Spočteme jen pro pár směrů a pak interpolujeme dle normály z normal map. Pouze difuzní povrchy a statická scéna.
Různé báze pro reprezentaci radiosity light map: *Half-life 2 basis* (předpočítaná light map ve 3 ortogonálních směrech), *Irradiance EM* (předpočítané SH koeficienty), *H-basis* (SH funkce přizpůsobené pro polokouli).

**Photon mapping** - Zvolíme zdroj světla (náhodně dle zářivého toku), bod na ploše, náhodný směr (dle zářivosti), po nárazu uložíme do photon map (pozice, směr, tok), odrazíme (dle BRDF), sledujeme cestu (ruská ruleta na ukončení). Při renderování distribuovaný raytracing, místo rekurze odhad z photon map (z $k$​ nejbližších fotonů, převážíme tok BRDF), pro velmi odrazivé použijeme i rekurzi.
**Irradiance caching** - Urychluje photon mapping. Výpočet provádíme jen na několika vybraných místech, jinde interpolujeme (místo mnoha sekundárních paprsků). Lazy výpočet, udržujeme irradiance cache s již spočítanými hodnotami. Pokud neobsahuje vhodnou, spočítáme a uložíme.
**Radiance caching** - Pro různé pozorovací směry různý BRDF lobe, ukládáme tedy distribuci $L_i(\omega)$​ v závislosti na směru (reprezentujeme jako SH koeficienty, spočteme vzorkováním přes polokouli). Do radiance cache ukládáme vektory koeficientů pro přicházející radianci v bodě, ty pak také interpolujeme. Jen pro BRDF nízké frekvence.

**Ambient occlusion mapping** - Předpočítá se integrál viditelnosti přes polokouli v daném bodě, $A(x) = \frac{1}{\pi} \int_\Omega V(x, \omega) \cdot \cos \omega \, d\omega$. Pomocí Monte Carlo metody (raycasting). Tím převážíme konstantní ambientní složku. Můžeme spočítat i dominantní směr světla a použít pro (rozmazanou) mapu prostředí. Pro urychlení lze využít raytraced distance field.

**Předpočítaný přenos radiance**

### Výpočet globálního osvětlení v reálném čase

**Real-time aproximace** - Pouze přímé osvětlení (+ ambientní člen), pouze bodová světla, vzdálené mapy prostředí.

**Real-time raytracing** - Moderní HW je toho schopen. Např. ray-traced ambient occlusion.

**SSAO** - Aproximace, separátní průchod. Na základě depth map pro každý pixel spočteme occlusion factor (vzorky v polokouli nebo porovnání s okolím). Nakonec rozmažeme. Buď při výpočtu světla, nebo až nakonec.

**Light probes** - Něco jako mapa prostředí (jak světlo prochází prázdným prostorem). Umístíme do několika míst scény, v každém framu updatujeme. Při renderingu najdeme nejbližší nebo interpolujeme. Typicky do interiérů.

**Voxel-based global illumination** (VXGI) - Scénu převedeme do voxelů (v geometry shaderu), sestrojíme octree. Renderujeme z pohledu světel, voxelům přiřadíme barvu přímého osvětlení. Pak filtrujeme na mip-maps. Nakonec samplujeme pomocí cone-tracing (pro každý pixel na obrazovce, sbíráme globální osvětlení, několik paprsků ve směru normály).

**Point-based global illumination** (PBGI) - Renderujeme scénu s přímým osvětlením, současně generujeme point cloud (pozice, normála, poloměr, barva), tj. části přímo osvětlených povrchů (surfely). Sestrojíme octree, surfely s podobnou normálou spojíme do jednoho, s odlišnou reprezentujeme pomocí SH. Renderujeme globální osvětlení, pro každý bod rasterová krychle, vynásobíme BRDF a sečteme. Pro difuzní odrazy vynecháme pixely pod horizontem, pro lesklé vynecháme pixely s BRDF 0.

**Light-propagation volumes** (LPV) - Jen difuzní povrchy. Paprsky od světel, vzorkujeme osvětlené povrchy (generujeme point cloud), seskupíme do 3D mřížky (sečteme a zprůměrujeme radiance), reprezentují VPL (virtual point light, dle normály a odraženého toku). Iterativně propagujeme radiance do sousedních buněk, dokud neprojde celou mřížkou.  Při renderingu se díváme do 3D textury.

### Stínování založené na sférických harmonických funkcích

**Sférická funkce** (*funkce na jednotkové kouli*) - Jako parametr má směr ve 3D.

**Aproximace funkce** - Funkci $G(x)$ vyjádříme jako lineární kombinaci bází $G(x) = \sum_{i=1}^n c_iB_i(x)$. Pak stačí uložit jen konečný počet koeficientů $c_i$.
Chceme minimalizovat míru chyby:
$\begin{bmatrix} \left< B_1 | B_1 \right> & \left< B_1 | B_2 \right> & \dots & \left< B_1 | B_n \right> \\ \left< B_2 | B_1 \right> & \left< B_2 | B_2 \right> & \dots & \left< B_2 | B_n \right> \\ \vdots & \vdots &\ddots & \vdots \\ \left< B_n | B_1 \right> & \left< B_n | B_2 \right> & \dots & \left< B_n | B_n \right> \end{bmatrix} \cdot \begin{bmatrix} c_1 \\ c_2 \\ \vdots \\ c_n \end{bmatrix} = \begin{bmatrix} \left< G | B_1 \right> \\ \left< G | B_2 \right> \\ \vdots \\ \left< G | B_n \right> \end{bmatrix}$, kde $\left< F | H \right> = \int_I F(x)H(x) \, dx$
Pro ortonormální bází je $\mathbf{B} = \mathbf{I}$, takže
$\begin{bmatrix} c_1 \\ c_2 \\ \vdots \\ c_n \end{bmatrix} = \begin{bmatrix} \left< G | B_1 \right> \\ \left< G | B_2 \right> \\ \vdots \\ \left< G | B_n \right> \end{bmatrix}$ (projekce funkce $G$ do prostoru báze)

Také $\int f(x) g(x) \, dx = \sum_i f_i \cdot g_i$ (integrál součinu funkcí je skalární součin koeficientů)

**Sférické harmonické funkce** - Ortonormální báze funkcí na jednotkové kouli, $Y_{l,m}(\theta, \varphi)$ (band $l$ má $2l+1$ koeficientů $m$). Funkci pak aproximujeme $G(\theta, \varphi) = \sum_{l = 0}^{n-1} \sum_{m = -l}^{m=l} c_{l,m} Y_{l,m}(\theta, \varphi)$, kde $n$ je řád aproximace (používá $n^2$ harmonických funkcí).
$\begin{equation}y_{l,m}(\theta, \varphi) = \begin{cases} \sqrt{2} K_{l,m} \cos(m \varphi)P_{l,m}(\cos\theta), & m>0 \\ \sqrt{2} K_{l,m} \sin(-m\varphi) P_{l,-m}(\cos \theta), & m < 0 \\ K_{l,0} P_{l,0}(\cos\theta), & m = 0 \end{cases} \end{equation}$
Obecně $Y_{l,m}(\theta, \varphi) = K \cdot \Psi(\varphi) \cdot P_{l,m}(\cos\theta)$, kde $K$ je normalizační konstanta, $P$ je odpovídající Legendre polynomiál.
*Výpočet koeficientů*: $c_{l,m} = \left< G | Y_{l,m} \right> = \int_S G(\omega) Y_{l,m}(\omega) \, d\omega = \int_0^{2\pi} \int_0^\pi G(\theta, \varphi) Y_{l,m}(\theta, \varphi) \sin \theta \, d\theta \, d\varphi$
*Zjednodušené indexování*: $Y_i = Y_{l,m}$ a $i = l(l+1)+m$

**Stínování** - Každé BRDF ve scéně reprezentujeme pomocí SH. Zdiskretizujeme $\omega_o$, pro každé máme sadu SH koeficientů, předpočítáme a uložíme do textury (dle $\omega_o$ vybereme řádek textury, dostaneme koeficienty $f_i$). Podobně reprezentujeme mapu prostředí (čím nižší řád, tím rozmazanější), jen jedna sada koeficientů (přepočítáme, kdykoliv se změní). Při renderingu získáme koeficienty, přeneseme BRDF do global frame (pomocí SH-rotation), spočteme skalární součin koeficientů $L_o(\omega_o) = \sum \lambda_i f_i(\omega_o)$ (místo integrálu $L_r(\omega_o) = \int_{H(x)} L_{em}(\omega_i) \cdot f_r(\omega_i \rightarrow \omega_o) \cdot \cos \theta_i \, d\omega_i$, ale kosinus schované v EM).

### Předpočítaný přenos radiance

**Předpočítaný přenos radiance** (*PRT*) - Předpočítají se efekty přenosu světla ve statické scéně, modelují se pomocí projekce do SH. Využijeme pro rendering s dynamickým osvětlením (pouze EM).
Předpočítáváme transfer koeficienty (předem integrovaná visibility and cosine map) zvlášť pro každou funkci SH báze Vzhled při osvětlení pouze danou bázovou funkcí). Odraženou radianci spočteme lineární kombinací transfer koeficientů (škála z EM).

**Neumannova expanze** $L_o(x, \omega_o) = L_0(x, \omega_o) + L_1(x, \omega_o) + ...$
$L_0(x, \omega_o) = \int_\Omega L_{env}(\omega_i) \cdot f_r(x, \omega_i \rightarrow \omega_o) \cdot V(x \rightarrow \omega_i) \cdot \cos \theta_i \, d\omega_i$ (přímé osvětlení)
$L_n(x, \omega_o) = \int_\Omega L_{n-1}(x, \omega_i) \cdot f_r(x, \omega_i \rightarrow \omega_o) \cdot (1 - V(x \rightarrow \omega_i)) \cdot \cos \theta_i \, d\omega_i$​ (odrazy)

**Diffuse PRT** - Odražené světlo nezávisí na pozorovacím směru. Tím se zjednoduší výpočty. První člen je $L_0(x) = \frac{\rho_d}{\pi} \int{L_{env}}(\omega_i) \cdot V(x \rightarrow \omega_i) \cos \theta_i \, d\omega_i$ (BRDF je konstantní $\frac{\rho_d}{\pi}$). EM se může měnit, ale viditelnost a kosinus ne (máme statickou scénu). Zkombinujeme je do transfer function, pak $L_0(x) = \frac{\rho_d}{\pi} \int{L_{env}}(\omega_i) \cdot T(x, \omega_o, \omega_i) \, d\omega_i$. Radianci z prostředí aproximujeme pomocí SH, $L_{env(\omega_i)} \approx \sum_i l_i Y_i(\omega_i)$. Pak $L_0(x) = \frac{\rho_d}{\pi} \int_\Omega \sum_i l_i Y_i(\omega_i) \cdot T(x, \omega_o, \omega_i) \, d\omega_i$, po úpravě $L_0(x) = \frac{\rho_d}{\pi} \sum_i l_i \int_\Omega Y_i(\omega_i) \cdot T(x, \omega_o, \omega_i) \, d\omega_i$ (i zlomek můžeme schovat do $T$).
Integrál není závislý na přicházejícím světle, můžeme ho předpočítat. Je to ale operace projekce do báze $Y_i$, takže $t_{x,i} = \int T(x, \omega_o, \omega_i) Y_i(\omega_i) \, d\omega_i$. Reprezentuje transfer koeficienty (mapuje přímé světlo v bázové funkci na vycházející radianci v bodě). Nakonec tedy máme za runtime jen skalární součin.
Podobně můžeme modelovat i další odrazy, nakonec spočteme finální transfer vektor $t_{x, i}$ (sada transfer koeficientů). Ten mapuje radianci z prostředí na vycházející radianci v bodě.
Nakonec $L(x, \omega_o) = L_0(x, \omega_o) + L_1(x, \omega_o) + ... = \sum_i l_i (t_{x,i}^0 + t_{x,i}^1 + ...) = \sum_i l_i t_{x,i}$.
Za runtime vyhledáme transfer vector bodu, spočítáme skalární součin.

Kdybychom neměli ideálně difuzní povrch, museli bychom mít jednu sadu transfer koeficientů pro každé $\omega_o$.

## 5. Animace postav (rigging, skinning, morphing)

**Animace** - Iluze pohybu přepínáním obrázků/póz v čase, můžeme interpolovat.
**2D animace**: Sprite sheet s framy, určí se rychlost přehrávání.
**3D animace**: *Full vertex animation* (pozice vrcholu pro každý frame, velmi expresivní), *Keyframes* (několik framů s pozicemi vrcholů (pózy), interpolace), *Bone animation* (keyframy s pozicemi kostí, interpolace).

**Komprese animací** - Lze využít kvantizaci, predikci (I-frames pro keyframy, B-frames s rozdíly), entropy coding (např. LZ77, Deflate).

Ve hrách máme různé akce, mezi nimi chceme blendovat, také třeba přizpůsobit animaci terénu.

**Bone animation** - Vytvoří se rig (kostra), vrcholy modelu (kůže) se přiřadí ke kostem, pohybujeme kostrou. Keyframes s pozicemi kostí (matice transformace), mezi nimi se interpoluje dle křivek. Hodí se pro realtime, ale trpí různými vizuálními artefakty (protínání, roztahování, překrucování).
*Rigging* - Vytvoření kostry jako stromu kostí (ve skutečnosti kloubů), matice transformací (translace a rotace oproti předchozí), různá omezení (třeba max. 180°).
*Skinning* - Vážené mapování vrcholů meshe na kosti (pak vážená interpolace).
*Vertex blending* - Vrchol je ovlivněn několika kostmi dle vah, $\mathbf{u}(t) = \sum_{i=0}^{n-1} w_i \mathbf{B}_i(t) \mathbf{M}_i^{-1} \mathbf{p}$, kde $\mathbf{p}$ je vrchol v rest pose, $\mathbf{M}_i^{-1}$ převede vrchol do kosti (z model space), $\mathbf{B}_i(t)$ převede z kosti do model space v čase $t$ (obvykle konkatenace hierarchie matic).
*Skinning matice* $\mathbf{B}_i(t)\mathbf{M}_i^{-1}$ (v praxi se obvykle konkatenují a předávají jako jedna do vertex shaderu).

**Animační graf** - Bezestavový DAG, vstupy z herní logiky (kontext), animace v listech. Projdeme z kořene do listu, akumulujeme matice kostí. *Blend nodes*, *Logical nodes* (přepnutí mezi různými cestami v grafu).

**Forward kinematics** - Přímá manipulace transformací kostí. Těžké dostat konkrétní výsledky.
**Inverze kinematics** - Chceme kost na určité pozici, zbytek se dopočítá. Umožňuje přizpůsobení se terénu. Řeší se obvykle iterativní aproximací pomocí inverze Jakobiánu (jak moc se změní konečná pozice, když změníme jednu ze složek po cestě). Z $\Delta p = J(\theta) \cdot \Delta \theta$ máme $\Delta \theta = J^{-1}(\theta) \cdot \Delta p$ ($J^{-1}$ odhadneme transpozicí, $\Delta p$ je rozdíl cílové a aktuální pozice), to je jeden krok iterace, aplikujeme $\Delta\theta$, spočteme nové $\mathbf{p}$.

----

**Facial animations** - *Bone animations*, *free form deformations*, *blend shapes and morph targets*, *fyziologické modely svalových kontrakcí*.

**Blend shapes and morph targets** - Podobné interpolaci mezi keyframy animací. Používá se v realtime aplikacích. Zachytíme několik výrazů obličeje (morph targets, stejná topologie, mohou být difference vectors od neutrální pózy), mezi nimi blendujeme pomocí vah, vytváříme finální blend shapes $\mathbf{f} = \sum_{k = 0}^n w_k \cdot \mathbf{b}_k$​​.
Velmi expresivní, ale spoustu práce.
**Warping/morphing** - Retargeting animace pro jiné topologie obličeje.

**Fyziologické modely** - Spíše se používají pro vyrobení něčeho, co je použitelné v realtime (např. blend shapes and morph targets).

**Motion Capture** - Získání dat. Lze i pro výrazy obličeje. Obvykle vyžaduje drahý hardware a software a důkladnou kalibraci. Systémy mohou být optické (spoustu kamer, značky), nebo neoptické (gyroskopy, elektromagnety); založené na značkách (speciální oblek s aktivními/pasivními značkami), nebo ne (počítačové vidění, deep learning)

**Physically-based Animation** - Fyzikální simulace, animace se počítají za běhu, např. částice, vlasy, látky, kapaliny. Anatomicky přesné skeletální modely (kontrakce a relaxace svalů).

**LOD** - Blízko ke kameře přesné animace, dál menší počet animovaných kostí, bez interpolace/blendingu nebo třeba jen každý $n$-tý frame.

## 6. Architektura GPU

**Programmable rendering pipeline** - 3D data scény se posílají skrz sérii kroků a převedou do výsledného 2D obrázku. Zpracovává se paralelně.

**Scene description** (reprezentace 3D scény v paměti počítače (vrcholy, hrany, trojúhelníky)), **API** (data se předají řadiči GPU, přenos dat do paměti GPU, nastavení pipeline, draw volání), **VBOs + Primitive processing** (příprava dat pro první fázi shaderu), **Vertex shader** (transformace bodů do NDS (model-view-projection), přibalí atributy), **Tesselation shader** (zjemnění sítě), **Geometry shader** (transformace, generování, zahození primitiv, změna formátu), **Vertex post-processing** (perspektivní dělení, view frustum clipping, clamp hloubky $[0, 1]$, viewport transform), **Primitive assembly** (z vrcholů primitiva pro rasterizér, face culling), **Rasterization** (převedení na množinu fragmentů, interpolace atributů), **Early depth test** (pokud neměníme $z$, nezahazujeme a neděláme alpha-blending), **Fragment shader** (perspektivní korekce (HW), výsledná barva), **Per-fragment operations** (color blending, depth test, stencil test, MSAA), **Framebuffer** (zápis do color/z/stencil bufferu), **Screen** (zobrazení na obrazovku)

----

**Scissor test** - Jednoduchý a rychlý test, obdélníková oblast.
**Alpha test** - Porovnání alpha s referenční hodnotou.
**Depth test** - Porovnání $z$​​ s hodnotou v depth bufferu.
**Stencil test** - Dle pozice a hodnoty ve stencil bufferu, stencil buffer se také updatuje.
**Color blending** - Kombinace barvy fragmentu s barvou pixelu v color bufferu. Průhlednost.
**Dithering** - Přidání šumu pro randomizaci kvantizační chyby.
**Logical operations** - Logické operace pro kombinaci barvy fragmentu a cílového pixelu.

### Architektura grafického akcelerátoru

**Multi-core computing** (max. desítky) - CPU, plná instrukční sada, hodně sekvenčních algoritmů.
**Many-core computing** (i stovky) - GPU, jednodušší instrukční sada, paralelní algoritmy.

**CPU** (Central Processing Unit) - Nízká latence. Hodně cache (každé jádro vlastní L1, L3 sdílená), rychlé přepínání mezi operacemi, paralelismus sekvenčních tasků (explicitní správa vláken).
**GPU** (Graphics Processing Unit) - Vysoká propustnost. Několik SMs, každý vlastní L1 cache na instrukce, L2 sdílená, spoustu FPUs, datový paralelismus (správa vláken v HW).

**SIMD** (Single Instruction Multiple Data) - Jen jedna instrukční paměť, instrukce rozkopírována na každé jádro, ale dedikovaná paměť (paralelismus na úrovni dat).
**SIMT** (Single Instruction Multiple Threads) - Navíc multithreading, vlákna seskupená do warpů (ta samá instrukce, na různých datech).

----

**Grafický akcelerátor (GPU)** - Masivně paralelní zpracování dat (datový paralelismus). Funguje jako stavový automat.

**CUDA Architecture** - GPU je skupina vysoce vláknových Streaming Multiprocessors (SMs), každý má sadu Streaming Processors (SPs, CUDA cores), ty sdílejí řídicí obvody, dekodér instrukcí a instrukční cache.

*Příklad*: 6x **Graphics Processing Cluster** (sdílená L2 cache), každý má 10x **Streaming Multiprocessor** (sdílená paměť a stav pipeline), každý má 2x **warp**, každý vlastní instrukční buffer, warp scheduler, DPs, SFUs, LD/ST a 32x **Streaming Processor** (~jádro), každý má pak několik ALUs a FPUs.

**Řadič** - Vezme volání pro GPU a rozdělí práci mezi SMs.

**Problémy**: *Branching kódu* (warp musí vykonávat stejnou instrukci, zdržuje to), *sdílená paměť* (SMs v jednom GPC sdílí paměť a stav pipeline, změna stavu zahazuje všechny).

**Architektury jader**: *CUDA cores* (int/float operace, rendering pipeline), *ray-tracing cores* (BVH průchod, průsečíky), *tensor cores* (maticové výpočty $D = A \cdot B + C$).

### Předávání dat do GPU

**Datové přenosy mezi CPU (RAM) a GPU** - Bottleneck. Scény/objekty, materiály, shadery, textury.

**Předávání dat** do GPU primárně skrz API programovatelné pipeline.

**Vertex Buffer** - Pole atributů vrcholů pro zobrazení (pozice, normálový vektor, texturové souřadnice, barva). Ve vertex shaderu pak navážeme dle indexu (`layout`).
**Index Buffer** - Indexy vrcholů, které tvoří primitiva.

**Texture Mapping Units** (TMUs) - Pro předání textur.

**Uniform Variables** - Pro posílání atributů do shaderů (globální pro celou scénu).
**Uniform Block** - Nastavení sdílených parametrů pro shadery.

### Textury a GPU buffery

**Vertex Array Object** (VAO) - Ukládá stav, tedy informace o ostatních bufferech (VBO) a jejich pointerech a atributech. Umožňuje rychle přepínat mezi kontexty.

**Workflow** - Vygenerujeme `glGenBuffers(n, &p)`, navážeme `glBindBuffer(type, pointer)` (nastavíme v kontextu, specifikujeme cíl, např. `GL_ARRAY_BUFFER`, `GL_ELEMENT_ARRAY_BUFFER` nebo `GL_UNIFORM_BUFFER`), nahrajeme data `glBufferData(target, size, data, usage)`, pro vertex buffer nastavíme atributy (`glVertexAttribPointer` a `glEnableVertexAttribArray`).

**Framebuffer** - Obrazová paměť, bitmapa pro zobrazení na obrazovce.
*Double buffering* - Zatímco zobrazujeme front buffer, kreslíme do back bufferu, pak prohodíme.
*V-Sync* - Renderování synchronizované s refresh rate monitoru.
*Triple buffering* - Double buffering + V-Sync, navíc další back buffer, cyklíme mezi třemi.

**Frame Buffer Object** (FBO) - Vlastní off-screen framebuffer. Lze nastavit rozlišení, attachmenty (je to kolekce attachmentů/bufferů). Využívá se pro post-processing, deferred shading.
**Multiple render targets** - Máme více výstupních bufferů.
**Off-screen rendering** - Renderujeme do jiného než defaultního bufferu, výsledek se tak nezobrazuje.

**Color attachment** (`GL_COLOR_ATTACHMENTi`) - Můžeme jich připojit víc, musí odpovídat výstupu z fragment shaderu. Update pouze pokud fragment projde všemi testy. Sémantika nemusí být jen barva.
**Depth buffer** (`GL_DEPTH_ATTACHMENT`) - Hloubka pro každý pixel ($z$​ z view space se převádí nelineárně), nutný pro depth test. Přesnost závisí na typu elementů a také na projekci.
**Stencil buffer** (`GL_STENCIL_ATTACHMENT`) - 8b číslo pro každý pixel, pro stencil testy.

**Render buffer object** (`glFramebufferRenderbuffer()`) - Pokud chceme zapisovat. Podpora MSAA. Např. depth, stencil nebo zobrazení
**Textura** (`glFramebufferTexture2D()`) - Pokud chceme třeba později číst (použít v shaderech). Render to texture.

**Vyčištění bufferů** např. `glClear(GL_COLOR_BUFFER_BIT | GL_DEPTH_BUFFER_BIT | GL_STENCIL_BUFFER_BIT)`.

----

**Textury** - Jako buffer. Vytvoříme handle, navážeme (můžeme aktivovat konkrétní TMU), pošleme data (`glTexImage2D()`), nastavíme parametry.
**Parametry**:
*Wrap* (`GL_TEXTURE_WRAP`): Co dělat, když jsou UV souřadnice mimo rozsah [0,1], např. repeat, mirrored repeat, clamp to edge, clamp to border.
*Filtering* (`GL_TEXTURE_MIN_FILTER` a `GL_TEXTURE_MAG_FILTER`): Nastavení samplingu/filteringu, např. nearest, linear.

Různí výrobci GPU využívají různé kompresní algoritmy pro textury.

**Texture Mapping Units** (TMUs) - Hardwarová komponenta pro zpracování texelů jedné bitmapy  (adresování, filtrování). Speciální typ paměti se spatial caching.
**Sampler** - GLSL protějšek TMU, odpovídá jedné textuře přiřazené do TMU.
`glActiveTexture()`, `glBindTexture()`

----

**Uniform Buffer Object** (UBO) - Sdílená data dáme do bufferu, programy pak hledají uniform data v konkrétním bufferu. Lze snadno nastavit data naráz pro všechny programy s daným UBO.
**OpenGL kontext** - Má pole uniform buffer binding points, na indexy se vážou UBO.
**Uniform block** v shaderu - Data nejsou v objektu programu, ale v UBO na konkrétním indexu.

**Bindless texture** - Textury se nemusí navázat do kontextu před renderováním, shadery získají texturu pomocí handle (musí být rezidentní; nepřímý přístup do paměti; převede se na sampler).
*Postup*: Z textury vytvoříme handle, uděláme rezidentní, předáme shaderu (jako vstup, atribut vrcholu nebo uniform), převedeme na sampler.

### Programování GPU - shaderů

**Shadery** - Malé programy běžící na GPU.
**Parametry** - in (VS z vertex bufferu, FS z VS), out (z VS do FS, z FS barva vec4), uniform (`glGetUniformLocation(...)`, `glUniform*(...)`).

Shadery zkompilujeme (`glCreateShader()`, `glShaderSource()`, `glCompileShader()`), spojíme do programu (`glCreateProgram()`, `glAttachShader()`, `glLinkProgram()`) a ten pak použijeme (`glUseProgram()`).

----

**Vertex Shader** (``GL_VERTEX_SHADER``) - Transformace vrcholu z model space do NDS (MVP maticí). Jediný povinný.

**Tessellation Shader** - Dělení plochy na detailnější, přidávání vrcholů. Vstupem je patch (pole vrcholů, pevná délka, libovolná sémantika). Např. terén s LOD.
*Tessellation Control Shader* (`GL_TESS_CONTROL_SHADER`) - Běží jednou pro každý vrchol, ale má dostupné všechny vrcholy/atributy patche. Definuje topologii, počítá parametry pro tesselation. Můžeme ho nechat vygenerovat automaticky, pokud je velikost vstupní a výstupní patche stejná a je jen pass-through.
*Tessellation Primitive Generator* - Fixní, generuje primitiva dle parametrů podle předdefinovaných vzorů.
*Tessellation Evaluation Shader* (`GL_TESS_EVALUATION_SHADER`) - Počítá geometrii, běží jednou pro každý nově vytvořený vrchol, má přístup do dat patche. Přidává atributy (pozice, UV souřadnice).

**Geometry Shader** (`GL_GEOMETRY_SHADER`) - Jako vstup primitivum (všechny jeho vrcholy), jako výstup libovolný počet primitiv (transformuje, generuje, zahazuje, mění formát). Jako vstup navíc nová primitiva se sousedností (máme pak víc vrcholů). `EmitVertex()`, `EndPrimitive()`. Obecně mnohem pomalejší než tessellation. Např. particles, shadow volume.

**Fragment Shader** (`GL_FRAGMENT_SHADER`) - Dostane fragment, vrací barvu (osvětlení, textury). Může být poměrně drahý.

**Mesh shader** - Alternativa k vertex + tesselation + geometry. Pracuje s meshlet (trojúhelníková mesh, limit na počet vrcholů), generuje primitiva, poměrně obecný (blízké compute shaderu). Separátní task-mesh pipeline.
*Task shader* - Dynamicky generuje práci (work groups) nebo filtruje pro optimální rozvržení.
*Mesh/task generation* - Fixní krok.
*Mesh shader* - Vytváří trojúhelníky/primitiva pro rasterizér.

**Compute Shader** (`GL_COMPUTE_SHADER`) - Mimo rendering pipeline. Pro rendering i obecné výpočty (simulace, particles, post-processing). Work groups jako až 3D prostor work items (vlákna). Žádné vlastní vstupy a výstupy, vše pomocí textur nebo shader storage blocks. Např. particles, pohyb vody, image post-processing.

**Raytracing shader** - Separátní pipeline, je třeba odpovídající GPU. Definujeme několik shaderů (Ray Generation, Intersection, Any Hit, Closest Hit, Miss). Zařizuje efektivní průchod BVH.

### ~~Pokročilé techniky práce s GPU~~

**GPGPU** (*General purpose GPU*) - Využití GPU pro obecné masivně paralelní výpočty. Např. CUDA, OpenCL, compute shadery.

**Forward rendering** - Rendering do jednoho render targetu, postupně vše přidáváme. Umožňuje průhlednou geometrii. Cena framu se zvyšuje s počtem světel a s komplikovanými materiály, spoustu drahých fragmentů se zahodí.
**Deferred rendering** - Oddělíme zpracování geometrie a světel. Renderujeme geometrii do G-buffer (několik render targets), uložíme data pro výpočet světel (pozice, hloubka, normály, ...). Pak počítáme barvu už jen pro viditelné fragmenty pomocí hodnot z G-bufferu. Nepodporuje průhledné objekty, větší spotřeba paměti, omezené možnosti anti-aliasingu (renderujeme do textur, nelze HW MSAA)

**Instanced rendering** (instancing) - Jedním draw voláním nakreslíme několik instancí stejného modelu (stejná vertex data, různé transformace, textury apod.). Např. `glDrawElementsInstanced()`, pak v shaderu `gl_InstanceID`. Parametry v array uniforms (pro malý počet instancí) nebo přes vertex attributes (`glVertexAttribDivisor(index, 1)`).
**Multi-draw commands** - Zkratka za několik `glDrawElements` volání, specifikujeme víc sad geometrických primitiv.
**Indirect rendering** - Většina parametrů draw příkazu (např. `glDrawArraysIndirect`) se vezme z právě navázaného `GL_DRAW_INDIRECT_BUFFER` (např. počáteční vrchol a počet vrcholů). GPU procesy mohou naplnit hodnoty.

**Decals** - Projekce 2D textury na 3D objekt. Lze kreslit do megatexture (permanentí změny), nebo na speciální geometrii nebo do G-bufferu. Např. efekty interakce s prostředím (díry po kulkách, graffiti).
**Billboards** - Renderování 2D elementů (poloprůhledná textura na obdélníku) ve 3D světě. Otáčí se vždy směrem k pozorovateli. Např. keře, tráva, particles, HUD. Lze použít pro LOD. Pomocí depth map offsetů se mohou i správně zastiňpvat při použití shadow maps.
**Impostors** - Textura se předgeneruje pod několika úhly, pak se přepíná dle úhlu pohledu.
**Proxy impostor** - Např. 3 navzájem kolmé roviny, na nich textura.
**3D impostor** - Jen lehce tesselated quad s aplikovanou texturou a displacement mappingem.

**Level of Detail** (LOD) - Různá úroveň detailu (geometrie, počet materiálu, mipmap) dle různých metrik (vzdálenost, důležitost, rychlost pohybu, velikost na obrazovce). Diskrétní (přepnutí, nebo blend) nebo spojitý.

## 7. Technologie

### Základy OpenGL

**OpenGL** (Open Graphics Library) - API pro komunikaci s GPU, abstrahuje funkcionalitu GPU a komunikuje s driverem. Cross-language, cross-platform, asynchronní.

**Gen/Delete paradigm** - Vytvořit handle, zničit handle.
**Bind before usage** - Před použitím svázat handle s konkrétním cílem v kontextu.

*Right-handed* - $x$ doprava, $y$ nahoru, $z$ za pozorovatele.

**Vertex Buffer Objects (VBO)** - Buffery v OpenGL, např. vrcholy a atributy.
`GL_ARRAY_BUFFER` (vertex data), `GL_ELEMENT_ARRAY_BUFFER`,  `GL_UNIFORM_BUFFER` apod.
**Vertex Array Objects (VAO)** - Všechny VBO bindingy.

**Shader program object** - Kód shaderu, zkompilujeme, připojíme k programu.

**Geometrická primitiva**: `GL_POINTS`, `GL_LINES`, `GL_LINE_STRIP`, `GL_LINE_LOOP`, `GL_TRIANGLES`, `GL_TRIANGLE_STRIP`, `GL_TRIANGLE_FAN`, `GL_QUADS`, `GL_QUAD_STRIP`, `GL_POLYGON`.

**Základní volání**:
*Shadery*: `glCreateShader()`, `glDeleteShader()`, `glShaderSource()`, `glCompileShader()`, `glCreateProgram()`, `glAttachShader()`, `glLinkProgram()`, `glGetUniformLocation()`, `glUniform*()`, ...
*Buffer objects*: `glGenBuffers()`, `glBindBuffer()`, `glBufferData()`, `glBindVertexArray()`, `glGenVertexArrays()`, `glVertexAttribPointer()`, `glEnableVertexAttribArray()`, `glDeleteVertexArrays()`
*Textury*: `glGenTextures()`, `glActiveTexture()`, `glBindTexture()`, `glTexImage2D()`, `glTexParameteri()`
*Framebuffer*: `glGenFramebuffers()`, `glBindFramebuffer()`, `glFramebufferTexture2D()`, `glFramebufferRenderbuffer()`,  `glCheckFramebufferStatus()`
*Kreslení*: `glDrawElements()`, `glDrawArrays()`
*Další*: `glViewPort()`, `glClearColor()`, `glClear()`, `glFinish()`

**Extensions** - Rozšíření s dalšími featurami.

### Základy GLSL

**GLSL** (Open GL Shading Language) - Specifikace shaderů, založený na C.

**Datové typy**: *základní* (int, float, double, uint, bool), *vektory* (vecn, bvecn, ivecn, uvecn, dvecn; swizzling), *matice* (matnxm, matn, dmatn), *pole* (`length()`), *struktury*
**Speciální**: `in`, `out`, `uniform`, samplery (pro přístup do textury)

```glsl
#version 430 core

uniform mat4 MVP;

layout (location = 0) in vec3 in_vert;
out vec4 vertexColor;

const vec3 color = vec3(1.0, 0.0, 0.0);

void main() {
    gl_Position = MVP * vec4(in_vert, 1.0);
    vertexColor = vec4(color, 1.0);
}
```

```glsl
#version 430 core
out vec4 FragColor;
  
in vec4 vertexColor;

void main()
{
    FragColor = vertexColor;
} 
```

### Základy CUDA

**CUDA** (Compute Unifier Device Architecture) - Masivně paralelní programování na GPU. Posíláme vlastní vlákna a bloky. Můžeme alokovat paměť.

`__global__ float KernelFunc()` - Běží na GPU, může se volat ze CPU i GPU.
`__device__ float DeviceFunc()` - Běží na GPU, může se volat z GPU.
`__host__ float HostFunc()` - Běží na CPU, může se volat ze CPU.
`__host__ __device__ float HostDeviceFunc()` - Běží na CPU nebo GPU, může se volat ze CPU nebo GPU.

**Grid** - Mřížka bloků (bloky se rozdělují mezi SMs).
**Block** - Matice vláken (1D-3D), sdílejí paměť, provádějí ty samé instrukce.

```c++
// Kernel definition
__global__ void VecAdd(float *A, float *B, float *C) { // kernel
    int i = threadIdx.x;
    C[i] = A[i] + B[i];
}

int main() {
    ...
    VecAdd<<<1, N>>>(A, B, C); // 1 block with N threads
    ...
}
```

**Spuštění kernelu**: `MyKernel<<<gridSize, blockSize, dynamicSharedMemorySize, streamID>>>(arg1, arg2, ...)`
*Grid* - Mřížka bloků (`int` nebo `dim3`), až 3D.
*Block* - Matice vláken (`int` nebo `dim3`), až 3D.

`threadIdx`, `blockIdx`, `blockDim`

**Typy paměti**: *Host memory* (RAM), *Global memory* (DRAM na GPU), *Read-only memory* (pro SM, spatial caching, filtering), *Shared memory* (pro blok), *L1 cache* (pro blok), *L2 cache* (pro grid), *Registry* (pro vlákno).

### Základy OpenCL

**OpenCL** Open-source verze CUDA (některé části jsou ekvivalentní). Masivně paralelní tasky na GPU i CPU. Podobná filozofie jako OpenGL (syntax, navázání bufferů).
`clBuildProgram()`, `clCreateCommandQueue()`, `clCreateBuffer()`, ...

**NDRange** (grid), **Work group** (block), **Work item** (thread)

**Typy paměti**: *Global memory*, *Constant memory*, *Local memory* (pro blok), *Private memory* (pro vlákno).

`get_global_id(0)` - CUDA: `blockIdx.x * blockDim.x + threadIdx.x`
`get_local_id(0)` - CUDA: `threadIdx.x`
`get_global_size(0)` - CUDA: `gridDim.x * blockDim.x`
`get_local_size(0)` - CUDA: `blockDim.x`

```c++
// Kernel definition
__kernel void VecAdd(
	__global const float *A,
    __global const float *B,
    __global float *C
) {
    int id = get_global_id(0);
    C[id] = A[id] + B[id];
}
```

## 8. Komprese dat

**Komprese dat** - Převádíme proud vstupních dat na proud výstupních dat s menší velikostí.
**Kompresní metody** - *Encoding* (komprese) + *Decoding* (dekomprese).

**Kódování** $f: A^* \rightarrow A_C^*$​ (z posloupnosti znaků abecedy do posloupnosti znaků kódovací abecedy).
**Uniquely decodable kódování** - Pokud je zobrazení prosté.

**Prefixový kód** - Žádné codeword (komprese fráze/podřetězce) není prefixem jiného.

**Hartley's formula** - Pokud je $x$ členem $n$-prvkové množiny, pak $x$ nese $\log_2n$ bitů informace.
**Entropie** diskrétní náhodné veličiny $X$ je $H(X) = - \sum_{i = 1}^n p(x_i) \log p(x_i)$​. Platí $H(X) \geq 0$​​.
**Redundance zprávy** označuje, o kolik bitů víc se na ni použije, než kolik je její informační hodnota.

----

**RLE** (Run-length encoding) - Kopírovací a opakovací paket.

**Statistické metody** - Kratší kódová slova pro častější symboly. Modelování + kódování.
Např. Huffmanovo kódování, aritmetické kódování, kontextové metody (PPM), integer coding, slovníkové metody.

**Slovníkové metody** - Odkazy na opakované fráze.
LZ77 s posuvným okénkem (LZSS, Deflate)
LZ78 s explicitním slovníkem (LZW)

----

**Kvantizace** - Mapujeme prvky z větší množiny do prvků menší množiny. Dochází ke ztrátě informace.
*Uniformní kvantizace* - Intervaly mají stejné délky, reprezentanti jsou prostřední body. Midrise (sudý počet intervalů, 0 je hraniční bod), midtread (lichý počet intervalů, 0 je střed prostředního).
*Adaptivní kvantizace* - Hranice intervalů se přizpůsobují aktuální situaci. Forward (rozdělíme na bloky, spočteme min a max, rozsah rozdělíme), backward (intervaly mají multiplikátor, délku kroku přenásobíme multiplikátorem posledního použitého intervalu).
*Neuniformní kvantizace* - Intervaly různých délek, dle pravděpodobnosti výskytu vzorků.

**Differential encoding** - Kódujeme rozdíly po sobě jdoucích vzorků. Aby se ale neakumulovala kvantizační chyba, kódujeme rozdíl aktuální hodnoty a předchozí hodnoty po rekonstrukci. Např. *Delta-modulation*.

**Transform coding** - Posloupnost vzorků rozdělíme do bloků, každý transformujeme (lineární transformace, lze reprezentovat maticí), chceme koncentrovat informaci do určitých složek, nakonec kvantizace a kódování. Např. *DCT*, *DWHT*.

**Subband coding** - Signál rozložíme do $M$ pásem různých frekvencí pomocí filtrů, pak provedeme downsampling (každý $M$-tý vzorek), nakonec kvantizace a kódování (různý počet bitů pro různé pásmo).

### Principy komprese rastrové 2D grafiky

**Barevné prostory**: *RGB*, *CMY(K)*, *HSV*, *YUV*
**Reprezentace obrázků**: *Monochrome* (1 b), *Shades of gray* (static gray 1 B), *Color* (true color 8-16 b, $\alpha$), *Color palette* (1B pointer)

**Redundance**: Prostorová korelace, spektrální korelace, časová korelace, lidské vnímání.
**Formáty**: Formát uložení barev, komprese (ztrátová/bezeztrátová), prokládání, metadata.

*Bezeztrátová*: **RLE** (TGA, BMP), **LZ\*** (PNG, GIF, TIFF)
*Ztrátová*: **JPEG** (JFIF, TIFF)

----

**Přehled používaných metod**: *RLE* (BMP, TGA), *LZW* (explicitní slovník, GIF), *Deflate* (posuvné okénko, PNG), *delta-modulation* (TGA), DCT (JPEG, WebP), *subband coding* (JPEG 2000).

*Pomocné metody*: Predikce, Huffmanovo kódování, aritmetické kódování, kvantizace.

----

**PBM/PGM/PPM** - Zcela nekomprimovaný formát. Hlavička a sekvenčně hodnoty barev pixelů.

----

**GIF** (*Graphics Interchange Format*) - Bezeztrátová komprese (pokud už je omezený počet barev). Barevná paleta (max. 256 barev). Jedna barva může být průhledná. Lze více obrázků v jednom. Pokládací schéma (4 průchody po řádcích).

**LZW komprese** - Statistická slovníková metoda. Založené na LZ78 (s explicitním slovníkem, trie), ale s odloženou inovací. Do slovníku se vloží všechny symboly abecedy. Pak se čte nejdelší prefix vstupu, který je ve slovníku. Na výstup index fráze, do slovníku se přidá $fs$ (s následujícím symbolem). Pokračujeme od $s$. Na začátku má slovník velikost $2^{b+1}$ (kde $b$ je počet bitů na pixel), zdvojnásobuje se až do $2^{12}$​, pak statický. Pokud se zhoršuje compression rate, začne se znovu. Na pointery do slovníku se používá binární kód narůstající délky (pevná délka, ale zvyšuje se s narůstajícím slovníkem). Vstup rozdělen na bloky max. 255 B.

----

**PNG** (*Portable Network Graphics*) - Bezeztrátová komprese. Prokládací schéma (bloky 8x8, v 7 průchodech).

**Předzpracování** - Na základě sousedů se predikuje hodnota pixelu, pak se kóduje rozdíl skutečné od predikované. Různé metody (lze i kombinovat): None, Sub, Up, Average, Paeth (nejblíž $a + b - c$).

**Deflate metoda** - Statistická slovníková metoda. Založená na LZ77 (s posuvným okénkem (search buffer | look-ahead buffer)) a LZSS (výstupem je dvojice (offset, délka), nebo literál, bitem se rozliší; cyklický buffer pro okénko). Hledáme nejdelší prefix look-ahead bufferu, který začíná někde v search bufferu. Pokud najdeme, pak dvojice (offset, délka), jinak znak. Okénko posuneme o délku prefixu + 1. Pomalá komprese, rychlá dekomprese.
Vstup rozdělíme na bloky, každému předchází 3-bit hlavička (jestli je poslední a jaký je typ). Může být bez komprese, LZSS a Huffmanův kód s pevnými tabulkami, nebo Huffmanův kód ze statistik vstupních dat (spočítáme frekvence, sestavíme strom).
Dva Huffmanovy stromy, jeden pro literály a délky matchů (společná abeceda, 0..255 pro znaky, 256 EOB, 257-285 pro délky matchů (kategorie, pak bity navíc)), druhý pro offset (kategorie, bity navíc).
Hashovací tabulky pro zrychlení vyhledávání v search bufferu. Ukládají stringy délky 3 ve spojovém seznamu podle stáří. Z prefixu look-ahead bufferu délky 3 spočteme hash, pak porovnáme po znacích.
*Zlepšení*: Kromě primárního matche ještě sekundární match (okénko o 1 posunuté)m zakóduje se ten lepší.

**Huffmanovo kódování** - Tabulka četností, spojujeme dva prvky s nejnižší, tvoříme strom. Kód prvku dle cesty stromem.

----

**BMP** a **TGA** - Používají **RLE** kompresi (bezeztrátová). Kontrolní byte, obsahuje typ paketu a počet $N$. Buď opakovací ($N+1$ krát zopakovat následující) nebo kopírovací paket (zkopírovat $N + 1$ následujících).

**TGA** - Má i další možnosti komprese (žádný/RLE/Huffman/Delta-modulation), údaj v hlavičce.

**Delta-modulation** - Extrémní aplikace differential encoding (kóduje se rozdíl aktuální hodnoty a předchozí po rekonstrukci, $d_n = x_n - \hat{x}_{n-1}$). Dvouúrovňový kvantizér, přičítáme/odčítáme pevné $\Delta$ podle toho, jestli je hodnota větší/menší než předchozí.

----

**WebP** - Transform coding (posloupnost vzorků rozdělíme na bloky a transformujeme pro redistribuci informace, pak kvantizace a kódování), podobné strategie jako JPEG, navíc macroblock prediction. Převod RGB na YC~B~C~R~, chroma subsampling 4:2:0, rozdělení na macroblocks (16x16 Y, 8x8 C~B~, 8x8 C~R~, 4x4 je oblast s vysokými detaily), macroblock prediction (lineární predikce jako PNG, $X_{ij} = L_i + A_j - C$, pro $i,j = 0..3$, kde $L$ je levý sloupec, $A$ je horní řádek, $C$ je průměr $A$ a $L$ (vlevo nahoře od bloku)), pak DCT a DWHT transformace, kvantizace, aritmetické kódování.

**DCT** - Vstup rozdělíme na bloky 8x8, vynásobíme $\Theta = AXA^T$ (transformace maticí $A$ na řádky i sloupce). Matice $A$ obsahuje bázové 2D signály s různými frekvencemi.
**DWHT** (diskrétní Walsh-Hadamardova transformace) - Hadamardovu matici řádu $N$ vynásobíme $1/ \sqrt N$ (tak získáme ortogonální matici), pak uspořádáme řádky podle stoupajícího počtu změn znaménka (první řádek konstantní, pak postupně zvyšující se rozptyl).

**Aritmetické kódování** - Kóduje celý vstup jako celek na jedno desetinné číslo v intervalu $[0,1]$. Máme relativní frekvence symbolů ve zprávě, přiřadíme podintervaly. Čteme znaky, interval zužujeme. Nakonec kód čísla z podintervalu.

----

**JPEG 2000** - Novější verze JPEG standardu, subband coding (dva jednorozměrné filtry)

**Subband coding** - Signál pomocí filtrů (low-pass, high-pass, band-pass) rozložíme do několika pásů frekvencí. Pak provedeme downsampling (každý $N$-tý vzorek, kde $N$ je počet pásem). Nakonec kvantizace a kódování (různý počet bitů pro různé subbands).
Začneme obrázkem $N \times N$. Každý řádek filtrujeme (high-pass, low-pass), pak downsampling, máme 2 obrázky $N \times N/2$ (obrázek průměrů a obrázek detailů). Každý sloupec filtrujeme (high-pass, low-pass), pak downsampling, máme 4 obrázky $N/2 \times N/2$​ (průměr průměrů, detaily průměrů, průměr detailů, detaily detailů). Můžeme pokračovat dál.

**Wavelet transformace**

### Standard JPEG

**JPEG standard** (*Joint Photographic Experts Group*) - Popisuje ztrátovou i bezeztrátovou kompresi (bezeztrátová se ale v praxi nepoužívá), progresivní přenosové schéma. Definuje postupy a standardy, nezabývá se přímo implementací.

**JFIF**, **EXIF** - Implementují podmnožinu JPEG standardu.

**JPEG komprese** - Transform coding. Odstraňuje to, co oči snadno nerozliší.
*Převod barevného prostoru* z RGB do YC~B~C~R~, pak *chroma subsampling* (Y beze změny, 4:4:4, 4:2:2, 4:2:0).
Rozdělíme na *bloky* 8x8, *posuneme* z $0..255$ do $-128..127$​​. Duplikujeme poslední řádek/sloupec, aby byla velikost dělitelná 8.
*DCT* - Tabulka koeficientů 8x8, skládáme 64 bázových obrázků.
*Kvantizace* - Dělení hodnotami z kvantizační tabulky (různá pro různé složky a různé kvality), zaokrouhlení (tím se ztratí informace).
*Zig-zag scan + Huffman coding* - Diferenciální kódování DC koeficientů (rozdíl sousedních bloků), rozdělíme do kategorií, pak Huffmanův kód kategorie $n$ a $n$ bitů navíc pro určení hodnoty. Pro AC koeficienty kategorie, ale Huffmanův kód dvojice (počet přeskočených nulových, kategorie), pak $n$ bitů navíc. Samé nuly na konci nahradíme EOB.

### Komprese videosignálu

**Digitalizované video** - Časová posloupnost obrázků.
**Kontejnery** - Spoustu známých formátů videa jsou ve skutečnosti kontejnery. Spojují dohromady formát komprese videa a formát komprese audia, můžou podporovat různé.

----

**Důležité principy**: Block-based motion compensation (macroblock prediciton), predikce (I-frames, P-frames, B-frames), loop filter, rate control, GOP (vzor), DCT, kvantizace, subsampling, diferenciální kódování.

----

**CCIR 601** - Mezinárodní standard. Používá YC~B~C~R~ barevný prostor. Definuje základní vzorkovací frekvenci, pro každou komponentu zvlášť se pak používá nějaký celočíselný násobek, např. 4:4:4. Vzorky se normalizují ($Y \in \left<0, 1 \right>$, $C_bC_r \in \left< -0.5, 0.5 \right>$​), pak se převedou na 8b řetězce pomocí konkrétních vzorečků.

**Block-based motion compensation** - Frame rozdělíme na bloky, hledáme pak co nejpodobnější v předchozím framu. Pokud nenajdeme, kódujeme původní blok, jinak motion vector společně s rozdílem  bloků.

**H.261** - Standard, implementuje ho formát CIF. Pro UV se používá poloviční rozlišení oproti Y. Každý frame rozdělíme na bloky 8x8, použijeme predikci (predikovat každý frame z předchozího) a diferenciální kódování (kóduje se rozdíl skutečných a rekonstruovaných dat). Pokud je předchozí frame moc odlišný (nebo neexistuje), komprimují se původní data. Pak aplikujeme DCT, kvantizaci, nakonec entropy coder s proměnnou délkou codeword (podobné JPEG).
**Macroblock prediction** - Používají se macroblocks (16x16 pro Y, 8x8 pro UV). Hledáme nejlepší match v předchozím framu jen pro Y, motion vector pro UV je poloviční.
**Loop filter** - Ostré hrany vedou k velkému rozptylu rozdílu. Použijeme smoothing filter na blok, který se používá pro predikci.
*DCT* - Na bloky 8x8
*Kvantizace* - Podobné JPEG, ale různé kvantizéry. Pro intra (blok původních hodnot) DC koeficient použijeme uniformní midrise (0 je hranice mezi 2 intervaly), pro ostatní uniformní midtread (0 je uprostřed intervalu). V hlavičce makrobloku uložíme, jaký kvantizér se použil.
*Kódování* - Jako JPEG, kódujeme dvojice (počet přeskočených 0, aktuální koeficient).
**Rate Control** - Transmission buffer drží fixní output rate. Může být prázdný, nebo přetékat, proto může požádat o změnu transmission rate (kodér podle toho zvolí kvantizér s větším/menším krokem pro více/méně nulových koeficientů). V nejhorším se zahazují framy.

### Standard MPEG

**MPEG** (*Moving Picture Experts Group*) - Standard pro kompresi videa a audia. Popisuje formát a dekodér, ale ne enkodér (implementace je na dané aplikaci). Popisuje i spoustu dalších věcí.

**MPEG-1** - YUV barevný prostor. Založený na 4:2:2 CCIR 601 (dvojnásobná vzorkovací frekvence pro Y).
*Filtr a subsampling*: Komponenta Y, zvolíme jen liché řádky, pak subsampling (každý druhý vzorek), navíc horizontální filtr. Komponenty U, V (chrominance), máme poloviční počet sloupců, zvolíme jen liché řádky, aplikujeme horizontální filtr a subsampling (každý druhý sloupec), pak vertikální filtr a subsampling (každý druhý řádek), dostaneme polovinu Y v obou směrech.
*H.261* - Stejný základ, bloky 8x8, rozlišují se inter (blok rozdílů) a intra (blok původních hodnot) bloky, motion-compensated prediction (na úrovni makrobloků), DCT, kvantizace, kódování, rate control.
**Typy framů** - Framy 3 typů, I-frame (intraframe, původní data), P-frame (rozdíl mezi aktuálním a nejbližším předchozím I/P, frame se tak predikuje na základě pohybu), B-frame (rozdíly mezi aktuálním a dvěma nejbližšími I/P před a za, predikce i na základě budoucnosti).
**GOP** (Group of Pictures) - Opakující se sekvence 10-30 framů fixních typů (vzor), umožňuje fast-forward, jump-forward.
*Motion compensation* - Standard nespecifikuje konkrétní metodu.
*Kódování a kvantizace* - Podobné JPEG, různé kvantizační tabulky pro různé framy (v souboru).
*Rate control* - Pokud je třeba, jako první se zahazují B-framy, pak se zvýší krok kvantizéru, nakonec se jště vynechávají koeficienty vyšších frekvencí.

**MPEG-2** - Řada vylepšení, 6 různých profilů.
**Více vrstev** - Základní vrstva kóduje signál nízké kvality, pak několik pomocných vrstev (diferenciální kódování, kvantizují rozdíly s menším krokem), při rekonstrukci sečteme vrstvy.
**Interlacing** - Framy rozdělíme na sudé a liché fieldy, predikce je pak založena na fieldech (v I framu predikujeme hodnoty v sudém fieldu z lichého), musí se ale upravit zig-zag scan.

**MPEG-4** - spoustu nových featur. **AVC/H.264** (Advanced Video Coding):
*Motion compensation* - Framy v určitých rozestupech jako I-frame (keyframe), pro ostatní (obousměrná) predikce (rozdíl a motion). Bloky 16x16 (makrobloky, ale mohou se dále dělit), v I-framech se hledá match.
*Chroma subsampling* a transformace podobná *DCT*
**Kontextově-adaptivní binární aritmetické kódování** - Celé jako jedno desetinné číslo. Pravděpodobnosti ale nejsou pevně dané, mění se během kódování dle předchozího znaku (např. jaký je nejčastější znak po e).

**MPEG-M** - Řada vylepšení. **HEVC/H.265** (High Efficiency Video Coding):
**Block structure** - Frame je místo makrobloků 16x16 rozdělený na tzv. coding tree unit (CTU), ty mohou mít různé velikosti. Je to jeden 2Nx2N luma blok a dva NxN chroma bloky.
**Quadtree partitioning** - Rozdělíme na 4 čtvercové podbloky, pokud nemá dobrou rekonstrukci, tak na další čtyři.
*Zig-zag scan* - Přes podbloky, v každém zase cik cak.

## ~~9. Detekce kolizí~~

**Kolize** - Dvojice těles s neprázdným průnikem. Čas a místo kontaktu, hloubka vnoření, vektor separace.

**Collider** - Aproximace tvaru. Např. sphere/circle, AABB, OBB, k-DOP (discrete oriented polytopes, dvojice rovnoběžných rovin), konvexní obal.

**Broad phase** - Najdeme dvojice objektů, které mohou potenciálně kolidovat. Pomocí space partitioning a bounding volumes.
**Narrow phase** - Počítáme detaily (jestli ke kolizi opravdu došlo, body kontaktu, hloubka vnoření).

**Sort and sweep algoritmus** - Projektujeme AABB na jednu osu, vytvoříme setříděný seznam začátků a konců intervalů. Procházíme, udržujeme seznam aktivních. Kolidovat mohou vždy ty, které jsou právě v seznamu aktivních. Pro další krok simulace můžeme jen přetřídit insertion sortem.

**Spatial partitioning** - Rozdělíme svět na části, kontrolujeme kolize jen v rámci skupin.
**Fixed/uniform/regular grid** - Pravidelná mřížka.
**Quadtree/Octree** - Rekurzivní podrozdělení. Adaptivní, rychlý update.
**BSP tree** - Rekurzivně na poloviny (zhruba stejně objektů), nadrovina s libovolnou orientací.
**k-d tree** - Rekurzivně na dvě části (ne přesně poloviny), střídají se osy.
**BVH** (bounding volume hierarchy) - Strom bounding volumes.

**Separating axes theorem** (SAT) - Dva konvexní tvary se neprotínají právě a pouze tehdy, pokud existuje alespoň jedna osa, na které se ortogonální projekce těchto tvarů neprotínají/nepřekrývají.

**Gilbert-Johnson-Keerthi algoritmus** (*GJK*) - Detekce kolize dvou konvexních tvarů, určení vzdálenosti a nejbližších bodů. Implicitní reprezentace (nekonečná množina bodů), tvary se protínají, pokud počátek leží v Minkowského rozdílu. Pomocí support function (pro vektor vrátí nejvzdálenější bod daným směrem) postupně vytváříme simplex, snažíme se obalit počátek (Voronoi regions a vector triple product pro určení). Začneme náhodným směrem, pak od bodu do počátku, pak normálu směrem k počátku (vector triple product), atd.

**Expanding polytope algoritmus** (*EPA*) - Navazuje na GJK algoritmus, umožňuje zjistit normálu a hloubku kolize, vrací tzv. minimum translation vector (*MTV*). Postupně rozšiřujeme simplex na polytope, od hrany nejblíž k počátku ve směru její normály ven (pomocí support function), dokud se délka a směr vektoru moc nemění.

----

**Discrete collision detection** - Detekce kolizí se provádí jen pro statický snapshot simulace.
*Tunneling* - Kolize nastane mezi dvěma kroky, nedetekujeme ji.
**Continuous collision detection** - Dívá se mezi kroky somulace, počítá time of impact. Uděláme konvexní obal dvou snapshotů (nebo jen primitivní tvar), pak detekujeme kolize. Pro time of impact předpokládáme lineární pohyb.
*Conservative advancement algoritmus* - Posouváme tělesa v každé iteraci blíž a blíž, dokud nejsou dost blízko. Tak určíme čas kolize. Hodí se pro komplexnější pohyb, ale může konvergovat pomalu.
