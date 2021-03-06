# elm-styleguide

## Table of Contents
- [Rules](#rules)
- [Basic Template](#basic-template)
- [Effectual Template](#effectual-template)

## Rules

- Qualify as many imports as possible before pushing to production:
```diff
- import Html exposing (..)
- import Html.Events exposing (..)
+ import Html exposing (Html, div, input, text)
+ import Html.Events exposing (onInput)

view { content } = div [] [
  input [onInput Content] [],
  text content
  ]
```

## Basic Template

```elm
import Html exposing (..)
import Html.App exposing (beginnerProgram)
import Html.Events exposing (..)

main = beginnerProgram { model = null, update = update, view = view }

type alias Model = {
  content : String
}

type Msg = Content String

null : Model
null = { content = "" }

update : Msg -> Model -> Model
update msg model =
  case msg of
    Content s -> { model | content = s }

view : Model -> Html Msg
view { content } = div [] [
  input [onInput Content] [],
  text content
  ]
```

**Differences from Elm Guide:**
- `import Html.App as Html` and using `Html.beginnerProgram` conflates the exports of `Html.App` those of `Html`.
This guide defers to `import Html.App exposing (beginnerProgram)`.
- The Elm Guide calls `beginnerProgram` with `{ model, view, update }`, but this guide uses `{ model, update, view }`, as it is preferable to list the most data-centric components first.
- For `type alias`es, a more traditional Javascript-esque style is used:
```elm
type alias Model = {
  content : String
}
```
vs.
```elm
type alias Model =
  { content : String
  }
```
- It is more correct to call the initial state `null` than `model`, as the model changes throughout the life of program.
- While the Elm Guide uses the `Model` constructor, it is better to explicitly use the Record style:
```elm
null = { content = "" }
```
vs.
```elm
null = Model ""
```
- For `Html` functions, a more traditional Javascript-esque style is used:
```elm
view { content } = div [] [
  input [onInput Content] [],
  text content
  ]
```
vs.
```elm
view model =
  div []
    [ input [ onInput Content ] []
    , text model.content
    ]
```

## Effectual Template
```elm
import Html exposing (..)
import Html.App exposing (program)
import Html.Events exposing (..)

main = program { init = null, subscriptions = subscriptions, update = update, view = view }

type alias Model = {
  content : String
}

type Msg = Content String

null : (Model, Cmd Msg)
null = ({ content = "" }, Cmd.none)

update : Msg -> Model -> (Model, Cmd Msg)
update msg model =
  case msg of
    Content s -> ({ model | content = s }, Cmd.none)

view : Model -> Html Msg
view { content } = div [] [
  input [onInput Content] [],
  text content
  ]

subscriptions : Model -> Sub Msg
subscriptions _ = Sub.none
```
