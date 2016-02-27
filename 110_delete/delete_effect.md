# Responding to DeletePlayer

When the user clicks yes on the confirmation dialogue we receive a message from JavaScript. This message is then mapped to the `DeletePlayer` action.

## DeletePlayer Action

Let's add the code in __src/Players/Update.elm__ to respond to this action, add a new branch:

```elm
    DeletePlayer playerId ->
      ( model.players, delete playerId )
```

<https://github.com/sporto/elm-tutorial-app/blob/110-delete-player/src/Update.elm>

This returns a `delete` effect we will add next.

## delete effect

Next, add the effects to delete the player. Add this to __src/Players/Effects.elm__:

```elm
deleteUrl : PlayerId -> String
deleteUrl playerId =
  "http://localhost:4000/players/" ++ (toString playerId)


deleteTask : PlayerId -> Task.Task Http.Error ()
deleteTask playerId =
  let
    config =
      { verb = "DELETE"
      , headers = [ ( "Content-Type", "application/json" ) ]
      , url = deleteUrl playerId
      , body = Http.empty
      }
  in
    Http.send Http.defaultSettings config
      |> Http.fromJson (Decode.succeed ())
```

<https://github.com/sporto/elm-tutorial-app/blob/110-delete-player/src/Players/Effects.elm>

`deleteTask` takes a player id and returns a task to delete the player.

`Http.send` returns a task of type `Task.Task Http.RawError Http.Response`. But for consistency with other effects we want `Task.Task Http.Error a` where `a` is the parsed JSON.

`Http.fromJson decoder` will parse the returned body into Json and return a type of type `Task.Task Http.Error a` which is what we want. We don't care about the returned body so we use `(Decode.succeed ())` as the decoder. This is a decoder that always succeeds and returns empty. 

So here we are using `Http.fromJson` just for the side effect of converting the task to `Task.Task Http.Error ()`.

In the same file also add:

```elm
delete : PlayerId -> Effects Action
delete playerId =
  deleteTask playerId
    |> Task.toResult
    |> Task.map (DeletePlayerDone playerId)
    |> Effects.task
```

This takes the previous `deleteTask`, converts the result of the task to a `Result` type. Then wraps the result with the `DeletePlayerDone` action and finally converts it to an effect.
