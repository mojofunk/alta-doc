# Mojo library

The mojo library is the top level library on top of which a User Interface is
built.

libmojo should have as few direct link time library dependencies as possible
with most functionality implemented in modules that contain implementations of
interfaces defined in mojo/interaces.

libmojo intends to directly depend on as few libraries as possible and if
possible not expose any library dependencies to client code.

libmojo is written in C++14 but uses a minimal set of language features, which
should be outlined at some point.

## Library dependencies

There is a compilication with depending on c++ libraries in that if we want to
test and use several compilers then pretty much all c++ code has to be compiled
with the same compiler we are using for the whole stack. It is somewhat
possible apparently to mix c++ libraries compiled with clang and gcc but it
probably just isn't worth the risk. This would suggest importing any c++
libraries we want to use into the source tree, or only depending on external C
based libraries. Neither option is appealing.

I think the best option is to only support a single compiler per platform and
ensure the whole stack is built with it.

# Boost

Exposed in headers for sys::path and boost::any but possibly using alias in
case we want to reimplement those in the future(I hope not)

For filesystem and possibly file streams at some point.

For Lock free lib, FIFO, Stack(memory alloc?)

# Glib

Not exposed in the headers

For cross platform module/library stuff, although that should be fairly
straight forward to reimplement.

For character encoding conversion?

## Exceptions

Use noexcept everywhere possible

## Amalgamation

Amalgamation is where compilation units are concatenated into a single file and
then compiled together rather than separately. It can allow for faster compile
times and better optimisation by the compiler

## Application

The Application class is the Singleton class that initialises the libmojo
library

## Core

The core library contains headers that are necessary for all mojules. This will
be visibility macros.

## Type System

The type system is used to map types to names.

## Object class

All classes deriving from mojo::Object are required to be non-copyable and
default constructible(can we enforce this)?

## Event Callbacks

Each class defines ## Threads

## Modules

## System

The system directory contains all the system level abstractions things like
filesystem, resource management

## API

## Interfaces

## History

## On Disk Project Format

## In Memory Project Format

## Task based Interface

## Engine

The Engine is a reusable component that brings together data from devices and
the application and the data is processed according to an acyclic
graph which will be called a Pipeline.

The Engine has a Pipeline and a ClockSource

A Pipeline could be used outside of engine context, put in separate component?
(e.g A Track will probably have an internal Pipeline)

A Pipeline has a number of Elements and Connections

An Element has a number of Ports

A Port is a connection point and has a type, and Buffer?

The Engine should not depend on specific data types ports should have a URI
like scheme to indicate data type.

The Engine cycle is triggered externally by a clock source such as from the
audio device.
