# Release Management

The release cycle is made up of roughly four stages or time periods. They are
the Release Planning Period, Development Period, Testing Period and Release Period.

## Release Planning Period

## Development Period

## Testing Period

## Release Period

# Time Based Release Cycle

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

All code changes proposed for merging into master branch during the development
period must exist and be accessible for review before the development period
starts.

All code committed during the development period must be reviewed and approved
by the lead developers before the period starts.

New library dependencies can only be introduced during development period and
must be approved by platform maintainers.
 
# Goal/Feature Based Release Cycle

It may be necessary for some large features or for other reasons to change from
a time based release cycle to a goal or feature based release cycle. If a new
feature or set of changes is very disruptive and affects are large portion of
the code base it may not be possible to use a time based release cycle.

# Release Support

The last release should be the only supported release of the software.
