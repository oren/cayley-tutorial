# DeletePlayerDone


When the `delete` effect is done `DeletePlayerDone` will be triggered. Add another branch to __src/Players/Update.elm__:

```elm
    DeletePlayerDone playerId result ->
      case result of
        Ok _ ->
          let
            notDeleted player =
              player.id /= playerId

            updatedCollection =
              List.filter notDeleted model.players
          in
            ( updatedCollection, Effects.none )

        Err error ->
          let
            message =
              toString error

            fx =
              Signal.send model.showErrorAddress message
                |> Effects.task
                |> Effects.map TaskDone
          in
            ( model.players, fx )
```

<https://github.com/sporto/elm-tutorial-app/blob/110-delete-player/src/Players/Update.elm>

Note how we pattern match `playerId` as the first parameter of `DeletePlayerDone` instead of grabbing it form result. The result just comes with an empty payload as we don't care about the body from the server response.

Then we filter that player out with `notDeleted`.

