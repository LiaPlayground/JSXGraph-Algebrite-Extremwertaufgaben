<!--
author:   Sebastian Zug

email:    sebastian.zug@informatik.tu-freiberg.de

version:  2.0.0

language: de

narrator: Deutsch Female

comment:  Interaktives Aufgabenblatt zu Extremwertaufgaben für die
          Ingenieurausbildung. Dynamische Skizzen mit JSXGraph, Kontrolle
          der Antworten durch das Computer-Algebra-System Algebrite.

tags:     Mathematik, Analysis, Optimierung, Extremwertaufgaben, Ingenieurmathematik

import:   https://raw.githubusercontent.com/LiaTemplates/algebrite/0.7.1/README.md

import:   https://raw.githubusercontent.com/LiaTemplates/JSXGraph/0.0.3/README.md
-->

[![LiaScript](https://raw.githubusercontent.com/LiaScript/LiaScript/master/badges/course.svg)](https://LiaScript.github.io/course/?https://github.com/LiaPlayground/JSXGraph-Algebrite-Extremwertaufgaben/blob/main/README.md)

# Extremwertaufgaben

                        --{{0}}--
Willkommen zum interaktiven Übungsblatt Extremwertaufgaben. Drei Aufgaben aus
der Ingenieurpraxis und ein Selbsttest. Jede Aufgabe hat eine bewegliche
Skizze, und die Eingabefelder prüfen Ihre Antwort mathematisch statt als
Zeichenkette.

**Übungsblatt 7 · Mathematik für Ingenieure**

Extremwertaufgaben sind der Punkt, an dem aus Differentialrechnung
Ingenieurarbeit wird:
*Wieviel Blech verbraucht die Dose? Wie breit wird der Balken?*

<!-- style="border-left: 4px solid #1971c2; padding-left: 1rem" -->
> **So arbeiten Sie mit diesem Blatt**
>
> * 🔍 **Ziehen Sie an den Skizzen.** Regler und Punkte sind beweglich –
>   probieren Sie, bevor Sie rechnen.
> * ✏️ **Tippen Sie Ihre Lösung ein.** Ein Computer-Algebra-System vergleicht
>   sie mathematisch, nicht als Zeichenkette: `1/2`, `0,5`, `50%` und
>   `\frac{1}{2}` gelten als dieselbe Antwort. Nur vor einer Klammer braucht
>   es ein `*` – also `x*(30-2x)^2`, nicht `x(30-2x)^2`.
> * 💡 **Hinweise und Musterlösung** liegen hinter der Glühbirne und dem
>   Haken unter jedem Eingabefeld. Durchfallen kann man hier nicht, gezählt
>   werden nur die Versuche.

## 1. Das Kochrezept

                        --{{0}}--
Zuerst das Schema, nach dem jede Extremwertaufgabe abläuft. Sechs Schritte,
und der sechste ist der, den man am häufigsten vergisst.

1. **Zielgröße** benennen: Was soll maximal oder minimal werden?
2. **Zielfunktion** aufstellen – meist noch in *zwei* Variablen.
3. **Nebenbedingung** auflösen und einsetzen $\Rightarrow$ Funktion *einer*
   Variablen $f(x)$.
4. **Definitionsbereich** $D$ festlegen – physikalisch, nicht formal.
5. **Notwendige Bedingung** $f'(x)=0$ lösen, **hinreichende Bedingung**
   $f''(x_0)\neq 0$ prüfen.
6. **Ränder vergleichen** und das Ergebnis **mit Einheit** interpretieren.

                        --{{1}}--
Ziehen Sie den Punkt P entlang der blauen Kurve. Die gestrichelte Tangente
kippt mit, und der rote Punkt wandert auf der Ableitungskurve. Genau dann,
wenn die Tangente waagerecht liegt, sitzt der rote Punkt auf der x-Achse.

     {{1}}
🔍 **Ziehen Sie P.** Blau ist $f$, rot gestrichelt die Ableitung $f'$. Wann
liegt die Tangente waagerecht – und wo steht dann der rote Punkt?

``` javascript @JSX.Graph.withParams(`boundingbox="[-3, 5, 3, -4.5]" showNavigation="false" grid="true"`)
var f  = function (x) { return x * x * x - 3 * x; };
var df = function (x) { return 3 * x * x - 3; };

// f(x) in blau ...
var graph = board.create('functiongraph', [f, -2.2, 2.2],
  { strokeWidth: 3, strokeColor: '#1971c2' });

// ... und f'(x) rot gestrichelt darunter
board.create('functiongraph', [df, -2.2, 2.2],
  { strokeWidth: 2, strokeColor: '#c92a2a', dash: 2 });

var P = board.create('glider', [-1.4, f(-1.4), graph],
  { name: 'P', size: 5, strokeColor: '#e8590c', fillColor: '#e8590c' });

board.create('tangent', [P],
  { strokeColor: '#e8590c', strokeWidth: 2, dash: 2 });

// derselbe x-Wert auf der Ableitungskurve
var Q = board.create('point',
  [function () { return P.X(); }, function () { return df(P.X()); }],
  { name: "f'", size: 4, fixed: true, strokeColor: '#c92a2a', fillColor: '#c92a2a',
    label: { offset: [10, -4], fontSize: 15 } });

board.create('segment', [P, Q],
  { strokeColor: '#adb5bd', strokeWidth: 1, dash: 1, highlight: false });

board.create('text', [1.5, -2.6, function () {
  return 'x = ' + P.X().toFixed(2);
}], { fontSize: 16, cssStyle: 'font-family: monospace' });

board.create('text', [1.5, -3.3, function () {
  return "f'(x) = " + df(P.X()).toFixed(2);
}], { fontSize: 16, cssStyle: 'font-family: monospace; color: #c92a2a' });

board.create('text', [1.5, -4.0, function () {
  var d = df(P.X());
  if (Math.abs(d) > 0.05) { return 'Tangente kippt noch'; }
  return (P.X() > 0) ? 'Tiefpunkt (Minimum)' : 'Hochpunkt (Maximum)';
}], { fontSize: 16, strokeColor: '#2f9e44' });
```

     {{1}}
<!-- style="border-left: 4px solid #1971c2; padding-left: 1rem" -->
> Die rote Kurve schneidet die x-Achse an **zwei** Stellen, $x=-1$ und
> $x=+1$. Beide erfüllen $f'(x)=0$ – die eine gehört zum Hochpunkt, die
> andere zum Tiefpunkt. Die notwendige Bedingung findet also **Kandidaten**,
> sie entscheidet nicht.

### Aufwärmfrage

Welche Aussage ist korrekt?

[( )] Aus $f'(x_0)=0$ folgt, dass $f$ an der Stelle $x_0$ ein Extremum hat.
[(X)] $f'(x_0)=0$ ist notwendig, aber nicht hinreichend für ein lokales Extremum.
[( )] Ein Maximum auf $[a,b]$ kann nur an einer Stelle mit $f'(x)=0$ liegen.
[( )] Ist $f''(x_0)=0$, so liegt bei $x_0$ garantiert ein Sattelpunkt.
[[?]] Denken Sie an $f(x)=x^3$ bei $x_0=0$.
[[?]] Und denken Sie an $f(x)=x$ auf dem Intervall $[0,1]$.
*********************************************************************

$f'(x_0)=0$ ist **notwendig** für ein *inneres* Extremum einer
differenzierbaren Funktion – mehr nicht.

* $f(x)=x^3$ hat bei $0$ die Ableitung $0$, aber keinen Extremwert
  (Sattelpunkt). Also nicht hinreichend.
* $f(x)=x$ auf $[0,1]$ nimmt Maximum und Minimum an den **Rändern** an, ganz
  ohne Nullstelle der Ableitung. Deshalb Schritt 6 im Kochrezept.
* $f''(x_0)=0$ lässt alles offen: $x^4$ hat dort ein Minimum, $x^3$ einen
  Sattelpunkt.

*********************************************************************

## 2. Aufgabe A – Die Blechschachtel

                        --{{0}}--
Der Klassiker aus der Blechbearbeitung. Je größer der Ausschnitt, desto höher
die Schachtel – aber desto kleiner auch ihr Boden. Irgendwo dazwischen liegt
das Optimum.

> Aus einer quadratischen Blechtafel mit der Kantenlänge $a = 30\ \text{cm}$
> werden an den vier Ecken Quadrate der Seitenlänge $x$ ausgeklinkt. Die
> überstehenden Ränder werden zu einer oben offenen Schachtel hochgekantet.
>
> **Für welches $x$ wird das Volumen maximal?**

🔍 Verschieben Sie den Regler $x$ – grün ist der Boden, rot der Verschnitt:

``` javascript @JSX.Graph.withParams(`boundingbox="[-4, 34, 58, -6]" axis="false" showNavigation="false" keepaspectratio="true"`)
var s = board.create('slider', [[36, 30], [56, 30], [0.5, 2, 14.5]],
  { name: 'x', snapWidth: 0.1, size: 5, strokeColor: '#c92a2a', fillColor: 'white' });

var X    = function () { return s.Value(); };
var zero = function () { return 0; };
var far  = function () { return 30 - X(); };
var near = function () { return X(); };

var hidden = { visible: false, fixed: true, name: '' };

// --- Blechtafel 30 x 30 ---------------------------------------------------
board.create('polygon', [[0, 0], [30, 0], [30, 30], [0, 30]], {
  fillColor: '#dbe4ff', fillOpacity: 0.8, vertices: { visible: false },
  borders: { strokeColor: '#3b5bdb', strokeWidth: 2 }, fixed: true
});

// --- Boden der Schachtel --------------------------------------------------
var b1 = board.create('point', [near, near], hidden);
var b2 = board.create('point', [far,  near], hidden);
var b3 = board.create('point', [far,  far ], hidden);
var b4 = board.create('point', [near, far ], hidden);
board.create('polygon', [b1, b2, b3, b4], {
  fillColor: '#8ce99a', fillOpacity: 0.7, vertices: { visible: false },
  borders: { strokeColor: '#2f9e44', strokeWidth: 2, dash: 2 }
});

// --- die vier ausgeklinkten Eckquadrate -----------------------------------
function klinke(fx, fy) {
  var p1 = board.create('point', [fx, fy], hidden);
  var p2 = board.create('point', [function () { return fx() + X(); }, fy], hidden);
  var p3 = board.create('point', [function () { return fx() + X(); },
                                  function () { return fy() + X(); }], hidden);
  var p4 = board.create('point', [fx, function () { return fy() + X(); }], hidden);
  board.create('polygon', [p1, p2, p3, p4], {
    fillColor: '#ffa8a8', fillOpacity: 0.9, vertices: { visible: false },
    borders: { strokeColor: '#c92a2a', strokeWidth: 1 }
  });
}
klinke(zero, zero);
klinke(far,  zero);
klinke(far,  far );
klinke(zero, far );

// --- Beschriftung ---------------------------------------------------------
board.create('text', [36, 24, function () {
  return 'x = ' + X().toFixed(1) + ' cm';
}], { fontSize: 17, cssStyle: 'font-family: monospace' });

board.create('text', [36, 20, function () {
  return 'Boden: ' + (30 - 2 * X()).toFixed(1) + ' cm';
}], { fontSize: 17, cssStyle: 'font-family: monospace' });

board.create('text', [36, 16, function () {
  return 'V = ' + (X() * (30 - 2 * X()) * (30 - 2 * X())).toFixed(0) + ' cm³';
}], { fontSize: 17, cssStyle: 'font-family: monospace; color: #2b8a3e' });

board.create('text', [36, 10, 'rot = Verschnitt'],
  { fontSize: 14, strokeColor: '#c92a2a' });
board.create('text', [36, 7, 'grün = Boden'],
  { fontSize: 14, strokeColor: '#2f9e44' });
```

**a)** Zielfunktion (in $\text{cm}^3$, ohne Einheit eintippen):

$V(x) = $ [[x*(30 - 2x)^2]]
[[?]] Die Schachtel hat die Höhe $x$.
[[?]] Der Boden ist quadratisch – an **beiden** Seiten fällt je $x$ weg.
@Algebrite.check(`x*(30-2*x)^2`)

**b)** Welcher Definitionsbereich ist *physikalisch* sinnvoll?

