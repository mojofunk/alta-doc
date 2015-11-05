# Code Style Conventions

## Indentation and formatting

A source code formatting tool should be used to keep source code formatting
consistent.

Tabs vs Spaces

I don't think it matters so much and I have typically used tabs for indentation
because of the projects I've been involved in. It is a trade off between being
able to control the indentation to suit individual preference(tabs) vs strict
formatting(spaces).

I tend to think that all code should be able to fit in a 80 character wide
terminal and all functions should be able to fit on a single page(wishful
thinking perhaps), as it enforces/restricts the level of indentation and nested
blocks which I think improves code quality.

Using tabs with a line width restriction isn't really compatible as the whole
purpose of using tabs over spaces is so that the code can be displayed to a
users taste but unless the maximum line width is large then they are forced to
use 2 or perhaps 4 space tabs anyway.

## Namespace Identifiers

Namespace identifiers should be a short and lower case. The name of
the top level namespace of a library should be the same name as the
library.

## Nested Namespaces

All public classes and symbols in a library should be in the top level
namespace. Nested namespaces should be used for classes/types/functions that
are internal to the library.

It is common for a large library collection that is divided into modules to
have a top level namespace for the library and a nested namespace for each
module. If there are two classes in two modules that seem like they should have
the same name then use a different class name, even just prefixing the module
name to the class name like `GraphicsPoint` and `TimePoint`(TODO think of a
better example) rather than using nested namespaces as this leads to less
confusion when discussing types and for documentation.

## Namespace Indentation

Classes inside namespaces should not use any indentation and the
namespace name should be added in a single line comment after the
closing bracket(on the same line). The source code formatter should take care
of this.

## Function Names

Use verbs for functions and place verbs at the start of the function name as is
the norm for transitive verbs in english. e.g. `create_object()` not
`object_create()` etc. or `find_files_in_directory_matching()` not
`matching_files_in_directory()`. TODO, think of some better examples.

## Class Names

Class names use CamelCase notation.

## Class Files

Each C++ class is defined in header file and a single source file. A C++ header
file must use the hpp extension and the source file must use the cpp extension.

The name of the source file should be the class name in lower case and
use underscores between words. The name of the files do not need prefixing with
the library or namespace name as any public headers will be located in a
subdirectory in the include path.

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

The guard name used is determined by the namespace name and class name so for
example if the class name was Polygon in the geom namespace then the guard name
would be the following:

```c++
#ifndef GEOM_POLYGON_H
#define GEOM_POLYGON_H
...
#endif // GEOM_POLYGON_H
```

## Typedefs

Typedef statements should not use lower case letters with an underscore
used for word separation with a postfix `_type` as `_t` is reserved(ref?)

## Member Variable Names

Member variable and member function identifiers are all lower case letters. An
underscore is used for word separation.

Member variables should not be prefixed with and underscore as this is
reserved. The other option is a trailing underscore which is fairly common but
I think makes the code less readable.

Use a prefix to indicate to indicate whether the variable is an instance
variable or a class static variable.

Member variables are prefixed by `m_`, so for instance `name` would become
`m_name`.

Static member variables are prefixed by `s_`.

Member names should be similar as the accessor function names associated with
the variable.

For data members that are non-POD types it will usually make sense to encode
the type in the name. When it is appropriate postfix the variable type in the
name.

For instance if I have a std::map that contains some data protected by a
std::mutex then the map name might be `m_thread_name_map` and the mutex
variable name might be `m_thread_name_map_mutex`.

## Control Structures

see the [Mozilla Code Style Guide](http://developer.mozilla.org/en-US/docs/Mozilla/Developer_guide/Coding_Style)

## Anonymous Namespaces

see the [Mozilla Code Style Guide](http://developer.mozilla.org/en-US/docs/Mozilla/Developer_guide/Coding_Style)
prefer use of static for now for compatability with debuggers

