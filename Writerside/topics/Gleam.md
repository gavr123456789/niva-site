# Gleam
Not finished
<compare>
    <code-block lang="Scala">
        import gleam/io
        pub fn main() {
            io.println("Hello, Joe!")
        }
    </code-block>
    <code-block lang="Scala">
        "Hello, Gavr!" echo
    </code-block>
</compare>

<compare>
    <code-block lang="Scala">
        import gleam/io
        import gleam/string as text
        pub fn main() {
            // Use a function from the `gleam/io` module
            io.println("Hello, Mike!")
            // Use a function from the `gleam/string` module
            io.println(text.reverse("Hello, Joe!"))
        }
    </code-block>
    <code-block lang="Scala">
        // echo is a message for Any type, so here it works for String too
        "Hello, Gavr!" echo
        // reverse is a message for String
        "Hello, Gavr!" reverse echo
    </code-block>
</compare>

## Type checking

<compare>
    <code-block lang="Scala">
        import gleam/io
        pub fn main() {
            io.println("My lucky number is:")
            io.println(4) // Expected type: String Found: Int
            // 👆️ Uncomment this line
        }
    </code-block>
    <code-block lang="Scala">
        // since echo defined for Any type, this example is a bit different
        x = "1"
        1 + x // Same
    </code-block>
</compare>

## Ints
unary > binary > keyword, so we need parentheses here  
over-vice it would be  
`1 + 1 echo` == `1 + (1 echo)` and `1 + Unit` is a type mismatch
<compare>
    <code-block lang="Scala">
        // Int arithmetic
        io.debug(1 + 1)
        io.debug(5 - 1)
        io.debug(5 / 2)
        io.debug(3 * 3)
        io.debug(5 % 2)
        // Equality works for any type
        io.debug(1 == 1)
        io.debug(2 == 1)
    </code-block>
    <code-block lang="Scala">
        // Int arithmetic
        (1 + 1) echo
        (5 - 1) echo
        (5 / 2) echo
        (3 * 3) echo
        (5 % 2) echo
        // Equality works for any type
        // it is also possible to use pipe operator
        1 == 1 |> echo
        2 == 1 |> echo
    </code-block>
</compare>

## Floats

We don't need to use +. because niva type system is not Hindley-Mindler.  
Every message has a receiver, so there + for Float and + Int types. 
<compare>
    <code-block lang="Scala">
        io.debug(1.0 +. 1.5)
        io.debug(5.0 -. 1.5)
        io.debug(5.0 /. 2.5)
        io.debug(3.0 *. 3.5)
        // Float comparisons
        io.debug(2.2 >. 1.3)
        io.debug(2.2 >=. 1.3)
        // Equality works for any type
        io.debug(1.1 == 1.1)
        io.debug(2.1 == 1.2)
        // Division by zero is not an error
        io.debug(3.14 /. 0.0)
        // Standard library float functions
        io.debug(float.max(2.0, 9.5))
        io.debug(float.ceiling(5.4))
    </code-block>
    <code-block lang="Scala">
        1.0 + 1.5 |> echo
        5.0 - 1.5 |> echo
        5.0 / 2.5 |> echo
        3.0 * 3.5 |> echo
        // Float comparisons
        2.2 > 1.3 |> echo
        2.2 >= 1.3 |> echo
        // Equality works for any type
        1.1 == 1.1 |> echo
        2.1 == 1.2 |> echo
        // Division by zero IS an error
        3.14 / 0.0 |> echo
        // Standard library float functions
        io.debug(float.max(2.0, 9.5))
        io.debug(float.ceiling(5.4))
    </code-block>
</compare>

## Equality
The operators can be used with values of any type, but both sides of the operator must be of the same type.  
Equality is checked nominally, meaning that two objects are equal if they have the same memory location.  
There are plans to add structural typing(but that will require some runtime cost, since type under interface need late bindings).
<compare>
    <code-block lang="Scala">
        import gleam/io
        pub fn main() {
            io.println("My lucky number is:")
            io.println(4) // Expected type: String Found: Int
            // 👆️ Uncomment this line
        }
    </code-block>
    <code-block lang="Scala">
        // since echo defined for Any type, this example is a bit different
        x = "1"
        1 + x // Same
    </code-block>
</compare>  
