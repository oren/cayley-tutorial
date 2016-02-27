# Players Effects

Before adding the code to respond to the action we created let's add the effect we will need to use.

In __src/Players/Effects.elm__ add:

```elm
...
import Json.Encode as Encode

...

create : Player -> Effects Action
create player =
  let
    body =
      memberEncoded player
        |> Encode.encode 0
        |> Http.string

    config =
      { verb = "POST"
      , headers = [ ( "Content-Type", "application/json" ) ]
      , url = createUrl
      , body = body
      }
  in
    Http.send Http.defaultSettings config
      |> Http.fromJson memberDecoder
      |> Task.toResult
      |> Task.map CreatePlayerDone
      |> Effects.task


createUrl : String
createUrl =
  "http://localhost:4000/players"

```

`create` takes a `Player` record and returns an effect. At the moment there is no `post` function in the Http module so we need to use `send` instead. Read more about `send` [here](http://package.elm-lang.org/packages/evancz/elm-http/3.0.0/Http).

We create the body of the request by encoding the given player record with `memberEncoded` that we will define next. `Json.Encode.encode` takes an argument for the indentation in the produced JSON string and a `Json.Encode.Value` which `memberEncoded` returns.

Add ` memberEncoded` to this module:

```elm
...

memberEncoded : Player -> Encode.Value
memberEncoded player =
  let
    list =
      [ ( "id", Encode.int player.id )
      , ( "name", Encode.string player.name )
      , ( "level", Encode.int player.level )
      ]
  in
    list
      |> Encode.object
```

This function maps the attributes of the `player` record to Json values.


