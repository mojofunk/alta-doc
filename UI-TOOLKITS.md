# Toolkit Requirements

- Support for at least Windows, Linux, OS X.
- Support for the typical developer toolchains on the supported
	platforms/targets
- Open Source
- Well tested/Continuous Integration on all supported platforms
- Good documentation
- Well maintained
- ABI and API stable with no functionality changes that might break client code
- Support for at least 5 years with well defined update path to new versions
- Good community support
- C++ API or Bindings(that also meet the other requirements)
- In use by a variety of large clients/groups
- Large developer pool

# Toolkits

## Gtk+

What are the benefits/tradeoffs with using Gtk+ directly and removing
dependency on gtkmm. Current UI(such that it is) uses gtkmm but with ui files
and resources.

Gtkmm Positives vs Gtk+:
- C++ style API template based signals/events

Gtkmm Negatives vs Gtk+:
- More dependencies, makes MSVC build harder
- Not as well tested API wise as Gtk+?
- More overhead with wrapper/signal emission(possibly negligible)

Gtk+ Negatives:
- Fast moving API? deprecations etc ABI and API stable but has functional
	changes between major releases
- Cross platform support is weak

## QT

# Other UI Toolkit options

## Chromium

## Firefox

## Native/Custom