[( )] alle reellen $x$
[( )] $0 \le x \le 30$
[(X)] $0 < x < 15$
[( )] $0 < x < 30$
[[?]] Was passiert bei $x = 15$ mit dem Boden?

**c)** Ableitung und Optimum:

$V'(x) = $ [[12x^2 - 240x + 900]]
[[?]] Multiplizieren Sie zuerst aus: $V(x)=4x^3-120x^2+900x$.
[[?]] Oder Produktregel: $V' = (30-2x)^2 + x\cdot 2(30-2x)\cdot(-2)$.
@Algebrite.check(`12*x^2-240*x+900`)

$x_{\text{opt}} = $ [[5]] $\ \text{cm}$, $\quad V_{\max} = $ [[2000]] $\ \text{cm}^3$
[[?]] $12x^2-240x+900 = 12(x-5)(x-15)$ – eine Nullstelle liegt nicht in $D$.
@Algebrite.check(`[ 5 ; 2000 ]`)
*********************************************************************

**Zielfunktion und Definitionsbereich**

$$
V(x) \;=\; \underbrace{(30-2x)^2}_{\text{Boden}}\cdot \underbrace{x}_{\text{Höhe}}
       \;=\; 4x^3 - 120x^2 + 900x, \qquad 0 < x < 15
