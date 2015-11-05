# libmojo

Decide on license

## Build System

Be able to build static binary with all modules built in. How will this affect
licensing? It won't as all non-GPL licensed code will remain in modules and
module API is appropriately licenced.

## Directory Structure

## C++11

Use range based for loops where applicable

Which ones of the following should be used? most have overlapping functionality
with glib. So is it better to rely on glib or c++ compiler seeing as both are
dependencies? Does it really matter if functionality of public API is the same.
It really only becomes an issue if for instance MSVC has bogus implementations
that require work-arounds etc.

switch to std:: smart pointer types

switch to std::bind where possible/necessary

use std::chrono

use std::function for connecting signals?

use std::atomic

use std::regex

use std:: random number generators

Replace pointer typedefs with c++11 using keyword

## Library dependencies

Exposing std:: types in headers is ok, or types that are due for
standardization like boost::filesystem::path.

Library dependencies should not be exposed in the public headers like glib or
boost. Any use should be wrapped in an implementation class or module/interface
this will allow changing the implementation class or using a specific
implementation/library etc on a certain platform or configuration

### Glib

Don't expose Glib in mojo headers

depend on glib for filesystem stuff? or convert between native paths and utf-8?
or use boost and install UTF-8 as default global encoding for narrow API?

### Boost libs

Don't expose boost in mojo headers...eventually

Use boost::filesystem?

Use boost::any?

Use boost::typeindex? in mojo/typesystem rather than std::type_info

use mojo::any alias for any boost classes so reimplementation etc is easier

### Juce libs

The JUCE library can be used under the terms of the GPL so it makes sense to
use it in some modules to avoid having to reimplement things like support
for plugin API's and audio driver API's etc.

Unlike Glib and Boost which are commonly distributed, it seems to make sense to
include JUCE source code in the tree and compile it. The best way to do this
would be to use submodule support of git.


## Core

Containers? typedef common std:: container types like vector/list/map with a
custom allocator?

Add StackVector similar to chromium

Add cppformat to core or add in external/submodules?

Add pugixml to core or add in external/submodules?

FilePath/filesystem::Path class instead of boost::filesystem::path?
benefit/cost. It would just be a simple class with is_utf8/16_encoded with
str() method to get a std::string. Then we can use external API for conversion
etc, or we could just use std::string for simplicity

Define NO_EXCEPT or just use noexcept? no need for compat with cpp03

Define MOJO_DECLARE_NON_COPYABLE

Rename DEFINE_ALL_TYPEDEFS to DEFINE_ALL_ALIASES

Add positional arguments to mojo::compose

Add single header and multiple implementation files and then select which will
be used/compiled at build time based on platform/options

data type converter int/float/etc implementation

AudioData class to hold audio data with specializations for data type/format
and channels etc. functions for accelerated de/interleaving etc

Add functions/class for swapping byte order

dither interface/implementation

Add unique pointer typedefs to mojo/typesystem/smart_pointer_macros.hpp

Make DebugRegistry thread safe

Add Thread debug data to MOJO_DEBUG

Add Quark implementation or properly wrap glib impl so glib types are not
exposed

Add core/system/threads.hpp to register thread names/memory pools for at least
debugging etc?

All methods are sync unless a "async" suffix is appended to the function/method
name

DONE - change samplerate_t type to double

DONE - add get_common_samplerates to core/audio/utils.hpp

define monotonic_time_t?

move core/time into core/system/

