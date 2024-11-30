# let 解构

解构是用来方便赋值的，否则获取值的时候就要使用大量的 next ,first 之类的 seq 操作，不方便，尤其是嵌套的 collection，取值就更麻烦。有了解构以后，把 collection 的值取出来就非常的简单和方便，除了可以用于 let 以外，还可以用于函数参数的赋值等地方。Clojure 的解构特性提供了一种简洁的语法来声明式地从一个集合里面选取某些元素，并且把这些元素绑定到一个本地 let 绑定上去。并且因为解构这个特性是由 let 提供的， 它可以在任何间接使用了 let 的地方使用，比如 fn、defn、loop。

# 序列解构

```clj
;; let is a Clojure special form, a fundamental building block of the language.
;;
;; In addition to parameters passed to functions, let provides a way to create
;; lexical bindings of data structures to symbols. The binding, and therefore
;; the ability to resolve the binding, is available only within the lexical
;; context of the let.
;;
;; let uses pairs in a vector for each binding you'd like to make and the value
;; of the let is the value of the last expression to be evaluated. let also
;; allows for destructuring which is a way to bind symbols to only part of a
;; collection.

;; A basic use for a let:
user=> (let [x 1]
         x)
1

;; Note that the binding for the symbol y won't exist outside of the let:
user=> (let [y 1]
         y)
1
user=> (prn y)
java.lang.Exception: Unable to resolve symbol: y in this context (NO_SOURCE_FILE:7)

;; Note that if you use def inside a let block, your interned variable is within
;; the current namespace and will appear OUTSIDE of the let block.
user=> (let [y 1]
         (def z y)
         y)
1
user=> z
1

;; Another valid use of let:
user=> (let [a 1 b 2]
         (+ a b))
3

;; The forms in the vector can be more complex (this example also uses
;; the thread macro):
user=> (let [c (+ 1 2)
             [d e] [5 6]]
         (-> (+ d e) (- c)))
8

;; The bindings for let need not match up (note the result is a numeric
;; type called a ratio):
user=> (let [[g h] [1 2 3]]
         (/ g h))
1/2

;; From http://clojure-examples.appspot.com/clojure.core/let with permission.
```

## 使用 `_` 占位

如果有的值不需要，但是又需要展位，clojure 中有一个**惯用法** `_` 下划线，来占用，但是不使用他的值。用下划线的方式来忽略掉一个或多个自己不在意的值。

```clojure
(let [[a [_ [c]]] [1 [2 [3]]]]
    (println a ", " c))
;=> 1, 2, 3
```

可以看到，只需要我们的取值的解构和原始数据结构一致，就可以很方便的把值一次取到，方便使用。

## & 获取剩余值

& 符号会把剩下的值以 list 的形式全部放入 b 中：

```clj
(let [[a & b] [1 2 3]]
    (println a ", " b))
;=> 1, (2 3)
```

## :as 获取整个集合

:as 可以在解构形式中使用:as 来获得对于被解构的集合的引用

```clojure
(let [[a & b :as c] [1 2 3]]
    (println (a ", " b ", " c)))
;=> 1, (2 3), [1 2 3]
```

# Map 解构

```clj
(let [{a :a b :b c :c} {:a 1 :b 2 :c 3}]
  (println a ", " b ", " c))
;=> 1, 2, 3

;;; map destructuring, all features
user=>
(let [
      ;;Binding Map
      {:keys [k1 k2]        ;; bind vals with keyword keys
       :strs [s1 s2]        ;; bind vals with string keys
       :syms [sym1 sym2]    ;; bind vals with symbol keys
       :or {k2 :default-kw, ;; default values
            s2 :default-s,
            sym2 :default-sym}
       :as m}  ;; bind the entire map to `m`
      ;;Data
      {:k1 :keyword1, :k2 :keyword2,  ;; keyword keys
       "s1" :string1, "s2" :string2,  ;; string keys
       'sym1 :symbol1,                ;; symbol keys
       ;; 'sym2 :symbol2              ;; `sym2` will get default value
       }]
  [k1 k2 s1 s2 sym1 sym2 m])  ;; return value

[:keyword1, :keyword2,
 :string1, :string2,
 :symbol1, :default-sym, ;; key didn't exist, so got the default
 {'sym1 :symbol1, :k1 :keyword1, :k2 :keyword2,
  "s1" :string1, "s2" :string2}]

;; remember that vector and map destructuring can also be used with
;; other macros that bind variables, e.g. `for` and `doseq`
```
