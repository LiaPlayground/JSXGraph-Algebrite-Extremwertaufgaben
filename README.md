<!--
author:   Sebastian Zug

email:    sebastian.zug@informatik.tu-freiberg.de

version:  1.0.0

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
Herzlich willkommen zum interaktiven Übungsblatt Extremwertaufgaben. Auf den
folgenden Seiten optimieren Sie Blechzuschnitte, Weideflächen, Konservendosen
und Holzbalken. Jede Aufgabe hat eine bewegliche Skizze, in der Sie die freie
Größe selbst verschieben können, und Eingabefelder, die Ihre Antwort nicht
mit einer Zeichenkette vergleichen, sondern mathematisch prüfen.

**Übungsblatt 7 · Mathematik für Ingenieure**

Extremwertaufgaben sind der Punkt, an dem aus Differentialrechnung
Ingenieurarbeit wird:
*Wieviel Blech verbraucht die Dose? Wie breit wird der Balken? Wo sitzt das Optimum?*

<!-- style="border-left: 4px solid #1971c2; padding-left: 1rem" -->
> **So arbeiten Sie mit diesem Blatt**
>
> * **Ziehen Sie an den Grafiken.** Jede Skizze ist ein *JSXGraph*-Board:
>   Schieberegler, Punkte und Ecken lassen sich mit der Maus bewegen.
> * **Tippen Sie Ihre Lösung ein.** Die Eingabefelder werden vom
>   Computer-Algebra-System *Algebrite* ausgewertet. `1/2`, `0,5`, `50%` und
>   `\frac{1}{2}` gelten alle als dieselbe Antwort – Sie müssen nicht die
>   "richtige" Schreibweise raten. Nur eines erwartet das System:
>   **vor einer Klammer ein `*`** – also `x*(30-2x)^2`, nicht
>   `x(30-2x)^2`, denn das wäre ein Funktionsaufruf.
> * **Rechnen Sie nach.** Die grauen Codeblöcke sind lebendig: Doppelklick
>   öffnet sie zum Bearbeiten, der Play-Button rechnet neu.
> * **Hinweise sind erlaubt.** Die Glühbirne unter jeder Aufgabe gibt
>   gestaffelte Tipps, der Haken darunter die ausführliche Musterlösung.

                        --{{1}}--
Ein Hinweis zur Bewertung: In LiaScript kann man nicht durchfallen. Sie dürfen
beliebig oft probieren, gezählt werden nur die Versuche. Nutzen Sie das.

     {{1}}
| Zeichen | Bedeutung                                                  |
| :-----: | ---------------------------------------------------------- |
|   🔍    | bewegliche Skizze – erst ziehen, dann rechnen               |
|   🧮    | ausführbarer CAS-Block – Doppelklick öffnet ihn             |
|   💡    | Knopf unter dem Eingabefeld: gestaffelte Hinweise           |
|   ✓     | Knopf unter dem Eingabefeld: Musterlösung anzeigen          |

## 1. Das Kochrezept

                        --{{0}}--
Bevor es an die Aufgaben geht, das Schema, nach dem jede Extremwertaufgabe
abgearbeitet wird. Sechs Schritte, und der sechste ist der, den man im Studium
am häufigsten vergisst.

Jede Extremwertaufgabe läuft nach demselben Schema ab:

1. **Zielgröße** benennen: Was soll maximal oder minimal werden?
   ($V$, $A$, $O$, $W$, Kosten, Verluste ...)
2. **Zielfunktion** aufstellen – meist noch in *zwei* Variablen.
3. **Nebenbedingung** auflösen und einsetzen $\Rightarrow$ Funktion *einer*
   Variablen $f(x)$.
4. **Definitionsbereich** $D$ festlegen. Nicht formal, sondern *physikalisch*:
   negative Blechdicken gibt es nicht.
5. **Notwendige Bedingung** $f'(x)=0$ lösen, **hinreichende Bedingung**
   $f''(x_0)\neq 0$ prüfen.
6. **Ränder vergleichen** und das Ergebnis **mit Einheit** interpretieren.

### Was $f'(x_0)=0$ anschaulich heißt

                        --{{0}}--
Ziehen Sie den Punkt P entlang der Kurve. Die gestrichelte Tangente kippt mit.
Genau dort, wo sie waagerecht liegt, ist die Ableitung null. Beachten Sie, dass
das zweimal passiert: einmal im Hochpunkt, einmal im Tiefpunkt. Die notwendige
Bedingung allein unterscheidet die beiden nicht.

🔍 **Ziehen Sie P**, bis die Tangente waagerecht liegt:

