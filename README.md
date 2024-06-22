# Args

A simple argument parser. It can automatically generate the usage based on the configuration.

Look at this example:

```umka
import "args.um"

type Mode = enum {
    normal
    special
}

fn main() {
    args := args::mk("Your program", "1.0.0", "Here goes description of your program.")

    // Works on various types automatically!
    var numeric: int
    var boolean: bool = true
    var list: []str
    var record: map[str]int
    var classifier: map[str][]int
    var mode: Mode = .normal

    // You can also create custom modes.
    args.mode("special", &mode, Mode.special, "A special mode")

    // Creates a help flag.
    // Set the parameter to true to show help if no mode is specified.
    args.help(true) 
    // Simple boolean switch.
    args.optional("boolean", &boolean, "A simple boolean.")
    // A required argument.
    args.required("numeric", &numeric, "A simple number.")
    // Can insert into a list.
    args.optional("list", &list, "Appends a number to a list.")
    // Can insert into a map.
    args.optional("record", &record, "Sets a key-value pair to a record.")
    // And can insert into a map of a list.
    args.optional("classifier", &classifier, "Appends a number to a list of a key.")

    // Validates the arguments for you.
    // Automatically shows generated help if no mode is specified.
    args.check(args::stdargv())

    // If an error occured, the program would've exited before this point.

    // For more advanced usage use `args.getusage()` to get the help string,
    // and access args.errors to get the errors.

    printf("numeric: %v\n", numeric)
    printf("boolean: %v\n", boolean)
    printf("list: %v\n", list)
    printf("record: %v\n", record)
    printf("classifier: %v\n", classifier)
    printf("mode: %v\n", mode)
}
```

Output:

```
W:\umka-extras\args>umka example.um test --help
Your program 1.0.0 - Here goes description of your program.

Arguments:
        --help -h                   - Show this help message
        --boolean -b                (default: true)
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
        --help -h                   - Show this help message
        --boolean -b                (default: true)
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

- Easy to use.
- Automatically generates usage based on the configuration.
- Validates the arguments for you, shows you the exact errors.
- Short argument syntax.
- Positional arguments and list positional arguments.
- Built-in help flag.
- Supports for modes (commands).
- Works on various types:
        * Booleans/Strings/Integers/Floats(Reals)
        * Maps (of Booleans/Strings/Integers/Floats(Reals)/Arrays)
        * Arrays (of Booleans/Strings/Integers/Floats(Reals))
- Has a more complex API, if needed.
