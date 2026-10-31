---
id: litvis
follows: titanicData
---

@import "../css/datavis.less"

```elm {l=hidden}
fmColours =
    categoricalDomainMap [ ( "female", "rgb(180,90,90)" ), ( "male", "rgb(100,130,170)" ) ]
```

## Passengers who died

Gender and ticket class comparison:

```elm {l v}
barsByClass : Spec
barsByClass =
    let
        data =
            dataFromColumns []
                << dataColumn "pclass" (numColumn "pclass" deathsTable |> nums)
                << dataColumn "sex" (strColumn "sex" deathsTable |> strs)

        enc =
            encoding
                << position X
                    [ pName "pclass"
                    , pAxis [ axTitle "Passenger class", axLabelAngle 0, axTicks False ]
                    ]
                << position Y [ pAggregate opCount, pTitle "Number of deaths" ]
                << color [ mName "sex", mScale fmColours, mTitle "" ]
    in
    toVegaLite [ width 180, data [], enc [], bar [] ]
```

Gender and age profile:

```elm {l v}
barsByAge : Spec
barsByAge =
    let
        data =
            dataFromColumns []
                << dataColumn "age" (numColumn "age" deathsTable |> nums)
                << dataColumn "sex" (strColumn "sex" deathsTable |> strs)

        enc =
            encoding
                << position X [ pName "age", pOrdinal, pBin [ biStep 5 ] ]
                << position Y [ pAggregate opCount, pTitle "Number of deaths" ]
                << color [ mName "sex", mScale fmColours, mTitle "" ]
    in
    toVegaLite [ width 300, data [], enc [], bar [] ]
```

# Demo starts here

```elm {v l highlight=[7,8,9,20]}
histo : Spec
histo =
    let
        data =
            dataFromUrl "https://gicentre.github.io/data/titanic/titanic.json"

        trans =
            transform
                << filter (fiExpr "datum.survived == 0")

        enc =
            encoding
                << position X
                    [ pName "pclass"
                    , pAxis [ axTitle "Passenger class", axLabelAngle 0, axTicks False ]
                    ]
                << position Y [ pAggregate opCount ]
                << color [ mName "sex", mScale fmColours, mLegend [ leTitle "" ] ]
    in
    toVegaLite [ width 180, data [], trans [], enc [], bar [] ]
```

The filter function takes an [fiExpr](https://package.elm-lang.org/packages/gicentre/elm-vegalite/latest/VegaLite#fiExpr) expression that specifies the conditions required to keep the data. To refer to any data value, we prefix the name of the data field (i.e. column) with `datum.` in the expression.

Here is the same approach, applied to show the distribution of the age and gender of survivors:

```elm {l v highlight=[9, 13]}
survivorsByAge : Spec
survivorsByAge =
    let
        data =
            dataFromUrl "https://gicentre.github.io/data/titanic/titanic.json"

        trans =
            transform
                << filter (fiExpr "datum.survived == 1 && datum.age > 0")

        enc =
            encoding
                << position X [ pName "age", pOrdinal, pBin [ biStep 5 ] ]
                << position Y [ pAggregate opCount ]
                << color [ mName "sex", mScale fmColours, mLegend [ leTitle "" ] ]
    in
    toVegaLite [ width 300, data [], trans [], enc [], bar [] ]
```

but we don't know if the variations above are due to variations in the numbers of passengers in each group or true variations in likelihood of survival across groups... perhaps showing the full counts of passengers helps?

```elm {l v highlight=[9]}
passengersByAge : Spec
passengersByAge =
    let
        data =
            dataFromUrl "https://gicentre.github.io/data/titanic/titanic.json"

        trans =
            transform
                << filter (fiExpr "datum.age > 0")

        enc =
            encoding
                << position X [ pName "age", pOrdinal, pBin [ biStep 5 ] ]
                << position Y [ pAggregate opCount ]
                << color [ mName "sex", mScale fmColours, mLegend [ leTitle "" ] ]
    in
    toVegaLite [ width 300, data [], trans [], enc [], bar [] ]
```

```elm {l v highlight=[15]}
passengersBySurvival : Spec
passengersBySurvival =
    let
        data =
            dataFromUrl "https://gicentre.github.io/data/titanic/titanic.json"

        trans =
            transform
                << filter (fiExpr "datum.age > 0")

        enc =
            encoding
                << position X [ pName "age", pOrdinal, pBin [ biStep 5 ] ]
                << position Y [ pAggregate opCount ]
                << color [ mName "survived", mLegend [ leTitle "" ] ]
    in
    toVegaLite [ width 300, data [], trans [], enc [], bar [] ]
```

This data may be better represented as proportions rather than absolute counts

```elm {l v highlight=[16]}
propBySurvival : Spec
propBySurvival =
    let
        data =
            dataFromUrl "https://gicentre.github.io/data/titanic/titanic.json"

        trans =
            transform
                << filter (fiExpr "datum.age > 0")

        enc =
            encoding
                << position X [ pName "age", pOrdinal, pBin [ biStep 5 ], pTitle "" ]
                << position Y
                    [ pAggregate opCount
                    , pStack stNormalize
                    , pAxis [ axFormat "%", axValues (nums [ 0, 0.25, 0.5, 0.75, 1 ]), axTitle "" ]
                    ]
                << color [ mName "survived", mLegend [ leTitle "" ] ]
    in
    toVegaLite [ width 300, data [], trans [], enc [], bar [] ]
```

But remember our questions were Q1: What are the relationships (if any) between likelihood of survival, _gender_, and _ticket class_? Q2: To what extent do these relationships (if they exist) vary with age of passenger?

```elm {l v highlight=[24]}
propBySurvivalAndGender : Spec
propBySurvivalAndGender =
    let
        data =
            dataFromUrl "https://gicentre.github.io/data/titanic/titanic.json"

        trans =
            transform
                << filter (fiExpr "datum.age > 0")

        enc =
            encoding
                << position X [ pName "age", pOrdinal, pBin [ biStep 5 ], pTitle "" ]
                << position Y
                    [ pAggregate opCount
                    , pStack stNormalize
                    , pAxis [ axFormat "%", axValues (nums [ 0, 0.25, 0.5, 0.75, 1 ]), axTitle "" ]
                    ]
                << color
                    [ mName "sex"
                    , mScale fmColours
                    , mTitle ""
                    ]
                << opacity [ mName "survived" ]
    in
    toVegaLite [ width 300, data [], trans [], enc [], bar [] ]
```

## What about ticket class?

```elm {l v highlight=[25]}
propBySurvivalAndGenderAndClass : Spec
propBySurvivalAndGenderAndClass =
    let
        data =
            dataFromUrl "https://gicentre.github.io/data/titanic/titanic.json"

        trans =
            transform
                << filter (fiExpr "datum.age > 0")

        enc =
            encoding
                << position X [ pName "age", pOrdinal, pBin [ biStep 5 ], pTitle "" ]
                << position Y
                    [ pAggregate opCount
                    , pStack stNormalize
                    , pAxis [ axFormat "%", axValues (nums [ 0, 0.25, 0.5, 0.75, 1 ]), axTitle "" ]
                    ]
                << color
                    [ mName "sex"
                    , mScale fmColours
                    , mTitle ""
                    ]
                << opacity [ mName "survived" ]
                << column [ fName "pclass" ]
    in
    toVegaLite [ width 300, data [], trans [], enc [], bar [] ]
```