``` javascript @JSX.Graph.withParams(`boundingbox="[-3, 4.5, 3, -4.5]" showNavigation="false" grid="true"`)
var f  = function (x) { return x * x * x - 3 * x; };
var df = function (x) { return 3 * x * x - 3; };

var graph = board.create('functiongraph', [f, -2.2, 2.2],
  { strokeWidth: 3, strokeColor: '#1971c2', name: 'f', withLabel: false });

var P = board.create('glider', [-2, f(-2), graph],
  { name: 'P', size: 5, strokeColor: '#e8590c', fillColor: '#e8590c' });

board.create('tangent', [P],
  { strokeColor: '#e8590c', strokeWidth: 2, dash: 2 });

board.create('text', [-2.8, 4.0, function () {
  return 'x = ' + P.X().toFixed(2);
}], { fontSize: 16, cssStyle: 'font-family: monospace' });

board.create('text', [-2.8, 3.4, function () {
  return "f'(x) = " + df(P.X()).toFixed(2);
}], { fontSize: 16, cssStyle: 'font-family: monospace' });

board.create('text', [-2.8, 2.8, function () {
  var d = df(P.X());
  if (Math.abs(d) > 0.05) { return 'noch nicht waagerecht'; }
  return (6 * P.X() > 0) ? "Tiefpunkt  (zweite Ableitung > 0)"
                         : "Hochpunkt  (zweite Ableitung kleiner 0)";
}], { fontSize: 16, strokeColor: '#2f9e44' });
```

                        --{{1}}--
Und so sieht derselbe Vorgang im Computer-Algebra-System aus. Der Block ist
ausführbar: Doppelklick öffnet ihn, dann können Sie die Funktion austauschen
und erneut rechnen.

     {{1}}
🧮 **Dieselbe Rechnung im CAS** – Doppelklick zum Bearbeiten:

``` Maxima
f(x) = x^3 - 3*x

derivative(f(x), x)          -- notwendige Bedingung: das = 0 setzen
roots(3*x^2 - 3, x)          -- die beiden Kandidaten
derivative(f(x), x, 2)       -- hinreichende Bedingung
eval(6*x, x, 1)              -- f''(1)  > 0  ->  Tiefpunkt
eval(6*x, x, -1)             -- f''(-1) < 0  ->  Hochpunkt
```
@Algebrite.pretty

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
Aufgabe A, der Klassiker aus der Blechbearbeitung. Aus einer quadratischen
Tafel werden an den vier Ecken Quadrate ausgeklinkt, die Ränder werden
hochgekantet. Je größer der Ausschnitt, desto höher die Schachtel –
aber desto kleiner auch ihr Boden. Irgendwo dazwischen liegt das Optimum.

> Aus einer quadratischen Blechtafel mit der Kantenlänge $a = 30\ \text{cm}$
> werden an den vier Ecken Quadrate der Seitenlänge $x$ ausgeklinkt. Die
> überstehenden Ränder werden zu einer oben offenen Schachtel hochgekantet.
>
> **Für welches $x$ wird das Volumen maximal?**

🔍 Verschieben Sie den Regler $x$ und beobachten Sie Boden (grün) und
Verschnitt (rot):

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

### a) Zielfunktion

                        --{{0}}--
Schritt zwei und drei des Kochrezepts in einem: Boden mal Höhe. Achten Sie
darauf, dass die Bodenkante an *beiden* Seiten um x kürzer wird.

Stellen Sie $V$ als Funktion von $x$ auf (in $\text{cm}^3$, ohne Einheit
eintippen):

$V(x) = $ [[x*(30 - 2x)^2]]
[[?]] Die Schachtel hat die Höhe $x$.
[[?]] Der Boden ist quadratisch – an **beiden** Seiten fällt je $x$ weg.
[[?]] $V = \text{Grundfläche} \cdot \text{Höhe}$
@Algebrite.check(`x*(30-2*x)^2`)
*********************************************************************

$$
V(x) \;=\; \underbrace{(30-2x)^2}_{\text{Boden}}\cdot \underbrace{x}_{\text{Höhe}}
       \;=\; 4x^3 - 120x^2 + 900x
$$

Beide Schreibweisen werden akzeptiert – das CAS vergleicht die
Funktionen, nicht die Zeichenketten. Probieren Sie ruhig
`4x^3-120x^2+900x` oder `\frac{8x^3-240x^2+1800x}{2}`.

*********************************************************************

### b) Definitionsbereich

Welcher Definitionsbereich ist *physikalisch* sinnvoll?

[( )] alle reellen $x$
[( )] $0 \le x \le 30$
[(X)] $0 < x < 15$
[( )] $0 < x < 30$
[[?]] Was passiert bei $x = 15$ mit dem Boden?
*********************************************************************

Die Bodenkante $30-2x$ muss positiv bleiben, also $x < 15$. Und ohne
Ausklinkung ($x=0$) gibt es keine Schachtel. Die Randwerte $x=0$ und $x=15$
liefern beide $V=0$ – formal darf man sie mitnehmen, sie scheiden als
Maximum aber sofort aus. Genau das ist Schritt 6 des Kochrezepts.

*********************************************************************

### c) Ableiten und lösen

                        --{{0}}--
Jetzt die notwendige Bedingung. Tippen Sie erst die Ableitung ein, dann die
Lösung.

$V'(x) = $ [[12x^2 - 240x + 900]]
[[?]] Multiplizieren Sie zuerst aus: $V(x)=4x^3-120x^2+900x$.
[[?]] Oder Produktregel: $V' = (30-2x)^2 + x\cdot 2(30-2x)\cdot(-2)$.
@Algebrite.check(12*x^2-240*x+900)

