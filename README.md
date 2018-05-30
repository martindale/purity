# Purity — purely functional smart contracts
Purity is an approachable smart contracting language for distributed systems
that compiles down to [Simplicity][simplicity-paper], an underlying "assembly
language" that the Fabric Virtual Machine executes over what it calls "Fibers".

## Basic Syntax
### Function definition:
```
:method
  :stack[0]
  :stack[1]
  :stack[2]
```

Functions are executed bottom to top, and receive the final stack output from
their children as the input into their execution.

### Function execution:
Functions may add to the stack by supplying their input inline:
```
:method :input
```

### Annotation
```
:method#:name(:assert=":challenge", :also=":foo")
```

### Examples
#### List of Items
```jade
contract#list
  item One...
  item Two...
  item Three!
```

#### Simple Adder
This simple program adds two `integer2` types using the `sum` primitive:
```jade
contract#add
  verify(type="integer2") 2
  compute#output
    sum
      integer2 1
      integer2 1
```

As parsed into a Fiber script:
```bash
  1 1 sum compute verify
```

## Fibers
Fibers are ephemeral data structures which have deterministic execution and
forward-only progress.  Purity targets this execution environment by requiring
all contracts take exactly one input and one output.  Simple processes can thus
be easily modeled:

```jade
contract
  take 0
```

This contract takes the top element on the stack and pushes it onto the display.

More complex programs are also easily modeled:

```jade
contract#simple-application
  interface semantic
    header Hello, world!
    subtitle This is a simple interface.
```

Here, we've created the `simple-application` contract, indicated that we'd like
to use the `semantic` interface library (Fabric's default UI), and rendered the
application to the screen by supplying the output of the `header` and `subtitle`
functions.

## Interfaces
Purity also places a focus on delivering compelling user experiences to programs
built with it, making interface components a first-class citizen.

Interfaces may be defined for any program by specifying the `interface` keyword,
which initializes to a pre-defined state, and indicating the name (or hash) of
the desired interface as the parameter.

## Types
Types may be proposed during run-time, which create "branches" in the running
Fiber.  Only one branch may "win", as resolution requires a commitment which
satisfies the original proposal's destruction proof.

## Scopes
Scopes require an interface to have a defined `#:name`, which enables
Fabric-based messaging between local interface components.  Messages are sent
over routable channels using the `:name/:route` syntax, and will only be
received by their parent components.

## Quick Start
**TODO:**
- [ ] write introduction
- [ ] formalize specification
- [ ] publish AST

[simplicity-paper]: https://blockstream.com/simplicity.pdf