$$

Die Bodenkante $30-2x$ muss positiv bleiben, und ohne Ausklinkung gibt es
keine Schachtel. Die Randwerte $x=0$ und $x=15$ liefern beide $V=0$ und
scheiden damit sofort aus.

**Notwendige und hinreichende Bedingung**

$$
V'(x) = 12x^2-240x+900 = 12\,(x-5)(x-15) \stackrel{!}{=} 0
\quad\Longrightarrow\quad x_1 = 5,\; x_2 = 15 \notin D
$$

$$
V''(x) = 24x-240, \qquad V''(5) = -120 < 0 \;\Rightarrow\; \text{Maximum}
$$

$$
V(5) = 5\cdot 20^2 = 2000\ \text{cm}^3 = 2\ \text{Liter}
$$

Die Schachtel ist $5\ \text{cm}$ hoch mit einem Boden von
$20 \times 20\ \text{cm}$. Bemerkenswert: Das Optimum liegt immer bei
$x = a/6$, unabhängig von der Tafelgröße.

🧮 Kontrolle im CAS – Doppelklick öffnet den Block zum Bearbeiten:

``` Maxima
V(x) = x * (30 - 2*x)^2

expand(V(x))                 -- ausmultipliziert
derivative(V(x), x)          -- V'(x)
roots(12*x^2 - 240*x + 900, x)
derivative(V(x), x, 2)       -- V''(x)
eval(24*x - 240, x, 5)       -- V''(5) < 0  ->  Maximum
eval(V(x), x, 5)             -- maximales Volumen

draw(V(x), x, 0, 15, "green mark=5 vline=5")
```
@Algebrite.pretty

