Java Formatting Standards
=========================

General
-------

### Files

All Java files MUST use the Unix LF (linefeed) line ending.

All Java files MUST end with a single blank line.

### Lines

There MUST NOT be a hard limit on line length.

The soft limit on line length MUST be 120 characters; automated style checkers
MUST warn but MUST NOT error at the soft limit.

Lines SHOULD NOT be longer than 80 characters; lines longer than that SHOULD
be split into multiple subsequent lines of no more than 80 characters each.
Blank lines MAY be added to improve readability and to indicate related
blocks of code.

There MUST NOT be more than one statement per line.

### Indenting

Code MUST use an indent of 4 spaces, and MUST NOT use tabs for indenting.

> N.b.: Using only spaces, and not mixing spaces with tabs, helps to avoid
> problems with diffs, patches, history, and annotations. The use of spaces
> also makes it easy to insert fine-grained sub-indentation for inter-line
> alignment.

Package Namespace and import Declarations
-----------------------------------------

Packages MUST be declared in all-lowercase, not `camelBack` or `snake_case`.

There MUST be one blank line after the `package` declaration.

When present, all `import` declarations MUST go after the `package`
declaration.

Wildcard imports SHOULD NOT be used.

There MUST be one blank line after the `import` block.

For example:

```java
package vendor.package;

import somevendor.somepackage.Foo;
import othervendor.otherpackage.Bar;

public class Baz
{
    //etc.
}
```

Classes, Properties, and Methods
--------------------------------

Class names MUST be declared in `CamelCase`.

The term "class" refers to all classes, interfaces, and traits.

### Extends and Implements

The `extends` and `implements` keywords MUST be declared on the same line as
the class name.

The opening brace for the class MUST go on its own line; the closing brace
for the class MUST go on the next line after the body.

```java

public class Baz extends ParentClass implements ClickListener
{
    //etc.
}
```

Lists of `implements` MAY be split across multiple lines, where each
subsequent line is indented once. When doing so, the first item in the list
MUST be on the next line, and there MUST be only one interface per line.

```java

public class Baz extends ParentClass implements
    ClickListener,
    Serializable,
    Countable
{
    //etc.
}
```

### Properties

Properties MUST be declared using `camelCase`.

Property names MUST NOT be prefixed to indicate visibility or scope.

### Constants

Class constants MUST be declared in all upper case with underscore separators.

### Methods

Method names MUST be declared in `camelCase`.

Method names MUST NOT be prefixed to indicate visibility or scope.

Method names MUST NOT be declared with a space after the method name. The
opening brace MUST go on its own line, and the closing brace MUST go on the
next line following the body. There MUST NOT be a space after the opening
parenthesis, and there MUST NOT be a space before the closing parenthesis.

A method declaration looks like the following. Note the placement of
parentheses, commas, spaces, and braces:

```java
public class ClassName
{
    public void fooBarBaz(int arg1, int arg2)
    {
        // method body
    }
}
```

### Method Arguments

In the argument list, there MUST NOT be a space before each comma, and there
MUST be one space after each comma.

Argument lists MAY be split across multiple lines, where each subsequent line
is indented once. When doing so, the first item in the list MUST be on the
next line, and there MUST be only one argument per line.

When the argument list is split across multiple lines, the closing parenthesis
and opening brace MUST be placed together on their own line with one space
between them.

```java
public class ClassName
{
    public void aVeryLongMethodName(
        Foo arg1,
        Bar arg2,
        Baz arg3
    ) {
        // method body
    }
}
```

### `abstract`, `final`, and `static`

When present, the `abstract` and `final` declarations MUST precede the
visibility declaration.

When present, the `static` declaration MUST come after the visibility
declaration.

```java
abstract public class ClassName
{
    final private static int foo = 42;

    abstract protected void zim();

    final public static void bar()
    {
        // method body
    }
}
```

### Method Calls

When making a method call, there MUST NOT be a space between the
method name and the opening parenthesis, there MUST NOT be a space
after the opening parenthesis, and there MUST NOT be a space before the
closing parenthesis. In the argument list, there MUST NOT be a space before
each comma, and there MUST be one space after each comma.

```
this.bar();
foo.bar(arg1);
Foo.bar(arg2, arg3);
```

Argument lists MAY be split across multiple lines, where each subsequent line
is indented once. When doing so, the first item in the list MUST be on the
next line, and there MUST be only one argument per line.

