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

# Session 3: Practical Exercises

{(task|}

### TASK 1

Only table 'a' is tidy because each column is a seperate variable and each row represents one party.

New table 'b'

| Rank | Country | Medal Type | Count |
| ---- | ------- | ---------- | ----- |
| 1    | China   | Gold       | 96    |
| 1    | China   | Silver     | 60    |
| 1    | China   | Bronze     | 51    |
| 2    | GB      | Gold       | 41    |
| 2    | GB      | Silver     | 38    |
| 2    | GB      | Bronze     | 45    |
| 3    | US      | Gold       | 37    |
| 3    | US      | Silver     | 36    |
| 3    | US      | Bronze     | 31    |
| 4    | RPC     | Gold       | 36    |
| 4    | RPC     | Silver     | 33    |
| 4    | RPC     | Bronze     | 49    |

New table 'c'

| id      | date       | Temperature Type | Temperature |
| ------- | ---------- | ---------------- | ----------- |
| MX17004 | 2010-01-30 | Max              | 27.8        |
| MX17004 | 2010-01-30 | Min              | 14.5        |
| MX17004 | 2010-02-02 | Max              | 27.3        |
| MX17004 | 2010-02-02 | Min              | 14.4        |
| MX17004 | 2010-02-03 | Max              | 24.1        |
| MX17004 | 2010-02-03 | Min              | 14.4        |
| MX17004 | 2010-02-11 | Max              | 29.7        |
| MX17004 | 2010-02-11 | Min              | 13.4        |
| MX17004 | 2010-02-23 | Max              | 29.9        |
| MX17004 | 2010-02-23 | Min              | 10.7        |
| MX17004 | 2010-03-05 | Max              | 32.1        |
| MX17004 | 2010-03-05 | Min              | 14.2        |
| MX17004 | 2010-03-10 | Max              | 34.5        |
| MX17004 | 2010-03-10 | Min              | 16.8        |
| MX17004 | 2010-03-16 | Max              | 31.1        |
| MX17004 | 2010-03-16 | Min              | 17.6        |
| MX17004 | 2010-04-27 | Max              | 36.3        |
| MX17004 | 2010-04-27 | Min              | 16.7        |
| MX17004 | 2010-05-27 | Max              | 33.2        |
| MX17004 | 2010-05-27 | Min              | 18.2        |

New table 'd'

| Party                     | Year | percentVote | numMPs |
| ------------------------- | ---- | ----------- | ------ |
| Conservative              | 2017 | 42.3        | 317    |
| Conservative              | 2019 | 43.6        | 365    |
| Labour                    | 2017 | 40.0        | 262    |
| Labour                    | 2019 | 32.2        | 202    |
| Scottish National Party   | 2017 | 3.0         | 35     |
| Scottish National Party   | 2019 | 3.9         | 48     |
| Liberal Democrats         | 2017 | 7.4         | 12     |
| Liberal Democrats         | 2019 | 11.6        | 11     |
| Democratic Unionist Party | 2017 | 0.9         | 10     |
| Democratic Unionist Party | 2019 | 0.8         | 8      |

### TASK 2

Table b

```elm {l}
medalsTable : Table
medalsTable =
    """Rank,Country,NumGold,NumSilver,NumBronze
1,China,96,60,51
2,GB,41,38,45
3,US,37,36,31
4,RPC,36,33,49"""
        |> fromCSV


tidyMedals : Table
tidyMedals =
    medalsTable
        |> gather "Medal Type"
            "Count"
            [ ( "NumGold", "Gold" )
            , ( "NumSilver", "Silver" )
            , ( "NumBronze", "Bronze" )
            ]
```

```elm {m}
medalsSummary : List String
medalsSummary =
    tableSummary -1 tidyMedals
```

Table C

```elm {l}
tempTable : Table
tempTable =
    """id,date,maxTemperature,minTemperature
MX17004,2010-01-30,27.8,14.5
MX17004,2010-02-02,27.3,14.4
MX17004,2010-02-03,24.1,14.4
MX17004,2010-02-11,29.7,13.4
MX17004,2010-02-23,29.9,10.7
MX17004,2010-03-05,32.1,14.2
MX17004,2010-03-10,34.5,16.8
MX17004,2010-03-16,31.1,17.6
MX17004,2010-04-27,36.3,16.7
MX17004,2010-05-27,33.2,18.2"""
        |> fromCSV


tidyTemp : Table
tidyTemp =
    tempTable
        |> gather "Temperature type"
            "Temperature"
            [ ( "maxTemperature", "Max" )
            , ( "minTemperature", "Min" )
            ]
```

```elm {m}
tempSummary : List String
tempSummary =
    tableSummary -1 tidyTemp
```

Table d

```elm {l}
electionTable : Table
electionTable =
    """Party,percentVote2017,numMPs2017,percentVote2019,numMPs2019
Conservative,42.3,317,43.6,365
Labour,40.0,262,32.2,202
Scottish National Party,3.0,35,3.9,48
Liberal Democrats,7.4,12,11.6,11
Democratic Unionist Party,0.9,10,0.8,8"""
        |> fromCSV


electionGathered : Table
electionGathered =
    electionTable
        |> gather "MeasureYear"
            "Value"
            [ ( "percentVote2017", "percentVote2017" )
            , ( "numMPs2017", "numMPs2017" )
            , ( "percentVote2019", "percentVote2019" )
            , ( "numMPs2019", "numMPs2019" )
            ]
```

### TASK 3

{|task)}
