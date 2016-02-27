# Imports and Modules

In Elm you import a module by using the `import` keyword e.g.

```
import Html
```

This imports the `Html` module from core. Then you can use functions and types from this module by using it fully qualified path:

```
Html.div [] []
```

You can also import modules and expose functions and types on it:

```
import Html exposing (div)
```

`div` is mixed in the current scope. So you can use it directly:

```
div [] []
```

You can even expose everything in a module:

```
import Html exposing (..)
```

Then you would be able to use every function and type in that module directly. But this is not recommended most of the time because we end up with ambiguity and possible clashes between modules.

## Modules and types with the same name

Many modules export types with the same name as the module. For example the `Html` module has a `Html` type and the `Signal` module has a `Signal` type.

So this function that returns an `Html` element:

```elm
import Html

myFunction : Html.Html
myFunction =
  ...
```

Is equivalent to:

```elm
import Html exposing (Html)

myFunction : Html
myFunction =
  ...
```

In the first one we use only import the `Html` module and use the fully qualified path `Html.Html`.

In the second one we expose the `Html` type from the `Html` module. And use the `Html` type directly.

## Module declarations

When you create a module in Elm you add the `module` declaration at the top:

```
module Main (..) where
```

`Main` is the name of the module. `(..)` means that you want to expose all functions and types in this module. Elm expect to find this module in a file called __Main.elm__, so a file with the same name as the module.

You can have deeper file structures in an application, for example the file __Players/Utils.elm__ should have the declaration:

```
module Players.Utils (..) where
```

You will be able to import this module from anywhere in your application by:

```
import Players.Utils
```




