# Mojo library

The mojo library is the top level library on top of which a User Interface is
built.

libmojo should have as few direct link time library dependencies as possible
with most functionality implemented in mojules(modules) that contain
implementations of interfaces defined in mojo/interaces.

libmojo intends to directly depend on as few libraries as possible and if
possible not expose any library dependencies to client code.

## Library dependencies

# Boost

Exposed in headers for sys::path and boost::any but possibly using alias in
case we want to reimplement those in the future(I hope not)

For filesystem and possibly file streams at some point.

# Glib

Not exposed in the headers

For cross platform module/library stuff, although that should be fairly
straight forward to reimplement.

For character encoding conversion?

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

All classes deriving from mojo::Object are required to be default
constructible.

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
