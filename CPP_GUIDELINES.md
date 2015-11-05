# C++ Specific Coding Guidelines

These are a list of my ideas about some guidelines to adhere to while
developing software using C++.

I also generaly agree with the
[CppCoreGuidelines](https://github.com/isocpp/CppCoreGuidelines/blob/master/CppCoreGuidelines.md)

## Smart Pointers

pass `shared_ptr` by const reference to avoid copying and better performance.

http://herbsutter.com/2013/06/05/gotw-91-solution-smart-pointer-parameters/

## Return Values

If a function needs to indicate whether or not the call succeeded use a bool
with true to indicate success and false failure.

If a function needs to return an error code or some sort then define an enum
and use it. Don't use integers as return values and in C++ code never use
integers and 0 to indicate success. Returning 0 on success is common C
convention and works fine in C code but when mixed with C++ it is confusing.

## Use of goto

There should not be a need to use goto at all in C++ code.

## Allocations/Deallocations

Allocations should always be done into a container/RAII smart pointer class for
exception safety.

Use new/delete in preference to malloc/free

Only use malloc/free and necessary, like implementing an allocator/overriding
class new/delete operators etc or if forced to because of external API use etc.

Use allocators where control of memory usage/caching/pooling is required.

## Errors and Exceptions

Errors should always be handled.

Exceptions should only be used in exceptional circumstances. Sounds silly but
by that I mean for handling error conditions that don't frequently occur. An
example might be parsing a file that has been written to previously and is
expected to be valid state but an error is encountered during parsing(perhaps
corruption or some other reason has led to it being modified). Throwing an
exception in that case would be appropriate as it is tedious to return an error
code and handle it all the way back up the stack.

If an error condition is more frequent then it should be handled by returning
an error code or passing in a pointer to an Error class that is set on error
for instance.

Returning a bool to indicate an error is only appropriate if there is only one
possible error condition. If there is more than one then another error handling
technique should be used.

Errors should not be handled by logging them to disk/UI/etc and returning a
generic error code/bool. Logging the error is a good idea for many reasons but the
logging should be able to be disabled at compile time and the error should
still be handled properly.
