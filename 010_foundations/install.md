# Installing Cayley

Grab the recent release from the [releases page](https://github.com/google/cayley/releases), unzip it and put it somewhere on your PATH so you can use it from anywhere. There are binaries for Mac, Linux, and Windows.

Here is a quick way to do that on Linux:

```
curl -L https://github.com/google/cayley/releases/download/v0.4.1/cayley_0.4.1_linux_amd64.tar.gz | tar xz
sudo cp cayley_0.4.1_linux_amd64/cayley /usr/local/bin/
rm -r cayley_0.4.1_linux_amd64
```

Verify that it works by running `cayley`. You should see something like this:

```
Cayley is a graph store and graph query layer.

Usage:
  cayley COMMAND [flags]

Commands:
  init      Create an empty database.
  load      Bulk-load a quad file into the database.
  http      Serve an HTTP endpoint on the given host and port.
  repl      Drop into a REPL of the given query language.
  version   Version information.
```

Woha. That was easy!