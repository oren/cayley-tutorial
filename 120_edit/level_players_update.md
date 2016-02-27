# Players Update

We have an `ChangeLevel` that triggers when the user hits both the decrease or increase level buttons. We need to account for this action in `Players.Update`.

In __src/Players/Update.elm__ add one branch for this:

```elm
    ...
    ChangeLevel playerId howMuch ->
      let
        fxForPlayer player =
          if player.id /= playerId then
            Effects.none
          else
            let
              updatedPlayer =
                { player | level = player.level + howMuch }
            in
              if updatedPlayer.level > 0 then
                save updatedPlayer
              else
                Effects.none

        fx =
          List.map fxForPlayer model.players
            |> Effects.batch
      in
        ( model.players, fx )
```

When we get `ChangeLevel` we want to create an effect to save one player on the API. `ChangeLevel` gives us the player id that we need to update.

We could try to find this player in `model.players` using `List.filter` so we can build the effects to return. But using `List.filter` would involve adding conditional logic to deal with the potential case of not finding the player we want in the list.

Rather than doing that it is much easier to just map over all the players and return a list of effects:

```elm
  List.map fxForPlayer model.players
    |> Effects.batch
```

In here we map through all the players. `fxForPlayer` returns an effect for each player. In the first line we will have a `List` of `Effects`.

Using `Effects.batch` we convert this list of effects into one `Effects`. Elm will run all these effects one after the other.

### fxForPlayer

```elm
fxForPlayer player =
  if player.id /= playerId then
    Effects.none
  else
    let
      updatedPlayer =
        { player | level = player.level + howMuch }
    in
      if updatedPlayer.level > 0 then
        save updatedPlayer
      else
        Effects.none
```

`fxForPlayer` takes a player a returns and `Effect` for that player. But most of the time we just return `Effects.none`. We only return a `save` effect if this is the player we are updating.

We also don't want a player level to go less than 1. So we have a `if updatedPlayer.level > 0 then` condition that only returns the `save` effect if the updated player level will be more than 0.

## SaveDone


Upon receiving `ChangeLevel` we return a batch of effects to run (which are a lot of `Effect.none` and one `save`). Elm will run the `save` effect and send the request to the API.

When the request is done we will get `SaveDone (Result Http.Error Player)`.

Let's handle this in __src/Players/Update.elm__, add new branch:

```elm
    SaveDone result ->
      case result of
        Ok player ->
          let
            updatedPlayer existing =
              if existing.id == player.id then
                player
              else
                existing

            updatedCollection =
              List.map updatedPlayer model.players
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

If the result is `Ok` then we match the `player.id` and updated that particular player.


