# Mojo library

The mojo library is the top level library on top of which a User Interface is built.

libmojo should have as few direct link time library dependencies as possible with most functionality implemented in mojules(modules) that contain implementations of interfaces defined in mojo/interaces.

libmojo intends to directly depend on as few libraries as possible and if possible not expose any library dependencies to client code.

## Application

The Application class is the Singleton class that initialises the libmojo library

## Core

The core directory contains headers that are necessary for all mojules. This
will be visibility macros.

## Type System

The type system is used to map types to names.

## Object class

All classes deriving from mojo::Object are required to be default constructible.

## Event Callbacks

Each class defines
## Threads

## Mojules

## System

The system directory contains all the system level abstractions
things like filesystem, resource management

## API

## Interfaces

## History

## On Disk Project Format

## In Memory Project Format

## Task based Interface
