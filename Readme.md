# Javascript Style Guide

This is a guide for writing consistent and aesthetically pleasing JS code.
It is inspired by what is popular within the community, and flavored with some
personal opinions.

This guide was created by [Stuart P. Bentley](http://stuartpb.com), based on
the style guide written by [Felix Geisendörfer](http://felixge.de/) licensed
under the [CC BY-SA 3.0](http://creativecommons.org/licenses/by-sa/3.0/)
license. You are encouraged to fork this repository and make adjustments
according to your preferences.

![Creative Commons License](http://i.creativecommons.org/l/by-sa/3.0/88x31.png)

## 2 spaces for indention

Use 2 spaces for indenting your code. If your IDE doesn't insert spaces when
you press the "tab" key, you insert the spaces with the spacebar until you can
get to a decent text editor.

## Newlines

Files should use LF newlines (`\n`) rather than CRLF newlines (`\r\n`) in their
repo-committed blob form. (If your development platform requires CRLF files,
`git config core.autocrlf true` should be set.)

Files should end with a newline at end of file.

## UTF-8 encoding

All JS files should be encoded as UTF-8.

## No trailing whitespace

The line feed character should never be preceded by any other whitespace
characters. Set your editor to strip trailing whitespace on save.

## Use semicolons

There are arguments in favor of [perserving][hnsemicolons] and [omitting][izs]
semicolons at the end of statements in JS. Understand the reasoning behind
both, but

[izs]: http://blog.izs.me/post/2353458699/an-open-letter-to-javascript-leaders-regarding
[hn]: http://news.ycombinator.com/item?id=1547647

## 80 characters per line

Limit your lines to 80 characters wherever possible. This allows for easier
visual parsing (less eye travel), as well as providing the ability to keep
multiple files open side-by-side.

## Quoting strings

Strings should be quoted in whatever form will need the fewest quote-escapes:
when writing strings with embedded code (such as HTML), single-quotes should be
used to delimit the JS string and double-quotes for the embedded strings.

When neither quote style appears in the string, double quotes should be used
(although single-quotes are acceptable for more 'token'-esque string usages).

## Opening braces go on the same line

Your opening braces go on the same line as the statement.

*Right:*

```js
if (true) {
  console.log('winning');
}
```

*Wrong:*

```js
if (true)
{
  console.log('losing');
}
```

Also, notice the use of whitespace before and after the condition statement.

## Declare one variable per var statement

Declare one variable per var statement. This makes it easier to re-order the
lines. Declare variables wherever they make sense, but be aware of how
Javascript hoists variable declarations to the top of the function scope.

*Right:*

```js
var keys   = ['foo', 'bar'];
var values = [23, 42];

var object = {};
while (items.length) {
  var key = keys.pop();
  object[key] = values.pop();
}
```

*Wrong:*

```js
var keys = ['foo', 'bar'],
    values = [23, 42],
    object = {},
    key;

while (items.length) {
  key = keys.pop();
  object[key] = values.pop();
}
```

## Use lowerCamelCase for variables, properties and function names

Variables, properties and function names should use `lowerCamelCase`.  They
should also be descriptive. Single character variables and uncommon
abbreviations should generally be avoided.

*Right:*

```js
var adminUser = db.query('SELECT * FROM users ...');
```

*Wrong:*

```js
var admin_user = db.query('SELECT * FROM users ...');
```

## Use UpperCamelCase for class names

Class names should be capitalized using `UpperCamelCase`.

*Right:*

```js
function BankAccount() {
}
```

*Wrong:*

```js
function bank_Account() {
}
```

## Use UPPERCASE for Constants

Constants should be declared as regular variables or static class properties,
using all uppercase letters.

Node.js / V8 actually supports mozilla's [const][const] extension, but
unfortunately that cannot be applied to class members, nor is it part of any
ECMA standard.

*Right:*

```js
var SECOND = 1 * 1000;

function File() {
}
File.FULL_PERMISSIONS = 0777;
```

*Wrong:*

```js
const SECOND = 1 * 1000;

function File() {
}
File.fullPermissions = 0777;
```

[const]: https://developer.mozilla.org/en/JavaScript/Reference/Statements/const

## Object / Array creation

Use trailing commas and put *short* declarations on a single line. Only quote
keys when your interpreter complains:

*Right:*

```js
var a = ['hello', 'world'];
var b = {
  good: 'code',
  "is generally": 'pretty',
};
```

*Wrong:*

```js
var a = [
  'hello', 'world'
];
var b = {"good": 'code'
        , is generally: 'pretty'
        };
```

## Use the == operator

Understand how type coercion works, and only use === and !== in places where
you know type coercion will produce unwanted results (usually only when you're
using functions that legitimately expect multiple types for their arguments).
In all other cases, use == and !=.

## Use single-line ternary operators

If your ternary operator can't fit on a single line, you might be abusing the
ternary operator. Consider putting more complex ternary statements into a
conditional variable assignment.

```js
return a > b ? a : b;
```

## Do not extend built-in prototypes

Unless you're a polyfill, don't extend the prototype of native Javascript
objects.

## Name anonymous functions

Named anonymous functions produce better stack traces, heap and cpu profiles.

*Right:*

```js
req.on('end', function onEnd() {
  console.log('winning');
});
```

*Wrong:*

```js
req.on('end', function() {
  console.log('losing');
});
```

## Use nested functions

Functions should be scoped within the context in which they're relevant, if
doing so allows them to omit parameters that would otherwise have to be passed
as arguments..

## Use slashes for comments

Use slashes for both single line and multi line comments. Try to write
comments that explain higher level mechanisms or clarify difficult
segments of your code. Don't use comments to restate trivial things.

*Right:*

```js
// 'ID_SOMETHING=VALUE' -> ['ID_SOMETHING=VALUE'', 'SOMETHING', 'VALUE']
var matches = item.match(/ID_([^\n]+)=([^\n]+)/));

// This function has a nasty side effect where a failure to increment a
// redis counter used for statistics will cause an exception. This needs
// to be fixed in a later iteration.
function loadUser(id, cb) {
  // ...
}

var isSessionValid = (session.expires < Date.now());
if (isSessionValid) {
  // ...
}
```

*Wrong:*

```js
// Execute a regex
var matches = item.match(/ID_([^\n]+)=([^\n]+)/));

// Usage: loadUser(5, function() { ... })
function loadUser(id, cb) {
  // ...
}

// Check if the session is valid
var isSessionValid = (session.expires < Date.now());
// If the session is valid
if (isSessionValid) {
  // ...
}
```