*********************************************************************

## 3. Aufgabe B – Die Konservendose

                        --{{0}}--
Die erste Aufgabe, deren Lösung keine glatte Zahl mehr ist. Hier zeigt sich,
warum die Antwortprüfung mit einem Algebrasystem angenehmer ist als ein
String-Vergleich: Sie dürfen den exakten Ausdruck oder den gerundeten Wert
eintippen.

> Eine zylindrische Konservendose soll $V = 1\ \text{Liter} = 1000\ \text{cm}^3$
> fassen und dabei **möglichst wenig Blech** verbrauchen (Mantel + Boden +
> Deckel, Falze vernachlässigt).
>
> **Welcher Radius ist optimal?**

🔍 Der Regler ändert $r$, die Höhe $h$ zieht über das feste Volumen nach:

``` javascript @JSX.Graph.withParams(`boundingbox="[-32, 25, 32, -13]" axis="false" showNavigation="false" keepaspectratio="true"`)
var V = 1000;

var s = board.create('slider', [[-29, 21], [-15, 21], [4.0, 8.0, 8.5]],
  { name: 'r', snapWidth: 0.01, size: 5, strokeColor: '#e8590c', fillColor: 'white' });

var R = function () { return s.Value(); };
var H = function () { return V / (Math.PI * R() * R()); };
var O = function () { return 2 * Math.PI * R() * R() + 2 * V / R(); };
var rOpt = Math.pow(V / (2 * Math.PI), 1 / 3);   // 5.419... cm

var hidden = { visible: false, fixed: true, name: '' };

// --- Mantel als Rechteck --------------------------------------------------
var m1 = board.create('point', [function () { return -R(); }, function () { return 0; }], hidden);
var m2 = board.create('point', [R, function () { return 0; }], hidden);
var m3 = board.create('point', [R, H], hidden);
var m4 = board.create('point', [function () { return -R(); }, H], hidden);

board.create('polygon', [m1, m2, m3, m4], {
  fillColor: '#ffd8a8', fillOpacity: 0.8, vertices: { visible: false },
  borders: { strokeColor: '#e8590c', strokeWidth: 2 }
});

// --- Deckel und Boden als Ellipsen ---------------------------------------
board.create('curve', [
  function (t) { return R() * Math.cos(t); },
  function (t) { return H() + 0.3 * R() * Math.sin(t); },
  0, 2 * Math.PI
], { strokeColor: '#d9480f', strokeWidth: 2, fillColor: '#ffc078', fillOpacity: 0.9 });

board.create('curve', [
  function (t) { return R() * Math.cos(t); },
  function (t) { return 0.3 * R() * Math.sin(t); },
  Math.PI, 2 * Math.PI
], { strokeColor: '#d9480f', strokeWidth: 2, dash: 2 });

// --- Maßlinien -----------------------------------------------------------
board.create('segment', [m2, m3],
  { strokeColor: '#495057', strokeWidth: 1, dash: 1, highlight: false });

board.create('text', [function () { return R() + 1.5; },
                      function () { return H() / 2; }, function () {
  return 'h = ' + H().toFixed(2) + ' cm';
}], { fontSize: 15, anchorY: 'middle', cssStyle: 'font-family: monospace' });

board.create('text', [function () { return 0; }, function () { return -6; },
  function () { return 'r = ' + R().toFixed(2) + ' cm'; }],
  { fontSize: 15, anchorX: 'middle', cssStyle: 'font-family: monospace' });

// --- Zielgröße ----------------------------------------------------------
board.create('text', [11, 21, function () {
  return 'O(r) = ' + O().toFixed(1) + ' cm²';
}], { fontSize: 17, cssStyle: 'font-family: monospace; color: #c92a2a' });

board.create('text', [11, 17, function () {
  return 'V    = ' + (Math.PI * R() * R() * H()).toFixed(0) + ' cm³';
}], { fontSize: 15, cssStyle: 'font-family: monospace; color: #2b8a3e' });

board.create('text', [11, 13, function () {
  return (Math.abs(R() - rOpt) > 0.2) ? '' : 'h = 2r  -  Optimum!';
}], { fontSize: 16, strokeColor: '#2f9e44' });
```

**a)** Nebenbedingung $V = \pi r^2 h = 1000$ nach $h$ auflösen und einsetzen:

$h = $ [[1000/(pi r^2)]]
@Algebrite.check(`1000/(pi*r^2)`)

