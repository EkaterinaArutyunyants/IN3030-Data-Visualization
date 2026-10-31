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

# Cycle Hire Build-up

## No visual channels

```elm {l v}
hireScatterplotNoChannels : Spec
hireScatterplotNoChannels =
    let
        data =
            dataFromUrl "https://gicentre.github.io/data/bicycleHiresLondon.csv"

        enc =
            encoding
    in
    toVegaLite [ width 640, data [], enc [], circle [ maSize 9000 ] ]
```

The dataset contains multiple records, but without encoding channels, all records occupy the same default position. The circles overlap.

## Adding position channels

```elm {l v}
hireScatterplotPChannels : Spec
hireScatterplotPChannels =
    let
        data =
            dataFromUrl "https://gicentre.github.io/data/bicycleHiresLondon.csv"

        enc =
            encoding
                << position X [ pName "NumberOfHires", pQuant ]
                << position Y [ pName "AvHireTime", pQuant ]
    in
    toVegaLite [ width 640, data [], enc [], circle [ maSize 90 ] ]
```

We have not changed the data or the mark type. We have mapped two quantitative attributes to spatial position.

## Use a different mark

We can change the circle mark to a point mark. This is a small but important step because the point mark supports a configurable shape, including custom SVG paths.

```elm {l v}
hireScatterplotPointMark : Spec
hireScatterplotPointMark =
    let
        data =
            dataFromUrl "https://gicentre.github.io/data/bicycleHiresLondon.csv"

        enc =
            encoding
                << position X [ pName "NumberOfHires", pQuant ]
                << position Y [ pName "AvHireTime", pQuant ]
    in
    toVegaLite [ width 640, data [], enc [], point [ maSize 90 ] ]
```

A point mark can take other shapes. It can also be filled.

```elm {l v}
hireScatterplotPointMark2 : Spec
hireScatterplotPointMark2 =
    let
        data =
            dataFromUrl "https://gicentre.github.io/data/bicycleHiresLondon.csv"

        enc =
            encoding
                << position X [ pName "NumberOfHires", pQuant ]
                << position Y [ pName "AvHireTime", pQuant ]
    in
    toVegaLite [ width 640, data [], enc [], point [ maShape symTriangleUp, maSize 200, maFilled True ] ]
```

## Can you replace the triangles with bicycles?

```elm {l v}
hireScatterplotPointMark3 : Spec
hireScatterplotPointMark3 =
    let
        data =
            dataFromUrl "https://gicentre.github.io/data/bicycleHiresLondon.csv"

        bikePath =
            "M17.1945 6H14.25V7.5H15.8055L16.0362 10.5H13.5H12.9343H9V9.75H9.75V8.25H6V9.75H7.5V11.145L7.0383 12.7609C6.94315 12.7537 6.847 12.75 6.75 12.75C4.67893 12.75 3 14.4289 3 16.5C3 18.5711 4.67893 20.25 6.75 20.25C8.56422 20.25 10.0775 18.9617 10.425 17.25H12.5657L14.0657 12H16.1516L16.2203 12.8931C14.6499 13.3406 13.5 14.786 13.5 16.5C13.5 18.5711 15.1789 20.25 17.25 20.25C19.3211 20.25 21 18.5711 21 16.5C21 14.5867 19.5671 13.0081 17.716 12.7787L17.1945 6ZM6.61165 14.2542L6.02886 16.294L7.47114 16.706L8.05397 14.6662C8.62657 15.074 9 15.7434 9 16.5C9 17.7426 7.99264 18.75 6.75 18.75C5.50736 18.75 4.5 17.7426 4.5 16.5C4.5 15.3038 5.43344 14.3257 6.61165 14.2542ZM8.48075 13.1724L8.81573 12H12.5057L11.4343 15.75H10.425C10.1971 14.6273 9.46767 13.6868 8.48075 13.1724ZM16.3395 14.4419L16.5022 16.5575L17.9978 16.4425L17.835 14.3268C18.794 14.5843 19.5 15.4597 19.5 16.5C19.5 17.7426 18.4926 18.75 17.25 18.75C16.0074 18.75 15 17.7426 15 16.5C15 15.5814 15.5504 14.7914 16.3395 14.4419Z"

        enc =
            encoding
                << position X [ pName "NumberOfHires", pQuant ]
                << position Y [ pName "AvHireTime", pQuant ]
    in
    toVegaLite
        [ width 640
        , data []
        , enc []
        , point
            [ maShape (symPath bikePath)
            , maSize 4
            , maFilled False
            ]
        ]
```

