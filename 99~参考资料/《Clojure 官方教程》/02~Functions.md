## Creating Functions

Clojure is a functional language. Functions are first-class and can be passed-to or returned-from other functions. Most Clojure code consists primarily of pure functions (no side effects), so invoking with the same inputs yields the same output.

`defn` defines a named function:

```
;;    name   params         body
;;    -----  ------  -------------------
(defn greet  [name]  (str "Hello, " name) )
```

This function has a single parameter `name`, however you may include any number of parameters in the params vector.

Invoke a function with the name of the function in "function position" (the first element of a list):

```clj
user=> (greet "students")
"Hello, students"
```

### Multi-arity functions

Functions can be defined to take different numbers of parameters (different "arity"). Different arities must all be defined in the same `defn` - using `defn` more than once will replace the previous function.

Each arity is a list `([param*] body*)`. One arity can invoke another. The body can contain any number of expressions and the return value is the result of the last expression.

```
(defn messenger
  ([]     (messenger "Hello world!"))
  ([msg]  (println msg)))
```

This function declares two arities (0 parameters and 1 parameter). The 0-parameter arity calls the 1-parameter arity with a default value to print. We invoke these functions by passing the appropriate number of arguments:

```
user=> (messenger)
Hello world!
nil

user=> (messenger "Hello class!")
Hello class!
nil
```

### Variadic functions

Functions may also define a variable number of parameters - this is known as a "variadic" function. The variable parameters must occur at the end of the parameter list. They will be collected in a sequence for use by the function.

The beginning of the variable parameters is marked with `&`.

```
(defn hello [greeting & who]
  (println greeting who))
```

This function takes a parameter `greeting` and a variable number of parameters (0 or more) that will be collected in a list named `who`. We can see this by invoking it with 3 arguments:

```
user=> (hello "Hello" "world" "class")
Hello (world class)
```

You can see that when `println` prints `who`, it is printed as a list of two elements that were collected.

### Anonymous Functions

An anonymous function can be created with `fn`:

```
;;    params         body
;;   ---------  -----------------
(fn  [message]  (println message) )
```

Because the anonymous function has no name, it cannot be referred to later. Rather, the anonymous function is typically created at the point it is passed to another function.

Or it’s possible to immediately invoke it (this is not a common usage):

```
;;     operation (function)             argument
;; --------------------------------  --------------
(  (fn [message] (println message))  "Hello world!" )

;; Hello world!
```

Here we defined the anonymous function in the function position of a larger expression that immediately invokes the expression with the argument.

Many languages have both statements, which imperatively do something and do not return a value, and expressions which do. Clojure has **only** expressions that return a value. We’ll see later that this includes even flow control expressions like `if`.

### `defn` vs `fn`

It might be useful to think of `defn` as a contraction of `def` and `fn`. The `fn` defines the function and the `def` binds it to a name. These are equivalent:

```
(defn greet [name] (str "Hello, " name))

(def greet (fn [name] (str "Hello, " name)))
```

### Anonymous function syntax

There is a shorter form for the `fn` anonymous function syntax implemented in the Clojure reader: `#()`. This syntax omits the parameter list and names parameters based on their position.

- `%` is used for a single parameter
- `%1`, `%2`, `%3`, etc are used for multiple parameters
- `%&` is used for any remaining (variadic) parameters

Nested anonymous functions would create an ambiguity as the parameters are not named, so nesting is not allowed.

```clj
;; Equivalent to: (fn [x] (+ 6 x))
#(+ 6 %)

;; Equivalent to: (fn [x y] (+ x y))
#(+ %1 %2)

;; Equivalent to: (fn [x y & zs] (println x y zs))
#(println %1 %2 %&)
```

### Gotcha

One common need is an anonymous function that takes an element and wraps it in a vector. You might try writing that as:

```
;; DO NOT DO THIS
#([%])
```

This anonymous function expands to the equivalent:

```
(fn [x] ([x]))
```

