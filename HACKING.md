# Adding New Tests, Testing New Nodes

The most common thing to do is to add a new test for a new feature.

## Adding A New Test

To add a new test, simply add a file starting with `test_` to the
tests/ directory.  Every function in this file starting with `test_`
will be run (the rest, presumably, are helpers you need).

For every test, there is a runner which wraps a particular node
implementation: using the default "DummyRunner" helps debug the tests
themselves.

A test consists of one or more Event (e.g. send a message, receive a
message), in a DAG.  The test runner repeats the test until every
Event has been covered.  The most important event is probably
TryAll(), which gives multiple alternative paths of Events, each of
which should be tried (it will try the "most Events" path first, to
try to get maximum coverage early in testing).

Tests which don't have an ExpectError event have a check at the end to
make sure no errors occurred.

## Test Checklist

1. Did you quote the part of the BOLT you are testing?  This is vital
   to make your tests readable, and to ensure they change with the
   spec.  `make check-quotes` will all the quotes (starting with `#
   BOLT #N:`) are correct based on the `../lightning-rfc` directory,
   or run `tools/check_quotes.py testfile`.  If you are creating tests
   for a specific (e.g. non-master) git revision, you can use `#
   BOLT-commitid #N:` and use `--include-commit=commitid` option for
   every commit id it should check.

2. Does your test check failures as well as successes?

3. Did you test something which wasn't clear in the spec?  Consider
   opening a PR or issue to add an explicit requirement.

4. Does it pass `make check-source` a.k.a. flake8?

## Adding a New Runner

You can write a new runner for an implementation by inheriting from
the Runner class.  This runner could live in this repository or in
your implementation's repository: you can set it with
`--runner=modname.classname`.

This is harder than writing a new test, but ultimately far more
useful, as it expands the coverage of every new test.