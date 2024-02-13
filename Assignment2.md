# General Feedback - Assignment 2


## The operation `::`

The `::` operator (called "cons") is used to construct a new list by adding an element to the front of an existing list.

It can also be used to "deconstruct" a list. For instance, in a pattern like `x :: xs`, `x` represents the head (first element) of the list, and `xs` represents the tail (rest of the list). The type of `x` is the element type, e.g., `int`, and the type of `xs` is a `list` type, e.g., `int list`.


Below is there an example of how to use the operation.

```fsharp
let rec prependToHead element lst =
    match lst with
    | [] -> [element]
    | x :: xs when x = 0 -> element :: x :: xs   // Add the element to the front of the existing list if the first element x is 0
    | _ -> lst
```

In the provided code snippet, the first element `x` is extracted from the list `lst` using the `::` operator. This allows us to check whether it is equal to 0. On the right side of the `->`, we use the `::` operator to prepend the specified element to the front of the list if the condition is satisfied.


### Get the two first elements from a list

When for instance extracting every other element from a list, you can utilize pattern matching with the syntax `x1::x2::xs`. This allows you to capture the first two elements efficiently. Here's a simple example to illustrate

```fsharp
let rec everyOtherElements lst =
    match lst with
    | x1::x2::xs -> ...
    | ...
```

Given the list `lst = [1; 2; 3; 4; 5; 6; 7; 8; 9]`, `x1 = 1`, `x2 = 2`, and `xs = [3; 4; 5; 6; 7; 8; 9]`.

### The difference between `@` and `::`

In F#, the `@` operator is used to concatenate two lists, while the `::` operator is employed to prepend an element to a list. In the given example, the number 1 is added to the list using both `@` and `::`.

```fsharp
// Using "@"
let originalList = [2; 3; 4]
let newList = [1] @ originalList
// newList is now [1; 2; 3; 4]
```

```fsharp
// Using "::"
let originalList = [2; 3; 4]
let newList = 1 :: originalList
// newList is now [1; 2; 3; 4]
```



## fold and foldBack

`fold` and `foldBack` are two higher-order functions. They both take an initial element, a list and a "folder" function as argument. The "folder" defines how the elements of the list are combined or accumulated. The folder function is applied iteratively to each element of the list, resulting in a single accumulated value.

***But what is the difference between fold and foldBack?***

- `List.fold` traverses the list from left to right.
Usage: `List.fold folder initial list`, where a template to the folder function could be
`(fun acc elem ->  <compute new accumulator>)`

- `List.foldBack` traverses the list from right to left.
Usage: `List.foldBack folder list initial`, where a template to the folder function could be
`(fun elem acc ->  <compute new accumulator>)`

This is illustrated in the code examples below. Both examples traverses `myList`, and prepend the element on the accumulated value (which in this case is a list).
`fold` is used in the first example. Since it traverses the list from left, 1 is prepended on the list first, and then 2 etc.:

```fsharp
let myList = [1; 2; 3; 4]

let foldedList = List.fold (fun acc elem -> elem :: acc) [] myList
// Result: (4 :: (3 :: (2 :: (1 :: []))))   --> [4; 3; 2; 1]
```

`foldBack` is used in the second example. Since it traverses the list from right, 4 is prepended on the list first, and then 3 etc. This will keep the order.

```fsharp
let myList = [1; 2; 3; 4]

let foldedBackList = List.foldBack (fun elem acc -> elem :: acc) myList []
// Result: (1 :: (2 :: (3 :: (4 :: []))))  --> [1; 2; 3; 4]
```

The example above demonstrate that `List.foldBack` is beneficial when the goal is to maintain the original order of elements in the list, especially when employing the `::` operation. This operation prepends each current element to the front of the accumulating result. Consequently, when the list is traversed from left to right, it effectively reverses the list, as illustrated in the first example above.

## Long expressions

In F# and other functional languages it is easy to put long, complex expressions on single lines. Long lines are often much harder to read and understand than many shorter lines. For example, many people wrote the |/| operator as follows:

```fsharp
let (|/|) (a : complex) (b : complex) =
    ((fst a) * (fst b) / ((fst b) * (fst b) + (snd b) * (snd b)), (snd a) * -(snd b) / ((fst b) * (fst b) + (snd b) * (snd b)))
```

In this case it is hard to understand what the function is doing. Instead, consider using let-bindings to store the results of evaluating sub-expressions:

```fsharp
let (|/|) (a : complex) (b : complex) =
    let divisor = fst b * fst b + snd b * snd b
    (fst a * (fst b) / divisor, snd a * -(snd b) / divisor)
```

There is no hard rule on how long lines should be. However, a lot of people prefer to keep lines less than 80 characters wide.


## Tuples

### Defining Tuple Types

In F#, a tuple type is defined using the `*` symbol. For example, the following declaration defines a type named `Ints` representing a tuple of two integers `(int, int)`.

```fsharp
type Ints = int * int
```

This also works for tuples with more than two elements. The following code defines a tuple type with three integers.

```fsharp
type Ints = int * int * int
```

### Accessing the elements

When working with tuples there are multiple ways of accessing the elements. When using pairs (tuples with two elements) one can access the elements using the fst and snd functions:

```fsharp
let myPair = (1, 'a')
let firstElement = fst myPair // firstElement is equal to 1
let secondElement = snd myPair // secondElement is equal to 'a'
```

This can often cause lines and expressions to become longer than necessary. Another way of accessing the elements of tuples is to use a let-binding:

```fsharp
let firstElement, secondElement = someExpressionEvaluatingToAPair
```

This also works for tuples with more than two elements.

You can also access the elements of tuples directly in the parameter list of a function. For example, the implementation of |/| from above can be written as follows:

```fsharp
let (|/|) ((a, b) : complex) ((c, d) : complex) =
    let c2d2 = c * c + d * d
    (a * c / c2d2, b * -d / c2d2)
```

Finally, you can use a match expression:

```fsharp
match myPair with
| (firstElement, secondElement) ->
    // Do something
```


## Parentheses Enhance Clarity

The code provided may lead to compiler complaints due to the absence of parentheses, causing potential confusion for compilers. Specifically, the expression `n + sum n-1` might be interpreted ambiguously, with uncertainty about whether 'n' and '-1' should be treated as separate arguments to the function 'sum'.


```fsharp
let rec sum n =
    match n with
    | 0 -> 0
    | _ -> n + sum n-1  (* Missing parentheses around n-1 *)
```

To resolve this issue, it is advisable to enhance clarity by enclosing the argument in parentheses, as demonstrated below:

```fsharp
let rec sum n =
    match n with
    | 0 -> 0
    | _ -> n + sum (n-1)  (* Added parentheses around n-1 *)
```