This form will wrap in a vector **and** try to invoke the vector with no arguments (the extra pair of parentheses). Instead:

```
;; Instead do this:
#(vector %)

;; or this:
(fn [x] [x])

;; or most simply just the vector function itself:
vector
```

## Applying Functions

### `apply`

The `apply` function invokes a function with 0 or more fixed arguments, and draws the rest of the needed arguments from a final sequence. The final argument **must** be a sequence.

```clj
(apply f '(1 2 3 4))    ;; same as  (f 1 2 3 4)
(apply f 1 '(2 3 4))    ;; same as  (f 1 2 3 4)
(apply f 1 2 '(3 4))    ;; same as  (f 1 2 3 4)
(apply f 1 2 3 '(4))    ;; same as  (f 1 2 3 4)
```

All 4 of these calls are equivalent to `(f 1 2 3 4)`. `apply` is useful when arguments are handed to you as a sequence but you must invoke the function with the values in the sequence.

For example, you can use `apply` to avoid writing this:

```
(defn plot [shape coords]   ;; coords is [x y]
  (plotxy shape (first coords) (second coords)))
```

Instead you can simply write:

```
(defn plot [shape coords]
  (apply plotxy shape coords))
```

## Locals and Closures

### `let`

`let` binds symbols to values in a "lexical scope". A lexical scope creates a new context for names, nested inside the surrounding context. Names defined in a `let` take precedence over the names in the outer context.

```
;;      bindings     name is defined here
;;    ------------  ----------------------
(let  [name value]  (code that uses name))
```

Each `let` can define 0 or more bindings and can have 0 or more expressions in the body.

```
(let [x 1
      y 2]
  (+ x y))
```

This `let` expression creates two local bindings for `x` and `y`. The expression `(+ x y)` is in the lexical scope of the `let` and resolves x to 1 and y to 2. Outside the `let` expression, x and y will have no continued meaning, unless they were already bound to a value.

```
(defn messenger [msg]
  (let [a 7
        b 5
        c (clojure.string/capitalize msg)]
    (println a b c)
  ) ;; end of let scope
) ;; end of function
```

The messenger function takes a `msg` argument. Here the `defn` is also creating lexical scope for `msg` - it only has meaning within the `messenger` function.

Within that function scope, the `let` creates a new scope to define `a`, `b`, and `c`. If we tried to use `a` after the let expression, the compiler would report an error.

### Closures

The `fn` special form creates a "closure". It "closes over" the surrounding lexical scope (like `msg`, `a`, `b`, or `c` above) and captures their values beyond the lexical scope.

```
(defn messenger-builder [greeting]
  (fn [who] (println greeting who))) ; closes over greeting

;; greeting provided here, then goes out of scope
(def hello-er (messenger-builder "Hello"))

;; greeting value still available because hello-er is a closure
(hello-er "world!")
;; Hello world!
```

## Java Interop

### Invoking Java code

Below is a summary of calling conventions for calling into Java from Clojure:

| Task            | Java                | Clojure            |     |
| :-------------- | :------------------ | :----------------- | :-- |
| Instantiation   | `new Widget("foo")` | `(Widget. "foo")`  |     |
| Instance method | `rnd.nextInt()`     | `(.nextInt rnd)`   |     |
| Instance field  | `object.field`      | `(.-field object)` |     |
| Static method   | `Math.sqrt(25)`     | `(Math/sqrt 25)`   |     |
| Static field    | `Math.PI`           | `Math/PI`          |     |

### Java Methods vs Functions

- Java methods are not Clojure functions
- Can’t store them or pass them as arguments
- Can wrap them in functions when necessary

```
;; make a function to invoke .length on arg
(fn [obj] (.length obj))

;; same thing
#(.length %)
```

## Test your knowledge

