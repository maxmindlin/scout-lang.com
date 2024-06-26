+++
title = "Fundamentals"
description = "Overview of the ScoutLang fundamentals."
date = 2021-05-01T08:20:00+00:00
updated = 2021-05-01T08:20:00+00:00
draft = false
weight = 30
sort_by = "weight"
template = "docs/page.html"

[extra]
lead = 'Overview of the Scout programming language fundamentals.'
toc = true
top = false
+++

## Comments

Scout comments are started with `//` and ended by newlines.

```
// I'm a comment

x = 1 // And so am I!
```
## Imports

In Scout the `use` keyword will read another `.sct` file and import it:

```
use lib
```

Currently scout looks for these files from the working directory. So in this example `lib.sct` must be `<WORKING_DIR>/lib.sct`.

You can import files from folders as you would expect:

```
use lib::index // <WORKING_DIR>/lib/index.sct
```

Importing a directory will import all modules within that directory.

You can then access members of an imported module via the `::` operator:

```
use std::keys
use std

enter = keys::ENTER

keys::press(enter)

links = std::utils::links()
```

## Command-line Args

ScoutLang comes with builtin support for reading script args:

```
>> scout file.sct hello world!

// file.sct

args() == ["file.sct", "hello", "world!"]
```

## Crawl Statements

Scout's `crawl` statement allows you to execute a depth-first search crawl through the website, following URLs as it finds them on each page. A simple crawl statement like so will find and visit every single link and print the URL it lands on:

```
crawl do
  url() |> print()
end
```

The crawl statement has additional options to bind and filter the URLs it finds in the format:

```
crawl <link>, <depth> where <expression> do
  <block>
end
```

The `link` and `depth` binds the found URL and current crawl depth to identifiers and executes the `expression`. Only if the `expression` evaluates to true will the `block` be evaluated. For example:

```

// For every URL found, only visit it if it contains
// the string "twitter.com" and we are less than 5 visited URLs deep. When a URL is visited,
// simply print it out.
crawl link, depth where link |> contains("twitter.com") and depth < 5 do
  print(link)
end
```

Crawl statements are basically depth-first searches of websites and can be used to create powerful web crawling scripts with very few lines of code.


## Selects

Select expressions are one of the key elements of Scout. There are two variants:
- single selects: `$"<css selector>"`
  - returns a single `Node`
- multi-selects: `$$"<css selector>"`
  - returns a list of `Node`

Selects can also be scoped to a specific `Node`. When scoped, the select statement will only search children of that node scope.

```
scope = $".my-class"

for link in $$(scope)"a" do
  link |> href() |> print()
end
```

Scoping selects is often useful when looping through elements and you want to group data together:

```
for row in $$"table tr" do
  scrape {
    text: row |> textContent(),
    link: $(row)"a" |> href()
  }
end
```

Selects will be the main way you access elements on the webpage and are parameters for many of the builtin standard library functions.

## Pipes

A common operator you will see in Scout scripts is the `pipe`. A `pipe` allows you to "pipe" expressions into the next expression, like a chain. When piped, the result of an expression is inserted as the first argument to the following function call. This can be chained as many times as needed.

A nested function call like so:

```
print(contains(href($"a"), ".com"))
```

would be equivalent to the following expression pipe:

```
$"a"
  |> href()
  |> contains(".com")
  |> print()
```

Pipes tend to produce more readable code instead of nested function calls, but both are valid Scout code!

# Control Flow

## If/Else

An if/else statement is started by the `if` keyword. Following `elif` and `else` cases are optional. However, like in other languages, if an `else` is placed before an `elif` then that `elif` will be unreachable.

```
if <condition> do
  <block>
elif <condition> do
  <block>
else
  <block>
end
```

## Try-Catch

Scout has simple try catches that work as you might expect:

```
try
  <block>
catch
  <block>
end
```

The `catch` is optional, you can try without an explicit catch:

```
try
  <block>
end

// This is equivalent to:

try
  <block>
catch
end
```


# Functions

Like most programming languages, Scout has user-defined functions. They take the format:

```
def <identifier>([parameters]) do
  <content>
end
```

A function to return all the links at a given url could look like so:

```
def links(url) do
  goto url
  $$"a" |> href()
end

"https://google.com"
  |> links()
  |> print() // would print all the URLs on the Google homepage
```

Scout functions return the evaluation of the final expression - no need for a return keyword. Like other languages, you can use a return statement to return early:

```
def links(url) do
  if url == "https://mysite.com" do
    return null
  end

  goto url
  $$"a" |> href()
end
```

## Standard Library

The scout standard library gets installed alongside the interpreter when installed via the python script `installer.py`. By default it is installed at `$HOME/scout-lang/scout-lib/`. You can access it via the `std` module:

```
use std
use std::keys
use std::utils
```

The entire library can be explored in the ScoutLang [repository](https://github.com/maxmindlin/scout-lang/tree/main/scout-lib).


## Builtin Functions

- `print(Object, ...) -> Null`
  - Will print each object to stdout that is provided as parameters. Any number of objects can used as parameters.

- `textContent(Node) -> Str`
  - Returns the text content of a `Node` as a `Str`.

- `href(Node) -> Str`
  - Returns the `href` attribute of a provided `Node`.

- `trim(Str) -> Str`
  - Removes leading and trailing whitespace from a provided `Str` and returns a new `Str`.

- `click(Node) -> Null`
  - Executes a click action on the provided `Node`.

- `results() -> Null`
  - Prints out the current result state.

- `len(List | Str) -> Number`
  - Returns the number of elements in a provided `List` or the number of characters in a provided `Str`.

- `input(Node, Str, Object) -> Null`
  - Inputs a provided string to a provided input element. If the `Object` parameter is truthy, then a `return` key action will be executed - defaults to `false`.

- `contains(List | Str, Object) -> Boolean`
  - Returns whether or not the given object is in the list, or if a given substring is in the provided `Str`.

- `url() -> Str`
  - Returns the current URL.

- `number(Str) -> Number`
  - Parses a string to a Number.

- `args() -> List[Str]`
  - Returns an array containing the command line args.

- `sleep(Number) -> Null`
  - Sleeps for a provided number of milliseconds.
