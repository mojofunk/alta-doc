# General

Add license headers to files where necessary, using scripts to add copyright
headers and update year etc

# Build System

automatic formatting of python code

make each sub wscript standalone

# Libgleam

make dispatcher and interface and have ManualDispatcher and AutomaticDispatcher

# User Interface

ui::Application inherits from Gtk::Application or stick to composition?

Track Canvas
	- change height from the track view list
	- add border/frame to TrackViewListItem

Add ToolButtons class

Rename TransportToolbar TransportButtons

Add Stop, Locate to start, Locate to End buttons to TransportButtons

Add Inspector class
Add StatusLine class
Add InfoLine class
Add ProjectOverview class

Add WindowLayoutDialog and button to ProjectWindowToolbar

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
