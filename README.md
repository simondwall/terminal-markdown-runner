# Terminal-Markdown-Runner

This is a project for making markdown files more interactive.
**This project is under development and is not functional as of right now.**

## Usage:

### Command line interface:

#### Executing code:

Run a specific code block or all code block under a heading (until heading of higher or same level)
```sh
terminal-markdown-runner run <markdown file> <cell id | heading name>
```

#### Interoperation with jupyter notebooks:

Export a markdown file into a .ipynb file
```sh
terminal-markdown-runner export <markdown file> [-o <path>]
```

Import a .ipynb file to a markdown file
```sh
terminal-markdown-runner import <.ipynb file> [-o <path>]
```

#### Literate programming:

Snips all code blocks for a given file together and saves it at the location given in the markdown
```sh
terminal-markdown-runner snip <markdown file> <source file>
```

#### Session Management:

Lists all active kernels (sessions)
```sh
terminal-markdown-runner session list
```

Stops a kernel (session)
```sh
terminal-markdown-runner session stop <session id | 'all'>
```

### Markdown extension:

#### Fenced code blocks:

- `language` can be set as usual
- `id` setting an identifier to be able to run this specific cell (instead of only running everything under a heading)
- `sesstion` lets you run multiple sessions of the same language in parallel without them interfering each other
- `file` determines where this code will be put when snipping (can be used with a number to write code out of order)

````markdown
```<language> [id=<cell id>] [session=<session id>] [file=<path>[:<number>]]
    code
```
````

## Possible future features:

- Allowing to copy content of code block with special token (e.g. `[content](<cell id>)`)
- WASM interoperation between code to input result of one cell to another (Would depend on interface types if they ever happen and then only maybe this would work)
- Update tables and maybe other parts of markdown with code blocks
- Giving arguments to code in fenced code block head (e.g. `[var <var name> = <value>]`)
- Read arguments from tables and other parts of markdown into code block
- Use lua for config and scripting if there are good use cases

## Architecture:

- Parse cli arguments with clap
- Maybe own markdown parser (for more control?)
- Run Jupyter kernels in docker containers
    - Set default kernel when building image (thus data in /api/kernels POST is not needed and does not need to be obtained)
- Use Jupyter Rest API for communication with kernels
    - Start a default kernel `curl -X POST localhost:8888/api/kernels -H "Authorization: Token <TOKEN>"'`
    - Connect with a WebSocket to send code and receive results
- Use a yaml config (serde)
    - Set docker image for a given language (different images could run different jupyter kernel implementations of the same language)