$O(r) = $ [[2 pi r^2 + 2000/r]]
[[?]] Der Mantel ist ein Rechteck mit Umfang $2\pi r$ mal Höhe $h$.
[[?]] $2\pi r \cdot \dfrac{1000}{\pi r^2} = \dfrac{2000}{r}$
@Algebrite.check(`2*pi*r^2 + 2000/r`)

**b)** Optimum, auf zwei Nachkommastellen (der exakte Ausdruck wird auch
akzeptiert):

$r_{\text{opt}} = $ [[5.42]] $\ \text{cm}$
[[?]] $O'(r) = 4\pi r - \dfrac{2000}{r^2}$
[[?]] Multiplizieren Sie $O'(r)=0$ mit $r^2$, dann $r^3 = \dfrac{500}{\pi}$.
@Algebrite.check2(`(500/pi)^(1/3)`, 0.02, units=0)

$h = $ [[10.84]] $\ \text{cm}$, $\quad O_{\min} = $ [[553.6]] $\ \text{cm}^2$
@Algebrite.check2(`[ 2*(500/pi)^(1/3) ; 6*pi*(500/pi)^(2/3) ]`, [ 0.05 ; 1 ], units=0)
*********************************************************************

**Zielfunktion**

$$
h = \frac{1000}{\pi r^2},
\qquad
O(r) = \underbrace{2\pi r^2}_{\text{Boden + Deckel}} + \underbrace{2\pi r h}_{\text{Mantel}}
     = 2\pi r^2 + \frac{2000}{r}
$$

Die beiden Anteile laufen gegeneinander: Boden und Deckel wachsen quadratisch
mit $r$, der Mantel fällt wie $1/r$. Dieser Widerstreit erzeugt das Minimum.

**Optimum**

$$
O'(r) = 4\pi r - \frac{2000}{r^2} \stackrel{!}{=} 0
\;\Big|\cdot r^2
\quad\Longrightarrow\quad
r = \sqrt[3]{\frac{500}{\pi}} \approx 5{,}42\ \text{cm}
$$

$$
O''(r) = 4\pi + \frac{4000}{r^3} > 0 \quad \text{für alle } r>0
\;\Rightarrow\; \text{Minimum}
$$

$$
h = \frac{1000}{\pi r^2} \approx 10{,}84\ \text{cm},
\qquad O_{\min} \approx 553{,}6\ \text{cm}^2
$$

**Die Faustregel.** Aus $r^3 = \dfrac{V}{2\pi}$ folgt
$h = \dfrac{V}{\pi r^2} = 2r = d$ – **unabhängig vom Volumen**. Die optimale
Dose ist so hoch wie breit, ihr Querschnitt ein Quadrat. Dass echte
Getränkedosen schlanker sind, liegt am Modell: Falze verbrauchen Blech,
Deckel sind dicker als der Mantel, Paletten geben Formate vor.

🧮 Kontrolle im CAS:

``` Maxima
V = 1000
O(r) = 2*pi*r^2 + 2*V/r

derivative(O(r), r)              -- O'(r)
r0 = (V/(2*pi))^(1/3)            -- Nullstelle von O'
float(r0)                        -- optimaler Radius in cm
float(V/(pi*r0^2))               -- zugehörige Höhe
float(O(r0))                     -- minimale Oberfläche
float(V/(pi*r0^2)) / float(r0)   -- das Verhältnis h/r
```
@Algebrite.pretty

*********************************************************************

## 4. Aufgabe C – Balken aus dem Rundholz

                        --{{0}}--
Aus der Festigkeitslehre, und die älteste Extremwertaufgabe der
Ingenieurausbildung überhaupt – Galilei hat sie 1638 diskutiert. Neu ist hier
nur, dass die Nebenbedingung der Satz des Pythagoras ist.

> Aus einem Rundholz mit dem Durchmesser $d = 30\ \text{cm}$ soll ein Balken mit
> rechteckigem Querschnitt $b \times h$ geschnitten werden. Bei Biegung um die
> waagerechte Achse ist das **Widerstandsmoment**
>
> $$W = \frac{b\,h^2}{6}$$
>
> maßgebend. **Welcher Querschnitt trägt am meisten?**

🔍 Ziehen Sie am Regler $b$ – die Ecken bleiben auf dem Stamm:

``` javascript @JSX.Graph.withParams(`boundingbox="[-24, 26, 24, -22]" axis="false" showNavigation="false" keepaspectratio="true"`)
var d = 30;

var s = board.create('slider', [[-23, 21], [-9, 21], [2, 26, 29.6]],
  { name: 'b', snapWidth: 0.01, size: 5, strokeColor: '#5c3d0c', fillColor: 'white' });

var B = function () { return s.Value(); };
var H = function () { return Math.sqrt(d * d - B() * B()); };
var W = function () { return B() * H() * H() / 6; };
var bOpt = d / Math.sqrt(3);                      // 17.32... cm

var hidden = { visible: false, fixed: true, name: '' };

// --- Stammquerschnitt -----------------------------------------------------
board.create('circle', [[0, 0], 15], {
  strokeColor: '#8b6914', strokeWidth: 3,
  fillColor: '#f0dbb5', fillOpacity: 0.7, fixed: true, highlight: false
});

// --- Balkenquerschnitt ----------------------------------------------------
var q1 = board.create('point', [function () { return -B() / 2; }, function () { return -H() / 2; }], hidden);
var q2 = board.create('point', [function () { return  B() / 2; }, function () { return -H() / 2; }], hidden);
var q3 = board.create('point', [function () { return  B() / 2; }, function () { return  H() / 2; }], hidden);
var q4 = board.create('point', [function () { return -B() / 2; }, function () { return  H() / 2; }], hidden);

board.create('polygon', [q1, q2, q3, q4], {
  fillColor: '#74c0fc', fillOpacity: 0.65, vertices: { visible: false },
  borders: { strokeColor: '#1864ab', strokeWidth: 2 }
});

// --- Diagonale = Durchmesser ---------------------------------------------
board.create('segment', [q1, q3],
  { strokeColor: '#c92a2a', strokeWidth: 1, dash: 2, highlight: false });
board.create('text', [7.5, -4.5, 'd = 30'],
  { fontSize: 13, strokeColor: '#c92a2a' });

// --- Beschriftung ---------------------------------------------------------
board.create('text', [function () { return 0; }, function () { return -H() / 2 - 2.5; },
  function () { return 'b = ' + B().toFixed(2); }],
  { fontSize: 15, anchorX: 'middle', cssStyle: 'font-family: monospace' });

board.create('text', [function () { return B() / 2 + 2.8; }, function () { return 0; },
  function () { return 'h = ' + H().toFixed(2); }],
  { fontSize: 15, anchorY: 'middle', cssStyle: 'font-family: monospace' });

board.create('text', [1, 21, function () {
  return 'W = ' + W().toFixed(0) + ' cm³';
}], { fontSize: 17, cssStyle: 'font-family: monospace; color: #1864ab' });

board.create('text', [1, 17.5, function () {
  return 'h/b = ' + (H() / B()).toFixed(3);
}], { fontSize: 15, cssStyle: 'font-family: monospace' });

board.create('text', [-21, -22, function () {
  return (Math.abs(B() - bOpt) > 0.5) ? '' : 'Optimum:  h/b = Wurzel(2)';
}], { fontSize: 16, strokeColor: '#2f9e44' });
```

**a)** Nebenbedingung einsetzen:

$h = $ [[sqrt(900 - b^2)]]
[[?]] Die Diagonale des Querschnitts ist der Stammdurchmesser.
@Algebrite.check(`sqrt(900-b^2)`)

$W(b) = $ [[b*(900 - b^2)/6]]
[[?]] In $W$ steht $h^2$, nicht $h$ – die Wurzel fällt also weg.
@Algebrite.check(`b*(900-b^2)/6`)

**b)** Optimaler Querschnitt:

$b = $ [[17.32]] $\ \text{cm}$, $\quad h = $ [[24.49]] $\ \text{cm}$, $\quad W_{\max} = $ [[1732]] $\ \text{cm}^3$
[[?]] $W'(b) = 150 - \dfrac{b^2}{2}$
[[?]] $b^2 = 300$, also $h = \sqrt{900-300} = \sqrt{600}$
@Algebrite.check2(`[ 30/sqrt(3) ; sqrt(600) ; 1000*sqrt(3) ]`, [ 0.02 ; 0.02 ; 1 ], units=0)
*********************************************************************

**Zielfunktion**

$$
b^2 + h^2 = d^2 = 900
\qquad\Longrightarrow\qquad
W(b) = \frac{b\,(900-b^2)}{6} = 150b - \frac{b^3}{6},
\quad 0 < b < 30
$$

Ein Glücksfall: In der Zielfunktion steht $h^2$, deshalb verschwindet die
Wurzel aus der Nebenbedingung und $W$ ist ein einfaches Polynom.

**Optimum**

$$
W'(b) = 150 - \frac{b^2}{2} \stackrel{!}{=} 0
\quad\Longrightarrow\quad
b = \sqrt{300} = \frac{d}{\sqrt{3}} \approx 17{,}32\ \text{cm},
\qquad W''(b) = -b < 0
$$

$$
h = \sqrt{600} = d\sqrt{\tfrac{2}{3}} \approx 24{,}49\ \text{cm},
\qquad
W_{\max} = 1000\sqrt{3} \approx 1732\ \text{cm}^3
$$

**Die Zimmermannsregel.** Es ist
$\dfrac{h}{b} = \sqrt{2} \approx 1{,}414$, unabhängig vom Durchmesser. Die
alte Praxisregel $h:b = 7:5$ ist genau die Näherung dafür.

Optimiert man statt $W \sim bh^2$ die **Biegesteifigkeit**
$I = \tfrac{1}{12}bh^3$, ergibt sich $h:b = \sqrt{3}:1$; wer schlicht die
**Holzausbeute** $A = bh$ maximiert, landet beim Quadrat. Drei Ziele, drei
Optima, ein Rundholz – die Modellbildung entscheidet, nicht die Rechnung.