🔍 Ziehen Sie $P$ auf den Hochpunkt und lesen Sie ab:

``` javascript @JSX.Graph.withParams(`boundingbox="[-1.8, 2450, 16.5, -420]" showNavigation="false" grid="true"`)
var V  = function (x) { return x * (30 - 2 * x) * (30 - 2 * x); };
var dV = function (x) { return 12 * x * x - 240 * x + 900; };

var g = board.create('functiongraph', [V, 0, 15],
  { strokeWidth: 3, strokeColor: '#2f9e44' });

board.create('functiongraph', [dV, 0, 15],
  { strokeWidth: 2, strokeColor: '#c92a2a', dash: 2 });

var P = board.create('glider', [2, V(2), g],
  { name: 'P', size: 5, strokeColor: '#1971c2', fillColor: '#1971c2' });

board.create('tangent', [P], { strokeColor: '#1971c2', strokeWidth: 2, dash: 1 });

board.create('text', [9.4, 2350, function () {
  return 'x  = ' + P.X().toFixed(2) + ' cm';
}], { fontSize: 16, cssStyle: 'font-family: monospace' });

board.create('text', [9.4, 2200, function () {
  return 'V  = ' + V(P.X()).toFixed(0) + ' cm³';
}], { fontSize: 16, cssStyle: 'font-family: monospace; color: #2b8a3e' });

board.create('text', [9.4, 2050, function () {
  return "V' = " + dV(P.X()).toFixed(0);
}], { fontSize: 16, cssStyle: 'font-family: monospace; color: #c92a2a' });

board.create('text', [1.2, -300, "rot gestrichelt: V'(x)"],
  { fontSize: 14, strokeColor: '#c92a2a' });
```

Optimale Ausklinkung und zugehöriges Volumen:

$x_{\text{opt}} = $ [[5]] $\ \text{cm}$, $\quad V_{\max} = $ [[2000]] $\ \text{cm}^3$
[[?]] $12x^2-240x+900 = 12(x^2-20x+75)$
[[?]] Faktorisiert: $12(x-5)(x-15)$ – eine der beiden Nullstellen liegt nicht in $D$.
@Algebrite.check([ 5 ; 2000 ])
*********************************************************************

**Notwendige Bedingung**

$$
V'(x) = 12x^2-240x+900 = 12\,(x-5)(x-15) \stackrel{!}{=} 0
\quad\Longrightarrow\quad x_1 = 5,\; x_2 = 15 \notin D
$$

**Hinreichende Bedingung**

$$
V''(x) = 24x-240, \qquad V''(5) = -120 < 0 \;\Rightarrow\; \text{Maximum}
$$

**Ergebnis**

$$
V(5) = 5\cdot 20^2 = 2000\ \text{cm}^3 = 2\ \text{Liter}
$$

Die Schachtel ist also $5\ \text{cm}$ hoch und hat einen Boden von
$20 \times 20\ \text{cm}$. Bemerkenswert: Das Optimum liegt immer bei
$x = a/6$ – unabhängig von der Tafelgröße.

🧮 Kontrolle im CAS:

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

## 3. Aufgabe B – Weide am Fluss

                        --{{0}}--
Aufgabe B ist die Standardaufgabe zur Nebenbedingung. Der Zaun ist die knappe
Ressource, die Fläche das Ziel. Weil das Ufer gerade verläuft, muss nur drei
Mal gezäunt werden – und genau diese Asymmetrie macht das Ergebnis
interessant.

> Eine rechteckige Weide soll an einem geradlinigen Flussufer angelegt werden.
> Zur Verfügung stehen $400\ \text{m}$ Zaun; **am Ufer wird nicht gezäunt**.
>
> **Wie sind die Seiten zu wählen, damit die Weidefläche maximal wird?**

🔍 Ziehen Sie am Regler – der Zaun bleibt dabei immer $400\ \text{m}$ lang:

