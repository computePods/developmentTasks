# Task 006 initial ComputePods command line interface

## Goal

We want a simple (POSIX) command line interface which can be used to query
and/or control a running federation of ComputePods.

## Problem

The overall complexity of the "things" which could potentially be queried
or controlled in a running federation of ComputePods is both (potentially)
very large, and more importantly highly extensible.

This means that this command line interface (`cpcli`) must also be well
structured and extensible.

We need to allow the user to have their own collection of `cpcli`
commands.

## Solution

We plan to use the `click` Python interface tool to provide a structured
and extensible command line interface.

The ComputePods NATS interface consists of a collection of NATS subjects
which "return" information via JSON messages. The `cpcli` will send
messages on various NATS subjects and then multiplex the resulting answers
before displaying the results to the user via stdout.

The user will be able to specify a (POSIX) directory of `cpcli` commands
which will be integrated into the overall `cpcli` command infrastructure.

## Tasks

1. **DONE** Provide initial extsensibility infrastructure.

2. **ONGOING** Provide some initial commands to prove that the interface
works.

3. Alter to allow the use of cpcli as a *testing* framework. This means we
should be able to write tests consisting of a FastAPI call followed by a
comparison of the result to a standard example. We should also be able to
check the result conforms to the required JSON specification.

## Questions

**Q**: How do we *dynamically* load click command "plugins" (to allow the
end user to add new commands to suit their needs)?

**Q**: How do we "reuse" click command "parts" (to allow an end user a
clean way to "chain"/"sequence" existing click commands)? How do we pass
arguments to click command parts?

## Resources

[click](https://click.palletsprojects.com/en/8.0.x/)

[click
documentation](https://click.palletsprojects.com/en/8.0.x/#documentation)

[click api](https://click.palletsprojects.com/en/8.0.x/#api-reference)
