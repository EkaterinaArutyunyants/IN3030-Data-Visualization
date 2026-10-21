---
follows: imdbRoot
---

# Minimalist Tufte Histogram

```elm {l v}
tufteHistogram : Spec
tufteHistogram =
    let
        cfg =
            configure
                << configuration (coView [ vicoStroke Nothing ])
                << configuration (coAxis [ axcoTicks False, axcoDomain False ])

        enc =
            encoding
                << position X
                    [ pName "IMDB Rating"
                    , pBin []
                    , pAxis [ axTitle "IMDB Rating", axGrid False ]
                    ]
                << position Y
                    [ pAggregate opCount
                    , pAxis [ axTitle "Number of Movies", axGrid False ]
                    ]
    in
    toVegaLite
        [ width 500
        , height 280
        , cfg []
        , data
        , enc []
        , bar [ maColor "#547A93" ]
        ]
```

## Evaluation

Minimalist version is easier tor ead. It removes unnecessary design borders, grid lines and colours. However, removing too many design elements is a bad idea. It make the chart harder to understand.
