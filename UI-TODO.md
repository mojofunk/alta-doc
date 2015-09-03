# User Interface Goals

Open/Display/Play an audio file

Open/Display/Play a MIDI file


# User Interface Implementation

ui::Application inherits from Gtk::Application or stick to composition?

Track Canvas
- change height from the track view list
- add border/frame to TrackViewListItem

Add ToolButtons class

Rename TransportToolbar TransportButtons

Rename gmojo_search_path to alta_search_path

Add Stop, Locate to start, Locate to End buttons to TransportButtons

Add Classes:
- Inspector
- StatusLine
- InfoLine
- ProjectOverview

Add WindowLayoutDialog and button to ProjectWindowToolbar

# Gtk+

What are the benefits/tradeoffs with using Gtk+ directly and removing
dependency on gtkmm. Current UI(such that it is) uses gtkmm but with ui files
and resources.

Gtkmm Positives vs Gtk+:
- C++ style API template based signals/events

Gtkmm Negatives vs Gtk+:
- More dependencies, makes MSVC build harder
- Not as well tested API wise as Gtk+?
- More overhead with wrapper/signal emission(possibly negligible)

Gtk+ Positives:

Gtk+ Negatives:
- Fast moving API? deprecations etc
- Cross platform support seems poor

# Other UI Toolkit options

QT
Chromium
Native/Custom
