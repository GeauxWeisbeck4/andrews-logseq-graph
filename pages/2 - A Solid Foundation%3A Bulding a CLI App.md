# 2   A solid foundation: Building a command-line application
- ### This chapter covers
	- Working with command-line flags, options, and arguments
	- Passing configuration into an application
	- Starting and gracefully stopping a web server
	- Path routing for web and API servers
	  
	  
	  
	  
	  
	  
	  
	  
	  
	  
	  
	  
	  This chapter covers several foundational areas for developing 
	  command-line interface (CLI) applications. You’ll learn about handling 
	  command-line options—sometimes called *flags* or *getopts* —in a way that’s consistent with modern applications for Linux and other Portable Operating System Interface (POSIX) systems.
	  
	  
	  
	  
	  
	  
	  
	  
	  
	  
	  We’ll follow up by looking at several ways to pass configuration 
	  into an application, including environment variables and various popular
	  file formats used to store configuration. You’ll also learn a bit about
	  structs and interfaces; you’ll see how to couple them tightly to 
	  maintain state for configuration. You’ll look at Go’s enum support and 
	  drawbacks as part of this topic.
	  
	  
	  
	  
	  
	  
	  
	  
	  
	  
	  From there, you’ll move on to building and starting a simple 
	  server from the command line and learning best practices for starting 
	  and stopping it. Last, you’ll learn URL path-matching techniques for 
	  websites and servers, providing a Representational State Transfer (REST)
	  API, which will give your server a bit more functionality.
- ## 2.1  Building CLI applications the Go way
  
  
  
  
  
  
  
   
  
  Whether you’re using a console application (system commands, the 
  source-control management system Git, or an application such as the 
  MySQL database, among many others), command-line arguments and flags are
  part of the application user interface. The Go standard library has 
  built-in functionality for working with these arguments and flags, and 
  it handles many of the hard parts and edge cases easily and elegantly.
