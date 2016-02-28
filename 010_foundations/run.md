# Running Cayley

Create a config file called `cayley.cfg`

```
{
"database": "bolt",
"db_path": "/tmp/adventure-time",
"read_only": false
}
```

Ininitialize and Run the Repl

```
cayley init --config=cayley.cfg
cayley repl --config=cayley.cfg
```
You should be in the Repl and see this:

```
cayley>
```

Let's insert some information about Finn into our database:  
(Finn is a character from the awesome cartoon 'Adventure Time')

```
:a character:finn type human .
:a character:finn name Finn .
:a character:finn "in love with" character:princess-bubblegum .
```

Now let's find out all the characters that Finn is in love with:

```
g.V("character:finn").Out("in love with").All()
```

You should see something like this:

```
****
id : character:princess-bubblegum
=> <nil>
-----------
2 Results
Elapsed time: 0.895006 ms
```

Yeah! You just inserted two quads into Cayley and run your first query!

a Quad is a line like `character:finn type human .`. It's called a quad since it contain 4 parts. We'll get to that later. For now go and celebrate your success.

![Finn](https://media.giphy.com/media/rOTGSPxvJJY7m/giphy.gif)