See if you can change the colour and size of the bicycles. Also see if you can download a different SVG shape from https://www.svgrepo.com/

## Using Trail

Discuss: what are the pros and cons of the following design?

```elm {v}
hireScatterplotPointMark4 : Spec
hireScatterplotPointMark4 =
    let
        data =
            dataFromUrl
                "https://gicentre.github.io/data/bicycleHiresLondon.csv"
                []

        -- Keep your existing SVG path here
        bikePath =
            "M17.1945 6H14.25V7.5H15.8055L16.0362 10.5H13.5H12.9343H9V9.75H9.75V8.25H6V9.75H7.5V11.145L7.0383 12.7609C6.94315 12.7537 6.847 12.75 6.75 12.75C4.67893 12.75 3 14.4289 3 16.5C3 18.5711 4.67893 20.25 6.75 20.25C8.56422 20.25 10.0775 18.9617 10.425 17.25H12.5657L14.0657 12H16.1516L16.2203 12.8931C14.6499 13.3406 13.5 14.786 13.5 16.5C13.5 18.5711 15.1789 20.25 17.25 20.25C19.3211 20.25 21 18.5711 21 16.5C21 14.5867 19.5671 13.0081 17.716 12.7787L17.1945 6ZM6.61165 14.2542L6.02886 16.294L7.47114 16.706L8.05397 14.6662C8.62657 15.074 9 15.7434 9 16.5C9 17.7426 7.99264 18.75 6.75 18.75C5.50736 18.75 4.5 17.7426 4.5 16.5C4.5 15.3038 5.43344 14.3257 6.61165 14.2542ZM8.48075 13.1724L8.81573 12H12.5057L11.4343 15.75H10.425C10.1971 14.6273 9.46767 13.6868 8.48075 13.1724ZM16.3395 14.4419L16.5022 16.5575L17.9978 16.4425L17.835 14.3268C18.794 14.5843 19.5 15.4597 19.5 16.5C19.5 17.7426 18.4926 18.75 17.25 18.75C16.0074 18.75 15 17.7426 15 16.5C15 15.5814 15.5504 14.7914 16.3395 14.4419Z"

        -- Restrict the dataset to 2025
        trans =
            transform
                << filter
                    (fiExpr
                        "datum.Month >= '2025-01' && datum.Month <= '2025-12'"
                    )

        -- Common X and Y positions
        positions =
            encoding
                << position X
                    [ pName "Month"
                    , pOrdinal
                    , pTitle "Month"
                    ]
                << position Y
                    [ pName "NumberOfHires"
                    , pQuant
                    , pTitle "Number of hires"
                    ]

        -- Trail: thickness represents average hire time
        trailEnc =
            positions
                << size
                    [ mName "AvHireTime"
                    , mQuant
                    , mTitle "Average hire time (minutes)"
                    ]

        trailLayer =
            asSpec
                [ trail
                    [ maColor "#E08A45"
                    , maOpacity 0.7
                    ]
                , trailEnc []
                ]

        -- Bicycle symbols
        bikeLayer =
            asSpec
                [ point
                    [ maShape (symPath bikePath)
                    , maSize 4
                    , maFilled True
                    , maColor "#2878A8"
                    ]
                , positions []
                ]
    in
    toVegaLite
        [ width 640
        , height 400
        , data
        , trans []
        , layer
            [ trailLayer
            , bikeLayer
            ]
        ]
```
