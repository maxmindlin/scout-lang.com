+++
title = "Results"
description = "Overview of the ScoutLang results."
date = 2021-05-01T08:20:00+00:00
updated = 2021-05-01T08:20:00+00:00
draft = false
weight = 50
sort_by = "weight"
template = "docs/page.html"

[extra]
lead = "Understanding and utilizing scrape results."
toc = true
top = false
+++

Scout is opinionated about how to keep track of your web crawls and scrape results. As a result, you don't have to worry about schema or keep track of deeply nested objects in your scripts.

At the end of a script call Scout outputs the results as JSON to stdout. This can then be parsed by any JSON parser of your choice.

## Usage

Every time you call the `scrape` keyword, Scout appends your payload to the URL it was retrieved from. This allows you to keep track of where results were scraped, as well as allowing you to logically group data into objects as you choose.

## Example

Below is an example Scout script that goes to the ScoutLang GitHub page and scrapes the top level file structure:

```
goto "https://github.com/maxmindlin/scout-lang"

for row in $$"table tr td[colspan='1']" do
    scrape {
        text: row |> textContent(),
        link: $(row)"a" |> href(),
    }
end
```

And here is the result payload:

```JSON
{
  "results": {
    "https://github.com/maxmindlin/scout-lang": [
      {
        "link": "https://github.com/maxmindlin/scout-lang/tree/main/.github/workflows",
        "text": ".github/workflows"
      },
      {
        "link": "https://github.com/maxmindlin/scout-lang/tree/main/assets",
        "text": "assets"
      },
      {
        "link": "https://github.com/maxmindlin/scout-lang/tree/main/examples",
        "text": "examples"
      },
      {
        "link": "https://github.com/maxmindlin/scout-lang/tree/main/scout-interpreter",
        "text": "scout-interpreter"
      },
      {
        "link": "https://github.com/maxmindlin/scout-lang/tree/main/scout-lexer",
        "text": "scout-lexer"
      },
      {
        "link": "https://github.com/maxmindlin/scout-lang/tree/main/scout-parser",
        "text": "scout-parser"
      },
      {
        "link": "https://github.com/maxmindlin/scout-lang/tree/main/src",
        "text": "src"
      },
      {
        "link": "https://github.com/maxmindlin/scout-lang/blob/main/.gitignore",
        "text": ".gitignore"
      },
      {
        "link": "https://github.com/maxmindlin/scout-lang/blob/main/Cargo.lock",
        "text": "Cargo.lock"
      },
      {
        "link": "https://github.com/maxmindlin/scout-lang/blob/main/Cargo.toml",
        "text": "Cargo.toml"
      },
      {
        "link": "https://github.com/maxmindlin/scout-lang/blob/main/LICENSE-APACHE",
        "text": "LICENSE-APACHE"
      },
      {
        "link": "https://github.com/maxmindlin/scout-lang/blob/main/LICENSE-MIT",
        "text": "LICENSE-MIT"
      },
      {
        "link": "https://github.com/maxmindlin/scout-lang/blob/main/README.md",
        "text": "README.md"
      }
    ]
  }
}
```
