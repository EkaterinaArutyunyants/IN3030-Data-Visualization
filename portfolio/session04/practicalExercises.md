---
id: litvis

narrative-schemas:
  - ../../lectures/narrative-schemas/teaching.yml

elm:
  dependencies:
    gicentre/elm-vegalite: latest
    gicentre/tidy: latest
---

@import "../../lectures/css/datavis.less"

```elm {l=hidden}
import Tidy exposing (..)
import VegaLite exposing (..)
```

<!-- Everything above this line should probably be left untouched. -->

# Session 4: Practical Exercises

{(task|}

## TASK 1.

### a. Suitable Measurement Types

| Visual Variable   | Suitable Measurement Types     |
| ----------------- | ------------------------------ |
| Colour hue        | Nominal                        |
| Colour saturation | Ordinal, Quantitative          |
| Colour lightness  | Ordinal, Quantitative          |
| Orientation       | Nominal, Ordinal               |
| Shape             | Nominal                        |
| Texture           | Nominal, Ordinal               |
| Arrangement       | Nominal, Ordinal               |
| Size              | Ordinal, Quantitative          |
| Focus             | Nominal, Ordinal               |
| Location          | Nominal, Ordinal, Quantitative |

### b. Visual Variable Properties

Ratings: 1 = weak, 2 = moderate, 3 = strong

| Visual Variable   | Quantitative | Orderable | Selective | Associative | Dissociative |
| ----------------- | ------------ | --------- | --------- | ----------- | ------------ |
| Colour hue        | 1            | 1         | 3         | 3           | 3            |
| Colour saturation | 1            | 3         | 2         | 2           | 2            |
| Colour lightness  | 2            | 3         | 2         | 2           | 2            |
| Orientation       | 1            | 2         | 2         | 3           | 2            |
| Shape             | 1            | 1         | 3         | 3           | 2            |
| Texture           | 1            | 2         | 2         | 3           | 2            |
| Arrangement       | 1            | 2         | 2         | 3           | 2            |
| Size              | 2            | 3         | 3         | 1           | 3            |
| Focus             | 1            | 2         | 3         | 2           | 3            |
| Location          | 3            | 3         | 3         | 3           | 2            |

### TASK 2

```elm {l}
vvTable =
    let
        table =
            """visualVariable,q,o,s,a,d
colour hue,1,1,3,3,3
colour saturation,1,3,2,2,2
colour lightness,2,3,2,2,2
orientation,1,2,2,3,2
shape,1,1,3,3,2
texture,1,2,2,3,2
arrangement,1,2,2,3,2
size,2,3,3,1,3
focus,1,2,3,2,3
location,3,3,3,3,2"""
                |> fromCSV
                |> gather "property"
                    "rating"
                    [ ( "q", "Quantitative" )
                    , ( "o", "Orderable" )
                    , ( "s", "Selective" )
                    , ( "a", "Associative" )
                    , ( "d", "Dissociative" )
                    ]
    in
    dataFromColumns []
        << dataColumn "visualVariable" (table |> strColumn "visualVariable" |> strs)
        << dataColumn "property" (table |> strColumn "property" |> strs)
        << dataColumn "rating" (table |> numColumn "rating" |> nums)
```

```elm {l v}
summary : Spec
summary =
    let
        enc =
            encoding
                << position X
                    [ pName "property"
                    , pOrdinal
                    , pAxis [ axTitle "", axLabelAngle -45 ]
                    ]
                << position Y
                    [ pName "visualVariable"
                    , pNominal
                    , pAxis [ axTitle "" ]
                    ]
                << color [ mName "rating", mOrdinal ]
    in
    toVegaLite
        [ width 450
        , height 300
        , vvTable []
        , enc []
        , circle [ maSize 200 ]
        ]
```

{|task)}
