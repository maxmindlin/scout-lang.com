+++
title = "Quick Start"
description = "One page summary of how to start using Scout."
date = 2021-05-01T08:20:00+00:00
updated = 2021-05-01T08:20:00+00:00
draft = false
weight = 20
sort_by = "weight"
template = "docs/page.html"

[extra]
lead = "One page summary of how to start using Scout."
toc = true
top = false
+++

## Requirements

Scout currently requires the FireFox browser and [Geckodriver](https://github.com/mozilla/geckodriver) installed.

## Installation

Simply download and run the installation script. It will determine the appropriate binary for your operating system.

### Step 1: Download and run installation script:

```bash
curl --proto '=https' --tlsv1.2 -LsSf https://github.com/maxmindlin/scout-lang/releases/download/v0.2.0/scout-installer.sh | sh
```

### Step 2: Explore the REPL:

The REPL opens a debugging browser window and lets you execute crawling scripts line by line. Its a great place to explore the Scout language and test run your crawling scripts.

```bash
scout
```

### Step 3: Install the VSCode extension (if applicable):

ScoutLang maintains a VSCode [extension](https://marketplace.visualstudio.com/items?itemName=ScoutLang.scout-lang-vscode).

### Step 4: Create your first Scout file:

Now lets create your first Scout file

```bash
touch my-first-file.sct
```

and add this content to the file:

```
goto "https://github.com/maxmindlin/scout-lang"
about = $".f4.my-3"
about |> textContent() |> trim() |> print()
```

Then run it!

```bash
scout my-first-file.sct
```

You should see the `about` section printed out in your terminal, along with a results json object that we will cover later:

```
A web crawling programming language
```
