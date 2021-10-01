# Task 003 build artefact dependency graph

**Completed**: 2021-09-28

## Goal

The *real* goal is to populate the ArtefactManager's dependency graph.

## Problem

## Solution

## Tasks

- Load project files (ProjectManager)

- Listen for project messages (ArtefactManager)

- Create GraphViz dot Artefact Dependency Graph

## Questions

**Q**: How and when do we build the dependency graph?

  - all at once using all known artefact types?

  - on demand when the user asks to build something?

**A**: We (re)build the dependency graph by walking over the list of known
artefact types and soliciting knowledge of how to build each specific
type.

**Q**: When a new artefact type is registered do we rebuild the whole
dependency graph?

**A**: Yes, we (re)build the dependency graph any time a new artefact type
is registered. (We use a settling timer to deal with artefact registration
storms).

**Q**: Do we need a range of different settling timers associated with
each manager?

**A**: Yes, since different tasks may need to settle at different rates.

**Q**: We have *two* distinct types of "howToBuild" and "canBuildFrom",
the generic question for any instance of a given artefact type, and a
specific message for a specific artefact (on disk). How do we distinguish
these two types of messages (and their answers)?

**A**: We need four messages:

- "howToBuild" : is a generic request for information

- "canBuildFrom" : is a generic answer to "howToBuild"

- "whoCanBuild" : is a specific request to actually build somthing

- "canBuild" : is a specific offer to build something in response to a
  "whoCanBuild".

In this task, we only focus on the generic request and response.

**Q**: How do we structure the dependency graph?

**A**: We use a dict keyed on artefact types. Each entry contains a list
of producers that contains dependencies, outputs, and names of the chefs
who can build these outputs. Each entry also contains a list of
consumers... Both lists of consumers and producers are keyed by the name
of their corresponding chefs.

**Q**: How do we deal with "duplicate" dependency canBuildFrom answers?
