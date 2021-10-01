# Task 004 implement Tup based dependency computations

## Goal

Explicitly compute a build sequence from a potentially cyclic artefact
dependency graph.

## Problem

The ConTeXt typesetting system, like any TeX based system, requires
multiple passes to ensure all cross-references in the document are
correctly linked. This means that any dependency graph which involves
ConTeXt (as a single step task) *will* involve dependency cycles.

To keep the build sequence as small as possible (and the overall
computation as simple as possible), we will model our computation on those
suggested by the [Tup build system](http://gittup.org/tup/).

## Solution

## Tasks

1. Implement Project manager to provide target artefacts

2. Implement FileSystemWatcher to provide list of changed artefacts

3. build a build sequence graph with cycles

4. compute the corresponding a-cyclic build sequence graph by rolling on
the cycles

## Questions

**Q**: How do we deal with multiple "conflicting" types, rules, tasks, and
project registrations?

- **types**:

  - **needed for**: ??

  - **names might conflict**.... this is a problem of the ComputePods
    Chef maintainers.... how do we detect this early rather than at
    "runtime"?

- **rules**:

  - **needed for**: used by the build/dependency manager to identify which
    rule to ask which chef to perform to obtain a specific (set of)
    output(s).

  - **names might conflict**.... this is a problem of the ComputePods
    Chef maintainers.... how do we detect this early rather than at
    "runtime"?

  - since rules must be unique for a given chef, use chef-rule names
    globally.

- **tasks**:

  - **needed for**: define the dependency graph and record which rules to
    use to implement a given production in the build graph.

  - **names might conflict**.... this is much more likely, as many of
    these tasks will be semi-automatically created by, for example,
    ConTeXt as it typesets the documentation. Generally this might be a
    problem of the user's "documentation".

- **projects**:

  - **needed for**: to be presented to the user to allow the user to
    decide which targets are to be built.

  - **names might conflict**... but this is really a problem created
    by an oversight of the user when they specify the project.

- **chefs**:

  - **needed for**: to identify which worker will/can be asked to *do* the
    work.

  - **names might conflic**... no a running federation of ComputePods MUST
    have unique names for all of the Chefs (even if only unique per unique
    pod)

**Q**: How likely is it that registrations in a given user's project will
change?

- We have an essentially decentralised system, so the likelihood of
conflicting registrations is potentially fairly high.

- However... the original source texts for a given user's project come
from *one* user and so *should* not change too quickly, or fundamentally
contradict itself.

- There will be occasional radical change in the names and numbers of text
files making up a project... how do we deal with this sort of change?

**Q**: What is the problem with divergent registrations?

- Existing artefacts which are not registered... leads to missing
re-builds...

- Non-existing artefacts which are (still) registered... leads to build
failures.

- Periodic directory scans of the project directories could be used to
detect the above two cases... Then how do we deal with this? Send a report
to the user? HOW?

**Q**: How do we "retire" old registrations?

- When the system is re-started?

- After XX minutes... do a re-scan of the project directories looking for
registered artefacts which no longer exist, and unregistered artefacts
which do exist.

- What about transitory artefacts which only exist in one or more Chef
project workspaces? Do we automatically scan these directories as well?
How often will they conflict due to build system lags? When do we decide
to purge an "out-of-date" Chef's workspace? How do we detect that a Chef's
workspace is "out-of-date"?

- When the user requests a refresh? (Do we purge all Chef workspaces at
the same time?)

## Resources

- [Tup build system](http://gittup.org/tup/).

- [Tup Build System Rules and Algorithms
(PDF)](http://gittup.org/tup/build_system_rules_and_algorithms.pdf).