move filesystem/* to system/* or files/ ?

SystemInfo class like chromium with static methods? cross over with resource.hpp

Environment class with static methods?

CpuInfo class

Add mojo::debug::init/deinit? issue with static initialization order and
MOJO_DEBUG_DOMAIN macro

Add alias for mojo::path to boost::filesystem::path?

move mojo/core/audio/types.hpp to mojo/audio/types.hpp

std::thread type on mingw-w64 with gcc uses pthread/winpthread lib. Is this an
issue?

Move type names in core/typesystem/type_names.hpp somewhere more appropriate or
register them automatically in typesystem

Implement custom any type for TypeSystem?

Done - Add version of mojo::to_string that returns string not bool

Done - Copy string_convert.h and tests back from libpbd when finished/merged/not
merged/etc

Copy pbd/windows_timer_utils.h back into mojo/core/native

Copy pbd/windows_mmcss.h back into mojo/core/native

Add Context Interface to core?

Remove boost lib dependencies if possible or at least publicly exposed deps

Change fs::path to be std::string? or just change modules to take std::string

Add my code from pbd/file_utils.h to filesystem/utils.h that is relevant and
test

Add my code for setting/resetting time_begin_period from ardour branch

Context needs call_sync and call_async methods, call async for normal events
call_sync for disposing references in other thread contexts.

ContextManager Singleton class so that classes can use named contexts to
schedule changes, rather than explicit locking? needs further thought

mojo could offer a way to register a Context with a thread so that when
registering

Add float/double comparison to mojo/core/math.hpp with tests ref:
http://www.cygnus-software.com/papers/comparingfloats/comparingfloats.htm ref:
http://stackoverflow.com/questions/17333/most-effective-way-for-float-and-double-comparison

Add unique_ptr wrappers for std::FILE handles ref:
http://codereview.stackexchange.com/questions/4679/shared-ptr-and-file-for-wrapping-cstdio-update-also-dlfcn-h

## Project class

Should only be one method to add and remove tracks?

## New Signals System

Move away from the centralised event/signal system the is in place for
ApplicationEventHandler interface.

There are many ways to implement event propagation/signals. The two more common
techniques are using a Listener interface or Anonymous signalling system. Both
use function or class member pointers to communicate data to other areas of
code. 

A Listener interface has some level of class coupling via the Listener
interface. An anonymous signal system has no coupling between classes.

When a class is listening/connected to a signals of another class there are
various class lifetime management issues to be taken into consideration. When
methods of each class are executed in more than a single thread context it
complicates things further.

A signalling system that is used in a context with multiple threads has to
guarantee thread safety.

When a signal is emitted the functor that is dispatched will always know in
which context the callback should be executed in. In the case of a Gtk+ based
UI the callback should be executed in the default Gtk+ context. A class could
connect the same signal handler to several signals. These signals could be
emitted in several different threads, which means the class needs an internal
event processing mechanism to process the events in the right thread/context.

### Option 1: pass in pointer to "Context" class

There are two options: A pointer to a class instance could be passed to the
signal connection interface so that when a signal is emitted it can be emitted
in the context that has been registered.

This has the disadvantage that a pointer has to be stored which would increase
memory usage. Might not matter but it depends how many signals/listeners etc.

The signal could be emitted in two ways, either syncronously(sync) or
asyncronously(async). If it is emitted in sync it means that the callback must
complete(in another thread/context) before returning to the current execution
context/thread.

### Option 2: signal handler determines Context to process signal

The other option is to just pass in a normal function
pointer/lambda/std::function object and then when the callback/function is
executed the handler calls the appropriate context with either sync/async
depending on the requirements of the callback.

This means that it is the responsibility of the handler to get the sync/async
semantics correct. If the object that emits the signal expects all references
to be dropped and that the handler called syncronously but the handler doesn't
then there may be a issue.

Object
 - SignalConnectionUP connect_specific_signal (context, std::function<type>)
 - void disconnect_signal (signal_id, Callback)

Context class
 - call_sync (std::function<void()>)
 - call_async (std::function<void()>)

Signal class list of Callbacks

## Misc

DONE - Use MOJO_AMALGAMATED define throughout mojo lib instead of
MOJO_CORE_AMALGAMATED, MOJO_APPLICATION_AMALGAMATED etc

Rename mojo::Library to mojo::DynamicLibrary and have it as a final class that
has a glib implementation

rename typedef.h headers to types.h

Transport should be in Project not Application

Should there be a Session class that has an Engine and a Project?  Should
Session be a singleton or should we allow several Sessions to exist in the same
Process?

Prevent any mojo headers being included directly? Only include mojo.hpp?

Add ardour/jack_utils code to JackAudioDriverModule to get devices

Merge Worker/ApplicationWorker with gleam::dispatcher?

Rename Worker to Dispatcher

Rename ApplicationWorker FunctorDispatcher and inherit from
gleam::ManualDispatcher?

## Portaudio Module

Investigate whether UNICODE has to be defined to get UTF-8 encoded device
names?

## Modules

DONE - Make a generic module infrastructure for libmojo

Add --no-modules build option to build without any external library
dependencies

Add mojo-interfaces library that amalgamates all the interfaces into one
module?

Modules are located in there own directory and should not have to depend on any
other library but in practice will depend on mojo-core. This allows would allow
out of tree modules that don't depend on any mojo libraries. It also makes it
easier to use the interface and implementation in external code(ARDOUR)

Restrict interfaces definitions so that they don't require any external libs
and only use builtin types.  This suggests it better to use std::string for
path strings everywhere and just assume/enforce? it is UTF-8 encoded and
modules need to manage encoding conversion internally if using platform API's
that require a different encoding/wide strings etc.

Change module install path to be in /lib/$PRODUCT_EXE_NAME directory and not in
/bin

Try having only one instance of a Module class per module, change the module
function to mojo_module_init () that allocates Module class(if init hasn't
already been called) and returns a pointer to it, also add a mojo_module_fini
() function to deallocate any resources allocated by init for proper shutdown.

should be able to have built in modules aswell as external modules

libmojo must expose a way to discover/refresh new modules installed while
application is running

modules should only have to link to a small core library if at all

Should all platform dependent code be in a base library which modules can link
to? possibly in libgleam

Build Dummy/Skeleton module for each interface type that doesn't link to
mojo-core to test that module implementations aren't required to link to
mojo-core. mojo-core.hpp will be included by all modules for at least
mojo::Module but it should only need type definitions in headers etc.

Add TestModule implementation for each interface/module type

### Audio/Processor module

### PannerModule

### AudioFileModule

- SndfileAudioFileModule
- CoreaudioAudioFileModule
- MadAudioFileModule

### MidiFileModule

- SMFMidiModule

### AudioEffectModule

- LADSPAEffectModule
- LV2EffectModule
- VSTEffectModule
- DXEffectModule

### InstrumentModule

- VSTInstrumentModule
- DXInstrumentModule

### AudioDriverModules

- JACKAudioDriverModule
- ALSAAudioDriverModule
- PulseAudioDriverModule
- PortAudioDriverModule
- VSTAudioDriverModule

remove get suffix from all AudioDevice methods as there is no set

Add AudioDevice::open without input/output/samplerate parameters that just
opens the device with defaults?

Rename AudioDriver to AudioDeviceManager?

Define midi/audio_driver_error_t outside of MIDI/AudioDevice class? may
simplify return type definition in source files

Add DevicesChanged callback to AudioDriver

Add manual refresh_devices method to AudioDriver?

Add stop/close_devices method to AudioDriver and call on AudioDriver dtor?

Add AudioDriver::get_default_input_device

Add AudioDriver::get_default_output_device
AudioDriverModule:

portaudio returns half duplex devices for WMME, DirectX and WDMKS host api's
ASIO does return full duplex devices. When opening a stream in portaudio a
different input and output device parameter can be specified. but to open a device in full
duplex mode the AudioDevice interface will have to change to open via a stream
class or perhaps discovering which input and output device id's refer to the
same device so AudioDevice represents only a full duplex device.

Make portaudio audio driver optional

Test for sndfile library in sndfile module

Use consistent module file/target names

### MIDIDriverModule

- PortMIDIDriverModule
- WinMMEDriverModule

### AudioResamplerModule

- SRCResamplerModule
- SpeexResamplerModule

### ProjectImportModule

- ArdourImportModule
- AAFImportModule

### ProjectExportModule

- ArdourExportModule
- AAFExportModule

# SMPTE

Use libltc

smpte::time interface with implementations for each time type vs one class with
enum for each type(4)

The later may be slightly slower due to branching.

conversion to/from ltc/vitc/mtc.

# RT Graph / Engine

The graph must be directed and acyclic

graph processing must be able to support rt safe processing

There must be able to be more than one graph per process

A graph contains a number of nodes

The graph cannot have parallel edges between nodes?

Can a graph contain isolated nodes(no edges)?

A source node is a node with no incoming edges

A sink node is a node with no outgoing edges

Must be able to add and remove edges, although not while processing graph.

The source nodes in the graph are processed first as they are the source of the
data and the edges are followed to process the rest of the nodes.

The graph must be able to be processed in parallel(multiple threads).

Would breadth first processing be more cache friendly?

The API must expose a way to set how many threads are used to process the
graph. or leave threading out of the lib?

Graph processing threads may need to be created with a priority relative to other
threads in the process so thread creation may need to be delegated.

get_max_thread_count

get_thread_count set_thread_count

// optional get_optimal_thread_count

The test suite must be able to measure total process time

The test suite must be able to measure thread context switching time.

The test suite must be able to test the effect cache size has on processing
performance.

# Application API

Use `private_macros.hpp` in Application class and more all `*_internal` methods
to the private class

Application API should have a sync method for testing(at least)

Should the mojo public API also be in C? No

If the ApplicationWorker is calling a sync function and the UI thread is
waiting for the ApplicationWorker to finish then there is a deadlock?

DONE - Move public headers into directory structure that mirrors what would be if the
headers were installed. Instead of include <mojo/mojo.hpp> perhaps it should be
include <mojo.hpp> and include <mojo/project.hpp> ertc

Move Project classes into project directory

## Memory

Add some form of class instance leak detector

Add compile time define to enable process wide allocation checking via global
new/delete operators?

Add interface to allocators to check that no allocations have occurred between
calls of begin_check/end_check or some similar interface

Move MemoryPool implementation out of header and make implementation of stack
opaque so boost headers are an implementation detail.

Add mojo::aligned_alloc to core/memory/alloc.hpp?

How should references to mojo:: Objects be exposed?
- via raw pointers
- via weak_ptr
- create via factory returning unique_ptr and then manage in libmojo internally
  and expose raw pointers

- A [StackAllocator](https://src.chromium.org/chrome/trunk/src/base/containers/stack_container.h)

## lockfree

Move Pool into lockfree directory

Move RingBuffer class into lockfree directory

## Misc

Use boost::lockfree::queue in FunctorDispatcher


Move WorkerThread from ApplicationData into core/misc

Test for Worker, FunctorDispatcher and WorkerThread

## Logging/Debug Macros

Add log levels? get/set log handlers? for GUI display etc

rename mojo/core/logging/ mojo/core/log?

Add Timing data logging to MOJO_DEBUG

Use mojo::log for startup messages during initialization?

Add M_ASSERT macro

Logging library requirements:

- A compose template that only uses stack allocated memory
- log messages are composed using memory from the stack and then copied to a
  string from a pool allocator before being pushed to a lockfree logging queue.
- Don't expose any macros publicly, just the facilities to build them
- Macro for logging execution time of a particular block..M_LOG_TIME_BLOCK(DOMAIN, MSG)
  or just have it as part of `M_LOG` macro and be able to enable with define?
- M_LOG_TIMER(DOMAIN, NAME), M_LOG_TIME_START(TIMER)/M_LOG_TIME_STOP(TIMER)
- Rename DebugRegistry mojo::LogDomainRegistry
- Change MOJO_DEBUG_DOMAIN to `M_LOG_DECLARE_LOGGER` that defines a logger
  with a name and defines a specific macro for that domain like
  M_LOG_DECLARE_LOGGER(MMCSS) then using the logger via M_LOG_MMCSS("message")
- Rename MOJO_DEBUG to M_LOG
- add mojo/core/logging/log_domains.hpp to expose log domains
- rename MOJO_DEBUG_MSG to M_DEBUG_LOG
- MOJO_DEBUG_DOMAIN has problem with amalgamation if used in multiple source
  files with the same DEBUG domain name as it will cause double definition of
  variable. This could be solved by defining all debug domains in one source
  file and then including this first in
  common_source_includes.hpp or mojo-core.cpp that way all source files have
  access to all debug domains and aren't limited to only those declared in the
  source file.
- records line and file and time
- filters?
- optional namespace?
- command line args?
- MOJO_DEBUG env var
- header only?
- Debugging and logging separate API?
- use boost::lockfree::queue in logging library to send messages
- have a logging thread that emits callbacks/processes messages
  messages/strings?
- RT safe logging by default
- log streams, send to file, pipes etc for remote viewing
- log viewing
- Rather than checking if a logging domain is active, each logging domain has a
  pointer to a log instance. At library initialization time all the 'loggers'
  are created and registered then a reference is used to log all messages.
- filtering logging domains only at library initialization time is not
  sufficient, filtering at runtime is necessary.

waf test target build a single test executable

timer library for measuring execution time
- interface with backends
- macro for debug compiling
- records line and file
- per thread timers?
- BEGIN and END or RAII?
