### Simple CLI for your Go programs

[![Go](https://img.shields.io/badge/Go-1.6-blue.svg)](https://golang.org/) [![Build Status](https://travis-ci.org/noroutine/go-cli.svg?branch=master)](https://travis-ci.org/noroutine/go-cli)

Simple readline-based CLI for Go programs, that probably has lots of bugs

### How to use 

1. Initialize

    ```
    repl := cli.New()
    repl.Description = "High Orbit Satellite Control"
    repl.Prompt = "> "
    ```

2. Some initial guidance

    ```
    repl.EmptyHandler = func() {        
        fmt.Println("Feeling lost? Try 'help'")
        repl.EmptyHandler = nil
    }
    ```

3. Help screen
    ```
    repl.Register("help", func(args []string) {
        fmt.Printf("Commands: %s\n", strings.Join(repl.GetKnownCommands(), ", "))
    })
    ```

4. Finally launch the machinery 

    ```
    repl.Serve()
    ```

### How do I handle SIGINT (Ctrl+C) and other signals?

As simple as adding somehing like this to your code

    go func() {
        for s := range repl.Signals {
            if s == os.Interrupt {
                log.Printf("Interrupted")
                os.Exit(42)
            }
        }
    }()