``` javascript @JSX.Graph.withParams(`boundingbox="[-60, 250, 440, -40]" axis="false" showNavigation="false" keepaspectratio="true"`)
var s = board.create('slider', [[10, 232], [130, 232], [5, 60, 195]],
  { name: 'x', snapWidth: 1, size: 5, strokeColor: '#1c7ed6', fillColor: 'white' });

var X = function () { return s.Value(); };          // Tiefe, senkrecht zum Fluss
var W = function () { return 400 - 2 * s.Value(); }; // Breite am Ufer

var hidden = { visible: false, fixed: true, name: '' };

// --- Fluss ----------------------------------------------------------------
board.create('segment', [[-60, 0], [440, 0]],
  { strokeColor: '#4dabf7', strokeWidth: 14, fixed: true, highlight: false });
board.create('text', [350, -28, 'Fluss'],
  { fontSize: 18, strokeColor: '#1864ab' });

// --- Weidefläche ---------------------------------------------------------
var p1 = board.create('point', [0, 0], hidden);
var p2 = board.create('point', [W, 0], hidden);
var p3 = board.create('point', [W, X], hidden);
var p4 = board.create('point', [function () { return 0; }, X], hidden);

board.create('polygon', [p1, p2, p3, p4], {
  fillColor: '#b2f2bb', fillOpacity: 0.75, vertices: { visible: false },
  borders: { visible: false }
});

// --- der Zaun: nur drei Seiten -------------------------------------------
var zaun = { strokeColor: '#5c3d0c', strokeWidth: 5, highlight: false };
board.create('segment', [p1, p4], zaun);
board.create('segment', [p4, p3], zaun);
board.create('segment', [p3, p2], zaun);

// --- Beschriftung ---------------------------------------------------------
board.create('text', [function () { return -8; }, function () { return X() / 2; },
  function () { return 'x = ' + X().toFixed(0) + ' m'; }],
  { fontSize: 15, anchorX: 'right', anchorY: 'middle',
    cssStyle: 'font-family: monospace' });

board.create('text', [function () { return W() / 2; },
                      function () { return X() + 10; },
  function () { return 'y = ' + W().toFixed(0) + ' m'; }],
  { fontSize: 15, anchorX: 'middle', cssStyle: 'font-family: monospace' });

board.create('text', [175, 236, function () {
  return 'Zaun = 2x + y = ' + (2 * X() + W()).toFixed(0) + ' m';
}], { fontSize: 16, cssStyle: 'font-family: monospace; color: #5c3d0c' });

board.create('text', [175, 208, function () {
  return 'A(x) = ' + (X() * W()).toFixed(0) + ' m²';
}], { fontSize: 16, cssStyle: 'font-family: monospace; color: #2b8a3e' });
```

### a) Nebenbedingung und Zielfunktion

$x$ sei die Tiefe senkrecht zum Ufer, $y$ die Breite am Ufer.

Nebenbedingung nach $y$ aufgelöst: $\;y = $ [[400 - 2x]]
[[?]] Gezäunt werden zwei Seiten der Länge $x$ und eine der Länge $y$.
[[?]] $2x + y = 400$
@Algebrite.check(400-2*x)

Eingesetzt in $A = x\cdot y$: $\quad A(x) = $ [[x*(400 - 2x)]]
@Algebrite.check(`x*(400-2*x)`)
*********************************************************************

$$
2x + y = 400 \;\Longrightarrow\; y = 400-2x,
\qquad
A(x) = x\,(400-2x) = 400x - 2x^2, \quad 0 < x < 200
$$

Die Zielfunktion ist eine nach unten geöffnete Parabel – das Maximum
könnte man hier sogar ohne Ableitung finden, über den Scheitelpunkt.

*********************************************************************

### b) Optimum

$x = $ [[100]] $\ \text{m}$, $\quad y = $ [[200]] $\ \text{m}$, $\quad A_{\max} = $ [[20000]] $\ \text{m}^2$
[[?]] $A'(x) = 400 - 4x$
[[?]] $A'' = -4$, also unabhängig von $x$ immer negativ.
@Algebrite.check([ 100 ; 200 ; 20000 ])
*********************************************************************

$$
A'(x) = 400-4x \stackrel{!}{=} 0 \;\Longrightarrow\; x = 100\ \text{m},
\qquad A''(x) = -4 < 0 \;\Rightarrow\; \text{Maximum}
$$

$$
y = 400 - 200 = 200\ \text{m},\qquad
A_{\max} = 100\cdot 200 = 20\,000\ \text{m}^2 = 2\ \text{ha}
$$

*********************************************************************

### c) Die Verallgemeinerung

                        --{{0}}--
Diese Frage ist die eigentlich lehrreiche. Bei drei Zaunseiten ist das Optimum
kein Quadrat, sondern ein halbes Quadrat.

Welches Seitenverhältnis stellt sich bei *beliebiger* Zaunlänge $L$ ein?

[( )] $y = x$ – das Optimum ist immer ein Quadrat.
[(X)] $y = 2x$ – die Uferseite ist doppelt so lang wie die Tiefe.
[( )] $y = \tfrac{1}{2}x$
[( )] Das hängt von $L$ ab.
[[?]] Rechnen Sie dieselbe Aufgabe mit $L$ statt $400$ durch.
*********************************************************************

Mit $A(x) = x\,(L-2x)$ folgt $A'(x) = L-4x = 0$, also

$$
x = \frac{L}{4}, \qquad y = L - \frac{L}{2} = \frac{L}{2} = 2x
$$

Das Verhältnis ist **unabhängig von $L$**. Das ist typisch für
Extremwertaufgaben: Das Optimum ist meist eine *Proportion*, keine Zahl. Bei
vier gezäunten Seiten käme dagegen das Quadrat heraus.

🧮 Nachrechnen mit symbolischem $L$:

``` Maxima
A(x) = x * (L - 2*x)

derivative(A(x), x)            -- L - 4x
solve(L - 4*x, x)              -- x = L/4
eval(L - 2*x, x, L/4)          -- y = L/2
```
@Algebrite.pretty

