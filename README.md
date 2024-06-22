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
        // Can insert into a list.
        // Giving a positional argument to a list will span the rest of the arguments.
        args.optionalNext(&list, "list", "Appends a number to a list.")
        // Can insert into a map.
        args.optional(&record, "record", "Sets a key-value pair to a record.").short('r')
        // And can insert into a map of a list.
        args.optional(&classifier, "classifier", "Appends a number to a list of a key.")

        // Validates the arguments for you.
        // Automatically shows generated help if no mode is specified.
        args.check()

        // If an error occured, the program would've exited before this point.
        //
        // For more advanced usage use `args.getusage()` to get the help string,
        // args.parse, to manually parse and retrieve the errors.
        printf("numeric: %v\n", numeric)
        printf("boolean: %v\n", boolean)
        printf("list: %v\n", list)
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
Your program 1.0.0 - Here goes description of your program.

Arguments:
        --help -h                - Show this help message
        --boolean -b             (default: true)
        --numeric    <int>       - A simple number. (required)
        --list       <str>       - Appends a number to a list.
        --record -r  <key> <int> - Sets a key-value pair to a record.
        --classifier <key> <int> - Appends a number to a list of a key.

Format:
        <numeric> [list...]
```

It will gracefully handle the errors for you:

```
W:\umka-extras\args>umka example.um test
Your program 1.0.0 - Here goes description of your program.

Arguments:
        --help -h                - Show this help message
        --boolean -b             (default: true)
        --numeric    <int>       - A simple number. (required)
        --list       <str>       - Appends a number to a list.
        --record -r  <key> <int> - Sets a key-value pair to a record.
        --classifier <key> <int> - Appends a number to a list of a key.

Format:
        <numeric> [list...]

Error: Missing argument for numeric
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
