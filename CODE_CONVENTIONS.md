# Code Conventions

## Indentation and formatting

A source code formatting tool should be used to keep source code formatting
consistent.

Tabs vs Spaces

I don't think it matters so much and I have typically used tabs for indentation
because of the projects I've been involved in. It is a trade off between being
able to control the indentation to suit individual preference(tabs) vs strict
formatting(spaces)

I tend to think that all code should be able to fit in a 80 character wide
terminal. It enforces/restricts the level of indentation and nested blocks
which I think improves code quality. Using tabs in that instance is possible
but means using 2 or 4 space tabs.

## Namespace Identifiers

Namespace identifiers should be a short and lower case. The name of
the top level namespace of a library should be the same name as the
library.

## Namespace Indentation

Classes inside namespaces should not use any indentation and the
namespace name should be added in a single line comment after the
closing bracket(on the same line). The source code formatter should take care
of this.

## Class Names

Class names use CamelCase notation.

## Class Files

Each C++ class is defined in header file and a single source file. A C++ header
file must use the hpp extension and the source file must use the cpp extension.

The name of the source file should be the class name in lower case and
use underscores between words.

When calling a function there should not be a single space between the
function or method name and the starting bracket. This looks strange when you
are calling a method on an object that is returned from a method.

```c++
project ().get_directory ().root_path ();

// as opposed to

project().get_directory().root_path();
```

It can is somewhat more readable at times but it also uses more space.

## Constructor Initialization List

Each member initialization should occur on a new line and each line(apart from
the first) should start with a comma. The colon after the class constructor
declaration should be on a line by itself. If this style is followed adding or
removing a class member will only cause a one line change. The source code
formatter may do this automatically.

```c++
Foo::Foo ()
	: m_one(1)
	, m_two(2)
	, m_three(3)
{ }
```
## Header Include Guards

Header include guards are used to ensure that a header file is not included
more than once from a source file. The names used for the guards should be
uppercase with words separated by underscores followed by a `_H` postfix.

The guard name used is determined by the class name so for example if the class
name was Polygon in the Geom library then the guard name would be the
following:

```c++
#ifndef GEOM_POLYGON_H
#define GEOM_POLYGON_H
...
#endif // GEOM_POLYGON_H
```

## Typedefs

Typedef statements should not use lower case letters with an underscore
used for word separation with a postfix `_t` as this is reserved...

## Member Variable Names

Member variable and member function identifiers are all lower case letters. An
underscore is used for word separation.

Member variables should not be prefixed with and underscore as this is
reserved(and not visually pleasing IMO)

Member variables are prefixed by `m_`, so for instance `name` would become
`m_name`.

Static member variables are prefixed by `s_`.
