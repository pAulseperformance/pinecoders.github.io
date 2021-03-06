<!-- Global site tag (gtag.js) - Google Analytics -->
<script async src="https://www.googletagmanager.com/gtag/js?id=UA-147975914-1"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());

  gtag('config', 'UA-147975914-1');
</script>

[<img src="https://www.pinecoders.com/images/PineCodersLong.png">](https://www.pinecoders.com/)

# Pine Script Coding Conventions
The goal of these Coding Conventions is to present a set of best practices and style guidelines for Pine Script. By making Pine scripts easier to read, these guidelines make open source code more usable, while also providing safeguards that minimize the risk of errors for developers.

### Table of Contents

- [Script Structure](#script-structure)
- [Naming Conventions](#naming-conventions)
- [Spacing](#spacing)
- [Line Wrapping](#line-wrapping)
- [Example Scripts](#example-scripts)


<br>

## Script Structure

The Pine compiler is not very strict on exact positioning of specific statements or compiler directives. While many other arrangements are syntactically correct, these guidelines aim to provide a standard way of ordering elements in scripts:

1. The first line of a script should be the `//@version=X` compiler directive, where `X` is replaced by the version of Pine the script is written for. While the compiler defaults to Pine version 1 when no directive is used, scripts written using version 1 of Pine should nonetheless contain the `//@version=1` directive on their first line.

1. **Comments** describing the script are usually placed immediately after the `@version` compiler directive.

1. The first Pine statement in the script should be either the `study()` or `strategy()` declaration statement.

1. The next lines should contain the script's **inputs**.

1. The following lines can contain **variable initializations** and **function definitions** in any order required. Note that all Pine functions must be defined in the script's global scope, as nested function definitions are not allowed. Concerning variable initializations, some scripts lend themselves to mass initializations and others will be more readable with an *initialize as you need* style that places initializations with the code segments where the variables are used. It's up to each coder to adopt the most useful style. Local block variables must be declared in the local block where they will be used.

1. The rest of the script will contain **calculations**, followed by,

1. `plot()` calls,
1. `strategy.*()` calls (for strategies) and,
1. `alertcondition()` calls (for indicators), if needed.

Here is an example of a complete script:

```js
//@version=4
// MACD indicator, a Gerald Appel concept.
// Author: TradingView, mods by PineCoders, v1.0, 2019.07.31
study("MACD")

// ————— Inputs
i_fast = input(12, "Fast Length")
// Calculates slow length from fast length and normalizes it if needed.
f_getSlowLength(_len) =>
    _tempLen = _len * 2
    if _tempLen < 20 or _tempLen > 30
        _tempLen := 25
    _tempLen
slow = f_getSlowLength(i_fast)

// ————— Calculations
fastMa = ema(close, i_fast)
slowMa = ema(close, slow)
macd = fastMa - slowMa
signal = sma(macd, 9)

// ————— Plots
plot(macd, color = color.blue)
plot(signal, color = color.orange)
```


<br>

## Naming Conventions

### Variable Names

We recommend using camelCase for variable names. Example: `emaLength`, `obLevel`, `showSignal2`, `aLongVariableName`.

For large projects, you may find it useful to use prefixes for a few types of variables, to make them more readily identifiable. The following prefixes can then be used:

- `i_` for variables initialized through `input()` calls.
- `c_` for variables containing colors.
- `p_` for variables used as `plot` or `hline` identifiers for use in `fill()` calls.
- All caps for constants, i.e., variables often initialized at the beginning of scripts whose value will not change during execution.


### Function Names

For function names, we recommend using a Hungarian-style `f_` prefix in combination with the usual camelCase. The `f_` prefix guarantees disambiguation between user-defined and built-in functions. Example: `f_sinh`, `f_daysInMonth`.

### Function Parameter Names

Function parameters should be prefixed with the underscore in order to differentiate them from global scope variables. Example:
```js
daysInMonth(_year, _month) =>
```

### Function Dependencies

When a function requires global scope variables to perform its calculations, these dependencies should be documented in comments. Dependencies are to be avoided whenever possible, as they jeopardize function portability and make code more difficult to read.
```js
i_lenMultiplier = input(2, "Length Multiplier")

f_getSlowLength(_len) =>
    // Dependencies: i_lenMultiplier (initialized in inputs). 
    _tempLen = _len * i_lenMultiplier
    if _tempLen < 20 or _tempLen > 30
        _tempLen := 25
    _tempLen
```

This is a preferable way to write the same function, which eliminates dependencies:
```js
f_getSlowLength(_len, _mult) =>
    _tempLen = _len * _mult
    if _tempLen < 20 or _tempLen > 30
        _tempLen := 25
    _tempLen
```

### Local Scope Variable Names

The same underscore prefix used for function parameters should also be used for all local variables. Example:
```js
f_getSlowLength(_len) =>
    _tempLen = _len * 2
    if _tempLen < 20 or _tempLen > 30
        _tempLen := 25
    _tempLen
```
```js
if something
    _myLocalVar = something
```
```js
for _i = 0 to 100
    _myLocalVar = something[_i]
```


<br>

## Spacing

A space should be used on both sides of all operators, whether they be assignment, arithmetic (binary or unary) or logical. A space should also be used after commas. Example:
```js
a = close > open ? 1 : -1
var newLen = 2
newLen := min(20, newlen + 1)
a = - b
c = d > e ? d - e : d
index = bar_index % 2 == 0 ? 1 : 2
plot(series, color = color.red)
```


<br>

## Line Wrapping

When lines need to be continued on the next, use two spaces to indent each continuation line. Example:
```js
plot(
  series = close,
  title = "Close",
  color = color.blue,
  show_last = 10
  )
```

Tabs may be used to line up elements in order to increase readability.
```js
plot(
  series    = close,
  title     = "Close",
  color     = color.blue,
  show_last = 10
  )
```

<br>

## Example Scripts

Some authors use the Coding Conventions systematically:
- [Bar Balance [LucF]](https://www.tradingview.com/script/lcgCwWwI-Bar-Balance-LucF/)
- [[e2] Absolute Retracement](https://www.tradingview.com/script/X87V5IBs-e2-Absolute-Retracement/)
- [Color Gradient (16 colors) Framework - PineCoders FAQ](https://www.tradingview.com/script/EjLGV9qg-Color-Gradient-16-colors-Framework-PineCoders-FAQ/)


<br>

**[Back to top](#table-of-contents)**