_[Check your answers](https://clojure.org/guides/learn/answers#_functions)_

1. Define a function `greet` that takes no arguments and prints "Hello". Replace the `___` with the implementation: `(defn greet [] *_*)`
2. Redefine `greet` using `def`, first with the `fn` special form and then with the `#()` reader macro.

```
;; using fn
(def greet __)

;; using #()
(def greet __)
```

3. Define a function `greeting` which:

- Given no arguments, returns "Hello, World!"
- Given one argument x, returns "Hello, **x**!"
- Given two arguments x and y, returns "**x**, **y**!"

```
;; Hint use the str function to concatenate strings
(doc str)

(defn greeting ___)

;; For testing
(assert (= "Hello, World!" (greeting)))
(assert (= "Hello, Clojure!" (greeting "Clojure")))
(assert (= "Good morning, Clojure!" (greeting "Good morning" "Clojure")))
```

4. Define a function `do-nothing` which takes a single argument `x` and returns it, unchanged.

```
(defn do-nothing [x] ___)
```

In Clojure, this is the `identity` function. By itself, identity is not very useful, but it is sometimes necessary when working with higher-order functions.

```
(source identity)
```

5. Define a function `always-thing` which takes any number of arguments, ignores all of them, and returns the number `100`.

```
(defn always-thing [__] ___)
```

6. Define a function `make-thingy` which takes a single argument `x`. It should return another function, which takes any number of arguments and always returns x.

```
(defn make-thingy [x] ___)

;; Tests
(let [n (rand-int Integer/MAX_VALUE)
      f (make-thingy n)]
  (assert (= n (f)))
  (assert (= n (f 123)))
  (assert (= n (apply f 123 (range)))))
```

In Clojure, this is the `constantly` function.

```
(source constantly)
```

7. Define a function `triplicate` which takes another function and calls it three times, without any arguments.

```clj
(defn triplicate [f] ___)
```

8. Define a function `opposite` which takes a single argument `f`. It should return another function which takes any number of arguments, applies `f` on them, and then calls `not` on the result. The `not` function in Clojure does logical negation.

```
(defn opposite [f]
  (fn [& args] ___))
```

In Clojure, this is the complement function.

```clj
(defn complement
  "Takes a fn f and returns a fn that takes the same arguments as f,
  has the same effects, if any, and returns the opposite truth value."
  [f]
  (fn
    ([] (not (f)))
    ([x] (not (f x)))
    ([x y] (not (f x y)))
    ([x y & zs] (not (apply f x y zs)))))
```

9. Define a function `triplicate2` which takes another function and any number of arguments, then calls that function three times on those arguments. Re-use the function you defined in the earlier triplicate exercise.

```
(defn triplicate2 [f & args]
  (triplicate ___))
```

10. Using the [java.lang.Math](http://docs.oracle.com/javase/8/docs/api/java/lang/Math.html) class (`Math/pow`, `Math/cos`, `Math/sin`, `Math/PI`), demonstrate the following mathematical facts:

- The cosine of pi is -1
- For some x, sin(x)^2 + cos(x)^2 = 1
- Define a function that takes an HTTP URL as a string, fetches that URL from the web, and returns the content as a string.

Hint: Using the [java.net.URL](http://docs.oracle.com/javase/8/docs/api/java/net/URL.html) class and its `openStream` method. Then use the Clojure `slurp` function to get the content as a string.

```
(defn http-get [url]
  ___)

(assert (.contains (http-get "https://www.w3.org") "html"))
```

In fact, the Clojure `slurp` function interprets its argument as a URL first before trying it as a file name. Write a simplified http-get:

```
(defn http-get [url]
  ___)
```

12. Define a function `one-less-arg` that takes two arguments:

- `f`, a function
- `x`, a value

and returns another function which calls `f` on `x` plus any additional arguments.

```clj
(defn one-less-arg [f x]
  (fn [& args] ___))
```

In Clojure, the `partial` function is a more general version of this.

13. Define a function `two-fns` which takes two functions as arguments, `f` and `g`. It returns another function which takes one argument, calls `g` on it, then calls `f` on the result, and returns that.

That is, your function returns the composition of `f` and `g`.

```
(defn two-fns [f g]
  ___)
```