🧮 Kontrolle im CAS:

``` Maxima
d = 30
W(b) = b * (d^2 - b^2) / 6

expand(W(b))                     -- 150b - b^3/6
derivative(W(b), b)              -- W'(b)
solve(150 - b^2/2, b)            -- nur die positive Wurzel liegt im Bereich
float(sqrt(300))                 -- optimale Breite
float(sqrt(d^2 - 300))           -- optimale Höhe
float(sqrt(d^2 - 300)/sqrt(300)) -- Verhältnis h/b
float(W(sqrt(300)))              -- maximales Widerstandsmoment

draw(W(b), b, 0, 30, "blue width=3 mark=17.3205 vline=17.3205")
```
@Algebrite.pretty

*********************************************************************

## 5. Randextrema und typische Fallen

                        --{{0}}--
Der Teil, an dem in der Klausur die Punkte verloren gehen. Ziehen Sie die
beiden roten Punkte a und b. Der grüne Punkt markiert den größten
Funktionswert im Intervall – beobachten Sie, wann er auf einen Randpunkt
springt.

Auf einem **abgeschlossenen** Intervall muss das Maximum nicht dort liegen, wo
$f'(x)=0$ gilt. 🔍 Verschieben Sie $a$ und $b$:

``` javascript @JSX.Graph.withParams(`boundingbox="[-3.2, 6.5, 3.2, -6.5]" showNavigation="false" grid="true"`)
var f = function (x) { return x * x * x - 3 * x; };

board.create('functiongraph', [f, -2.3, 2.3],
  { strokeWidth: 1, strokeColor: '#adb5bd', dash: 2, fixed: true });

// unsichtbare Schiene, auf der a und b laufen
var schiene = board.create('segment', [[-2.3, 0], [2.3, 0]],
  { strokeColor: '#dee2e6', strokeWidth: 8, fixed: true, highlight: false });

var a = board.create('glider', [-2.0, 0, schiene],
  { name: 'a', size: 5, strokeColor: '#c92a2a', fillColor: '#c92a2a' });
var b = board.create('glider', [2.1, 0, schiene],
  { name: 'b', size: 5, strokeColor: '#c92a2a', fillColor: '#c92a2a' });

var lo = function () { return Math.min(a.X(), b.X()); };
var hi = function () { return Math.max(a.X(), b.X()); };

board.create('functiongraph', [f, lo, hi],
  { strokeWidth: 4, strokeColor: '#1971c2' });

function argmax() {
  var l = lo(), h = hi();
  var cand = [l, h];
  if (-1 > l && h > -1) { cand.push(-1); }
  if (1 > l && h > 1) { cand.push(1); }
  var best = cand[0];
  cand.forEach(function (c) { if (f(c) > f(best)) { best = c; } });
  return best;
}

board.create('point', [function () { return argmax(); },
                       function () { return f(argmax()); }],
  { name: 'Max', size: 6, strokeColor: '#2f9e44', fillColor: '#2f9e44',
    fixed: true, label: { offset: [10, 10], fontSize: 16 } });

board.create('text', [-3.0, 6.0, function () {
  var m = argmax();
  var innen = (Math.abs(m + 1) < 0.001) || (Math.abs(m - 1) < 0.001);
  return innen ? "Maximum im Inneren:  Ableitung = 0"
               : "Maximum am RAND:  Ableitung ungleich 0";
}], { fontSize: 16, cssStyle: 'font-weight: bold' });

board.create('text', [-3.0, 5.3, function () {
  return 'x* = ' + argmax().toFixed(2) + ',   f(x*) = ' + f(argmax()).toFixed(2);
}], { fontSize: 15, cssStyle: 'font-family: monospace' });
```

