# libmojo

Decide on license

## Build System

Be able to build static binary with all modules built in. How will this affect
licensing?

## Directory Structure

## C++11

switch to std:: smart pointer types

switch to std::bind where possible/necessary

use std::chrono

use std::function for connecting signals?

use std::atomic

use std::regex

use std:: random number generators

Replace pointer typedefs with c++11 using keyword

## Glib

depend on glib for filesystem stuff? or convert between native paths and
utf-8?

## Boost libs

Use boost::any

Use boost::typeindex? in mojo/typesystem rather than std::type_info

use mojo::any alias for any boost classes so reimplementation etc is easier

## Core

Make DebugRegistry thread safe

Add Timing data logging to MOJO_DEBUG

Add Thread debug data to MOJO_DEBUG

Add Spinwait class, mostly for testing timing atm

Add Quark implementation

Add log levels? get/set log handlers? for GUI display etc

Add threads.hpp to register thread names/memory pools for at least debugging etc?

All methods are sync unless a "async" suffix is appended to the function/method name

move Object and Typesystem into core?
Implement custom any type for TypeSystem?


platform defs in core/system/target_platform.hpp MOJO_WINDOWS MOJO_LINUX MOJO_MAC

Add version of mojo::to_string that returns string not bool

Add Context Interface to core

Context needs call_sync and call_async methods, call async for normal
events call_sync for disposing references in other thread contexts.

ContextManager Singleton class so that classes can use named contexts to schedule changes, rather
than explicit locking? needs further thought

mojo could offer a way to register a Context with a thread so that when
registering

Add mojo::aligned_alloc

Add float/double comparison to mojo/core/math.hpp with tests
ref: http://www.cygnus-software.com/papers/comparingfloats/comparingfloats.htm
ref: http://stackoverflow.com/questions/17333/most-effective-way-for-float-and-double-comparison

Add unique_ptr wrappers for std::FILE handles
ref: http://codereview.stackexchange.com/questions/4679/shared-ptr-and-file-for-wrapping-cstdio-update-also-dlfcn-h
## Project class

Should only be one method to add and remove tracks?

## New Signals System

Move away from the centralised event/signal system the is in place for
ApplicationEventHandler interface.

There are many ways to implement event propagation/signals. The two more common techniques are using a Listener interface or Anonymous signalling system. Both use function or class member pointers to communicate data to other areas of code. 

A Listener interface has some level of class coupling via the Listener interface. An anonymous signal system has no coupling between classes.

When a class is listening/connected to a signals of another class there are various class lifetime management issues to be taken into consideration. When methods of each class are executed in more than a single thread context it complicates things further.

A signalling system that is used in a context with multiple threads has to guarantee thread safety.

When a signal is emitted the functor that is dispatched will always know in which context the callback should be executed in. In the case of a Gtk+ based UI the callback should be executed in the default Gtk+ context. A class could connect the same signal handler to several signals. These signals could be emitted in several different threads, which means the class needs an internal event processing mechanism to process the events in the right thread/context.

There are two options: A pointer to a class instance could be passed to the signal connection interface so that when a signal is emitted it can be emitted in the context that has been registered. This has the advantage in that 

The signal could be emitted in two ways, either syncronously(sync) or asyncronously(async). If it is emitted in sync it means that the callback must complete(in another thread/context) before returning to the current execution context/thread.  


Some signals such as those that call for a dropping of any references when a
class instance is destroyed are required to be emitted syncronously which
means that...

Object
 - SignalConnectionUP connect_specific_signal (context, std::function<type>)
 - void disconnect_signal (signal_id, Callback)

Context class
 - call_sync (std::function<void()>)
 - call_async (std::function<void()>)

Signal class list of Callbacks

## Memory Management

How should references to mojo:: Objects be exposed?
- via raw pointers
- via weak_ptr
- create via factory returning unique_ptr and then manage in libmojo internally and expose raw pointers

## Misc

Implement checks for memory allocations in RT threads via operator new/malloc etc
Aim for an API with no raw pointers?

rename typedef.h headers to types.h

Transport should be in Project not Application

Should there be a Session class that has an Engine and a Project?
Should Session be a singleton or should we allow several Sessions to exist in
the same Process?

Prevent any mojo headers being included directly? Only include mojo.hpp?

Use Glib::Module and remove mojo/utils/library? or rename mojo::Library to
mojo::Module and mojo::Module to mojo::Mojule. No too hard to differentiate
when saying them.

Add ardour/jack_utils code to JackAudioDriverModule to get devices

