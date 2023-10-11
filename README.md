# optParse
A set of bash development tools to automatically generate option parsing code from a minimal and succinct option parsing definition table. Both extglob (via `optParse`) and regex (via `rParse`) option parsing are implemented.

In both `optParse` and `rParse` you will need to define and supply an "option parsing definition table", which allows you to quickly define what options you want to be parsed. This includes:

* what flags are matched via an extglob/regex filter,
* if there is an argument (and if so what variable to save it in)
* what commands should be triggered and run when the option flag is passed

`optParse` / `rParse` will then take this table and expand it into robust option parsing code that, when run, will run the option-triggered commands and save option-provided arguments in the specified variables.

NOTE: `optParse` is recommended over `rParse` for virtually all usage, outside of the very unusual circumstance where extglob is not sufficient to adequately match an option flag but regex is. This is because:

1. `optParse` is an order of magnitude (if not more) faster than `rParse`
2. `optParse` allows for an option with an argument to also trigger commands to run, whereas `rParse` does not
3. `optParse` allows for (and in some cases will automatically generate) `+` options that turn off whatever flag the analogous `-` option enabled
4. `optParse` is probably more robust and less prone to unusual "edge-case" behavior than `rParse` is


# using `optParse` for extglob-based option parsing



# using rParse for regex-based option parsing
