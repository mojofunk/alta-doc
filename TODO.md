# General

Add license headers to files where necessary, using scripts to add
copyright headers and update year etc

Add script to tools/ to add/replace vim/emacs file formatting directives
to the end of all files

DONE - Add client side pre-commit hook to check for source code formatting
Add server side hook to check for source code formatting?

Add client side pre-commit hook to check for wscript formatting

# Build System

DONE - Automatic formatting of python code

make each sub wscript standalone

DONE - Add script to check source with the clang scan-build tool

Add --with-docs build option to build doxygen based docs

Add test target?

# Libgleam

make dispatcher and interface and have ManualDispatcher and AutomaticDispatcher

Copy Timer implementation from libpbd and modify to depend on glib

only depend on glib, not glibmm?

# Docs

100% source code documentation

Dev guide section on typical development workflow.

Dev guide section on clang-format and vim integration

Dev guide section on tabs vs spaces and using tabs for indentation

Dev guide section on including headers using " instead of <

Dev guide section on using on the right style const placement

Dev guide section on why _t shouldn't be used for typedefs but why we do anyway

Convert docs to publician?

Update section on header include guards include style where there is no class
defined in the file. eg mojo/core/typedefs.hpp -> MOJO_CORE_TYPEDEFS

Add section on typedefs for Glib::RefPtr and boost::shared_ptr

Section on libmojo style forward class decl and typedefs.

Section on file name standard for test files

Add README
