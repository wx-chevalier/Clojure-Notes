## Statements vs. Expressions

In Java, expressions return values, whereas statements do not.

```clj
// "if" is a statement because it doesn't return a value:
String s;
if (x > 10) {
    s = "greater";
} else {
    s = "less or equal";
}
obj.someMethod(s);

// Ternary operator is an expression; it returns a value:
obj.someMethod(x > 10 ? "greater" : "less or equal");
```

In Clojure, however, everything is an expression! _Everything_ returns a value, and a block of multiple expressions returns the last value. Expressions that exclusively perform side-effects return `nil`.

## Flow Control Expressions

Accordingly, flow control operators are expressions, too!

Flow control operators are composable, so we can use them anywhere. This leads to less duplicate code, as well as fewer intermediate variables.

Flow control operators are also extensible via macros, which allow the compiler to be extended by user code. We won’t be discussing macros today, but you can read more about them at [Macros](https://clojure.org/reference/macros), [Clojure from the Ground Up](https://aphyr.com/posts/305-clojure-from-the-ground-up-macros), or [Clojure for the Brave and True](http://www.braveclojure.com/writing-macros/), among many other places.

### `if`

`if` is the most important conditional expression - it consists of a condition, a "then", and an "else". `if` will only evaluate the branch selected by the conditional.

```
user=> (str "2 is " (if (even? 2) "even" "odd"))
2 is even

user=> (if (true? false) "impossible!") ;; else is optional
nil
```

### Truth

In Clojure, all values are logically true or false. The only "false" values are `false` and `nil` - all other values are logically true.

```
user=> (if true :truthy :falsey)
:truthy
user=> (if (Object.) :truthy :falsey) ; objects are true
:truthy
user=> (if [] :truthy :falsey) ; empty collections are true
:truthy
user=> (if 0 :truthy :falsey) ; zero is true
:truthy
user=> (if false :truthy :falsey)
:falsey
user=> (if nil :truthy :falsey)
:falsey
```

### `if` and `do`

The `if` only takes a single expression for the "then" and "else". Use `do` to create larger blocks that are a single expression.

Note that the only reason to do this is if your bodies have side effects! (Why?)

```
(if (even? 5)
  (do (println "even")
      true)
  (do (println "odd")
      false))
```

### `when`

`when` is an `if` with only a `then` branch. It checks a condition and then evaluates any number of statements as a body (so no `do` is required). The value of the last expression is returned. If the condition is false, nil is returned.

`when` communicates to a reader that there is no "else" branch.

```
(when (neg? x)
  (throw (RuntimeException. (str "x must be positive: " x))))
```

### `cond`

`cond` is a series of tests and expressions. Each test is evaluated in order and the expression is evaluated and returned for the first true test.

```
(let [x 5]
  (cond
    (< x 2) "x is less than 2"
    (< x 10) "x is less than 10"))
```

### `cond` and `else`

If no test is satisfied, nil is returned. A common idiom is to use a final test of `:else`. Keywords (like `:else`) always evaluate to true so this will always be selected as a default.

```
(let [x 11]
  (cond
    (< x 2)  "x is less than 2"
    (< x 10) "x is less than 10"
    :else  "x is greater than or equal to 10"))
```

### `case`

`case` compares an argument to a series of values to find a match. This is done in constant (not linear) time! However, each value must be a compile-time literal (numbers, strings, keywords, etc).

Unlike `cond`, `case` will throw an exception if no value matches.

```clj
user=> (defn foo [x]
         (case x
           5 "x is 5"
           10 "x is 10"))
#'user/foo

user=> (foo 10)
x is 10

user=> (foo 11)
IllegalArgumentException No matching clause: 11
```

### `case` with `else`-expression

`case` can have a final trailing expression that will be evaluated if no test matches.

```
user=> (defn foo [x]
         (case x
           5 "x is 5"
           10 "x is 10"
           "x isn't 5 or 10"))
#'user/foo

user=> (foo 11)
x isn't 5 or 10
```

## Iteration for Side Effects

### `dotimes`

- Evaluate expression _n_ times
- Returns `nil`

```
user=> (dotimes [i 3]
         (println i))
0
1
2
nil
```

### `doseq`

- Iterates over a sequence
- If a lazy sequence, forces evaluation
- Returns `nil`

```
user=> (doseq [n (range 3)]
         (println n))
0
1
2
nil
```

### `doseq` with multiple bindings

- Similar to nested `foreach` loops
- Processes all permutations of sequence content
- Returns `nil`

```
user=> (doseq [letter [:a :b]
               number (range 3)] ; list of 0, 1, 2
         (prn [letter number]))
[:a 0]
[:a 1]
[:a 2]
[:b 0]
[:b 1]
[:b 2]
nil
```

## Clojure’s `for`

- List comprehension, **not** a for-loop
- Generator function for sequence permutation
- Bindings behave like `doseq`

```
user=> (for [letter [:a :b]
             number (range 3)] ; list of 0, 1, 2
         [letter number])
([:a 0] [:a 1] [:a 2] [:b 0] [:b 1] [:b 2])
```

## Recursion

### Recursion and Iteration

- Clojure provides recur and the sequence abstraction
- `recur` is "classic" recursion
  - Consumers don’t control it, considered a lower-level facility
- Sequences represent iteration as values
  - Consumers can partially iterate
- Reducers represent iteration as function composition
  - Added in Clojure 1.5, not covered here

### `loop` and `recur`

- Functional looping construct
  - `loop` defines bindings
  - `recur` re-executes `loop` with new bindings
- Prefer higher-order library functions instead

```
(loop [i 0]
  (if (< i 10)
    (recur (inc i))
    i))
```

### `defn` and `recur`

- Function arguments are implicit `loop` bindings

```
(defn increase [i]
  (if (< i 10)
    (recur (inc i))
    i))
```

### `recur` for recursion

- `recur` must be in "tail position"
  - The last expression in a branch
- `recur` must provide values for all bound symbols by position
  - Loop bindings
  - defn/fn arguments
- Recursion via `recur` does not consume stack

## Exceptions

### Exception handling

- `try`/`catch`/`finally` as in Java

```
(try
  (/ 2 1)
  (catch ArithmeticException e
    "divide by zero")
  (finally
    (println "cleanup")))
```

### Throwing exceptions

```
(try
  (throw (Exception. "something went wrong"))
  (catch Exception e (.getMessage e)))
```

### Exceptions with Clojure data

- `ex-info` takes a message and a map
- `ex-data` gets the map back out
  - Or `nil` if not created with `ex-info`

```clj
(try
  (throw (ex-info "There was a problem" {:detail 42}))
  (catch Exception e
    (prn (:detail (ex-data e)))))
```

### `with-open`

```clj
(let [f (clojure.java.io/writer "/tmp/new")]
  (try
    (.write f "some text")
    (finally
      (.close f))))

;; Can be written:
(with-open [f (clojure.java.io/writer "/tmp/new")]
  (.write f "some text"))
```
