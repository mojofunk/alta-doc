# General

Add license headers to files where necessary, using scripts to add copyright
headers and update year etc

# Build System

automatic formatting of python code

make each sub wscript standalone

# Libgleam

make dispatcher and interface and have ManualDispatcher and AutomaticDispatcher

Copy Timer implementation from libpbd and modify to depend on glib


# Docs

Dev guide section on including headers using " instead of <

Dev guide section on why _t shouldn't be used for typedefs but
why we do anyway

Convert docs to publician

Update section on header include guards include style where there
is no class defined in the file. eg mojo/core/typedefs.hpp ->
MOJO_CORE_TYPEDEFS

Add section on typedefs for Glib::RefPtr and boost::shared_ptr

Section on mojo/macros.hpp forward class decl and typedefs.

Section on file name standard for test files

Add README
