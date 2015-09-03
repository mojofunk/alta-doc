# libmojo Tests TODO

Add automated test executions for all compilers/platforms/build combinations

Fix tests as Boost.Test is not thread safe

Add more test strings for test_string_convert

Add functions to string/convert.hpp to set throw on parsing error or unexpected
input and also log errors

Tests need to be run for debug and release builds

Should tests be located in same directory(or tests subdir) as code or even same
file as code?

rename mojo/tests/test_log to test_logging

Add function to mojo/test_common.hpp to get a test::tmp_i18n_writable_directory

Use better names for BUILD_SINGLE_TESTS and MOJO_SINGLE_TEST_EXE to avoid
confusion

Use wscript test section from portaudio waf branch to generate tests without
having to have a test file list

Add test to save project/state async while performing random operations to
check for race conditions/locking/MT safe etc

Tests should try to provide full coverage of API

Add option to use gcov

Test that checks visibility

Write test output to a temp directory

Thorough testing of mojo::Typesystem is needed

Add init/deinit static functions to all "Singleton" classes

All tests should also test that undo/redo work properly. one way to do that
would be to make a copy of an object then change it, check for inequality then
call undo and check for equality. also copy the changed state(before undo) ,
call redo and check for equality

Create a complex new project using all data types, save, close and restore.

char encoding tests etc

test source code doc coverage

Event

- split events in a Sequence
- nudge forward events
- nudge backward events

Sequence

Test for Audio device sync/xrun at different settings with simulated load

Test for disk/storage read and write speed

Test for ability to write large files(> 4Gb)

Change string_convert test so that it changes locale and tests that there is no
effect on conversion

Write a TestAudioDevice that generates different test tones etc

Write a TestMIDIDevice that generates different MIDI event messages etc