```java
foo.bar(
    longArgument,
    longerArgument,
    muchLongerArgument
);
```

### Variable naming

Locally scoped variables, including method argumets, MUST be declared in
`camelBack`.

Variable names SHOULD NOT include suffixing to indicate their type. Ex.
`String versionString`.


### Shadowing and Scope Resolution

Non-locally scoped methods and property references MUST be prefixed with their
appropriate scope in order to work around shadowing.
ex. `this.method()` or `MyClass.myStaticMethod()`

### Empty Blocks

An empty block MAY be shortened to a single line for conciseness. When doing
this, the braces MUST appear on the same line with the statement, and there
MUST NOT be a space inbetween the opening and closing brace. Annotations MUST
appear on the same line as the declaration.

Naming
------

Construct prefix and suffixing such as `FooInterface`, `BarImpl` and
`AbstractBaz` SHOULD NOT be used.

Control Structures
------------------

The general style rules for control structures are as follows:

- There MUST be one space after the control structure keyword
- There MUST NOT be a space after the opening parenthesis
- There MUST NOT be a space before the closing parenthesis
- Structures MUST include braces, even in cases where the body is a single line
- There MUST be one space between the closing parenthesis and the opening
  brace
- The structure body MUST be indented once
- The closing brace MUST be on the next line after the body

The body of each structure MUST be enclosed by braces. This standardizes how
the structures look, and reduces the likelihood of introducing errors as new
lines get added to the body.


### `if`, `elseif`, `else`

An `if` structure looks like the following. Note the placement of parentheses,
spaces, and braces; and that `else` and `else if` are on the same line as the
closing brace from the earlier body.

```
if (expr1) {
    // if body
} else if (expr2) {
    // elseif body
} else {
    // else body;
}
```


### `switch`, `case`

A `switch` structure looks like the following. Note the placement of
parentheses, spaces, and braces. The `case` statement MUST be indented once
from `switch`, and the `break` keyword (or other terminating keyword) MUST be
indented at the same level as the `case` body. There MUST be a comment such as
`// no break` when fall-through is intentional in a non-empty `case` body.

```
switch (target) {
    case 0:
        // body
        break;
    case 1:
        // second body
        // no break
    case 2:
    case 3:
    case 4:
        // third body, with return.
        return;
    default:
        //default body
        break;
}
```


### `while`, `do while`

A `while` statement looks like the following. Note the placement of
parentheses, spaces, and braces.

```
while (expr) {
    // structure body
}
```

Similarly, a `do while` statement looks like the following. Note the placement
of parentheses, spaces, and braces.

```
do {
    // structure body;
} while (expr);
```

### `for`

A `for` statement looks like the following. Note the placement of parentheses,
spaces, and braces.

```
for (int i = 0; i < 10; i++) {
    // for body
}
```

A iterating for/each statement looks like the following. Note the placement of
parentheses, spaces, and braces.

```
for (int item : collection) {
    // for/each body
}
```

### `try`, `catch`

A `try catch` block looks like the following. Note the placement of
parentheses, spaces, and braces.

```
try {
    // try body
} catch (FirstExceptionType e) {
    // catch body
} catch (OtherExceptionType e) {
    // catch body
}
```

### Code Documentation

Documentation blocks SHOULD be done javadoc style, except as noted.

Blocks SHOULD be done above all methods and classes.

Documentation SHOULD include a single-line summary as the first line and
optionally include a more detailed description. Lines SHOULD be broken into
lines contained within the 80 character soft line-limit. Description text MAY
include formatting and code samples, if provided, these SHOULD be written in
[Markdown Syntax][1], not in HTML. (Do not include `<p>` tags)

Annotations SHOULD be provided for each:
 - Parameter in methods, and Generic type declarations (`@param`)
 - The return type (`@return`)
 - Any thrown exceptions in the method that are not caught, including those not
   in the method signature. (`@throws`)
 - Deprecated markings, if needed (`@deprecated`)
 - Any code cleanup TODO markings (`@todo`)
 - An Author tag on each class, containing all authors that have contributed
   significant portions of code (`@author`)

The above listed annotations MUST not be provided with empty body descriptions.
When an annotation doesn't fit within the soft limit, the line MAY be broken up
into multiple lines, indented either four spaces from the start or more to align
with the body start.
Code Documentation blocks MAY include any additional annotations within the
block as needed.


[1]: http://daringfireball.net/projects/markdown/syntax