Merge Worker/ApplicationWorker with gleam::dispatcher?

Rename ApplicationWorker FunctorDispatcher and inherit from gleam::ManualDispatcher

include facility for startup messages during initialization? using mojo::log

## Modules

Make a generic module infrastructure for libmojo, modules may include

Audio/Processor module

PannerModule

AudioFileModule:
- SndfileAudioFileModule
- CoreaudioAudioFileModule
- MadAudioFileModule

MidiFileModule:
- SMFMidiModule
	
AudioEffectModule:
- LADSPAEffectModule
- LV2EffectModule
- VSTEffectModule
- DXEffectModule

InstrumentModule:
- VSTInstrumentModule
- DXInstrumentModule

AudioDriverModule:
- JACKAudioDriverModule
- ALSAAudioDriverModule
- PulseAudioDriverModule
- PortAudioDriverModule
- VSTAudioDriverModule

AudioResamplerModule:
- SRCResamplerModule
- SpeexResamplerModule

ProjectImportModule
- ArdourImportModule
- AAFImportModule

ProjectExportModule
- ArdourExportModule
- AAFExportModule

Try having only one instance of a Module class per module, change
the module function to mojo_module_init () that allocates Module
class(if init hasn't already been called) and returns a pointer to
it, also add a mojo_module_fini () function to deallocate any
resources allocated by init for proper shutdown.

should be able to have built in modules aswell as external modules

libmojo must expose a way to discover new modules

modules should only have to link to a small core library if at all

Should all platform dependent code be in a base library
which modules can link to? possibly in libgleam

## Tests

rename mojo/tests/test_log to test_logging

Tests should try to provide full coverage of API

All tests should also test that undo/redo work properly. one way to do
that would be to make a copy of an object then change it, check for inequality
then call undo and check for equality. also copy the changed state(before undo)
, call redo and check for equality

Create a complex new project using all data types, save, close and restore.

char encoding tests etc

Event

- split events in a Sequence
- nudge forward events
- nudge backward events

Sequence

Test for Audio device sync/xrun at different settings with simulated load

Test for disk/storage read and write speed

Test for ability to write large files(> 4Gb)

Change string_convert test so that it changes locale and tests that there
is no effect on conversion


# SMPTE

Use libltc

smpte::time interface with implementations for each time type
vs
one class with enum for each type(4)

The later may be slightly slower due to branching.

conversion to/from ltc/vitc/mtc.

# RT Graph

The graph must be directed and acyclic

graph processing must be able to support rt safe processing

There must be able to be more than one graph per process

A graph contains a number of nodes

The graph cannot have parallel edges between nodes?

Can a graph contain isolated nodes(no edges)?

A source node is a node with no incoming edges

A sink node is a node with no outgoing edges

A processing node is a node with incoming and outgoing edges

Must be able to add and remove edges, although not while processing
graph.

The source nodes in the graph are processed first as they are
the source of the data and the edges are followed to process the
rest of the nodes.

The graph must be able to be processed in parallel.

Would breadth first processing be more cache friendly?

The API must expose a way to set how many threads are used to process
the graph. or leave threading out of the lib?

get_max_thread_count

get_thread_count
set_thread_count

// optional
get_optimal_thread_count

The test suite must be able to measure total process time

The test suite must be able to measure thread context switching time.

The test suite must be able to test the effect cache size has on processing
performance.

[ Application API ]

Application API should use method names like transport_* track_* project_* etc

Application API should have a sync method for testing(at least)

Should the mojo public API also be in C?

If the ApplicationWorker is calling a sync function and the UI thread is waiting for the
ApplicationWorker to finish then there is a deadlock?

If when the last project is removed can App::quit be called in an idle callback.

Move public headers into directory structure that mirrors what would be
if the headers were installed. Instead of include <mojo/mojo.hpp>
perhaps it should be include <mojo.hpp> and include <mojo/project.hpp> ertc

## Debug Macros

Need simple debug library for logging messages.
- only compiled in debug mode?
- M_DEBUG_ASSERT
- M_DEBUG_MSG just takes a string
- MOJO_DEBUG_DOMAIN has problem with amalgamation if used in multiple
  places with the same DEBUG domain name
- records line and file
- filters?
- optional namespace?
- command line args?
- MOJO_DEBUG env var
- header only?
- Debugging and logging separate API
- per thread logging streams?
- logging lib

waf test target

timer library for measuring execution time
- interface with backends
- macro for debug compiling
- records line and file
- per thread timers?
- BEGIN and END or RAII?