*********************************************************************

## 4. Aufgabe C – Die Konservendose

                        --{{0}}--
Aufgabe C ist die erste, deren Lösung keine glatte Zahl mehr ist. Hier
brauchen wir eine dritte Wurzel – und hier zeigt sich, warum die
Antwortprüfung mit einem Algebrasystem angenehmer ist als mit einem
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

### a) Zielfunktion aufstellen

Lösen Sie zuerst die Nebenbedingung $V = \pi r^2 h = 1000$ nach $h$ auf:

$h = $ [[1000/(pi r^2)]]
[[?]] Nur nach $h$ umstellen, $V=1000$ ist bereits eingesetzt.
@Algebrite.check(`1000/(pi*r^2)`)

Setzen Sie das in $O = 2\pi r^2 + 2\pi r h$ ein:

$O(r) = $ [[2 pi r^2 + 2000/r]]
[[?]] Der Mantel ist ein Rechteck mit Umfang $2\pi r$ mal Höhe $h$.
[[?]] $2\pi r \cdot \dfrac{1000}{\pi r^2} = \dfrac{2000}{r}$
@Algebrite.check(2*pi*r^2 + 2000/r)
*********************************************************************

$$
h = \frac{V}{\pi r^2} = \frac{1000}{\pi r^2},
\qquad
O(r) = \underbrace{2\pi r^2}_{\text{Boden + Deckel}} + \underbrace{2\pi r h}_{\text{Mantel}}
     = 2\pi r^2 + \frac{2000}{r}
$$

Die beiden Anteile laufen gegeneinander: Boden und Deckel wachsen quadratisch
mit $r$, der Mantel fällt wie $1/r$. Genau dieser Widerstreit erzeugt das
Minimum.

🧮 Beide Anteile und ihre Summe:

``` Maxima
O(r)      = 2*pi*r^2 + 2000/r     -- gesamter Blechbedarf
deckel(r) = 2*pi*r^2              -- Boden + Deckel
mantel(r) = 2000/r                -- Mantelfläche

draw([O(r), deckel(r), mantel(r)], r, 1, 12, ["red width=3", "blue dashed", "green dashed"])
```
@Algebrite.eval

*********************************************************************

### b) Das Optimum

                        --{{0}}--
Jetzt wird abgeleitet. Achten Sie darauf, dass die Ableitung keine Polynom-
gleichung liefert – Sie müssen mit r-Quadrat durchmultiplizieren, bevor
Sie die dritte Wurzel ziehen können.

Optimaler Radius, auf zwei Nachkommastellen (exakter Ausdruck wird auch akzeptiert):

$r_{\text{opt}} = $ [[5.42]] $\ \text{cm}$
[[?]] $O'(r) = 4\pi r - \dfrac{2000}{r^2}$
[[?]] Multiplizieren Sie die Gleichung $O'(r)=0$ mit $r^2$.
[[?]] $r^3 = \dfrac{500}{\pi}$
@Algebrite.check2(`(500/pi)^(1/3)`, 0.02, units=0)

Zugehörige Höhe und minimaler Blechverbrauch:

$h = $ [[10.84]] $\ \text{cm}$, $\quad O_{\min} = $ [[553.6]] $\ \text{cm}^2$
@Algebrite.check2(`[ 2*(500/pi)^(1/3) ; 6*pi*(500/pi)^(2/3) ]`, [ 0.05 ; 1 ], units=0)
*********************************************************************

$$
O'(r) = 4\pi r - \frac{2000}{r^2} \stackrel{!}{=} 0
\;\Big|\cdot r^2
\quad\Longrightarrow\quad
4\pi r^3 = 2000
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

🧮 Schritt für Schritt im CAS:

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

### c) Die Faustregel

Welche Beziehung gilt im Optimum zwischen Höhe $h$ und Durchmesser $d = 2r$?

[( )] $h = r$
[(X)] $h = d$, die Dose ist so hoch wie breit
[( )] $h = 2d$
[( )] Es gibt keine allgemeine Beziehung, sie hängt von $V$ ab.
[[?]] Lesen Sie die letzte Zeile des CAS-Blocks oben.
*********************************************************************

Aus $r^3 = \dfrac{V}{2\pi}$ folgt

$$
h = \frac{V}{\pi r^2} = \frac{2\pi r^3}{\pi r^2} = 2r = d
$$

– **unabhängig vom Volumen**. Der Querschnitt der optimalen Dose ist
ein Quadrat.

<!-- style="border-left: 4px solid #e8590c; padding-left: 1rem" -->
> **Warum sehen echte Dosen anders aus?**
>
> Eine 850-ml-Dose hat typisch $d \approx 10\ \text{cm}$ und
> $h \approx 11\ \text{cm}$ – nahe am Optimum. Getränkedosen sind
> dagegen deutlich schlanker. Das Modell ist eben unvollständig: Falze
> verbrauchen zusätzliches Blech, Deckel sind dicker als der Mantel,
> Paletten und Handhabung geben Formate vor. Ein schönes Beispiel dafür,
> dass das Optimum der *Zielfunktion* und das Optimum des *Produkts* zwei
> verschiedene Dinge sind.

