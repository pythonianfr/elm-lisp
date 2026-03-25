# elm-lisp

S-expression parser, serializer, and pretty-printer for Elm.

Parses a dialect of Lisp with symbols, strings, integers, floats,
booleans (`#t` / `#f`), and keywords (`#:name`).


## Install

```
elm install pythonian/elm-lisp
```


## Usage

### Parse

```elm
import Lisp

Lisp.parse "(add 1 2)"
-- Just (Expression [Atom (Symbol "add"), Atom (Int 1), Atom (Int 2)])

Lisp.parse "(series \"temperature\" #:from (today) #:timezone #t)"
-- Just (Expression [ Atom (Symbol "series")
--                  , Atom (String "temperature")
--                  , Atom (Keyword "from")
--                  , Expression [Atom (Symbol "today")]
--                  , Atom (Keyword "timezone")
--                  , Atom (Bool True)
--                  ])

Lisp.parse "not valid"
-- Nothing
```

### Serialize

```elm
import Lisp exposing (Atom(..), Expr(..))

Lisp.serialize (Expression [Atom (Symbol "+"), Atom (Int 1), Atom (Float 2.5)])
-- "(+ 1 2.5)"
```

### Pretty-print

`view` renders an `Expr` as syntax-highlighted HTML (using Pygments
CSS class names: `.nv` for symbols, `.s` for strings, `.mf` for
numbers, `.ss` for keywords, `.mv` for booleans, `.p` for
parentheses, `.w` for whitespace).

```elm
import Lisp

Lisp.view expr Dict.empty
-- List (Html msg) — highlighted, with automatic line-breaking at width > 70
```

The second argument (`Overrides msg`) lets you customize rendering of
specific operators (e.g., making series names clickable).


## Types

```elm
type Atom
    = Symbol String
    | Keyword String
    | String String
    | Float Float
    | Int Int
    | Bool Bool

type Expr
    = Atom Atom
    | Expression (List Expr)
```


## Design goals

- **Round-trip fidelity**: `parse >> serialize` normalizes whitespace
  but preserves structure.
- **Keyword support**: `#:name` keywords are first-class atoms, not
  parsed as symbols.
- **Minimal dependencies**: `elm/core`, `elm/parser`, `elm/html`,
  `Punie/elm-parser-extras`.


## License

LGPL-3.0-or-later. Copyright (C) 2026 Pythonian.