- ##### Windowed applications
  
   
  
  
   
  
  
    
  
  As a systems language, Go doesn’t provide native support for 
  building graphical user interface (GUI) applications like the ones you 
  find in Microsoft Windows or macOS. There are libraries and frameworks 
  that can create non-native GUI applications, but nothing is built into 
  the standard library. Along with several QT or GTK bindings, you may 
  find something similar to the popular JavaScript cross-platform 
  framework Electron in Wails, which allows you to combine Go as a backend
  and web technologies (HTML, CSS, and JavaScript) as a frontend to make 
  desktop applications. You can find out more at [https://github.com/wailsapp/wails](https://github.com/wailsapp/wails).
- ### 2.1.1  Command-line flags
  
  
  
  
  
  
  
   
  
  Argument and flag handling in the Go standard library is based on 
  Plan 9, which has a different style from the systems based on GNU/Linux 
  and Berkeley Software Distribution (BSD), such as macOS and FreeBSD, 
  that are wide used today. On Linux and BSD systems, for example, you can
  use the command `ls` `-la` to list all files in a directory. The command is `ls`, and the `-la` part of the command contains two flags, or options. The `l` flag tells `ls` to use the long form listing, and the `a` flag (for *all *)
  causes the list to include hidden files. The Go flag system won’t let 
  you combine multiple flags; instead, it sees one flag named `la`, partly because Go treats short command-line flags (`-la`) the same as long ones (`--la`).
  
  
  
  
  
  
  
  
   
  
  GNU-style commands such as `ls` support long options (such as `--color`) that require two dashes to tell the program that the string `color` isn’t five options (with a duplicate in this case), but one. Whether this style suits you boils down to preference.
  
  
  
  
  
  
  
  
   
  
  This built-in flag system doesn’t differentiate between short and 
  long flags. A flag can be short or long, and each flag must be separate 
  from the others. If you run `go` `help` `build`, for example, you’ll see flags such as `-v`, `-race`, `-x`, and `-work`.
  For the same option, you’ll see a single flag rather than a long or 
  short name. To illustrate default flag behavior, the following listing 
  shows a simple console application using the `flag` package.
- ##### Listing 2.1   `Hello`   `World`  CLI using  `flag`  package
  
   
  
  
    
  
  ```
  $ flag_cli
  Hello World!
  $ flag_cli -name Buttercup
  Hello Buttercup!
  $ flag_cli –s –name Buttercup
  Hola Buttercup!
  $ flag_cli –-spanish –name Buttercup
  Hola Buttercup!
  ```
  
  
   
  
  
  
  
  
  
  
  
   
  
  Each flag is separate from the rest and begins with `-` or `--`,
  which are used interchangeably. A method is available to define a flag 
  with a short or a long name, but as you see in the next listing, it’s 
  not implicit.
- ##### Listing 2.2   `Hello`   `World`  using command-line flags
  
   
  
  
    
  
  ```
  package main
  
  import (
     "flag"      #1
     "fmt"
  )
  
  var name = flag.String("name", "World", "A name to say 
  hello to.")  #2
  var inSpanish bool                                         #3
  func init() {
  
  flag.BoolVar(&inSpanish, "spanish", false, "Use 
  Spanish language.")
  
  flag.BoolVar(&inSpanish, "s", false, "Use Spanish 
  language.")  #4
     flag.Parse()                                        #5
  }
  
  func main() {
     if inSpanish {
            fmt.Printf("Hola %s!\n", *name)      #6
     } else {
            fmt.Printf("Hello %s!\n", *name)
     }
  }
  ```
  
  
    
  
     #1 Imports the standard flag package
     
  #2 Creates a new variable from a flag
     
  #3 New variable to store flag value
     
  #4 Sets variable to the flag value
     
  #5 Parses the flags, placing values in variables
     
  #6 Accesses name as a pointer
     
  
    
  
  
   
  
  
  
  
  
  
  
  
   
  
  Here you see two ways to define a flag. The first is to create from a flag a string typed `variable` with a default value of `"World"`. In this example, you do this by using `flag.String()`. `flag.String` takes a flag name, default value, and description as arguments. The value of `name` is an address containing the value of the flag. To access this value, you need to access `name` as a pointer (as you would in a C-variant language).
  
  
  
  
  
  
  
  
   
  
  The second method of handling a flag is the one that explicitly 
  defines both a long and a short variant. Start by creating a normal 
  variable of the same type as the flag. This variable will be used to 
  store the value of a flag. Then use one of the flag functions that 
  places the value of a flag in an existing variable. In this case, `flag.BoolVar` is used twice, once for the long name and once for the shorthand version.
  
  
  
  
  
  
  
  
   
  
  TIP  Flag functions exist for each basic data type, including `bool`, `string`, `int` (and unsigned and larger capacity variants), `byte`, `float32`, and `float64`. All these types follow a similar, predictable pattern. Both `flag.StringVar` and `flag.IntVar`, for example, try to assign an input argument to a variable of their respective types. To learn about them, see the `flag` package documentation at [https://pkg.go.dev/flag](https://pkg.go.dev/flag).
  
  
  
  
  
  
  
  
   
  
  Finally, to set the flag values in the variables`,` `run` `flag.Parse()`. The `flag`
  package provides an exit response with a default help response, but 
  only if invalid or erroneous flags are passed to your application. In 
  this case, what happens if you pass both `--spanish` and `-s`? They’re Booleans, so including them without a value assignment will result in a `true` value. If you pass `--spanish=true` and `-s=false`, the value will be `false` because `flag.Parse()`
  runs through the flags in the order in which you declare them. This 
  fact is important to keep in mind because you can allow one version of a
  flag to overwrite the others. We’ll look at some alternatives in 
  section 2.1.4.
- ##### Listing 2.3  Invoking built-in help descriptions
  
   
  
  
    
  
  ```
  $ flag_cli --invalidFlag        #1
  Usage of ./flag_cli:                   #2
  -name string
        A name to say hello to. (default "World")
  -s    Use Spanish language.
  -spanish
        Use Spanish language.
  ```
  
  
    
  
     #1 An invalid flag is passed through the command line.
     
  #2 The usage help text is returned in this case.
     
  
    
  
  
   
  
  
  
  
  
  
  
  
   
  
  This can also happen if you attempt to pass an incompatible type to your application, such as a number to the `-name` argument. Go won’t attempt to coerce the value into the expected type, so passing a `float` when your application expects an `int` also triggers an early exit. One exception is a `time.Duration`, which is automatically typecast from an integer.
  
  
  
  
  
  
  
  
   
  
  Tip  When you’re working with `time.Duration` in mathematical operations, sometimes you’ll have to cast an integer to a `time.Duration` manually by wrapping it with the `time.Duration`, as in `weekAgo` `:=` `time.Now().Add(time .Duration(7` `*` `24)` `*` `time.Hour)`.
  
  
  
  
  
  
  
  
   
  
  The `flag` package has two handy functions you can use to take more control of this messaging. The `PrintDefaults` function generates help text for flags. The line of help text for the preceding `name` option reads as follows:
  
  
  
  
  
  
  
  
   
  
  
    
  
  ```
  -name string
    A name to say hello to. (default "World")
  ```
  
  
   
  
  
  
  
  
  
  
  
   
  
  This feature is a nicety in Go that makes it easier to inform users how your program works and how they can interact with it.
  
  
  
  
  
  
  
  
   
  
  Flags also have a `VisitAll` function that accepts a 
  callback function as an argument. This function iterates over each of 
  the flags executing the callback function on it and allows you to write 
  your own help text for them. The following listing would display `-name:` `A` `name` `to` `say` `hello` `to.` `(Default:` `'World').`
- ##### Listing 2.4  Custom flag help text
  
   
  
  
    
  
  ```
  flag.VisitAll(func(flag *flag.Flag) {
     format := "\t-%s: %s (Default: '%s')\n"
     fmt.Printf(format, flag.Name, flag.Usage, flag.DefValue)
  })
  ```
  
  
   
  
  
  
  
  
  
  
  
   
  
  You can also run through the entire list of passed arguments by calling `flag.Args()`, which skips flags passed with `–` or `--`
  but shows other arguments given to your application as a slice of 
  strings. These arguments aren’t parsed and aren’t subject to validation 
  by `flag.Parse()`.
- ### 2.1.2  Defining valid values via enums
  
  
  
  
  
  
  
   
  
  Let’s detour briefly by expanding our example to support multiple 
  languages. We’ll do this to set the stage for demonstrating one of Go’s 
  less robust features and showing when and why to use it.
  
  
  
  
  
  
  
  
   
  
  Suppose that you want to support English, Spanish, German, and 
  French. In many strongly typed languages, you might reach for an 
  enumeration (*enum*) as shown in the following listing.
- ##### Listing 2.5  Defining valid options in an enumeration
  
   
  
  
    
  
  ```
  package main
  
  import (
    "flag"
    "log"
  )
  
  var name = flag.String("name", "World", "A name to say hello to.")
  
  type language = string        #1
  
  var userLanguage language       #2
  
  const (                      #3
    English language = "en"
    Spanish          = "sp"
    French           = "fr"
    German           = "de"
  )
  
  func init() {
    flag.StringVar(&userLanguage, "lang", "en", "language 
    to use (en, sp, fr, de).")     #4
    flag.Parse()
  }
  
  func main() {
    switch(userLanguage) {                  #5
    case English:                           #5
        log.Printf("Hello %s!\n", *name)
    case Spanish:
        log.Printf("Hola %s!\n", *name)
    case French:
        log.Printf("Bonjour %s!\n", *name)
    case German:
        log.Printf("Hallo %s!\n", *name)
    }
  }
  ```
  
  
    
  
     #1 Defines a type alias to string
     
  #2 Our destination value
     
  #3 An enumeration of constants
     
  #4 Assigns our variable’s value via flag
     
  #5 A switch around our defined values, outputting our response by language
     
  
    
  
  
   
  
  
  
  
  
  
  
  
   
  
  At the top, you declare an enum of languages. There is no `enum`
  keyword, which probably is helpful because enums don’t necessarily work
  as you might expect. If you run this code and pass any valid language, 
  you’ll get the output you expect.
  
  
  
  
  
  
  
  
   
  
  But if it’s an enumeration, what happens if you pass `--lang="xy"`?
  Don’t panic. There’s no output because you’ve simply assigned input to a
  string type alias. This example highlights a possible shortcoming of 
  enums compared with other languages: you can think of them as maps of 
  constants to values. They offer little value here. Yes, you can have a `default`
  switch case or get a list of values from a map to return feedback to 
  users, but this example mostly demonstrates a case in which Go doesn’t 
  do what you might expect of a language with true enum support. You’re 
  probably better off defining a slice or map of valid languages and 
  checking for validity inside `init()`.
  
  
  
  
  
  
  
  
   
  
  Here’s a quick example of validation. This code formalizes valid values in a slice and checks upon initialization:
  
  
  
  
  
  
  
  
   
  
  
    
  
  ```
  validLanguages = []string{
    "en",
    "sp",
    "fr",
    "de",
  }
  
  func init() {
    flag.StringVar(&userLanguage, "lang", "en", "language 
    to use (en, sp, fr, de).")
    flag.Parse()
    if slices.Contains(validLanguages, userLanguage) == false {
  log.Fatalf("Invalid language %s. Please use one of %v", 
  userLanguage, validLanguages)
    }
  }
  ```
- ### 2.1.3  Slices, arrays, and maps
  
  
  
  
  
  
  
   
  
  Go has several approaches for lists/arrays/vectors and 
  dictionaries. The standard library provides more data structure-like 
  lists in its container packages, but for homogeneous collections, slices
  and arrays suffice for most use cases. Slices and arrays differ only in
  that slices have an unbounded length and arrays have a fixed length. 
  The `flag.Args()` method returns a variable-length collection of strings and adds nonflagged, whitespace-delimited arguments to the slice.
  
  
  
  
  
  
  
  
   
  
  For dictionary-like support, maps allow assignment via any type as
  a key and any type as a value. Support for generics also allows some 
  flexibility on slice types, but you won’t see built-in tuple-like 
  heterogeneous support because slices need to satisfy the interfaces for 
  ordering and comparability. You can build this yourself on top of the `interface{}` or `any` types. These are known as *empty interfaces* because they have no methods that must be satisfied. We’ll talk more about this topic in chapter 3.
- ### 2.1.4  Command-line frameworks
  
  
  
  
  
  
  
   
  
  Although the built-in `flag` package is useful and 
  provides the basics, it doesn’t provide flags in the manner that most of
  us have come to expect. The difference in user interaction between the 
  style used in Plan 9 and the style in Linux- and BSD-based systems is 
  enough to cause users to stop and think, often due to problems they 
  encounter when trying to mix short flags, such as those used to execute a
  command like `ls` `-la`.
	- *Problem* —Those of us who
	  write for non-Windows systems will likely be working with a UNIX 
	  variant, and our users will expect UNIX-style flag processing. How do 
	  you write Go command-line tools that meet users’ expectations? Ideally, 
	  you want to do this without writing one-off specialized flag processing.
	  
	  
	  
	  
	  
	  
	  
	  
	  
	  
	  Processing flags certainly isn’t the only important part of 
	  building a command-line application. Although Go developers learn as a 
	  matter of course how to begin with a `main` function and 
	  write a basic program, we often find ourselves writing the same set of 
	  features for each new command-line program. Fortunately, tools can 
	  provide a better entry point for creating command-line programs, and 
	  we’ll look at one of them in this section.
	- *Solution* —This common 
	  problem has been solved in a couple of ways. Some applications, such as 
	  the software container-management system Docker, have a subpackage 
	  containing code to handle Linux-style flags. In some cases, these are 
	  forks of the Go `flag` package to which the extra needs are 
	  added. But maintaining per-project implementations of argument parsing 
	  results in a lot of duplicated effort to solve the same generic problem 
	  repeatedly. 
	  
	  
	  
	  
	  
	  
	  
	  
	  
	  A better approach is to lean on an existing command-line library. 
	  You can import several standalone packages into your own application. 
	  Many of these packages are based on the `flag` package 
	  provided by the standard library and have compatible or similar 
	  interfaces. Importing one of these packages and using it is faster than 
	  altering the `flag` package and maintaining the difference.
	  
	  
	  
	  
	  
	  
	  
	  
	  
	  
	  In advanced cases, you can parse a single argument for known 
	  tokens by using Go’s built-in flag and parser tooling. Some third-party 
	  packages take this approach to building something a little different 
	  from Go’s built-in `flag` package.
	  
	  
	  
	  
	  
	  
	  
	  
	  
	  
	  If you want something more opinionated and feature-rich for 
	  building a console application, frameworks are available to handle 
	  command routing, help text, subcommands, and shell autocompletion, in 
	  addition to flags. A popular framework used to build console-based 
	  applications is Cobra ([https://github.com/spf13/cobra](https://github.com/spf13/cobra)), which is used by Docker, Kubernetes, and many others. Another is cli ([https://github.com/urfave/cli](https://github.com/urfave/cli)),
	  which has been used by Docker and other projects such as the open 
	  source Platform as a Service (PaaS) project Cloud Foundry. You’ll see 
	  how to implement the more robust Cobra framework in your command-line 
	  applications in the next section.
- #### A simple console application
  
  
  
  
  
  
  
   
  
  It’s useful to look at a console application that executes a 
  single action. You’ll use this foundation to expand multiple commands 
  and subcommands. Figure 2.1 shows the structure a single-action 
  application can use from the command line.
  
  
  
  
  
  
  
  
   ![figure](https://learning.oreilly.com/api/v2/epubs/urn:orm:book:9781633436886/files/Images/2-1.png)
- ##### Figure 2.1  Structure of a simple application
  
  
  
  
  
  
  
   
  
  The following listing is a simple console application that displays `Hello` `World` or says `hello` to a name you choose.
- ##### Listing 2.6  Running  `Hello`   `World`  in CLI with flags
  
   
  
  
    
  
  ```
  $ hello_cli
  Hello World!
  $ hello_cli --name Inigo
  Hello Inigo!
  $ hello_cli –n Inigo
  Hello Inigo!
  $ hello_cli –help
  NAME:
   hello_cli - Print hello world
  USAGE:
   hello_cli [global options] command [command options] [arguments...]
  VERSION:
   0.0.0
  COMMANDS:
   help, h   Shows a list of commands or help for one command
  GLOBAL OPTIONS:
   --name, -n 'World'      Who to say hello to.
   --help, -h              show help
   --version, -v           print the version
  ```
  
  
   
  
  
  
  
  
  
  
  
   
  
  When you use Cobra, the application-specific code required to 
  generate this application is only a dozen lines; it handles short and 
  long flags, provides feedback for users, and more. You have to do a bit 
  of a restructuring of the standard Go application by wrapping your `main`’s functionality as a `&cobra.Command`, which contains the logic necessary to add all the nice features that otherwise would require a lot of boilerplate.
- ##### Listing 2.7   `Hello`   `World!`  CLI:  `hello_cli.go`
  
   
  
  
    
  
  ```
  package main
  
  import (
    "fmt"
  
    "github.com/spf13/cobra"
  )
  
  var helloCommand *cobra.Command     #1
  
  func init() {
    helloCommand = &cobra.Command{        #2
        Use:   "hello",
        Short: "Print hello world",
        Run:   sayHello,
    }
    helloCommand.Flags().StringP("name", "n", "World", 
    "Who to say hello to.")                                      #3
    helloCommand.MarkFlagRequired("name")                #4
    helloCommand.Flags().StringP("language", "l", "en", 
    "Which language to say hello in.")                     #5
    }
  
  func sayHello(cmd *cobra.Command, args []string) {
    name, _ := cmd.Flags().GetString("name")           #6
    greeting := "Hello"
    language, _ := cmd.Flags().GetString("language")  
    switch language {                                  #7
    case "en":
        greeting = "Hello"
    case "es":
        greeting = "Hola"
    case "fr":
        greeting = "Bonjour"
    case "de":
        greeting = "Hallo"
    }
    fmt.Printf("%s %s!\n", greeting, name)
  }
  
  func main() {
    helloCommand.Execute()      #8
  }
  ```
  
  
    
  
     #1 Creates a cobra.Command pointer, which wraps our app’s functionality
     
  #2 Initializes our command, with details on usage
     
  #3 Defines a flag with long and short version and help text
     
  #4 Marks the flag "name" as required
     
  #5 Defines our language flag but doesn’t mark it as required
     
  #6 Extracts the name and language from Cobra
     
  #7 Our language switch
     
  #8 Executes our command
     
  
    
  
  
   
  
  
  
  
  
  
  
  
   
  
  As you can see, a little more wrapping is happening here than in 
  your straightforward code, but it’s structurally similar. You use Cobra 
  to define commands with a slice of arguments, which can be extracted 
  within the function.
  
  
  
  
  
  
  
  
   
  
  The basic setup for grabbing the arguments themselves is fairly 
  ergonomic and similar to the standard library. These arguments aren’t 
  bound to a variable automatically, so we need to extract them to work 
  with them within our target command.
  
  
  
  
  
  
  
  
   
  
  `cli.go` enables you to have a default action when the 
  application is run, including commands and nested subcommands. (The next
  section covers commands and subcommands.) Here, you set the `Action` property that handles the default action. The value of `Action` is a function that accepts `*cli.Context`
  as an argument and returns an error. This function contains the 
  application code that should be run. In this case, it obtains the name 
  to say hello to by using `c.GlobalString("name")` before printing `hello` to the name. If you want the returned error to cause the application to exit with a nonzero exit code, return an error of `cli.ExitError`, which you can create using the `cli.NewExitError` function.
  
  
  
  
  
  
  
  
   
  
  The final step is running the application that you just set up. To do this, pass `os.Args` (the arguments passed into the application) into the `Run` method of the application.
  
  
  
  
  
  
  
  
   
  
  TIP  Arguments are available to the `Action` function, having been passed to the `Run` initializer method. In this case, you can obtain a string slice of arguments by calling `c.Args`. The first argument would be `c.Args()[0]`.
- #### Commands and subcommands
  
  
  
  
  
  
  
   
  
  Applications built for the command line often have multiple 
  context-specific conditional arguments, which we’ll call commands and 
  subcommands (figure 2.2). Think of an application such as Git, in which we can run commands such as `git` `add`, `git` `commit`, and `git` `push` `origin/main`. This isn’t uncommon, but using the wrong subcommand with a command can produce errors, as in `git` `add` `origin/main`.
  
  
  
  
  
  
  
  
   ![figure](https://learning.oreilly.com/api/v2/epubs/urn:orm:book:9781633436886/files/Images/2-2.png)
- ##### Figure 2.2  Usage structure of an application with commands and subcommands
  
  
  
  
  
  
  
   
  
  To illustrate using commands, the following example is a simple 
  calculator application that takes a command and two positional 
  arguments.
- ##### Listing 2.8  A basic CLI calculator
  
   
  
  
    
  
  ```
  package main
  
  import (
    "log"
    "strconv"
  
    "github.com/spf13/cobra"
  )
  
  func operation(op string, n1 string, n2 string) (float32, error) {
    num1, err := strconv.Atoi(n1)
    if err != nil {
        return 0, err
    }
    num2, err := strconv.Atoi(n2)
    if err != nil {
        return 0, err
    }
    fl1 := float32(num1)
    fl2 := float32(num2)
    switch op {
    case "add":
        return fl1 + fl2, nil
    case "sub":
        return fl1 - fl2, nil
    case "mul":
        return fl1 * fl2, nil
    case "div":
        return fl1 / fl2, nil
    }
  
    return 0, nil
  }
  
  var cmdAdd = &cobra.Command{
    Use:   "add",
    Short: "Add two numbers",
    Long:  "Add two numbers: add <number1> <number2>",
    Run: func(cmd *cobra.Command, args []string) {
        result, err := operation("add", args[0], args[1])
        if err != nil {
            log.Fatal(err)
        }
        log.Printf("result: %f\n", result)
    },
    Args: cobra.ExactArgs(2),
  }
  
  var cmdSub = &cobra.Command{
    Use:   "sub",
    Short: "Subtract two numbers",
    Long:  "Subtract two numbers: sub <number1> <number2>",
    Run: func(cmd *cobra.Command, args []string) {
        result, err := operation("sub", args[0], args[1])
        if err != nil {
            log.Fatal(err)
        }
        log.Printf("result: %f\n", result)
    },
    Args: cobra.ExactArgs(2),
  }
  
  var cmdMul = &cobra.Command{
    Use:   "mul",
    Short: "Multiply two numbers",
    Long:  "Multiply two numbers: mul <number1> <number2>",
    Run: func(cmd *cobra.Command, args []string) {
        result, err := operation("mul", args[0], args[1])
        if err != nil {
            log.Fatal(err)
        }
        log.Printf("result: %f\n", result)
    },
    Args: cobra.ExactArgs(2),
  }
  
  var cmdDiv = &cobra.Command{
    Use:   "div",
    Short: "Divide two numbers",
    Long:  "Divide two numbers: div <number1> <number2>",
    Run: func(cmd *cobra.Command, args []string) {
        result, err := operation("div", args[0], args[1])
        if err != nil {
            log.Fatal(err)
        }
        log.Printf("result: %f\n", result)
    },
    Args: cobra.ExactArgs(2),
  }
  
  func main() {
  var calculator = &cobra.Command{Use: "calculator", Short: "A simple 
  calculator"}
    calculator.AddCommand(cmdAdd)
    calculator.AddCommand(cmdSub)
    calculator.AddCommand(cmdMul)
    calculator.AddCommand(cmdDiv)
    calculator.Execute()
  }
  ```
  
  
   
  
  
  
  
  
  
  
  
   
  
  Although this calculator isn’t robust by any measure—it accepts 
  only two numbers and does no computation parsing—it shows some of 
  Cobra’s comfort features. We can add commands to commands, separate 
  commands to route based on an initial argument, and requirements for the
  number of subcommands needed to route and process the command properly.
- ## 2.2  Handling configuration
  
  
  
  
  
  
  
   
  
  A second foundational area that complements flag handling is 
  persistent application configuration. Examples of this form of 
  configuration include the files in the `etc` directory in some Linux installations and user-specific configuration dotfiles such as .gitconfig and .bashrc.
  
  
  
  
  
  
  
  
   
  
  Passing configuration to an application is a common need. 
  Virtually every nontrivial application, including console applications 
  and servers, needs some form of persistent configuration. How can 
  configuration information be easily passed to your applications and made
  available in them?
  
  
  
  
  
  
  
  
   
  
  Whereas the preceding section talks about passing in more 
  ephemeral configuration via command-line options, this section looks at 
  passing in configuration via files—or, in the case of 12-factor apps, 
  using environment variables. In this section, we will add support for 
  common configuration formats, including JSON, YAML, and INI. Then we 
  present the approach favored in the 12-factor pattern, which passes in 
  configuration through environment variables.
- ##### Understanding 12-factor apps
  
   
  
  
   
  
  
    
  
  This popular, widely used methodology for building web 
  applications, Software as a Service (SaaS), and similar applications 
  observes the following 12 factors:
	- Use a single codebase, tracked in revision control, that can be deployed multiple times.
	- Explicitly declare dependencies and isolate them from other applications.
	- Store application configuration in the environment.
	- Attach supporting services.
	- Separate the build and run stages.
	- Execute the application as one or more stateless processes.
	- Export services via Transmission Control Protocol (TCP) port binding.
	- Scale horizontally by adding processes.
	- Maximize robust applications with fast startup and graceful shutdown.
	- Keep development, staging, and production as similar as possible.
	- Handle logs as event streams.
	- Run admin tasks as separate processes. 
	  
	  
	  
	  
	  
	  
	  
	  
	  
	  You can find more details on these factors at [https://12factor.net](https://12factor.net).
- ### 2.2.1  Using configuration files
  
  
  
  
  
  
  
   
  
  Command-line arguments, like those covered earlier in this 
  chapter, are good for many things and should be used for applications 
  with functional variables in practical use. But when it comes to 
  one-time configuration of a program installed in a particular 
  environment, command-line arguments aren’t the right fit. They need to 
  be fed to the application repeatedly and are vulnerable to typos and 
  mistakes. The most common solution is to store configuration data in a 
  file that the program can load at startup. In the next few sections, 
  you’ll look at the three most popular configuration file formats and see
  how to work with them in Go:
	- *Problem* —A program requires persistent configuration that may be too complex for command-line arguments.
	- *Solution* —One of today’s
	  most popular configuration file formats is JavaScript Object Notation 
	  (JSON). The Go standard library provides built-in JSON parsing, 
	  unmarshaling, and marshaling.
	- *Discussion* —Consider a JSON configuration file, `config.json`, with the following structure: 
	  
	  
	  
	  
	  
	  
	  
	  
	  
	  
	  
	  
	  ```
	  {
	  "enabled": true,
	  "path": "/usr/local"
	  }
	  ```
	  
	  
	  
	  
	  
	  
	  
	  
	  
	  
	  
	  
	  
	  The following listing demonstrates a JSON configuration parser.
- ##### Listing 2.9  Parsing a JSON configuration file:  `json_config.go`
  
   
  
  
    
  
  ```
  package main
  
  import (
     "encoding/json"
     "fmt"
     "os"
  )
  type configuration struct {     #1
     Enabled bool               #1
     Path    string             #1
  }                               #1
  func main() {
     file, err := os.Open("config.json")     #2
     if err != nil {
         panic(err)
     }
     defer file.Close()
     decoder := json.NewDecoder(file)      #3
     conf := configuration{}               #3
     err = decoder.Decode(&conf)           #3
     if err != nil {
            fmt.Println("Error:", err)
     }
     fmt.Println(conf.Path)
  }
  ```
  
  
    
  
     #1 A type capable of holding the JSON values
     
  #2 Opens the configuration file
     
  #3 Parses the JSON into a variable with the variables
     
  
    
  
  
   
  
  
  
  
  
  
  
  
   
  
  This method has only a few parts. First, a type or collection of 
  types needs to be created via a struct representing the JSON file. The 
  names and nesting must be defined and mapped to the structure in the 
  JSON file. Because each field and property of this struct must be 
  accessible to a package, each must begin with an uppercase letter, which
  effectively makes the properties public. Linters will warn you if you 
  miss this requirement; otherwise, you may not notice when parts of your 
  structure aren’t unmarshaled properly. This is true not just of JSON 
  unmarshaling but also of any struct that must share properties with 
  another package. The main function begins by opening the configuration 
  file and decoding the JSON file into an instance of the configuration 
  struct. If no errors occur, the values from the JSON file will be 
  accessible in the `conf` variable and can be used in your application.
  
  
  
  
  
  
  
  
   
  
  Note  JSON parsing 
  and handling have many features and nuances. Chapter 11, which covers 
  working with JSON APIs, discusses JSON features in detail.
  
  
  
  
  
  
  
  
   
  
  TIP  Notice that we shadow the variable `err` the second time we assign it to the `Decode()`
  method’s return value. Error handling and bubbling in Go can be tricky 
  for users who are comfortable with other patterns, and in some cases, 
  you can ignore error return values by using the `_` blank identifier. It’s important to avoid getting into the habit of ignoring errors, though, and reassigning the `err` value or surrounding the code with `{` `...` `}`
  block scoping is a good way to handle errors without much boilerplate. 
  Chapter 4 talks more about error handling. Storing configuration in JSON
  is useful if you’re familiar with JSON, if you use a distributed 
  configuration store such as etcd, or if you want to stick with the Go 
  standard library. JSON files can’t contain comments, which is a common 
  complaint when developers are creating configuration files or generating
  examples of them. It can also be a little brittle because its structure
  is so rigid. But you can use other options.
  
  
  
  
  
  
  
  
   
  
  With the following two formats and techniques, you can use in-configuration comments to self-document the files better:
	- *Solution* —YAML (a recursive acronym that means *YAML Ain’t Markup Language*)
	  is a human-readable data serialization format. YAML is easy to read, 
	  can contain comments, and is fairly easy to work with. Using YAML for 
	  application configuration is common and a method that we recommend and 
	  use. Although Go doesn’t ship with a YAML processor, several third-party
	  libraries are readily available. You’ll look at one in this chapter.
	- *Discussion* —Consider this simple YAML configuration file: 
	  
	  
	  
	  
	  
	  
	  
	  
	  
	  
	  
	  
	  ```
	  # A comment line
	  enabled: true
	  path: /usr/local
	  ```
	  
	  
	  
	  
	  
	  
	  
	  
	  
	  
	  
	  
	  
	  The following listing is an example of parsing and printing configuration from a YAML file.
- ##### Listing 2.10  Parsing a YAML configuration file
  
   
  
  
    
  
  ```
  package main
  
  import (
     "fmt"
  
     "github.com/kylelemons/go-gypsy/yaml"      #1
  )
  
  func main() {
     config, err := yaml.ReadFile("conf.yaml")      #2
     if err != nil {
            fmt.Println(err)
        return
     }
    var path string
    var enabled bool
  
    path, err = config.Get("path")
    if err != nil {
        fmt.Println("`path` flag not set in conf.yaml", err)
        return
    }
  
    enabled, err = config.GetBool("enabled")
    if err != nil {
        fmt.Println("`enabled` flag not set in conf.yaml", err)
        return
    }
  
    fmt.Println("path", path)          #3
    fmt.Println("enabled", enabled)    #3
  
  }
  ```
  
  
    
  
     #1 Imports a third-party YAML parser
     
  #2 Reads a YAML file into a struct parser
     
  #3 Prints the values from the YAML file
     
  
    
  
  
   
  
  
  
  
  
  
  
  
   
  
  TIP  We are again 
  shadowing our error return value to avoid creating unnecessary 
  variables, but notice that we’re also returning early from `main`
  when an unrecoverable error occurs. This pattern is useful for avoiding
  boilerplate code, generating irrelevant errors, and dealing with `switch` and i`f`/`else` logic. We talk about error bubbling and handling, as well as the merits of early returns, in chapter 4.
  
  
  
  
  
  
  
  
   
  
  To work with YAML files and content, listing 2.10 imports the `github.com/kylelemons/ go-gypsy/yaml`
  package. This package, which we use and recommend, provides features 
  that read YAML as a string or from a file, deal with different types, 
  and turn configuration into YAML output.
  
  
  
  
  
  
  
  
   
  
  Using the function `ReadFile`, the configuration file is read in and returns a `File` struct from the `yaml` package. This struct provides access to the data in the YAML file. Using the `Get` method, we can obtain the value of a string. For our second value, `enabled`, we use the type-specific method `GetBool,`
  which coerces the type under the hood to a Boolean value. Other 
  type-specific methods return their respective coercions and return 
  types.
	- *Solution* —INI, a format
	  that’s been widely used for decades, is another format that your Go 
	  applications can use. Although the Go developers didn’t include a 
	  processor in the language, once again, libraries are readily available 
	  to meet your needs. Like YAML, INI has some readability advantages over 
	  JSON.
	- *Discussion* —Consider the following INI file: 
	  
	  
	  
	  
	  
	  
	  
	  
	  
	  
	  
	  
	  ```
	  ; A comment line
	  [Section]
	  enabled = true
	  path = /usr/local # another comment
	  ```
	  
	  
	  
	  
	  
	  
	  
	  
	  
	  
	  
	  
	  
	  In the following listing, this file is parsed, and you can see how to use the internal data.
- ##### Listing 2.11  Parsing an INI configuration file
  
   
  
  
    
  
  ```
  package main
  
  import (
    "fmt"
    "os"
  
    "github.com/go-ini/ini"                 #1
  
  )
  
  func main() {
    config, err := ini.Load("conf.ini")    
    if err != nil {
        fmt.Println(err)
        os.Exit(1)
    }
  
    fmt.Println(config.Section("Section").Key("path").String())  #2
    enabled, err := 
    config.Section("Section").Key("enabled").Bool() #3
    if err != nil {
        fmt.Println(err)
        os.Exit(1)
    }
    fmt.Println(enabled)     #4
  }
  ```
  
  
    
  
     #1 Includes third-party package to parse INI files
     
  #2 Prints the value of the "path" setting
     
  #3 Enables parsing while checking for the value’s existence
     
  #4 Prints the value of the "enabled" setting
     
  
    
  
  
   
  
  
  
  
  
  
  
  
   
  
  In this case, the third-party package `github.com/go-ini/ini`
  handles parsing the INI file into a data structure. This package 
  provides a means to parse INI files and strings similar to JSON handling
  in the standard library.
  
  
  
  
  
  
  
  
   
  
  Using the `config` value, the INI file’s sections and 
  respective values can be accessed via its getter function. In the case 
  of string data types, we get either a set value or Go’s default value 
  for the type. We access the value `path` within the INI section `Section` as a string value.
  
  
  
  
  
  
  
  
   
  
  When we attempt to read our Boolean value, though, we face a challenge. How do we know whether a value is explicitly `false` or simply falsey? In other words, does `false` mean that the value was set to `false`
  in the INI file or that it doesn’t exist? This library provides a few 
  methods for discerning the answer. In this case, we check for an error 
  if the value doesn’t exist; otherwise, we proceed as expected.
  
  
  
  
  
  
  
  
   
  
  The `github.com/go-ini/ini` package has useful 
  features, such as reading multiple INI files at the same time, providing
  default values for a type, and writing to INI files. For more 
  information, see the package documentation at [https://ini.unknwon.io](https://ini.unknwon.io).
  
  
  
  
  
  
  
  
   
  
  TIP  You may notice that although you’re again returning early from `main`
  if you encounter an unrecoverable problem, you’re also returning an OS 
  exit code. You can take multiple approaches to terminate a program early
  (such as `panic` or `log.Fatal`, as in chapter 
  1), and taking this one is useful when you want to distinguish between 
  error codes and have them logged properly at the OS level.
- ### 2.2.2  Configuration via environment variables
  
  
  
  
  
  
  
   
  
  The venerable configuration file certainly provides a great 
  vehicle for passing configuration data to programs. But some emerging 
  environments defy some of the assumptions we make about traditional 
  configuration files. Sometimes, the one configuring the application 
  doesn’t have access to the filesystem at the level we assume. Some 
  systems treat configuration files as part of the source codebase (and 
  thus as static pieces of an executable), which removes some of the 
  utility and flexibility of configuration files.
  
  
  
  
  
  
  
  
   
  
  Nowhere is this trend clearer than in emerging PaaS cloud 
  services. You usually deploy into these systems by pushing a source-code
  bundle to a control server (a `git` push, for example) into a
  continuous integration/continuous development (CI/CD) pipeline. But the
  only runtime configuration you get on such servers is done with 
  environment variables, eschewing config files deployed with containers. 
  Let’s take a look at a technique for working in such an environment.
	- *Problem* —Many PaaS 
	  systems don’t provide a way to specify per-instance configuration files.
	  Configuration opportunities are limited to a small number of 
	  environmental controls, such as environment variables. Containerized 
	  applications need these controls at build time. We’ll look at 
	  containerization techniques in chapter 12.
	- *Solution* —Today, 
	  12-factor apps, commonly deployed to Heroku, Amazon Web Services’ 
	  (AWS’s) Elastic Container Service, and other cloud equivalents, are 
	  becoming more common. One factor of a 12-factor app is storing the 
	  configuration in the environment, which provides a way to have a 
	  different configuration for each environment in which an application 
	  runs.
	- *Discussion* —Consider the environment variable `PORT`,
	  containing the port a web server should listen to. The following 
	  listing retrieves this piece of configuration and uses it when starting a
	  web server. 
	  
	  
	  
	  
	  
	  
	  
	  
	  
	  In the next listing, we show a common pattern for validating the 
	  existence of a critical environment variable and terminating if it 
	  doesn’t exist.
- ##### Listing 2.12  Environment variable-based configuration
  
   
  
  
    
  
  ```
  package main
  
  import (
    "fmt"
    "net/http"
    "os"
  )
  
  func main() {
    var port string
    if port = os.Getenv("PORT"); port == "" {              #1
    panic("environment variable PORT has not been set!")      #2
    }
    http.HandleFunc("/", homePage)
    http.ListenAndServe(":"+port, nil)      #3
  }
  
  func homePage(res http.ResponseWriter, req *http.Request) {
    if req.URL.Path != "/" {
        http.NotFound(res, req)
        return
    }
    fmt.Fprint(res, "The homepage.")
  }
  ```
  
  
    
  
     #1 Retrieves the PORT from the environment
     
  #2 Terminates if the environment variable isn’t set
     
  #3 Uses the specified port to start a web server
     
  
    
  
  
   
  
  
  
  
  
  
  
  
   
  
  This example uses the `http` package from the standard library. You may remember it from the simple `Hello` `World` web server in listing 1.15. We cover web servers in the next section.
  
  
  
  
  
  
  
  
   
  
  Retrieving configuration from the environment is fairly straightforward. From the `os` package, the `Getenv`
  function retrieves the value as a string. When no environment variable 
  is found, an empty string is returned. In this case, we choose to 
  terminate early by panicking if we haven’t set the value, preventing 
  unnecessary errors.
  
  
  
  
  
  
  
  
   
  
  Because all environment variables are technically stored and 
  retrieved as string values, you may need to convert the string to 
  another type. In this instance, you can use the `strconv` package. If the `PORT` in this example needed to be an integer, for example, you could use the `ParseInt` function.
  
  
  
  
  
  
  
  
   
  
  Warning  Be careful 
  with the information in environment variables and the processes that can
  obtain that information. A third-party subprocess started by your 
  application could have access to the environment variables, for example.
  It’s also good practice to namespace your configuration variables to 
  prevent conflicts. Instead of naming your target variable `PORT`, for example, you can call it `MYAPP_PORT`.
  When you’re building containerized apps, a build stage will help you 
  isolate these variables from the run stage. We look into this topic in 
  depth in chapter 12.
- ## 2.3  Working with real-world web servers
  
  
  
  
  
  
  
   
  
  Although the Go standard library provides a great foundation for 
  building web servers, you may want to change some options and add some 
  tolerances. Two common topics covered in this section are matching URL 
  paths to callback functions (known as *routing*) and starting and stopping servers with an interest in gracefully shutting down.
  
  
  
  
  
  
  
  
   
  
  Web servers are a core feature of the `http` package, which uses the `net`
  package as the foundation for handling TCP connections. Because web 
  servers are a core part of the standard library and are in common use, 
  we introduced simple web servers in chapter 1. This section moves beyond
  basic web servers to cover some practical gotchas in building 
  applications.
  
  
  
  
  
  
  
  
   
  
  NOTE  For more information on the `http` package, visit [https://pkg.go.dev/net/http](https://pkg.go.dev/net/http).
- ### 2.3.1  Starting up and shutting down a server
  
  
  
  
  
  
  
   
  
  Using the `net` or `http` package, you can 
  create a server, listen on a TCP port, and start responding to incoming 
  connections and requests. But what happens when you want to shut down 
  that server? What if you shut down the server while users are connected 
  or before all the data (such as logs and user information) has been 
  written to disk?
  
  
  
  
  
  
  
  
   
  
  The commands used to start and stop a server in the operating system should be handled by an initialization daemon. Using `go` `run`
  in a codebase is handy during development; you can use it with some 
  systems based on 12-factor apps, but this approach isn’t typical or 
  recommended. Starting an application manually is simple but isn’t 
  designed to integrate nicely with operations tools or to handle problems
  such as unexpected system restarts. Initialization daemons were 
  designed for these tasks and perform them well.
  
  
  
  
  
  
  
  
   
  
  Most operating systems have a default toolchain for initialization. The systemd tool ([https://www.freedesktop.org/wiki/Software/systemd](https://www.freedesktop.org/wiki/Software/systemd)),
  for example, is common in Linux distributions such as Debian and 
  Ubuntu. If systemd uses a script, you’ll be able to use commands like 
  those in the following listing.
- ##### Listing 2.13  Starting and stopping applications with upstart
  
   
  
  
    
  
  ```
  $ systemctl start myapp.service      #1
  $ systemctl stop myapp.service        #2
  ```
  
  
    
  
     #1 Starts the application myapp
     
  #2 Stops the running application myapp
     
  
    
  
  
   
  
  
  
  
  
  
  
  
   
  
  A wide variety of initialization daemons is available. The daemons
  vary depending on your flavor of operating system, and numerous ones 
  exist for various versions of Linux. You may be familiar with some of 
  their names, including upstart, init, supervisor, and launchd. Because 
  configuration scripts and commands vary widely among these systems, this
  book doesn’t cover them. These tools are well documented, and many 
  tutorials and examples are available.
  
  
  
  
  
  
  
  
   
  
  Note  We recommend 
  that you don’t run your applications as daemons; instead, use an 
  initialization daemon to manage the execution of the application as well
  as rules for service health and automatic restarts. Some reverse-proxy 
  web servers, such as NGINX, operate as daemons themselves. Although 
  NGINX offers a ton of features, Go’s built-in servers generally perform 
  without an overarching web server. So unless and until you need those 
  specific features, we recommend that you point a daemon directly to your
  service. Kubernetes and other cloud container orchestration tools work 
  like daemons without running them as standalone processes on the OS 
  level.
- ### 2.3.2  Graceful shutdowns using OS signals
  
  
  
  
  
  
  
   
  
  When a server shuts down, you’ll often want to stop receiving new 
  requests, save any data to disk, and end existing open connections 
  cleanly. The `http` package in the standard library shuts 
  down immediately and doesn’t provide an opportunity to handle any of 
  these situations. In the worst cases, this situation results in lost or 
  corrupted data:
	- *Problem* —To prevent data loss and unexpected behavior, a server may need to do some cleanup on shutdown.
	- *Solution* —To handle 
	  these, we can listen to OS-level signals and catch them in a context to 
	  ensure that we don’t drop existing connections. By choosing a reasonable
	  timeout, we can give existing connections a chance to complete before 
	  we close the server.
	- *Discussion* —Chapter 5 
	  digs deeper into concurrency. Here, we can combine our HTTP handlers 
	  with Go’s goroutines and OS signals to be smarter about how we stop our 
	  servers. Don’t worry too much about the details of the goroutine at this
	  point; we’ll get into the nitty-gritty in chapter 5. 
	  
	  
	  
	  
	  
	  
	  
	  
	  
	  To handle graceful shutdowns better, we can use a goroutine to 
	  manage a concurrent process. We can use that to listen to signals and do
	  other concurrent work, but in this case, we’ll arbitrarily stop our 
	  server with a timeout using the `Shutdown()` method. See the following listing.
- ##### Listing 2.14  More graceful shutdown using signals
  
   
  
  
    
  
  ```
  package main
  
  import (
    "fmt"
    "net/http"
    "os"
    "os/signal"
    "time"
  )
  
  func main() {
    handleFunc := newHandler()    #1
    server := &http.Server{       #1
        Addr:    “:8080",
        Handler: handleFunc,
    }
  
    ch := make(chan os.Signal, 1)               #2
    signal.Notify(ch, os.Interrupt, os.Kill)    #2
  
    go func() {
        server.ListenAndServe()     #3
    }()
  
    time.Sleep(5 * time.Second)                    #4
    <-ch                                          
    if err := server.Shutdown(nil); err != nil {
        panic(err)                                
    }
  }
  
  type handler struct{}
  
  func newHandler() *handler {
    return &handler{}
  }
  
  func (h *handler) ServeHTTP(res http.ResponseWriter, 
  req *http.Request) { #5
    query := req.URL.Query()
    name := query.Get("name")
    if name == "" {
        name = "Inigo Montoya"
    }
    fmt.Fprint(res, "Hello, my name is ", name)
  }
  ```
  
  
    
  
     #1 Gets instance of a server and handler
     
  #2 Sets up a channel to listen for specific signals
     
  #3 Starts the web server
     
  #4 On signal, adds a 5-second delay
     
  #5 Our handler
     
  
    
  
  
   
  
  
  
  
  
  
  
  
   
  
  The `main` function begins by getting an instance of a handler function capable of responding to web requests. This handler is a simple `Hello` `World`
  response method. In its place, you could use a more complex handler for
  routing rules, such as a path or regular expression handler (listing 
  2.17).
  
  
  
  
  
  
  
  
   
  
  To shut down gracefully, you need to know when it’s appropriate to do so. The `signal`
  package provides a means to get signals from the operating system, 
  including signals to interrupt or kill the application. The next step is
  setting up a concurrent channel that receives interrupt and kill 
  signals from the operating system so that the code can react to them. `ListenAndServe`, like its counterpart in the `http`
  package, blocks execution. To monitor signals, a goroutine needs to run
  concurrently. The channel waits until it receives a signal. After a 
  signal comes in, it sends a message to `Shutdown` on the 
  server. This message tells the server to stop accepting new connections 
  and shut down after all the current requests are completed. Calling `ListenAndServe` in the same manner as with the `http` package starts the server.
  
  
  
  
  
  
  
  
   
  
  Tip  The server waits
  only for request handlers to finish before exiting. If your code has 
  separate goroutines that need to be waited on, they would have to happen
  separately, using your own implementation of `WaitGroup`.
  
  
  
  
  
  
  
  
   
  
  This approach has several advantages:
	- It allows current HTTP requests 
	  to complete rather than stopping them mid-request. Although not 
	  guaranteed through this code, you could extend it to wait until there 
	  are no active connections to allow your signal channel listener to shut 
	  down the server.
	- It stops listening on the TCP 
	  port while completing the existing requests, opening the opportunity for
	  another application to bind to the same port and start serving 
	  requests. If you’re updating versions of an application, one version 
	  could shut down while completing its requests, and another version of 
	  the application could come online and start serving.
	- It ensures that any in-flight 
	  data processing (such as form/file uploads, multistatement database 
	  inserts, and file processing) is complete before the application exits. 
	  
	  
	  
	  
	  
	  
	  
	  
	  
	  One disadvantage exists under some conditions:
	- In some cases, one version of an
	  application wants to hand off exiting socket connections currently in 
	  use to another instance of the same application or another application. 
	  If you have long-running socket connections between a server and client 
	  applications, the shutdown will prevent this from ever happening.
- ### 2.3.3  Routing web requests
  
  
  
  
  
  
  
   
  
  One of the fundamental tasks of any HTTP server is to receive a 
  given request and map it to an internal function that can return a 
  result to the client. This routing of a request to a handler is 
  important; do it well, and you can build web services that are easy to 
  maintain and flexible enough to fit future needs. This section presents 
  various routing scenarios and solutions.
  
  
  
  
  
  
  
  
   
  
  We start with simple scenarios and solutions, but we encourage you
  to plan ahead. The simple solution we talk about first is great for 
  direct mappings but may not provide the flexibility that a contemporary 
  web application needs. Starting with version 1.22, the Go `http` library provides more flexibility for handling advanced routing.
- #### Matching paths to content
  
  
  
  
  
  
  
   
  
  Web applications and servers providing a REST API typically 
  execute different functionality for different path and request method 
  combinations. Figure 2.3 illustrates the path portion of the URL 
  compared with the other components. In the `Hello` `World` example from chapter 1, listing 1.15 uses a single function to handle all possible paths. For a simple `Hello` `World`
  application, this approach works. But a single function doesn’t handle 
  multiple paths well and doesn’t scale to real-world applications. This 
  section covers multiple techniques for handling distinct paths and, in 
  some cases, different HTTP methods, such as `GET`, `POST`, `PUT`, and `DELETE` (sometimes referred to as *verbs*).
  
  
  
  
  
  
  
  
   ![figure](https://learning.oreilly.com/api/v2/epubs/urn:orm:book:9781633436886/files/Images/2-3.png)
- ##### Figure 2.3  The path portion of the URL used in routing requests
	- *Problem* —To route 
	  requests correctly, a web server needs to be able to parse the path 
	  portion of a URL quickly and efficiently and direct incoming requests to
	  the proper response handler.
	- *Solution (multiple handlers)* —To
	  expand on the method used in listing 1.15, this technique uses a 
	  handler function for each path. This technique, presented in the guide 
	  Writing Web Applications ([https://go.dev/doc/articles/wiki](https://go.dev/doc/articles/wiki)),
	  uses a simple pattern that can be great for web apps with only a few 
	  simple paths. As you’ll see in a moment, this technique has nuances that
	  may make you consider it one of the most powerful techniques available.
	- *Discussion* —Let’s start with a simple program that uses multiple handlers, shown in the following listing. 
	  
	  
	  
	  
	  
	  
	  
	  
	  
	  In the next listing, we add a few more handlers and paths so we can direct the application’s logic based on the request.
- ##### Listing 2.15  Multiple handler functions:  `multiple_handlers.go`
  
   
  
  
    
  
  ```
  package main
  
  import (
    "fmt"
    "net/http"
    "strings"
  )
  
  func main() {
    http.HandleFunc("/hello", helloHandler)           #1
    http.HandleFunc("/goodbye/", goodbyeHandler)      #1
    http.HandleFunc("/", homePageHandler)             #1
    if err := http.ListenAndServe(":8080", nil); err != nil {
        panic(err)
    }                 #2
  }
  func helloHandler(res http.ResponseWriter, req *http.Request) {     #3
    query := req.URL.Query()          #4
    name := query.Get("name")         #4
    if name == "" {                   #4
        name = "Inigo Montoya"
    }
    fmt.Fprint(res, "Hello, my name is ", name)
  }
  func goodbyeHandler(res http.ResponseWriter, req *http.Request) {     #5
    path := req.URL.Path                      #6
    parts := strings.Split(path, "/")         #6
    name := parts[2]                          #6
    if name == "" {                           #6
        name = "Inigo Montoya"                #6
    }                                         #6
    fmt.Fprint(res, "Goodbye ", name) #6
  }
  func homePageHandler(res http.ResponseWriter, req *http.Request) {     #7
    if req.URL.Path != "/" {                 #8
        http.NotFound(res, req)              #8
        return                               #8
    }                                        #8
    fmt.Fprint(res, "The homepage.")
  }
  ```
  
  
    
  
     #1 Registers URL path handlers
     
  #2 Attempts to start the web server on port 8080 or quits if an error occurs
     
  #3 Handler function mapped to /hello
     
  #4 Gets the name from the query string
     
  #5 Handler function for /goodbye/
     
  #6 Looks in the path for a name
     
  #7 Home and not found handler function
     
  #8 Checks the path to decide whether the page is the home page or not found
     
  
    
  
  
   
  
  
  
  
  
  
  
  
   
  
  Note  Content 
  collected from an end user should be sanitized before using it. That 
  includes displaying the content back to a user because, in some cases, 
  unsanitized user content injected into a response could deliver a 
  malicious JavaScript payload. This functionality is part of the 
  templating Go package covered in chapter 9.
  
  
  
  
  
  
  
  
   
  
  Here, you use three handler functions for three paths or path 
  parts. When a path is resolved, it tries to direct traffic from the most
  specific to the least specific. In this case, any path that isn’t 
  resolved before the `/` path resolves to this one.
  
  
  
  
  
  
  
  
   
  
  The handler function `helloHandler` is mapped to the path `/hello`. As arguments, the handler functions receive an `http.ResponseWriter` and an `http.Request`. In this route, an optional name to say hello to can be in a query string with a key of `name`. The requested URL is a property on `http.Request` of the type `url.URL`. The `Query`
  method on the URL returns either the value for the key or an empty 
  string if no value is available for the key. If the value is empty, it’s
  set to `Inigo` `Montoya`.
  
  
  
  
  
  
  
  
   
  
  Tip  The `net/url` package, which contains the URL type, has many useful functions for working with URLs.
  
  
  
  
  
  
  
  
   
  
  Note  To differentiate among HTTP methods, check the value of `http.Request .Method`, which contains the method (`GET`, `POST`, and so on).
  
  
  
  
  
  
  
  
   
  
  The `goodbye` function handles the path `/goodbye/`,
  including the case in which additional text is appended. In this case, a
  name can optionally be passed in through the path. A path of `/goodbye/Buttercup`, for example, sets the name to `Buttercup`. You split the URL by using the `strings` package to find the part of the path following `/goodbye/`.
  
  
  
  
  
  
  
  
   
  
  The `homePage` function handles both the `/` path and any case in which a page isn’t found. To decide whether to return a `404` (`page` `not` `found`) message or home page content, check the `http.Request.Path`. The `http` package contains a `NotFound` helper function that can be used to set the response HTTP code to `404` and send the text `404` `page` `not` `found`.
  
  
  
  
  
  
  
  
   
  
  Tip  The `http` package contains the `Error` function, which you can use to set the HTTP error code and respond with a message. The `NotFound` function takes advantage of this for the 404 case.
  
  
  
  
  
  
  
  
   
  
  Using multiple function handlers is the core way to handle 
  different functions alongside different paths. The pros of this method 
  include the following:
	- As the basic method in the `http` package, it’s well documented and tested, and examples are right at hand. It requires no additional libraries or dependencies.
	- The paths and their mappings to 
	  functions are easy to read and follow. Keep in mind that these functions
	  can also be written inline, provided that they’re small enough to 
	  maintain readability. 
	  
	  
	  
	  
	  
	  
	  
	  
	  
	  Additions to the `net/http` library in Go version 1.22 largely invalidated some concerns about earlier routing capabilities, including the following:
	- Previously, there was no way to differentiate routes by HTTP method. When you create REST APIs, the verb (such as `GET`, `POST`, or `DELETE`)
	  often requires significantly different functionality, which forces a 
	  lot of boilerplate code in addition to your handler deciding what to do 
	  with each respective method.
	- Virtually every handler function needed to check for paths outside their bounds, returning a `Page` `Not` `Found` message. In listing 2.15, the handler `/goodbye/` receives paths prepended with `/goodbye`. Anything returned by any path is handled here, so if you want to return a `Page` `Not` `Found` message for the path `/goodbye/foo/bar/baz`,
	  that task needs to be addressed inside each handler. Changes in 
	  precedence and URL variables after Go version 1.22 have expanded options
	  in these handlers, which previously required a third-party routing/ 
	  muxing library. 
	  
	  
	  
	  
	  
	  
	  
	  
	  
	  Using multiple handlers is useful in simple cases. Because this approach doesn’t require packages outside the `http`
	  package, the external dependencies are kept to a minimum. If an 
	  application is going to move beyond simple use cases, one of the 
	  following techniques is likely to be a better fit, providing more 
	  advanced routing options.
- #### Handling complex paths with wildcards
  
  
  
  
  
  
  
   
  
  The preceding technique is straightforward, but as you saw, it’s 
  decidedly inflexible in path naming. You must list every single specific
  path that you expect to see. For larger applications or for 
  applications that follow the REST recommendations, you need a more 
  flexible solution.
	- *Problem* —Instead of specifying exact paths for each callback, an application may need to support wildcards or other `URL` patterns to respond accurately from the server.
	- *Solution* —Go provides 
	  named variables in its routes as well as more specific precedence. These
	  allow you to extract a variable from a path without needing 
	  deduplication logic inside your handlers.
	- *Discussion* —The following listing builds a router that uses path matching to map URL paths and HTTP methods to a handler function. 
	  
	  
	  
	  
	  
	  
	  
	  
	  
	  The next listing shows how to use patterns inside HTTP requests to
	  route as well as maintain a value for use within your handler.
- ##### Listing 2.16  Resolving URLs using  `path`  package
  
   
  
  
    
  
  ```
  package main
  
  import (
    "fmt"
    "net/http"
  )
  
  func main() {
    mux := http.NewServeMux()     #1
  
    mux.HandleFunc("/hello", helloHandler)                   #2
    mux.HandleFunc("GET /goodbye/", goodbyeHandler)          #2
    mux.HandleFunc("GET /goodbye/{name}", goodbyeHandler)    #2
  
    if err := http.ListenAndServe(":8084", mux); err != nil {    #3
        panic(err)
    }
  }
  
  func helloHandler(res http.ResponseWriter, req *http.Request) {
    query := req.URL.Query()                           #4
    name := query.Get("name")                          #4
    if name == "" {
        name = "Inigo Montoya"
    }
    fmt.Fprint(res, "Hello, my name is ", name)
  }
  
  func goodbyeHandler(res http.ResponseWriter, req *http.Request) {
    name := req.PathValue("name")             #5
    if name == "" {                           #5
        name = "Inigo Montoya"                 #6
    }
    fmt.Fprint(res, "Goodbye ", name)   #7
  }
  ```
  
  
    
  
     #1 Creates a new multiplexer (mux)
     
  #2 Adds various endpoints to handle
     
  #3 Starts the server or panics if we cannot start it
     
  #4 Takes the name variable from the query string, if available, or uses a default
     
  #5 Accepts an optional path parameter; if available, uses that parameter as name . . .
     
  #6 . . . otherwise, uses the default
     
  #7 Outputs the name as a response
     
  
    
  
  
   
  
  
  
  
  
  
  
  
   
  
  Here, we’ve created a multiplexer (mux) that allows us to route 
  requests properly. Note that in the second path, we include an optional `HTTP` method, which allows us to segment requests by method. This method is useful for following RESTful patterns.
  
  
  
  
  
  
  
  
   
  
  The precedence of a router/multiplexer is often based on a few 
  common patterns. Some patterns don’t allow overlapping routes, so they 
  would supercede our `/goodbye/` endpoints. Other patterns go 
  with last-in precedence. Another common pattern is longest-match-wins, 
  which is what Go chose. In our example, without two endpoints, we’d have
  to do more advanced logic inside the handler to pull our request apart.
  Using the path value gives us quick access to that value.
  
  
  
  
  
  
  
  
   
  
  The Go multiplexer handles more than just these patterns, and 
  we’ll dig a bit deeper into them in chapters 8 and 9. In some cases, 
  however, you need more complex routing. Although it’s not always 
  advisable to do so due to performance optimizations that other libraries
  have made, let’s walk through building our own simple muxer to allow 
  routing via regular expressions. This use case is not necessarily 
  common; it’s more of a demonstration.
  
  
  
  
  
  
  
  
   
  
  You should be aware of the pros and cons of path resolution when 
  using a custom multiplexer over the built-in. Here are the pros of 
  sticking with the standard `http` version:
	- It’s easy to get started with simple path matching.
	- The `path` package is rock-steady, being part of Go’s standard library.
	- Performance will likely exceed that of using pattern matching such as regular expressions. 
	  
	  
	  
	  
	  
	  
	  
	  
	  
	  The cons have a common thread: the `path` package is generic to paths and not specific to URL paths. The cons are as follows:
	- The wildcard capabilities of the `path` package are limited. A path of `foo/{bar}` will match `foo/bar` but not `foo/{bar}/baz`, for example. When `*` is used as a wildcard, it stops at the next `/`. To match `foo/bar/baz`, you’d need to look for a path like `foo/*/*`.
	- Because this `path` 
	  package is generic rather than specific to URLs, some nice-to-have 
	  features are missing, including redirects on bare/path endings. 
	  
	  
	  
	  
	  
	  
	  
	  
	  
	  This method is useful for simple path scenarios with a few routes.
- #### URL pattern matching
  
  
  
  
  
  
  
   
  
  For most REST-style apps, simple pattern matching with regular 
  expressions is more than sufficient. But what if you want to do 
  something fancy with your URLs? The `path` package isn’t well suited to this purpose because it supports only simple POSIX-style pattern matching.
	- *Problem* —Simple 
	  path-based matching isn’t enough for an application that needs to treat a
	  path more like a text string and less like a file path. This is 
	  particularly important for matching across a path separator (`/`).
	- *Solution* —The built-in 
	  path package enables simple path-matching schemes, but sometimes, you 
	  need to match complex paths or have detailed control of the path. In 
	  those cases, you can use regular expressions to match your paths. You’ll
	  combine Go’s built-in regular expressions with the HTTP handler and 
	  build a fast but flexible URL path matcher.
	- *Discussion* —In the next listing, you walk through using paths and a resolver based on regular expressions. 
	  
	  
	  
	  
	  
	  
	  
	  
	  
	  The following listing shows another approach to routing that some 
	  of the frameworks employ to route based on regular expressions. It’s not
	  something that should be done trivially, as there are some downsides 
	  (including security), though we want to demonstrate how built-in routing
	  can be extended to be more expressive.
- ##### Listing 2.17  Resolving URLs using regular expressions
  
   
  
  
    
  
  ```
  package main
  
  import (
    "fmt"
    "net/http"
    "regexp"      #1
    "strings"
  )
  
  func main() { #2
    rr := newPathResolver() #2
    rr.Add("GET /hello", helloHandler)                                  #2
    rr.Add("(GET|HEAD) /goodbye(/?[A-Za-z0-9]*)?", goodbyeHandler)     
    if err := http.ListenAndServe(":8080", rr); err != nil {
        panic(err)
    }
  }
  func newPathResolver() *regexResolver {
    return &regexResolver{
        handlers: make(map[string]http.HandlerFunc),
        cache:    make(map[string]*regexp.Regexp),
        cachedHandlers: make(map[string]http.HandlerFunc),
    }
  }
  
  type regexResolver struct {
    handlers map[string]http.HandlerFunc
    cache    map[string]*regexp.Regexp          #3
    cachedHandlers map[string]http.HandlerFunc         #4
  }
  
  func (r *regexResolver) Add(regex string, handler http.HandlerFunc) {
    r.handlers[regex] = handler
    cache, _ := regexp.Compile(regex)
    r.cache[regex] = cache                                               #5
  }
  func (r *regexResolver) ServeHTTP(res http.ResponseWriter, req *http.Request) {
    check := req.Method + " " + req.URL.Path                            
  
    if handlerFunc, ok := r.cachedHandlers[check]; ok {    #6
        handlerFunc(res, req)
        return
    }
  
    for pattern, handlerFunc := range r.handlers {         #7
        if r.cache[pattern].MatchString(check) == true {   #7
            handlerFunc(res, req)
            r.cachedHandlers[check] = handlerFunc
            return
        }
    }
    http.NotFound(res, req)      #8
  }
  func helloHandler(res http.ResponseWriter, req *http.Request) {
    query := req.URL.Query()
    name := query.Get("name")
    if name == "" {
        name = "Inigo Montoya"
    }
    fmt.Fprint(res, "Hello, my name is ", name)
  }
  func goodbyeHandler(res http.ResponseWriter, req *http.Request) {
    path := req.URL.Path
    parts := strings.Split(path, "/")
    name := ""
    if len(parts) > 2 {
        name = parts[2]
    }
    if name == "" {
        name = "Inigo Montoya"
    }
    fmt.Fprint(res, "Goodbye ", name)
  }
  ```
  
  
    
  
     #1 Imports the regular expression package
     
  #2 Registers paths to functions
     
  #3 Stores compiled regular expressions for reuse
     
  #4 If no path matches, returns a Page Not Found error
     
  #5 Sets the method lookup key
     
  #6 If in our cached handlers, skips the loop . . .
     
  #7 
     **. . . otherwise, finds a candidate and sets as cache if it’s found**
     
  #8 If no path matches, returns a Page Not Found error
     
  
    
  
  
   
  
  
  
  
  
  
  
  
   
  
  The layout of the regular expression-based path resolution 
  (listing 2.17) is the same as the path-resolution example (listing 
  2.16). The differences lie in the format of the path patterns registered
  for a function and in the `ServeHTTP` method handling the resolution.
  
  
  
  
  
  
  
  
   
  
  Paths are registered as regular expressions. The structure is the same as in the `net/http` example, with an `HTTP` method followed by the path, separated by a space. Whereas `GET` `/hello` showcases a simple path, a more complicated example is `(GET|HEAD)` `/goodbye(/?[A-Za-z0-9]*)?`. This more complicated example accepts either a `GET` or a `HEAD` `HTTP` method. The regular expression for the path accepts `/goodbye`, `/goodbye/` (the trailing `/` matters), and `/goodbye/`
  followed by letters and numbers. Keep in mind that the order of slices 
  and maps isn’t deterministic, so partial matches could be found before 
  longer, more comprehensive ones.
  
  
  
  
  
  
  
  
   
  
  In this case, `ServeHTTP` first generates our route as a
  lookup key. Then it checks whether we previously saw and resolved this 
  key. If so, we avoid the inefficiency of the loop and return the 
  handler; if not, the method iterates over the regular expressions 
  looking for a match. When the first match is found, the method executes 
  the handler function registered to that regular expression and set to 
  cache our path lookup to its respective handler. If more than one 
  regular expression matches an incoming path, the first one added is the 
  first one checked and used.
  
  
  
  
  
  
  
  
   
  
  Note  Compiled versions of the regular expressions are built and cached at the time they’re added. Go provides a `Match` function in the `regexp`
  package that can check for matches. The first step of this function 
  compiles the regular expression. When you compile and cache the regular 
  expression, you don’t need to recompile it each time the server handles a
  request.
  
  
  
  
  
  
  
  
   
  
  Using regular-expression checking for paths provides a significant
  amount of power, allowing you to fine-tune the paths you want to match.
  This flexibility is paired with the complicated nature of regular 
  expressions that may not be easy to read, and you’ll likely want to have
  tests to make sure your regular expressions match the proper paths. Go 
  also comes with a `path/filepath` package that can help you reduce complexity a bit by parsing a path, but it requires more high-level/generic routing to use.
  
  
  
  
  
  
  
  
   
  
  By adding a layer of cache, we keep this method a bit more 
  lightweight, but notice that we don’t save our cache misses to a handler
  cache. Each approach has tradeoffs, but assuming a relatively small 
  amount of paths, this prevents allocating growing memory for invalid 
  routes, allowing us to avoid introducing a potential denial-of-service 
  vector.
- #### Faster routing (without the work)
  
  
  
  
  
  
  
   
  
  One criticism of Go’s built-in `http` package is that 
  its routing and multiplexing (muxing) is basic. In the preceding 
  sections, we showed you some straightforward ways of working with the `http`
  package, but depending on your needs, you may not be satisfied with the
  configurability, performance, or capabilities of the built-in HTTP 
  server. Or you may want to avoid writing boilerplate routing code.
	- *Problem* —The built-in `http` package isn’t flexible enough or doesn’t perform well in a particular use case.
	- *Solution* —Routing URLs 
	  to functions is a frequent problem for web applications. Therefore, 
	  numerous packages have been built and tested and are commonly used to 
	  tackle the problem of routing. A widespread technique is to import an 
	  existing request router and use it within your application. 
	  
	  
	  
	  
	  
	  
	  
	  
	  
	  Popular solutions include the following:
	- `github.com/julienschmidt/httprouter`
	  is considered a fast routing package with a focus on using minimal 
	  memory and taking as little time as possible to handle routing. It has 
	  features such as using case-insensitive paths, cleaning up `/../` in a path, and dealing with an optional trailing `/`.
	- `github.com/gorilla/mux` is part of the Gorilla web toolkit. This loose collection of packages provides components you can use in an application. The `mux`
	  package provides a versatile set of criteria to perform matching 
	  against, including host, schemes, HTTP headers, and more. Gorilla was 
	  recently revived from deprecation and has a long history.
	- `github.com/gin-gonic/gin` is a framework similar to Gorilla that focuses on performance, particularly in its zero-allocation router.
	- `github.com/bmizerany/pat`
	  provides a router inspired by the routing in Sinatra. The registered 
	  paths are easy to read and can contain named parameters such as `/user/:name`. It inspired packages such as `github.com/gorilla/pat`.
- ##### Sinatra web application library
  
   
  
  
   
  
  
    
  
  Sinatra is an open source web application framework written in 
  Ruby. This framework is used by numerous organizations and has inspired 
  more than 50 frameworks in other languages, including several in Go.
  
  
   
  
  
  
  
  
  
  
  
   
  
  Each package has a different feature set and API. Numerous other 
  routing packages exist as well. With a little investigation, you can 
  easily find a quality third-party package that meets your needs. 
  Although improvements to the Go library have brought support for HTTP 
  method-based routing and path variables, there’s more complexity that 
  some websites might benefit from. Some outperform Go’s, which is 
  important if you find yourself in the enviable position of needing that 
  sort of scale.
- ## Summary
	- Command-line options can be 
	  handled in a comfortable and accessible manner, ranging from lightweight
	  solutions to a simple framework for building console-based applications
	  and utilities.
	- You can retrieve configuration information from files and environment variables in different ways using various data formats.
	- Starting and stopping a web 
	  server that works with ops tooling and graceful shutdowns will prevent a
	  bad user experience or loss of data.
	- You have several ways to resolve URL paths for an application and route to handler functions.