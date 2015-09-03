# Release Management

## Time Based Release Cycle

The release cycle works around a time period. Time period may be defined by
internal or external factors. 

Internal factors may be things like availability of people developing the
software, holidays, contract periods etc.

External factors may be things like releases of library dependencies,
platform/OS etc.

For Alta I think the best time based release cycle is 6 months.

This time period was chosen because it corresponds to the release cycle
length of Gnome and major linux distributions such as Fedora and Ubuntu.

This should mean that each new release can get a greater amount of testing.

There are two phases of a release, the development period and the testing
period.

All code committed during the development period must exist and be accessible
for review before the development period starts.

All code committed during the development period must be reviewed and approved
by the lead developers before the period starts.

New library dependencies can only be introduced during development period and
must be approved by platform maintainers.
 
## Goal/Feature Based Release Cycle
