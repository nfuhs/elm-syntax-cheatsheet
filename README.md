# Elm Syntax Cheatsheet

### Comments

```elm
-- a single line comment

{- a multiline comment
    {- can be nested  -}
-}

```

#### Comments trick: 

```elm

{--}
add x y = x +y
--}

```

Just add or remove ``` } ``` on the first line and you'll toggle between **commented** or **uncommented**

### Literals

```elm
-- Boolean
True    :   Bool
False   :   Bool

42      :   number  --  Int or Float depending on usage
3.14    :   Float

'a'     :   Char
"abc"   :   String

--mutline String
"""
This is useful for holding JSON or other content with "quotatation marks."
"""

```
Type manipulation of literals:

```elm
True && not (True || False)
(2 + 4) * (4^2 - 9)
"abc" ++ "def"
```

#### Lists

Here are three things that are equivalent:

```elm
[1,2,3,4,5]
1 :: [2,3,4]
1 :: 2 :: 3 :: 4 :: []
```

### Conditionals

```elm
if powerLevel > 3000 then "OVER 9000!!!" else "meh"
```

If you need to branch on many different conditions, you just chain this construct together.

```elm
if key == 40 then
    n + 1
else if key == 38 then
    n + 1
else
    n
```

You can also have conditional behavior based on the structure of algebraic data types and literals

```elm
case maybeList of
    Just xs -> xs
    Nothing _> []

case xs of
    [] ->
     Nothing
  first :: rest ->
    Just  (first, rest)

case n of
  0 -> 1
  1 -> 1
   _ fib (n-1) + fib (n-2)
```

Each pattern is indention sensitive, meaning that you have to align all your patterns.

### Records

```elm
-- create records
origin = { x = 0, Y = 0 }
point = { x = 3, y = 4 }

--access fields
origin.x == 0
point.x == 3

--field access function
origin.x == 0
point.x == 3

-- field access function
List.Map . x [ origin, point ] == [ 0, 3 ]

-- update a field
{ point | x = 6 } == { x = 6, y = 4 }

-- update many fields
{ point | x = point.x + 1, y = point.y + 1 }

```

### Functions

```elm
square n  =
  n^2
hypotenuse a b = 
  sqrt (square a + square b)

distance (a,b) (x,y) =
  hypotenuse (a - x) (b - y)
```

Anonymous functions:

```elm
square =
  \n -> n^2

squares =
  List.map (\n -> n^2) (List.range 1 100)
```

#### Operators

**TODO Explain Operators**

```elm
viewNames1 names =
  String.join ", " (List.sort names)

viewNames2 names =
  names
    |> List.sort
    |> String.join ", "

-- (arg |> func) is the same as (func arg)
-- Just keep repeating that transformation!
```
This is inspired by UNIX [pipes](https://en.wikipedia.org/wiki/Pipeline_(Unix)):
(<<) and (>>) are function composition operators.

### Let

let these values be defined in this specific expression.

```elm
let
  twentyFour =
    3 * 8

  sixteen =
    4 ^ 2
in
twentyFour + sixteen
```

This is useful to break large let definitions in smaller parts

You can define functions and use "destruction assignment" in let expressions too.

```elm
let
  ( three, four ) =
    ( 3, 4 )
  hypotenuse a b =
    sqrt (a^2 + b^2)
in
hypotenuse three four
```

Le-expressions are indention sensitive, so each definition **must** align with the one above it.

Finally, you can , add type annotations in let-expressions.

```elm
let
  name : String
  name =
    "Hermann"
  increment : Int -> Int
  increment n =
    n + 1
in
increment 10
```

It is the best to only do this on **concrete types** . Break generic functions into own top-level
definitions.

#### Apply functions

```elm
-- alias for appending lists and two lists
append xs ys = xs ++ ys
xs = [1,2,3]
ys = [4,5,6]

-- All of the following expressions are equivalent:
a1 = append xs ys 
a2 = xs ++ ys

b2 = (++) xs ys

c1 = (append xs) ys
c2 = ((++) xs) ys

```

The basic arithmetic infix operators **all** figure out what **type** they should have automatically.

```elm
23 + 19     : number
2.0 + 1     : Float

6 * 7       : number
10 * 4.2    : Float

100 // 2    : Int
1 / 2       : Float
```

### Modules

```elm
module MyModule exposing (..)

-- quailfied imports
import List                             -- List.map, List.fold1
import List as L                        -- L.map, L.fold1

-- open imports
import List exposing (..)               -- map, fold1,  concat, ...
import List exposing ( map, fold1 )     -- map, fold1

import Maybe exposing ( Maybe )         -- Maybe
import Maybe exposing ( Maybe(..) )     -- Maybe, Just, Nothing
```
Qualified imports are preferred. Module names must match their file name, so module ``` Parser.Utils``` needs to be in file ``` Parser.Utils ``` needs to be in file ``` Parser /Utils.elm. ```


### Type Annotation

```elm
answer  : Int
answer = 
  42

factorial : Int -> Int
factorial n =
  List.product (List.range 1 n)

distance : { x : Float, y : Float } -> Float
distance {x,y} =
  sqrt  (x^2 + y^2)

```
### Type Aliases

```elm
type alias Name = String
type alias Age = Int

info : (Name,Age)
info =
  ("John", 48)

type alias Point = { x:Float, y:Float }

origin : Point
origin =
  { x = 0, y = 0 }

```

### Custom Types

```elm
type User
  = Regular String Int
  | Visitor String
```
Link to **custom-types** [Custom Types in Elm](https://guide.elm-lang.org/types/custom_types.html)

### Connect Elm apps with JavaScript

```elm
-- incoming values
port prices : (Float -> msg) -> Sub msg

-- outgoing values
port time : Float -> Cmd msg
```

From JavaScript you can interact with **ports** like this, read here more about it [https://guide.elm-lang.org/interop/](https://guide.elm-lang.org/interop/):

```JavaScript
var app = Elm.Example.init(); 

app.ports.prices.send(42);
app.ports.prices.send(13);

app.ports.time.subscribe(callback);
app.ports.time.unsubscribe(callback);
```
