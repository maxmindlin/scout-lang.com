+++
title = "Standard Library"
description = "Overview of the Scout standard library"
date = 2021-05-01T08:20:00+00:00
updated = 2021-05-01T08:20:00+00:00
draft = false
weight = 20
sort_by = "weight"
template = "docs/page.html"
+++

Overview of what comes standard in the Scout programming language

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
