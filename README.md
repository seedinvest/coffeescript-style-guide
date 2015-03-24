# Apiary CoffeeScript Style Guide

This guide presents a collection of best-practices and coding conventions for the [CoffeeScript][coffeescript] programming language.

It intentionally diverges a bit from idioms, such as _optional function parentheses_ or _use the coolest available syntactic sugar to write it all on just a single line_. Aim of this style guide is to be _closer to Python than to Perl_ and to avoid [some readability problems][bad-readability].

**Please note that this is a work-in-progress. Use standard GitHub flow for issuing objections, ideas, contributions. Style Guide should be based on consensus, so your ideas are very welcome.**

## Inspiration

The details in this guide have been very heavily inspired by several existing style guides and other resources. In particular:

- [CoffeeScript Style Guide][csg] by Polar Mobile Group
- [PEP-8][pep8]: Style Guide for Python Code

## Table of Contents

* [The CoffeeScript Style Guide](#guide)
    * [Code Layout](#code_layout)
        * [Tabs or Spaces?](#tabs_or_spaces)
        * [Maximum Line Length](#maximum_line_length)
        * [Blank Lines](#blank_lines)
        * [Trailing Whitespace](#trailing_whitespace)
        * [Optional Commas](#optional_commas)
        * [Optional Braces](#optional_braces)
        * [Encoding](#encoding)
    * [Module Imports](#module_imports)
    * [Whitespace in Expressions and Statements](#whitespace)
    * [Comments](#comments)
        * [Block Comments](#block_comments)
        * [Inline Comments](#inline_comments)
    * [Naming Conventions](#naming_conventions)
    * [Functions](#functions)
    * [Deconstructions](#deconstructions)
    * [Strings](#strings)
    * [Conditionals](#conditionals)
    * [Looping and Comprehensions](#looping_and_comprehensions)
    * [Extending Native Objects](#extending_native_objects)
    * [Exceptions](#exceptions)
    * [Annotations](#annotations)
    * [Idioms](#idioms)
    * [Miscellaneous](#miscellaneous)
    * [Optimization](#optimization)

<a name="code_layout"/>
## Code layout

<a name="tabs_or_spaces"/>
### Tabs or Spaces?

Use **spaces only**, with **2 spaces** per indentation level. Never mix tabs and spaces.

<a name="maximum_line_length"/>
### Maximum Line Length

Limit all lines to a maximum of 79 characters.

<a name="blank_lines"/>
### Blank Lines

Separate top-level function and class definitions with two blank lines.

Separate method definitions inside of a class with a single blank line.

Use a single blank line within the bodies of methods or functions in cases where this improves readability (e.g., for the purpose of delineating logical sections).

<a name="trailing_whitespace"/>
### Trailing Whitespace

Do not include trailing whitespace on any lines.

<a name="optional_commas"/>
### Optional Commas

Avoid the use of commas before newlines when properties or elements of an Object or Array are listed on separate lines.

```coffeescript
# Yes
foo = [
  'some'
  'string'
  'values'
]
bar:
  label: 'test'
  value: 87

# No
foo = [
  'some',
  'string',
  'values'
]
bar:
  label: 'test',
  value: 87
```

Always use commas to separate function arguments:

```coffeescript
# Yes
moveTo(10,
  key: value
)

# No
moveTo(10
  key: value
)
```

<a name="optional_braces"/>
### Optional Braces

Avoid use of braces in multi-line object literals:

```coffeescript
# Yes
value =
  color: 'blue'
  size: 42

# No
value = {
  color: 'blue'
  size: 42
}
```

Use braces in case you need implicit key-value pairs:

```coffeescript
# Yes
value = {
  color: 'blue'
  size
}

# No
value = {
  color: 'blue'
  size: 42
}
```

Always use braces in one-line object literals:

```coffeescript
{color: 'blue', size: 42} # Yes
color: 'blue', size: 42 # No
```

Avoid extraneous whitespace in one-line object literals:

```coffeescript
{color: 'blue', size: 42} # Yes
{ color: 'blue', size: 42 } # No
```

<a name="encoding"/>
### Encoding

UTF-8 is the only allowed source file encoding.

<a name="module_imports"/>
## Module Imports

If using a module system (CommonJS Modules, AMD, etc.), `require` statements should be placed on separate lines.

```coffeescript
require('lib/setup')
Backbone = require('backbone')
```
These statements should be grouped in the following order:

1. Standard library imports _(if a standard library exists)_
2. Third party library imports
3. Local imports _(imports specific to this application or library)_

Do not use `.coffee` extension while specifying path to a local module.

```coffee
mod = require('module') # Yes
mod = require('module.coffee') # No
```

<a name="whitespace"/>
## Whitespace in Expressions and Statements

Avoid extraneous whitespace in the following situations:

- Immediately inside parentheses, brackets or braces

    ```coffeescript
       $('body') # Yes
       $( 'body' ) # No
    ```

- Immediately before a comma

    ```coffeescript
       console.log(x, y) # Yes
       console.log(x , y) # No
    ```

Additional recommendations:

- Always surround these binary operators with a **single space** on either side

    - assignment: `=`

        - _Note that this also applies when indicating default parameter value(s) in a function declaration_

           ```coffeescript
           test: (param = null) -> # Yes
           test: (param=null) -> # No
           ```

    - augmented assignment: `+=`, `-=`, etc.
    - comparisons: `==`, `<`, `>`, `<=`, `>=`, `unless`, etc.
    - arithmetic operators: `+`, `-`, `*`, `/`, etc.

    - _(Do not use more than one space around these operators)_

        ```coffeescript
           # Yes
           x = 1
           y = 1
           fooBar = 3

           # No
           x      = 1
           y      = 1
           fooBar = 3
        ```

<a name="comments"/>
## Comments

If modifying code that is described by an existing comment, update the comment such that it accurately reflects the new code. (Ideally, improve the code to obviate the need for the comment, and delete the comment entirely.)

The first word of the comment should be capitalized, unless the first word is an identifier that begins with a lower-case letter.

If a comment is short, the period at the end can be omitted.

<a name="block_comments"/>
### Block Comments

Block comments apply to the block of code that follows them.

Each line of a block comment starts with a `#` and a single space, and should be indented at the same level of the code that it describes.

Paragraphs inside of block comments are separated by a line containing a single `#`.

```coffeescript
  # This is a block comment. Note that if this were a real block
  # comment, we would actually be describing the proceeding code.
  #
  # This is the second paragraph of the same block comment. Note
  # that this paragraph was separated from the previous paragraph
  # by a line containing a single comment character.

  init()
  start()
  stop()
```

<a name="inline_comments"/>
### Inline Comments

Inline comments are placed on the line immediately above the statement that they are describing. If the inline comment is sufficiently short, it can be placed on the same line as the statement (separated by a single space from the end of the statement).

All inline comments should start with a `#` and a single space.

The use of inline comments should be limited, because their existence is typically a sign of a code smell.

Do not use inline comments when they state the obvious:

```coffeescript
  # No
  x = x + 1 # Increment x
```

However, inline comments can be useful in certain scenarios:

```coffeescript
  # Yes
  x = x + 1 # Compensate for border
```

<a name="naming_conventions"/>
## Naming Conventions

Use `camelCase` (with a leading lowercase character) to name all variables, methods, and object properties.

Use `CamelCase` (with a leading uppercase character) to name all classes. _(This style is also commonly referred to as `PascalCase`, `CamelCaps`, or `CapWords`, among [other alternatives][camel-case-variations].)_

_(The **official** CoffeeScript convention is camelcase, because this simplifies interoperability with JavaScript. For more on this decision, see [here][coffeescript-issue-425].)_

For constants, use all uppercase with underscores:

```coffeescript
CONSTANT_LIKE_THIS
```

Methods and variables that are intended to be "private" should begin with a leading underscore:

```coffeescript
_privateMethod: ->
```

### File names

Use dash-separated words for naming files:

```
user-rights.coffee
private-traffic.coffee
```

Use `camelCase` for objects imported from files with dash-separated names:

```coffeescript
userRights = require('../user-rights')
```

<a name="functions"/>
## Functions

_(These guidelines also apply to the methods of a class.)_

When declaring a function that takes arguments, always use a single space after the closing parenthesis of the arguments list:

```coffeescript
foo = (arg1, arg2) -> # Yes
foo = (arg1, arg2)-> # No
```

Do not use parentheses when declaring procedures (functions that take no arguments):

```coffeescript
bar = -> # Yes
bar = () -> # No
```

In cases where method calls are being chained and the code does not fit on a single line, each call should be placed on a separate line and indented by one level (i.e., two spaces), with a leading `.`.

```coffeescript
[1..3]
  .map((x) -> x * x)
  .concat([10..12])
  .filter((x) -> x < 11)
  .reduce((x, y) -> x + y)
```

When calling functions, never omit parentheses. It is a strict and strong rule, but it deals with significant majority of ambiguous cases and problems you can find in CoffeeScript:

```coffeescript
baz(12)

brush.ellipse({x: 10, y: 20})

foo(4).bar(8)

obj.value(10, 20) / obj.value(20, 10)

print(inspect(value))

new Tag(new Value(a, b), new Arg(c))
```

Do not use grouping functions instead of grouping parameters:

```coffeescript
$('#selektor').addClass('klass') # Yes
($ '#selektor').addClass 'klass' # No

foo(4).bar(8) # Yes
(foo 4).bar 8 # No
```

Use parentheses and commas properly when writing nested function definitions:

```coffeescript
fn(42, 'string', (err) ->
  console.error err.message
, (arg1, arg2) ->
  arg1 + arg2
)
```

Do not use inline functions unless they're very short and simple:

```coffeescript
# Yes
(x) -> x + x

# Yes
(x) ->
  if true
    42
  else
    x + x

# No
(x) -> if true then 42 else (x + x)
```

<a name="deconstructions"/>
## Deconstructions

Keep deconstructions one-line unless they are too many:

```coffeescript
# Yes
{one, two} = object

# Yes
{
  one
  two
  three
  four
  five
  six
} = object

# No
{
  one
  two
} = object
```

If there are too many deconstructed elements, consider not using deconstruction at all:

```coffeescript
# Yes
object.one
object.four
...

# No
{
  one
  two
  three
  four
  five
  six
} = object
```

The same rules apply to array deconstructions.

<a name="strings"/>
## Strings

Use string interpolation instead of string concatenation:

```coffeescript
"this is an #{adjective} string" # Yes
"this is an " + adjective + " string" # No
```

Prefer single quoted strings (`''`) instead of double quoted (`""`) strings, unless features like string interpolation are being used for the given string.

Use string blocks for large strings:

```coffeescript
str = '''
  Very long block of text, which has multiple lines.

  Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nullam vel
  finibus ante. Morbi venenatis ante lacus, vitae rutrum odio pharetra
  vitae. Quisque mollis augue ac nisl dignissim pulvinar. Etiam gravida
  viverra sollicitudin. Praesent eu ex ultrices, vulputate lectus eu,
  laoreet nisl.
'''
```

Mind that CoffeeScript supports indentation of such strings for better readability:

```coffeescript
if condition
  fn = (arg1, arg2) ->
    str = '''
      Very long block of text, which has multiple lines.

      Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nullam
      vel finibus ante. Morbi venenatis ante lacus, vitae rutrum odio
      pharetra vitae. Quisque mollis augue ac nisl dignissim pulvinar.
      Etiam gravida viverra sollicitudin. Praesent eu ex ultrices,
      vulputate lectus eu, laoreet nisl.
    '''
```

<a name="conditionals"/>
## Conditionals

Favor `unless` over `if` for negative conditions, but preferably only for short, almost one-line conditionals.

Instead of using `unless...else`, use `if...else`:

```coffeescript
  # Yes
  if true
    ...
  else
    ...

  # No
  unless false
    ...
  else
    ...
```

Multi-line if/else clauses should use indentation:

```coffeescript
  # Yes
  if true
    ...
  else
    ...

  # No
  if true then ...
  else ...
```

One-line if/then/else conditionals should be used cautiously, preferably only for very short and simple conditions in assignments:

```coffeescript
value = if true then 42 else 0 # Yes

if true then value = (42 or 38) else value = (5 unless false) or true # No
```

One-line trailing conditionals should be used cautiously, preferably only for very short and simple conditions:

```coffeescript
return cb(err) if err # Yes

value = k for k, v of object unless err # No
```

<a name="looping_and_comprehensions"/>
## Looping and Comprehensions

Take advantage of comprehensions whenever possible:

```coffeescript
  # Yes
  result = (item.name for item in array)

  # No
  results = []
  for item in array
    results.push item.name
```

To filter:

```coffeescript
result = (item for item in array when item.name is "test")
```

To iterate over the keys and values of objects:

```coffeescript
object = {one: 1, two: 2}
alert("#{key} = #{value}") for key, value of object
```

As CoffeeScript doesn't support multi-line comprehentions, in case your comprehension is too long for one line, use proper loop instead:

```coffeescript
# Yes
values = []
for keySomething, valueSomething of object.nestedObject.nestedArray
  if keySomething > 42
    values.push valueSomething

# No
values = (valueSomething for keySomething, valueSomething of object.nestedObject.nestedArray when keySomething > 42)
```

Use inline loops only for very short and simple constructs:

```coffeescript
# Yes
object[v] = k for k, v of data

# No
(if val.length then arr.push (i for i, j of val)) for val in things
```

<a name="extending_native_objects"/>
## Extending Native Objects

Do not modify native objects.

For example, do not modify `Array.prototype` to introduce `Array#forEach`.

<a name="exceptions"/>
## Exceptions

Do not suppress exceptions.

Never throw exceptions in asynchronous flow. Use first callback parameter for passing an error object.

<a name="annotations"/>
## Annotations

Use annotations when necessary to describe a specific action that must be taken against the indicated block of code.

Write the annotation on the line immediately above the code that the annotation is describing.

The annotation keyword should be followed by a colon and a space, and a descriptive note.

```coffeescript
  # FIXME: The client's current state should *not* affect payload processing.
  resetClientState()
  processPayload()
```

If multiple lines are required by the description, indent subsequent lines with two spaces:

```coffeescript
  # TODO: Ensure that the value returned by this call falls within a certain
  #   range, or throw an exception.
  analyze()
```

Annotation types:

- `TODO`: describe missing functionality that should be added at a later date
- `FIXME`: describe broken code that must be fixed
- `OPTIMIZE`: describe code that is inefficient and may become a bottleneck
- `HACK`: describe the use of a questionable (or ingenious) coding practice
- `REVIEW`: describe code that should be reviewed to confirm implementation

If a custom annotation is required, the annotation should be documented in the project's README.

<a name="idioms"/>
## Idioms

From [Zen of Python][zen-of-python]:

- Beautiful is better than ugly.
- Explicit is better than implicit.
- Simple is better than complex.
- Complex is better than complicated.
- Flat is better than nested.
- Sparse is better than dense.
- Readability counts.
- Special cases aren't special enough to break the rules.
- Although practicality beats purity.
- If the implementation is hard to explain, it's a bad idea.
- If the implementation is easy to explain, it may be a good idea.

And more:

- Consistency counts.
- DRY counts.

### Readability

Strive for readability. Your code is going to be written once, but read many, many times.

Too much syntactic sugar causes diabetes. Don't use something just because it's cool. Never use something in case you even _slightly_ hesitate that you may be the only person in the company being aware of how it works:

```coffeescript
included = 'a long test string'.indexOf('test') isnt -1 # Yes
included = !!~ 'a long test string'.indexOf 'test' # No
```

Ignore those parts of other guidelines which recommend to go against readability (i.e., [The Little Book on CoffeeScript][idioms]).

### DRY

Strive for DRY, mind that [premature DRY is the same evil as premature optimization][dry].

### Asynchronous flow

Unless specified otherwise, handle asynchronous flow using [async.js][async] library instead of nested callbacks:

```coffeescript
# Yes
async.waterfall [
  (next) -> ...
  (result, next) -> ...
  (result, next) -> ...
], cb

# No
fn((err, result) ->
  return cb(err) if err
  ...
)
```

### Filling gaps in standard library

Use [Underscore.js][underscore] when you're missing some very basic features (e.g., for object and array manipulation) and they're not present in standard library.

<a name="miscellaneous"/>
## Miscellaneous

`and` is preferred over `&&`.

`or` is preferred over `||`.

`is` is preferred over `==`.

`isnt` is preferred over `!=`.

`not` is preferred over `!`.

`?=` should be used when possible:

```coffeescript
temp ?= {} # Yes
temp = temp or {} # No
```

Prefer shorthand notation (`::`) for accessing an object's prototype:

```coffeescript
Array::slice # Yes
Array.prototype.slice # No
```

Prefer `@property` over `this.property`.

```coffeescript
return @property # Yes
return this.property # No
```

For consistency, use also standalone `@`:

```coffeescript
return @ # Yes
return this # No
```

Avoid `return` where not required, unless the explicit return increases clarity.

Use splats (`...`) when working with functions that accept variable numbers of arguments:

```coffeescript
console.log args... # Yes

(a, b, c, rest...) -> # Yes
```

<a name="optimization"/>
## Optimization

Use explicit empty `return` statements to preserve performance in case large objects would be returned unnecessarily (e.g., results of loops).

Use literal syntax where applicable. Use built-ins if they're available.

Use `Array::join` for programatically building up a string.

[bad-readability]: http://ceronman.com/2012/09/17/coffeescript-less-typing-bad-readability/
[coffeescript]: http://jashkenas.github.com/coffee-script/
[coffeescript-issue-425]: https://github.com/jashkenas/coffee-script/issues/425
[pep8]: http://www.python.org/dev/peps/pep-0008/
[csg]: https://github.com/polarmobile/coffeescript-style-guide
[camel-case-variations]: http://en.wikipedia.org/wiki/CamelCase#Variations_and_synonyms
[idioms]: http://arcturo.github.io/library/coffeescript/04_idioms.html
[dry]: http://blog.ploeh.dk/2014/08/07/why-dry/
[zen-of-python]: https://www.python.org/dev/peps/pep-0020/
[async]: https://github.com/caolan/async
[underscore]: http://underscorejs.org
