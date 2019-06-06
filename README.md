*(draft, in progress)* This is a placeholder repository for when things are sufficiently stable.

# Hoist

Hoist is a visual programming language with a strong concept of structural editing and symbolic accounting. Hoist takes cues from Clojure, Excel, control system theory, Javelin and reversible datalog. The term 'hoist' comes from growing a program by pulling (or hoisting) a hard-coded value 'up' and out of a chunk of code and replacing it with an abstract symbol that makes the code reusable and parameterisable. In Hoist, like in Excel, you don't need to name your variables, because all values are implicitly named.

Your program is a closed system of data and reversible functions, or *transverters*. Hoist supports growing your program by modifying the linkages between data and transformations in ways that support rapid ideation. Program changed? No problem, Hoist balances the books by reconciling differences between then and and now.

Probably the most valuable question Hoist can answer about your program is, "where did this data come from?".

## Transverters

 - *transvert* = trans "to move across" + vertere "to turn", meaning to traverse in both directions. Like reversible transducers. From Latin trānsversus.
 - From electrical engineering, a **transverter** is a universal power converter that can combine, convert, analyze and control any combinations of DC or AC power.
  - From radio engineering, a **transverter** is an up & downconverter in one unit. Used in conjunction with transceivers to change the range of frequencies over which the transceiver can communicate.

## How to Grow a Program

For example, let's say you give Hoist two numbers: 5 & 6. Hoist doesn't complain; it likes numbers and will remember them for you. Now you connect these two numbers in the most general way - Hoist is pleased with that. Other compilers might shout at you, but not Hoist. Hoist will merrily recommend some common functions that can deal with numbers.

Would you like to add your numbers together? Sure, let's throw a `+` function into the mix.  In this way, using Hoist feels like bottom-up programming. Now there are three things: two numbers and a `+` function. Hoist is fine with that too, and it will show you that `+` can ingest those numbers. So you feed the numbers into the `+` transformations, and naturally Hoist computes the output: `11`, which you can now feed into the next transformation. Or, you could just enter this program straight away: `(+ 5 6) => 11`. The internal representation is the same, though: a directed graph of nodes and edges where nodes can traverse along edges.

This way of working is not fundamentally new. Reactive cell-based programming environments like spreadsheets or Javelin have similar goals. But Hoist has some other tricks up its sleeve that you can't do with spreadsheets.

What do we mean by symbolic accounting? The pain of growing incomplete programs is about managing past traversals and replaying them without breaking your program. If you've felt this pain before, you may be thinking: event sourcing. But named events suffer from the same inverted labelling problem. If I knew all my business events ahead of time, I would have written that program instead. But that's not how real programs grow.

If you regard your program as a database with schema, Hoist deals with this "migration" problem by modelling the environment as a closed system that can replay transformations and reconcile differences. All data that enters the system must be drained from the system in an orderly fashion. It can bounce around for a while between transformations, but it has to leave order-and-file (read: queues). Some transformations produce new data (exothermic, fractionate), and some consume data (endothermic, or reducer). The key thing in accounting is that entries must balance.

### Symbolic Accounting

Let's say our first customer, Rich, signed up with their email address, but we forgot to take down their name. So we amend the database schema with a `:contact/full-name` column (or attribute). Problem is, since they signed up, we wrote an email template that depends on that column, and now we have customer records missing names. Normally, this would produce a bug, or an awkward email. Hoist offers a solution: the email template is a transform that demands customer records with a full name. Rich's customer record will bypass the traversal and end up in a pile of "missing name records". Essentially, all traversals must be balanced.

Here's how it works: Hoist is a closed system, like a reverb chamber. Any data (or energy) you inject into the system, has to leave the system. And Hoist keeps a very tight lock on the exits. If data left a system.

(expound on splicing / structural editing here)

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

Excel is arguably the world's most popular functional programming language. It's based on the lambda calculus. Every cell is derived from the functional transformation of one or more other cells.

The thing that made Excel so popular as an exploratory modelling environment (esp. for financial models) is that it does not suffer from the [inverted UI](petrustheron.com/posts/inverter-ui.html) problem that plagues all other programming environments. In Excel, you don't have to label your variables. Values are implicitly labelled `A1`, `B2`, `C3`, and so on. There is also the two-dimensional visual layout that lends itself well to working with tabular data. Growing a program by connecting the edges of implicitly named nodes is a more natural way to construct programs from the ground up. And it's the reason REPLs exists. But the REPL has a flaw: you can't connect unnamed data.

Some interactive notebooks number outputs to take a jab at this, but it feels unnatural. Hoist aims to visualize data composition.

Historically, visual programming languages have failed.

## Operations in Hoist

### Split vs. Splice or Fuse vs. Merge or Map/Map2

`(+ 5 6)` => `(+ 5)`, `[6]`.

The internal representation of this is `[5 6] -> +`, or `5 -> +` and `6 -> +`, where `->` means feed into.

### Hoist / Symbolise (Replace? Abstract?)

`(+ 5 6)` => (+ a 6), where `a` is an input.

### Fuse/Merge Link

Refer to map/map2.