## 5. Aufgabe D – Balken aus dem Rundholz

                        --{{0}}--
Aufgabe D kommt aus der Festigkeitslehre und ist die älteste Extremwertaufgabe
der Ingenieurausbildung überhaupt – Galilei hat sie bereits 1638
diskutiert. Neu ist hier nur, dass die Nebenbedingung der Satz des Pythagoras
ist.

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

### a) Nebenbedingung einsetzen

Die Diagonale des Querschnitts ist der Stammdurchmesser. Daraus folgt

$h = $ [[sqrt(900 - b^2)]]
[[?]] Satz des Pythagoras: $b^2 + h^2 = d^2$.
[[?]] $d = 30$, also $h^2 = 900 - b^2$.
@Algebrite.check(`sqrt(900-b^2)`)

Setzen Sie das in $W = \tfrac{1}{6}bh^2$ ein – beachten Sie, dass nur
$h^2$ gebraucht wird, die Wurzel fällt also weg:

$W(b) = $ [[b*(900 - b^2)/6]]
@Algebrite.check(`b*(900-b^2)/6`)
*********************************************************************

$$
b^2 + h^2 = d^2 = 900
\qquad\Longrightarrow\qquad
W(b) = \frac{b\,h^2}{6} = \frac{b\,(900-b^2)}{6} = 150b - \frac{b^3}{6},
\quad 0 < b < 30
$$

Ein Glücksfall: In der Zielfunktion steht $h^2$, nicht $h$. Deshalb bleibt die
Wurzel aus der Nebenbedingung verschwunden und $W$ ist ein einfaches Polynom.

*********************************************************************

### b) Optimaler Querschnitt

$b = $ [[17.32]] $\ \text{cm}$, $\quad h = $ [[24.49]] $\ \text{cm}$
[[?]] $W'(b) = 150 - \dfrac{b^2}{2}$
[[?]] $b^2 = 300$
[[?]] $h = \sqrt{900-300} = \sqrt{600}$
@Algebrite.check2(`[ 30/sqrt(3) ; sqrt(600) ]`, [ 0.02 ; 0.02 ], units=0)

Das Widerstandsmoment beträgt dann

$W_{\max} = $ [[1732]] $\ \text{cm}^3$
@Algebrite.check2(`1000*sqrt(3)`, 1, units=0)
*********************************************************************

$$
W'(b) = 150 - \frac{b^2}{2} \stackrel{!}{=} 0
\quad\Longrightarrow\quad
b = \sqrt{300} = \frac{d}{\sqrt{3}} \approx 17{,}32\ \text{cm}
$$

$$
W''(b) = -b < 0 \ \text{ für } b>0 \;\Rightarrow\; \text{Maximum}
$$

$$
h = \sqrt{900-300} = \sqrt{600} = d\sqrt{\tfrac{2}{3}} \approx 24{,}49\ \text{cm},
\qquad
W_{\max} = \frac{\sqrt{300}\cdot 600}{6} = 1000\sqrt{3} \approx 1732\ \text{cm}^3
$$

🧮 Vollständig im CAS, inklusive Kurve:

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

### c) Die Zimmermannsregel

Welches Verhältnis $h : b$ ergibt sich – unabhängig vom Durchmesser?

[( )] $1 : 1$ (quadratisch)
[(X)] $\sqrt{2} : 1 \approx 1{,}41 : 1$
[( )] $\sqrt{3} : 1 \approx 1{,}73 : 1$
[( )] $2 : 1$
[[?]] $b = d/\sqrt{3}$ und $h = d\sqrt{2/3}$.
*********************************************************************

$$
\frac{h}{b} = \frac{d\sqrt{2/3}}{d/\sqrt{3}} = \sqrt{2} \approx 1{,}414
$$

Das ist die alte **Zimmermannsregel**: Teile den Durchmesser in drei gleiche
Teile, einer davon ist die Breite – denn $b = d/\sqrt{3} \approx 0{,}577d$
liegt nahe an $0{,}6d$, und die Näherung $h:b = 7:5$ war jahrhundertelang die
Praxis auf dem Bau.

<!-- style="border-left: 4px solid #2f9e44; padding-left: 1rem" -->
> **Andere Zielfunktion, anderes Optimum**
>
> Optimiert man statt des Widerstandsmoments $W \sim bh^2$ die
> **Biegesteifigkeit** $I = \tfrac{1}{12}bh^3$, so ergibt sich
> $h : b = \sqrt{3} : 1$. Und wer schlicht die **Ausbeute an Holz** $A = bh$
> maximiert, landet beim Quadrat. Drei Ziele, drei Optima, ein Rundholz –
> die Modellbildung entscheidet, nicht die Rechnung.

## 6. Randextrema und typische Fallen

                        --{{0}}--
Jetzt der Teil, an dem in der Klausur die Punkte verloren gehen. Vier Fallen,
und die erste ist mit Abstand die häufigste.

