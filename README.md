*(draft, in progress)* This is a placeholder repository for when things are sufficiently stable.

# Hoist

Hoist is a visual programming language with a strong concept of structural editing and symbolic origin. Hoist is inspired by Clojure, Excel, Javelin and reversible datalog. The term 'hoist' comes from growing a program by pulling (or hoisting) a hard-coded value 'up' and out of a chunk of code and replacing it with an abstract symbol that makes the code reusable and parameterisable. In Hoist, like in Excel, you don't name your variables, because all values are implicitly named.

Your program = data + transformations.

Hoist supports growing your program by modifying the linkages between data and transformations in ways that support rapid ideation.

For example, in Hoist you write down two numbers. Hoist doesn't complain; it likes numbers. Then you connect the two numbers - Hoist is pleased with that. The compiler won't shout at you. It even recommends some common functions that can deal with numbers. In this way, it feels like bottom-up programming. Would you like to add your new numbers together? Sure, let's throw a `+` function into the mix. Now there are three things (two numbers, one function). Hoist is fine with that too, and it will show you that `+` can ingest those numbers. So you feed the numbers into the `+` transformations, so Hoist can compute the output. Out pops a new number: `11`, which you can feed into the next transformation.

This way of working is not fundamentally new - reactive cell-based programming environments like Javelin have similar goals, and spreadsheets are built on it. But Hoist has some other tricks up its sleeve.

(expound on splicing / structural editing here)

Don't worry, you also select your two numbers and type `+` and Hoist will do the right thing.

What about adding more numbers? That's fine too, because in Hoist everything is a reducible collection.

All values are implicitly parameterisable.

You can loosely link a value to a transformation, and Hoist will let you do that.

Let's look at some code that fetches a keyed value from Redis server URL:

```clojure
(carmine/wcar {:spec {:url "redis://127.0.0.1"}}
  (carmine/get "my_key"))
```

Note how the server URL and the key are hardcoded.

Let's hoist up the hard-coded Redis URL:

```clojure
(let [env-map   (->> (slurp "config.edn") (clojure.edn/read-string))
      redis-url (get env-map :redis-url)] ;; the Redis URL spec has been hoisted up to be parameterisable
  (carmine/wcar {:spec {:url redis-url}}  ;; here we use the symbol, redis-url
    (carmine/get "my_key")))
```

This seems to be a common way to grow programs. Hoist aims to make this way of growing your program a natural experience.

## Background

Excel is arguably the world's most popular functional programming language. It's based on the lambda calculus, where every cell is derived from the functional transformation of one or more other cells.

The thing that made Excel so popular as an exploratory modelling environment (esp. for financial models) is that it does not suffer from the [inverted UI](petrustheron.com/posts/inverter-ui.html) problem that plagues all other programming environments. In Excel, you don't have to label your variables. Values are implicitly labelled `A1`, `B2`, `C3`, and so on. There is also the two-dimensional visual layout that lends itself well to working with tabular data. Growing a program by connecting the edges of implicitly named nodes is a more natural way to construct programs from the ground up. And it's the reason REPLs exists. But the REPL has a flaw: you can't connect values without naming them explicitly.

Some interactive notebooks number outputs to take a jab at this, but it's by no means user-friendly.

This allows you to assemble a mental model piece-by-piece  up a model from scratch without being impeded by naming things you haven't decided yet.


Aside from the convenient vector compaction provided by a two-dimensional layout (fill column A with timestamps, column B with currencies).

Since cycles are disallowed.

## Fundamental Operations

## Symbolise (Replace? Abstract?)

Turn a value into a symbol you can inject.

### Fuse & Merge

Refer to map/map2.
