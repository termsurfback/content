
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>

<h3 align='center'>content</h3>
<p align='center'>
  Grammars for parsing and generating arbitrary strings, binaries, or other content in XO
</p>

<p align='center'>
  <a href='#summary'>Summary</a> •
  <a href='#contribute'>Contribute</a> •
  <a href='#license'>License</a>
</p>

<br/>
<br/>
<br/>

### Summary

A grammar is the first step in parsing. It is used as a _recognizer_ for some input sequence of symbols (utf-8 strings, binary, etc.). It then integrates with a _generator_ which constructs output based on what it matches. The generators can be found in the content-ast-generator (for AST construction) or content-cst-generator (for syntax-highlighting) repos. Other types of generators can also be constructed, not just these two types.

### DSL

The DSL is called the "match" DSL, for matching strings. There are 8 different types of match nodes:

- `sieve`: What is nested inside of it is optional.
- `chain`: Matches one or more of the nested chunks.
- `chunk`: A chunk of patterns to match.
- `crown`: Pick the first one from the set to match.
- `class`: Delegate to a class to match.
- `twist`: Do not match this.
- `check`: Delegate to more sophisticated checks.
- `write`: Match a literal value.

Each match node is matched with `match x`, as in `match chain`... Each match node has "mounts", which are passing variables into it if necessary. A mount is like `mount a, write 1`, where the input name is `a`, and the value is `1`.

You can also have `shift` nodes, for performing pure functions during parsing.

Finally, the `start` node is an input variable to the top-level match classes.

### Example

Here is a very basic example to demonstrate some of the nodes:

```
match x
  match crown
    match class, class a
    match class, class b

match a
  match write, write |a|

match b
  match write, write |b(|
  match sieve
    match class, class x
  match sieve
    match chain
      match write, write |,|
      match class, class x
  match write, write |)|
```

This should match any of these strings:

```
a
b()
b(a)
b(a,a)
b(b(b()))
b(a,b(a,a,b(),a,a))
```

### Internals

All of these nodes get represented with the `churn` type. This is a generalized abstraction for representing all sorts of DSL which ultimately result in some processing.

### Contribute

Contributions are greatly welcomed. Identify the key painpoints in the customer onboarding flow, and help us map out the best solutions. See the [contributor's guide](https://github.com/mountbuild/write/blob/build/contributing.md) for more info if you are just writeing out coding.

### License

Copyright 2021 <a href='https://mount.build'>Mount</a>

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.

### Mount

Content grammar is being developed by the folks at [Mount](https://mount.build), a California-based project for helping humanity master information and computation. Mount started off in the winter of 2008 as a spark of an idea, to forming a company 10 years later in the winter of 2018, to a seed of a project just beginning its development phases. Mount funds start's development. It is entirely bootstrapped by working full time and running [Etsy](https://etsy.com/shop/mountbuild) and [Amazon](https://www.amazon.com/s?rh=p_27%3AMount+Build) shops. Also find us on [Facebook](https://www.facebook.com/mountbuild), [Twitter](https://twitter.com/mountbuild), and [LinkedIn](https://www.linkedin.com/company/mountbuild). Check out our other GitHub projects as well!

<br/>
<br/>