### Falle 1: Das Maximum sitzt am Rand

                        --{{0}}--
Ziehen Sie die beiden roten Punkte a und b auf der x-Achse. Der grüne Punkt
markiert das größte Funktionswert im Intervall. Beobachten Sie, wann er auf
einen Randpunkt springt – dort ist die Ableitung ungleich null, und
trotzdem liegt das Maximum genau da.

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

                        --{{1}}--
Die Konsequenz für das Kochrezept: Auf einem abgeschlossenen Intervall
vergleicht man immer alle Kandidaten miteinander – die Nullstellen der
Ableitung und zusätzlich die beiden Randwerte.

     {{1}}
<!-- style="border-left: 4px solid #c92a2a; padding-left: 1rem" -->
> **Merksatz:** Auf $[a,b]$ sind die Kandidaten für das globale Maximum
>
> $$\{x : f'(x)=0\} \;\cup\; \{a,\;b\} \;\cup\; \{\text{Knickstellen}\}$$
>
> Man rechnet alle aus und vergleicht die Funktionswerte. Fertig.

### Falle 2, 3 und 4

Ordnen Sie zu, welcher Fehler jeweils vorliegt:

[  [Rand vergessen]  [Nebenbedingung nicht eingesetzt]  [hinreichende Bedingung nicht geprüft]  ]
[        ( )                       ( )                                  (X)                     ] In Aufgabe A wurde $V'(5)=0$ berechnet und sofort "Maximum" geschrieben.
[        ( )                       (X)                                  ( )                     ] In Aufgabe B wurde $A(x,y)=xy$ nach $x$ abgeleitet, $y$ blieb stehen.
[        (X)                       ( )                                  ( )                     ] Eine Kostenfunktion ist auf $[1;5]$ streng monoton fallend, es wird "kein Minimum" notiert.
[[?]] Eine Funktion ohne Nullstelle der Ableitung kann trotzdem ein Minimum haben.
*********************************************************************

* **Zeile 1** – $f'(x_0)=0$ allein sagt nichts. Es fehlt $V''(5)=-120<0$.
* **Zeile 2** – Eine Funktion zweier Variabler kann man nicht nach dem
  Schema "$f'=0$" behandeln. Erst die Nebenbedingung einsetzen, dann ableiten.
* **Zeile 3** – Streng monoton fallend auf $[1;5]$ heißt: Das Minimum
  liegt bei $x=5$, am rechten Rand. Es existiert sehr wohl.

*********************************************************************

Noch eine häufige Verwechslung. Ergänzen Sie:

Ist $f''(x_0) > 0$, so liegt bei $x_0$ ein [[ Maximum | (Minimum) | Sattelpunkt ]],
denn die Funktion ist dort [[ (linksgekrümmt) | rechtsgekrümmt ]].
Aus $f''(x_0) = 0$ folgt [[ ein Sattelpunkt | kein Extremum | (zunächst gar nichts) ]].

## 7. Selbsttest

                        --{{0}}--
Zum Abschluss eine Aufgabe, die Sie komplett alleine durchrechnen. Die
Zahlen sind so gewählt, dass alles glatt aufgeht – wenn bei Ihnen
krumme Werte herauskommen, steckt der Fehler meist schon in der
Nebenbedingung.

> Ein **oben offener** Behälter mit quadratischer Grundfläche soll
> $V = 32\ \text{dm}^3$ fassen. Gesucht ist der minimale Materialverbrauch
> (Boden und vier Wände).

$a$ sei die Grundkante, $h$ die Höhe.

**a)** Nebenbedingung nach $h$ aufgelöst: $\;h = $ [[32/a^2]]
[[?]] $V = a^2 h$
@Algebrite.check(32/a^2)

**b)** Zielfunktion: $\;O(a) = $ [[a^2 + 128/a]]
[[?]] Boden: $a^2$. Vier Wände: $4ah$. Der Deckel fehlt!
[[?]] $4a\cdot\dfrac{32}{a^2} = \dfrac{128}{a}$
@Algebrite.check(a^2 + 128/a)

**c)** Ableitung: $\;O'(a) = $ [[2a - 128/a^2]]
@Algebrite.check(2*a - 128/a^2)

**d)** Optimale Maße und minimale Oberfläche:

$a = $ [[4]] $\ \text{dm}$, $\quad h = $ [[2]] $\ \text{dm}$, $\quad O_{\min} = $ [[48]] $\ \text{dm}^2$
[[?]] $O'(a)=0 \Leftrightarrow 2a^3 = 128$
[[?]] $a^3 = 64$
@Algebrite.check([ 4 ; 2 ; 48 ])
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
O''(a) = 2 + \frac{256}{a^3} > 0 \;\Rightarrow\; \text{Minimum}
$$

$$
h = \frac{32}{16} = 2\ \text{dm} = \frac{a}{2},
\qquad O_{\min} = 16 + 32 = 48\ \text{dm}^2
$$

