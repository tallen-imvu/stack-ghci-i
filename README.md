# A repo with demonstrations of [stack bug #2895](https://github.com/commercialhaskell/stack/issues/2895)

This stack project contians 3 cabal files.

### working/
Demonstration of `stack ghci` working in the expected manner

### buggy/
Demonstration of the problem

### alib/
Showing that the problem happens for libraries too

## How to reproduce:

`stack build`
works as expected

`stack ghci working/Main.hs`
Loads Main as expected

`stack ghci buggy/Main.hs`
Fails to load interface for 'Bar'

`stack ghci alib/Lib.hs`
Fails to load interface for 'Bar'

### Example output:

```
$ stack build
alib-0.1.0.0: unregistering (local file changes: xx/stack-ghci-i/extra/Bar.hs Lib.hs alib.cabal)
buggy-0.1.0.0: configure (exe)
buggy-0.1.0.0: build (exe)
working-0.1.0.0: configure (exe)
working-0.1.0.0: build (exe)
alib-0.1.0.0: configure (lib)
buggy-0.1.0.0: copy/register
alib-0.1.0.0: build (lib)
working-0.1.0.0: copy/register
alib-0.1.0.0: copy/register
Completed 3 action(s).
Log files have been written to: xx/stack-ghci-i/.stack-work/logs/
$ stack ghci working/Main.hs
Using configuration for working:exe:working to load xx/stack-ghci-i/working/Main.hs
The following GHC options are incompatible with GHCi and have not been passed to it: -threaded
Configuring GHCi with the following packages: working
Using main module: 1. Package `working' component exe:working with main-is file: xx/stack-ghci-i/working/Main.hs
GHCi, version 8.0.1: http://www.haskell.org/ghc/  :? for help
[1 of 2] Compiling Bar              ( xx/stack-ghci-i/working/extra/Bar.hs, interpreted )
[2 of 2] Compiling Main             ( xx/stack-ghci-i/working/Main.hs, interpreted )
Ok, modules loaded: Bar, Main.
Loaded GHCi configuration from /tmp/ghci12094/ghci-script
*Main>
Leaving GHCi.
$ stack ghci buggy/Main.hs
Using configuration for buggy:exe:buggy to load xx/stack-ghci-i/buggy/Main.hs
The following GHC options are incompatible with GHCi and have not been passed to it: -threaded
Configuring GHCi with the following packages: buggy
Using main module: 1. Package `buggy' component exe:buggy with main-is file: xx/stack-ghci-i/buggy/Main.hs
GHCi, version 8.0.1: http://www.haskell.org/ghc/  :? for help
[1 of 1] Compiling Main             ( xx/stack-ghci-i/buggy/Main.hs, interpreted )

xx/stack-ghci-i/buggy/Main.hs:4:1: error:
    Failed to load interface for ‘Bar’
    It is a member of the hidden package ‘alib-0.1.0.0’.
    Use -v to see a list of the files searched for.
Failed, modules loaded: none.
Loaded GHCi configuration from /tmp/ghci12676/ghci-script
Prelude>
Leaving GHCi.
$ stack ghci alib/Lib.hs
Using configuration for alib:lib to load xx/stack-ghci-i/alib/Lib.hs
The following GHC options are incompatible with GHCi and have not been passed to it: -threaded
Configuring GHCi with the following packages: alib
GHCi, version 8.0.1: http://www.haskell.org/ghc/  :? for help
[1 of 1] Compiling Lib              ( xx/stack-ghci-i/alib/Lib.hs, interpreted )

xx/stack-ghci-i/alib/Lib.hs:5:1: error:
    Failed to load interface for ‘Bar’
    It is a member of the hidden package ‘alib-0.1.0.0’.
    Use -v to see a list of the files searched for.
Failed, modules loaded: none.
Loaded GHCi configuration from /tmp/ghci13108/ghci-script
Prelude>
Leaving GHCi.
```

