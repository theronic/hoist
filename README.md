*(draft, in progress)* This is a placeholder repository for when things are sufficiently stable.

# Hoist

Hoist is a visual programming environment that attempts to solve one of the two biggest problems in computer science: naming. With a strong concept of structural editing and symbolic accounting, Hoist takes cues from Clojure, Excel, control system theory, Javelin and reversible datalog.

The term 'hoist' comes from growing a program by pulling (or hoisting) a hard-coded value 'up' and out of a chunk of code and replacing it with an abstract symbol that makes the code reusable and parameterisable. Like in Excel, you don't need to name your variables in Hoist, because all values are implicitly named. However, whereas Excel has a Cartesian namespace, the Hoist namespace is a graph laid out on a two-dimensional canvas.

A Hoist program consists of a closed system of data and reversible functions, or *transverters*. You grow your program by modifying the links between values and transformations in a way that support rapid ideation. Program changed? No problem, Hoist balances the books by reconciling differences between then and and now.

The most valuable question Hoist can answer about your program is, "how did this data come to be?".

## Transverters

 - *transvert* = trans "to move across" + vertere "to turn", meaning to traverse in both directions. Like reversible transducers. From Latin trānsversus.
 - From electrical engineering, a **transverter** is a universal power converter that can combine, convert, analyze and control any combinations of DC or AC power.
  - From radio engineering, a **transverter** is an up & downconverter in one unit. Used in conjunction with transceivers to change the range of frequencies over which the transceiver can communicate.

## How to Grow a Program

Let's say you have two numbers: 5 and 6. Hoist doesn't complain; it likes numbers and will remember them for you. Now you can connect these two numbers in the most general way, and Hoist is pleased with that. Hoist will merrily recommend some common functions that can deal with two or more numbers.

Would you like to add your numbers together? Sure, let's throw a `+` function into the mix.  In this way, using Hoist feels like bottom-up programming. Now we have two numbers connected by a `+` function. Hoist is fine with that too, and it will show you that `+` can ingest those numbers. So you feed the numbers into the `+` transformation, and Hoist emits the sum: `11`, which can be fed into subsequent transformations. Alternatively, you can enter the same program as `(+ 5 6) => 11` - the internal representation is the same: a directed graph of nodes and edges where nodes can traverse edges and in so doing, be transformed.

This way of working is not fundamentally new. Reactive cell-based programming environments like spreadsheets or Javelin have similar goals. But Hoist has some other tricks up its sleeve that you can't do with spreadsheets.

What do we mean by symbolic accounting? The pain of growing incomplete programs is about managing past traversals and replaying them without breaking your program. If you've felt this pain before, you may be thinking: event sourcing. But named events suffer from the same inverted labelling problem. If I knew all my business events ahead of time, I would have written that program instead. And that's not how real-world programs grow.

If you regard your program as a database with schema, Hoist deals with The Migration Problem by modelling the environment as a closed system that can replay transformations and reconcile differences. All data that enters the system must be drained from the system in an orderly fashion. It can bounce around for a while between transformations, but it has to leave order-and-file (read: queues). Some transformations produce new data (exothermic, fractionate), and some consume data (endothermic, or reducing). The key thing in accounting is that entries must balance.

### Symbolic Accounting

Let's say our first customer, Rich, signed up with their email address, but we forgot to take down their name. So we amend the database schema with a `:contact/full-name` column (or attribute). Problem is, since they signed up, we wrote an email template that depends on that column, and now we have customer records missing names. Normally, this would produce a bug, or an awkward email. Hoist offers a solution: the email template is a transform that demands customer records with a full name. Rich's customer record will bypass the traversal and end up in a pile of "missing name records". Essentially, all traversals must be balanced.

Here's how data remains "balanced": the Hoist environment is a closed system. This means that any data (or energy) you inject into the system must accumulate somewhere in a sink. Hoist keeps a tight lock on the exits.

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
