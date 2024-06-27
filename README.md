# Args

* [Description](#description)
* [Features](#features)
* [Syntax](#syntax)

# Description

A simple argument parser. It can automatically generate the usage based on the configuration.

Look at this example:

```umka
import "args.um"

fn main() {
    args := args::mk("Your program", "1.0.0", "Here goes description of your program.", args::stdargv())

    // Works on various types automatically!
    var numeric: int
    var boolean: bool = true
    var list: []str
    var cutoff: []str
    var record: map[str]int
    var classifier: map[str][]int

    // Creates a help flag.
    args.help() 

    // You can also create custom modes.
    if args.mode("test", "Operation mode") {
        // Simple boolean switch.
        // Short flag allows you to use -b instead of --boolean.
        args.optional(&boolean, "boolean").short('b')
        // A required argument.
        // you can use *Next for positional arguments.
        args.requiredNext(&numeric, "numeric", "A simple number.")
        // Can insert into a map.
        args.optional(&record, "record", "Sets a key-value pair to a record.").short('r')
        // And can insert into a map of a list.
        args.optional(&classifier, "classifier", "Appends a number to a list of a key.")
        // Cutoff arguments, will be ignored and parsing will stop immediately, useful if you need to pass them to another program.
        args.requiredCutoff(&cutoff, "cutoff");

        // Validates the arguments for you.
        // Automatically shows generated help if no mode is specified.
        args.check()

        // If an error occured, the program would've exited before this point.
        //
        // For more advanced usage use `args.getusage()` to get the help string,
        // args.parse, to manually parse and retrieve the errors.
        printf("numeric: %v\n", numeric)
        printf("boolean: %v\n", boolean)
        printf("cutoff: %v\n", cutoff)
        printf("record: %v\n", record)
        printf("classifier: %v\n", classifier)
    } else {
        // Here we'll print the usage, if the `test` mode isn't selected.
        args.usage()
    }
}
```

Output:

```
W:\umka-extras\args>umka example.um
Your program 1.0.0 - Here goes description of your program.

Arguments:
        --help, -h             - Show this help message

Modes:
        test - Operation mode
```


Let's select the `test` mode:

```
W:\umka-extras\args>umka example.um test --help
Arguments:
        --help, -h                - Show this help message
        --boolean, -b             (default: true)
        --numeric     <int>       - A simple number. (required)
        --record, -r  <key> <int> - Sets a key-value pair to a record.
        --classifier  <key> <int> - Appends a number to a list of a key.
        --cutoff      <str>       (required)

Format:
        <numeric> : <cutoff...>
```

It will gracefully handle the errors for you:

```
W:\umka-extras\args>umka example.um test
Your program 1.0.0 - Here goes description of your program.

Arguments:
        --help, -h                - Show this help message
        --boolean, -b             (default: true)
        --numeric     <int>       - A simple number. (required)
        --record, -r  <key> <int> - Sets a key-value pair to a record.
        --classifier  <key> <int> - Appends a number to a list of a key.
        --cutoff      <str>       (required)

Format:
        <numeric> : <cutoff...>

Error: Missing argument for numeric
Error: Need at least one argument for cutoff
```

And more! Consult documentation for `args.um` for more information.

# Features

* Easy to use.
* Automatically generates usage based on the configuration.
* Validates the arguments for you, shows you the exact errors.
* Short argument syntax.
* Positional arguments and list positional arguments.
* Built-in help flag.
* Supports for modes (commands).
* Allows you to cutoff arguments, which stops parsing, useful for transferring arguments to another program.
* Works on various types:
  - Booleans/Strings/Integers/Floats(Reals)
  - Maps (of Booleans/Strings/Integers/Floats(Reals)/Arrays)
  - Arrays (of Booleans/Strings/Integers/Floats(Reals))
* Has a more complex API, if needed.


# Syntax

- `-p` shortname parameter
- `--param` long name parameter
- `--map a=b` or `--map a b` map parameter assignment
- `--arr a` array parameter append
- `--help` or `-h` help flag
