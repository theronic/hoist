*(draft, in progress)* This is a placeholder repository for when things are sufficiently stable.

# Hoist

Hoist is a visual programming language with a strong concept of symbolic origin. Hoist is inspired by Excel and reversible datalog. The term 'hoist' comes from pulling hard-coded values 'up' out of a chunk of code and replacing them with abstract symbols that make the code reusable, parameterisable.

Something as simple as this:

```clojure
(carmine/wcar {:spec {:url "redis://127.0.0.1"}}
  (carmine/get "my_key"))
```


```clojure
(let [env-map   (->> (slurp "config.edn") (clojure.edn/read-string))
      redis-url (get env-map :redis-url)] ;; the Redis URL spec has been hoisted up to be parameterisable
  (carmine/wcar {:spec {:url redis-url}}  ;; here we use the symbol, redis-url
    (carmine/get "my_key")))
```

This seems to be a common way to grow programs. Hoist aims to make this way of growing programs a natural technique.

## Overview

Excel is arguably the world's most popular functional programming languages. It's based on the lambda calculus, where every cell is derived from the functional transformation of one or more other cells.

The thing that made Excel so popular as an exploratory modelling environment (esp. for financial models) is that it doesn't suffer from the [inverted UI](petrustheron.com/posts/inverter-ui.html) problem that plagues all other programming environments. In Excel, you don't have to label your variables before you know what they mean. Values are implicitly labelled as `A1`, `B2`, `C3`, and so on, and can be referenced when needed. This allows you to assemble a mental model piece-by-piece  up a model from scratch without being impeded by naming things you haven't decided yet.

Aside from the convenient vector compaction provided by a two-dimensional layout (fill column A with timestamps, column B with currencies).

Since cycles are disallowed.

## Fundamental Operations

## Symbolise (Replace? Abstract?)

Turn a value into a symbol you can inject.

### Fuse & Merge

Refer to map/map2.