<!-- style="border-left: 4px solid #c92a2a; padding-left: 1rem" -->
> **Merksatz:** Auf $[a,b]$ sind die Kandidaten für das globale Maximum
>
> $$\{x : f'(x)=0\} \;\cup\; \{a,\;b\} \;\cup\; \{\text{Knickstellen}\}$$
>
> Man rechnet alle aus und vergleicht die Funktionswerte. Fertig.

Ordnen Sie zu, welcher Fehler jeweils vorliegt:

[  [Rand vergessen]  [Nebenbedingung nicht eingesetzt]  [hinreichende Bedingung nicht geprüft]  ]
[        ( )                       ( )                                  (X)                     ] In Aufgabe A wurde $V'(5)=0$ berechnet und sofort „Maximum“ geschrieben.
[        ( )                       (X)                                  ( )                     ] In Aufgabe B wurde $O(r,h)=2\pi r^2+2\pi rh$ nach $r$ abgeleitet, $h$ blieb stehen.
[        (X)                       ( )                                  ( )                     ] Eine Kostenfunktion ist auf $[1;5]$ streng monoton fallend, es wird „kein Minimum“ notiert.
[[?]] Eine Funktion ohne Nullstelle der Ableitung kann trotzdem ein Minimum haben.
*********************************************************************

* **Zeile 1** – $f'(x_0)=0$ allein sagt nichts. Es fehlt $V''(5)=-120<0$.
* **Zeile 2** – Eine Funktion zweier Variabler kann man nicht nach dem
  Schema „$f'=0$“ behandeln. Erst die Nebenbedingung einsetzen, dann ableiten.
* **Zeile 3** – Streng monoton fallend auf $[1;5]$ heißt: Das Minimum liegt
  bei $x=5$, am rechten Rand. Es existiert sehr wohl.

*********************************************************************

## 6. Selbsttest

                        --{{0}}--
Zum Abschluss eine Aufgabe, die Sie komplett alleine durchrechnen. Die Zahlen
gehen glatt auf – wenn bei Ihnen krumme Werte herauskommen, steckt der Fehler
meist schon in der Nebenbedingung.

> Ein **oben offener** Behälter mit quadratischer Grundfläche soll
> $V = 32\ \text{dm}^3$ fassen. Gesucht ist der minimale Materialverbrauch
> (Boden und vier Wände).

$a$ sei die Grundkante, $h$ die Höhe.

**a)** $\;h = $ [[32/a^2]]
[[?]] $V = a^2 h$
@Algebrite.check(`32/a^2`)

**b)** $\;O(a) = $ [[a^2 + 128/a]]
[[?]] Boden: $a^2$. Vier Wände: $4ah$. Der Deckel fehlt!
@Algebrite.check(`a^2 + 128/a`)

**c)** $\;O'(a) = $ [[2a - 128/a^2]]
@Algebrite.check(`2*a - 128/a^2`)

**d)** $a = $ [[4]] $\ \text{dm}$, $\quad h = $ [[2]] $\ \text{dm}$, $\quad O_{\min} = $ [[48]] $\ \text{dm}^2$
[[?]] $O'(a)=0 \Leftrightarrow 2a^3 = 128$, also $a^3 = 64$
@Algebrite.check(`[ 4 ; 2 ; 48 ]`)
*********************************************************************

$$
h = \frac{32}{a^2}, \qquad
O(a) = a^2 + 4ah = a^2 + \frac{128}{a}, \qquad a > 0
$$

$$
O'(a) = 2a - \frac{128}{a^2} \stackrel{!}{=} 0
\;\Big|\cdot a^2 \;\Longrightarrow\; a^3 = 64 \;\Longrightarrow\; a = 4\ \text{dm}
$$

$$
O''(a) = 2 + \frac{256}{a^3} > 0 \;\Rightarrow\; \text{Minimum},
\qquad h = 2\ \text{dm} = \frac{a}{2},
\qquad O_{\min} = 48\ \text{dm}^2
$$

Auch hier ist die Erkenntnis die Proportion: Beim **offenen** Behälter ist
$h = a/2$. Beim **geschlossenen** Quader käme der Würfel heraus, $h=a$. Der
fehlende Deckel macht den Behälter flach.

🧮 Kontrolle im CAS:

``` Maxima
V = 32
O(a) = a^2 + 4*a*(V/a^2)

expand(O(a))
derivative(O(a), a)
(128/2)^(1/3)                -- aus 2a^3 = 128 folgt a = 4
eval(O(a), a, 4)             -- minimale Oberfläche
eval(V/a^2, a, 4)            -- zugehörige Höhe

draw(O(a), a, 1, 10, "red width=3 mark=4 vline=4 ymax=200")
```
@Algebrite.pretty

*********************************************************************

## 7. Rückmeldung und Lizenz

Wie sicher fühlen Sie sich jetzt bei Extremwertaufgaben?

[(1)] Sitzt – ich könnte das an der Tafel vorrechnen.
[(2)] Das Schema ist klar, beim Aufstellen der Zielfunktion hänge ich noch.
[(3)] Die Rechnung geht, die Modellbildung nicht.
[(4)] Ich brauche mehr Übung.

Was ist unklar geblieben? Die Antwort bleibt lokal in Ihrem Browser.

[[___ ___ ___]]

---

                        --{{0}}--
Zum Schluss noch, woraus das Blatt besteht, falls Sie selbst solche
Materialien erstellen möchten.

Gebaut aus einer einzigen Markdown-Datei, alles läuft im Browser:
[LiaScript](https://liascript.github.io) als Kursformat,
[JSXGraph](https://jsxgraph.org) für die beweglichen Skizzen und
[Algebrite](http://algebrite.org) für Rechenblöcke und Antwortprüfung.
Der Quelltext liegt auf
[GitHub](https://github.com/LiaPlayground/JSXGraph-Algebrite-Extremwertaufgaben).

<!-- style="font-size: 90%" -->
> Dieses Aufgabenblatt steht unter
> [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.de).
> Sie dürfen es teilen, umarbeiten und in eigene Lehrveranstaltungen
> übernehmen – unter Nennung der Quelle und unter gleichen Bedingungen.