Auch hier ist die eigentliche Erkenntnis die Proportion: Beim **offenen**
Behälter ist $h = a/2$, die Höhe also nur halb so groß wie die Grundkante.
Beim **geschlossenen** Quader käme der Würfel heraus, $h=a$. Der fehlende
Deckel macht den Behälter flach.

🧮 Kontrolle:

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

### Ihr Ergebnis

Wie sicher fühlen Sie sich jetzt bei Extremwertaufgaben?

[(1)] Sitzt – ich könnte das an der Tafel vorrechnen.
[(2)] Das Schema ist klar, beim Aufstellen der Zielfunktion hänge ich noch.
[(3)] Die Rechnung geht, die Modellbildung nicht.
[(4)] Ich brauche mehr Übung.

Welche Schritte möchten Sie noch einmal üben? (Mehrfachauswahl)

[[zielfunktion]]   Zielfunktion aufstellen
[[nebenbedingung]] Nebenbedingung einsetzen
[[ableiten]]       Ableiten und Nullstellen bestimmen
[[rand]]           Randwerte berücksichtigen
[[einheiten]]      Interpretation mit Einheiten

Was ist unklar geblieben? Die Antwort bleibt lokal in Ihrem Browser.

[[___ ___ ___]]

## 8. Zum Weiterarbeiten

### Woraus dieses Blatt gebaut ist

                        --{{0}}--
Zum Schluss ein Blick unter die Haube, falls Sie selbst solche Blätter
erstellen möchten. Der komplette Kurs ist eine einzige Markdown-Datei, alles
läuft im Browser, ohne Server und ohne Installation.

| Baustein                     | Wofür                                                      |
| ---------------------------- | ----------------------------------------------------------- |
| [LiaScript](https://liascript.github.io) | Markdown-Dialekt, aus dem der ganze Kurs besteht |
| [JSXGraph](https://jsxgraph.org)         | die beweglichen Skizzen (`@JSX.Graph`)           |
| [Algebrite](http://algebrite.org)        | CAS für Rechenblöcke und Antwortprüfung       |

Der Kopf der Datei besteht im Wesentlichen aus zwei Zeilen:

``` markdown
import: https://raw.githubusercontent.com/LiaTemplates/algebrite/0.7.1/README.md
import: https://raw.githubusercontent.com/LiaTemplates/JSXGraph/0.0.3/README.md
```

Eine Aufgabe mit CAS-Prüfung ist dann nicht mehr als:

``` markdown
$V(x) = $ [[x*(30 - 2x)^2]]
[[?]] Die Schachtel hat die Höhe x.
@Algebrite.check(`x*(30-2*x)^2`)
***********************************
Musterlösung ...
***********************************
```

<!-- style="border-left: 4px solid #e8590c; padding-left: 1rem" -->
> **Stolperstein beim Selberbauen:** Makro-Parameter in LiaScript enden am
> ersten `)` oder `,`. Eine Lösung wie `x*(30-2*x)^2` würde also nach
> `x*(30-2*x` abgeschnitten. Deshalb steht sie oben in **Backticks** – dann
> wird der ganze Ausdruck übergeben.
>
> Und noch eine Feinheit: Folgt nach einem Komma ein weiterer Parameter in
> Backticks, darf davor **kein Leerzeichen** stehen – sonst wandern die
> Backticks selbst mit in den Parameter und das Quiz zählt jede Antwort als
> falsch.

                        --{{1}}--
Der entscheidende Unterschied zu einem gewöhnlichen Multiple-Choice-System:
Geprüft wird nicht die Zeichenkette, sondern der mathematische Ausdruck. Alle
folgenden Eingaben gelten als dieselbe richtige Antwort.

     {{1}}
Probieren Sie es selbst aus – alle diese Eingaben sind korrekt:

`x*(30-2x)^2`  ·  `4x^3-120x^2+900x`  · 
`4*x^3 - 120*x^2 + 900*x`  ·  `\frac{8x^3-240x^2+1800x}{2}`

$V(x) = $ [[x*(30 - 2x)^2]]
@Algebrite.check(`x*(30-2*x)^2`)

### Eigene Funktion ausprobieren

🧮 Ein freier CAS-Block. Tippen Sie eine Funktion ein und lassen Sie Kurve,
Ableitung und Extremstellen berechnen:

``` Maxima
f(x) = x^4 - 8*x^2 + 3

derivative(f(x), x)            -- f'(x)
roots(4*x^3 - 16*x, x)         -- die drei Kandidaten
derivative(f(x), x, 2)         -- f''(x)

draw(f(x), x, -3.2, 3.2, "blue width=3 ymin=-20 ymax=30 mark=-2 mark=0 mark=2")
```
@Algebrite.repl

### Lizenz

<!-- style="font-size: 90%" -->
> Dieses Aufgabenblatt steht unter
> [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.de).
> Sie dürfen es teilen, umarbeiten und in eigene Lehrveranstaltungen
> übernehmen – unter Nennung der Quelle und unter gleichen Bedingungen.
>
> Die verwendeten Templates: JSXGraph (LGPL / MIT) und Algebrite (MIT),
> beide eingebunden über [LiaTemplates](https://github.com/LiaTemplates).
