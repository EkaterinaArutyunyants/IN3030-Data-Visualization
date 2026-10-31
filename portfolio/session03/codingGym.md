---
id: litvis

narrative-schemas:
  - ../../lectures/narrative-schemas/teaching.yml
---

@import "../../lectures/css/datavis.less"

<!-- Everything above this line should probably be left untouched. -->

# Coding Gym 3 : Parameterising and Transforming Lists

_These exercises are designed to help you build confidence programming with Elm. They assume no prior experience of coding in any language other than that arising from previous weeks of this module. This document is designed to be viewed in VSCode with litvis so you can add your own code blocks to answer each question._

## 3.1. Functions with Parameters

The functions you have created in the previous gyms return the same value every time they are called. You can customise what they return by providing input _parameters_ that are used to generate the output. Parameters are indicated in a type annotation, naming their type and separating each of them and the output with a `->` symbol. For example:

```elm {l}
add : Float -> Float -> Float
add firstNum secondNum =
    firstNum + secondNum


greeting : String -> String
greeting name =
    "Good evening " ++ name
```

Parameterised functions can be _called_ by providing values for each of the parameters after the function name. For example:

```elm {l r}
summary : Float
summary =
    add 120 84
```

```elm {l r}
welcomeMessage : String
welcomeMessage =
    greeting "Ada Lovelace"
```

{(task|}

Create separate parameterised functions, each in their own code block, that do the following. Each code block should have the header `elm {l}`. To see the result, you should create separate code blocks with the header `elm {l r}` and than call the functions you have created providing values for the parameters. See the examples `summary` and `welcomeMessage` above if you are unsure. If created successfully, you should see the evaluation of these functions appear in the preview window.

- A function that given two words as `String` parameters returns a single string with the words separated by the word 'or'.

```elm {l}
wordsWithOr : String -> String -> String
wordsWithOr word1 word2 =
    word1 ++ " or " ++ word2
```

```elm {l r}
wordsResult : String
wordsResult =
    wordsWithOr "tea" "coffee"
```

- A function that given a single `String` parameter representing a colour, returns "My second favourite colour is ", naming the colour provided.

```elm {l}
secondFavouriteColour : String -> String
secondFavouriteColour colour =
    "My second favourite colour is " ++ colour
```

```elm {l r}
colourResult : String
colourResult =
    secondFavouriteColour "blue"
```

- A function to add three numbers provided as parameters.

```elm {l}
addThree : Float -> Float -> Float -> Float
addThree a b c =
    a + b + c
```

```elm {l r}
threeNumbersResult : Float
threeNumbersResult =
    addThree 10 20 30
```

- A function that creates a list of integers from 1 to a number provided as a parameter.

```elm {l}
numbersTo : Int -> List Int
numbersTo n =
    List.range 1 n
```

```elm {l r}
numbersResult : List Int
numbersResult =
    numbersTo 6
```

- A function that given two `Float` parameters, provides the square root of the sum of their squares.

```elm {l}
squareRootSum : Float -> Float -> Float
squareRootSum a b =
    sqrt (a * a + b * b)
```

```elm {l r}
squareRootResult : Float
squareRootResult =
    squareRootSum 3 4
```

{|task)}

## 3.2. Transforming Lists

Just as we applied operators to individual items, we can apply them to all the items in a list. This is commonly achieved with the in-built function [List.map](https://package.elm-lang.org/packages/elm/core/latest/List#map). This function takes two parameters: the first is the function that will be applied to each item in the list and the second is the list itself. For example to increment all the values in a list by 1 we could do the following:

```elm {l}
incByOne : List Int -> List Int
incByOne myList =
    let
        inc num =
            num + 1
    in
    List.map inc myList
```

```elm {l r}
newList : List Int
newList =
    incByOne [ 10, 20, 30, 40 ]
```

{(task|}

Create separate parameterised functions, each in their own code block, that do the following. Each code block should have the header `elm {l}`. Test them by creating separate functions that supply the list as a parameter (like `newList` above)

- A function that given a list of words, adds the word "potato" to each of them.

```elm {l}
addPotato : List String -> List String
addPotato words =
    List.map (\x -> x ++ "potato") words
```

```elm {l r}
potatoResult : List String
potatoResult =
    addPotato [ "sweet", "mashed", "baked" ]
```

- A function that adds 2023 to all the items in a list of numbers.

```elm {l}
add2023 : List Int -> List Int
add2023 numbers =
    List.map (\n -> n + 2023) numbers
```

```elm {l r}
add2023Result : List Int
add2023Result =
    add2023 [ 1, 2, 3 ]
```

- A function that creates a new list that comprises the numbers in a given list multiplied by 5.

```elm {l}
multiplyBy5 : List Int -> List Int
multiplyBy5 numbers =
    List.map (\n -> n * 5) numbers
```

```elm {l r}
multiplyResult : List Int
multiplyResult =
    multiplyBy5 [ 2, 4, 6 ]
```

- A function that finds the sum of the squares of all numbers in a list (_Hint: You may find the function [List.sum](https://package.elm-lang.org/packages/elm/core/latest/List#sum) helpful here_).

```elm {l}
sumOfSquares : List Int -> Int
sumOfSquares numbers =
    List.sum (List.map (\n -> n * n) numbers)
```

```elm {l r}
squaresResult : Int
squaresResult =
    sumOfSquares [ 1, 2, 3 ]
```

- A function that given an integer, creates a list of multiples of 5 (5, 10, 15 etc.) from 5 to that integer.

```elm {l}
multiplesOf5 : Int -> List Int
multiplesOf5 n =
    List.filter (\x -> modBy 5 x == 0) (List.range 5 n)
```

```elm {l r}
multiplesResult : List Int
multiplesResult =
    multiplesOf5 23
```

{|task)}
