# optParse
A set of bash development tools to automatically generate option parsing code from a minimal and succinct option parsing definition table. Both extglob (via `optParse`) and regex (via `rParse`) option parsing are implemented.

In both `optParse` and `rParse` you will need to define and supply an "option parsing definition table", which allows you to quickly define what options you want to be parsed. This includes:

* what flags are matched via an extglob/regex filter,
* if there is an argument (and if so what variable to save it in)
* what commands should be triggered and run when the option flag is passed

`optParse` / `rParse` will then take this table and expand it into robust option parsing code that, when run, will run the option-triggered commands and save option-provided arguments in the specified variables. Each line in the option parsing definition table fully defines 1 option that you want to parse.

NOTE: `optParse` is recommended over `rParse` for virtually all usage, outside of the very unusual circumstance where extglob is not sufficient to adequately match an option flag but regex is. This is because:

1. `optParse` is an order of magnitude (if not more) faster than `rParse`
2. `optParse` allows for an option with an argument to also trigger commands to run, whereas `rParse` does not
3. `optParse` is probably more robust and less prone to unusual "edge-case" behavior than `rParse` is

NOTE: in early development (while options are being frequently changes) it may be appropriate to directly use `optParse` / `rParse` in the code and source the output it generates. For more stable codes with rarely changing options, however, it is best to run `optParse` / `rParse` externally and copy/paste the generated code into the function.


# using `optParse` for extglob-based option parsing

`optParse`'s option parsing definition table has lines defined via the following form:
 
    <EXTGLOB> :: <VAR> [<CMD_LIST>]

NOTE: `<EXTGLOB>` may contain multiple space-separated regex matches
NOTE: if the option does not have an argument, set `<VAR>` to `-`

`optParse` takes this table and constructs a series of `case` matches (1 per line), such that running the arguments (`printf '%s\n' "$@"`) through the `case` statement and sourcing the output will parse the options for you. 


# using `rParse` for regex-based option parsing

`rParse`'s option parsing definition table has a different syntax than `optParse`'s. Each line in the table can take 1 of   2 forms (depending if there is an argument or not:

     <REGEX> -> <CMD>    ( for options without arguments. <CMD> will be run when option is read. )
     <REGEX> => <VAR>    ( for options with arguments. Variable <VAR> will be set to the argument. )

NOTE: `<REGEX>` may contain multiple space-separated regex matches

`rParse` takes this table and constructs a series of `sed s/<FLAG_REGEX>/<CMD_TO_RUN>/` transformations (1 per line), such that running the arguments (`printf '%s\n' "$@"`) through the sed filter and sourcing the output will parse the options for you. 

See `rparse.speedtest.bash` for an example that (more-or-less) parses [forkrun's options](https://github.com/jkool702/forkrun/blob/76e6aa38580301bd376c5505f2eb64128b1c483b/forkrun.bash)
