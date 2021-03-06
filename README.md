# urlpath

[![Build Status](https://travis-ci.org/athanclark/urlpath.svg)](https://travis-ci.org/athanclark/urlpath)
[![Coverage Status](https://img.shields.io/coveralls/athanclark/urlpath.svg)](https://coveralls.io/r/athanclark/urlpath)

Dirt-simple, embarrassing, horribly unimaginative URL combinator library for 
Haskell.


## Installation

```bash
cabal install urlpath
```

## Usage

You can use the combinators purely, if you're into that:

```haskell
λ> render $ "foo.asp" <?> ("key1","bar") <&> ("key2","baz")

↪ "foo.asp?key1=bar&key2=baz"
```

Or you can use them with a configurable root, via the Reader monad:

```haskell
λ> runReader
     (expandAbsolute $ "foo.asp" <?> ("key1","bar") <&> ("key2","baz"))
     "http://example.com"

↪ "http://example.com/foo.asp?key1=bar&key2=baz"
```

... We can also do things in Lucid:

```haskell
λ> runReader ( renderTextT $
     (\a -> do
         foo <- lift $ expandAbsolute a
         script_ [src_ foo] ""
       ) $ "foo" <?> ("bar","baz")
     ) "http://example.com"

↪ "<script src=\"example.com/foo?bar=baz\"></script>"
```

and, in Scotty:

```haskell
main :: IO ()
main = scottyT 3000
    rootConf
    rootConf
    run

  where
    rootConf = flip runReaderT "http://example.com"

    run :: ( MonadIO m
           , MonadReader T.Text m ) =>
           ScottyT LT.Text m ()
    run = get "/" $ do
      path <- lift $ expandAbsolute $ "foo" <?> ("bar","baz")
      text $ LT.fromStrict path
```

```
λ> curl localhost:3000/
↪ "http://example.com/foo?bar=baz"
```

## How to run tests

HAHAHAAHAHahahahahahaha hahahaha..... haaaaa. Oh man, that was good.

## Contributing

`¯\_(ツ)_/¯`
