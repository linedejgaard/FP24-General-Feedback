# General Feedback - Assignment 1

## How to run the code?

It is **important** to be able to run the code. There two different ways to run the code:
1. F# Interactive
2. Run it as a project

There are two guides uploaded to this repo explaining how to set up the IDE and run the project as well as how to run the code using F# Interactive. These documents are new, so if there are any mistakes, please let us know.

## Deadline

It seems that it has been possible to hand-in after deadline. Eventhough it is possible, please respect the deadline. The deadline is **Tuesday at 08:00**.

## Check AutoTest and use the template

Check that CodeGrade compile your submission. Many of the compile errors could be fixed by using the template.
- It is important that the file has a specific name to make it compile. For the first assignment this was Assignment1.fs. One student submitted Program.fs. This won't compile.
- It is important to have the module name given in the template. In this case `module Assignment1`.
- All functions should have the signature and type specified in the assignment.


## Things relevant to the exercises

### Match over if-then-else expressions and when-clauses

We prefer match cases and the when keyword over if-then-else statements.
Here is a function `f : int -> string` using nested if-expressions.

```fsharp
let f n = 
    if n = 0 then 
        "Zero"
    else
        if n % 2 = 0 then
            "Even"
        else "Odd"
```

Here is the same function using match cases.

```fsharp
let f n =
    match n with
    | 0 -> "Zero"
    | m -> if m % 2 = 0 then "Even" else "Odd"
```

In many cases we can completely get rid of the if-then-else statements by using when-clauses. This can be rewritten as:

```fsharp
let f n =
    match n with
    | 0 -> "Zero"
    | n when n % 2 = 0 -> "Even"
    | _ -> "Odd"
```

This is a structure we are looking for. The match cases are more scalable and it is very clear what separates them from one another.

Notice that we are using the wildcard-pattern (_) in the last match case. It matches anything, and we can use it, because it makes it clear that we do not use the value we are matching against.

### match vs. function vs. fun?

Here is a function `isZero : int -> bool` that tells you whether or not an integer is equal to zero.

```fsharp
let isZero x = 
    match x with
    | 0 -> true
    | n -> false
```

Here is the same function using the function keyword.

```fsharp
let isZero = function
    | 0 -> true
    | n -> false
```

The `function` keyword allows us to completely get rid of the argument `x` and simplify `match x with`. So, should you use one over the other? There is no truly correct way, and depends on your preferences. If you would like to be clearly able to see what you are matching against, use the first one. If you are comfortable in knowing that function introduces an implicit argument and want to save some characters, use the last one.

Another way to write the `isZero` function is using the `fun` keyword.

```fsharp
let isZero = fun x ->
    match x with
    | 0 -> true
    | n -> false
```

The `fun` keywords allows you to declare nameless or anonymous functions.

### Explicit writing the type

Often the compiler figures it out itself, but sometimes we need to specify the type (it might also be useful in order to get more help from the compiler).

You specify the type by writing `<function name>:<type>`.

Below there is an example of specifying the type of an argument.

```fsharp
let isZero (x:int) = 
    match x with
    | 0 -> true
    | n -> false
```

It is important to wrap the argument in parenthesis s.t. the compiler is not confused about where the type "belongs". Without parenthesis, it means that the function `isZero` will return a `bool`:

```fsharp
let isZero x : bool = 
    match x with
    | 0 -> true
    | n -> false
```

Here we specify that `x` should be an `int`, and that `isZero` should return a `bool`:

```fsharp
let isZero (x:int) : bool = 
    match x with
    | 0 -> true
    | n -> false
```



Here we specify that the function `isZero` should be of type `int -> bool`.

```fsharp
let isZero : int -> bool = function
    | 0 -> true
    | n -> false
```


### For-loops and mutability

Don't do them. We will never ask you to do for-loops. Stick with recursion if you need to loop.

Values in F# are immutable by default. This means that once they are defined you can't change them again. You will have to bind a modified value to another identifier. It is possible to do mutable values in fsharp, it just has very little to do with the course. Here is an example on how to do it (you shouldn't), so you can see what that looks like.

```fsharp
// Bad - will print "42"
let mutable x = 5
x <- x + 37
printfn "%A" x

// Good - will print "42"
let x = 5
let y = x + 37
printfn "%A" y
```

For-loops change mutable values or data structures inside the loop. That is all they can do. And since we don't want mutable data structures we don't want for-loops either.

### Unnecessary semicolon

Some students write `;` or `;;` after each function. This is not necessary unless you run it using F# interactive. In this case, all code must be terminated with a double semicolon ";;" to be evaluated.

### Indentation

The match cases need to be on the same column as `match x with`, so we need to move this down to a line of its own to make it more readable.

```fsharp
// Bad
let someLongNameForIsZero x = match x with
                              | 0 -> true
                              | _ -> false

// Good
let someLongNameForIsZero x = 
    match x with
    | 0 -> true
    | _ -> false
```

### Too many cases
Another thing to mention is that some students have unnecessary many pattern matches. For instance the first match on `(0, 0)`, is not necessary, because `k = 0` and `k = n`, so it will be matched in the second case.

```fsharp
let rec bin = function
    | (0, 0) -> 1
    | (n, k) when k = 0 || k = n -> 1
    | ...
```