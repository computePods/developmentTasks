# Task 002 simple build messages

## Goal

The *real* goal is to populate the DependencyManager's dependency graph.

To do this, we start by developing the [simple "build"
messages](/interfaces/Build/):

- "howToBuild" : From the MajorDomo-DependencyManager to a
  Chef-RulesManager.

- "canBuildFrom" : From a Chef-RulesManager to a
  MajorDomo-DependencyManager.

## Problem

- Both the "howToBuild" and "canBuildFrom" messages make use of
  artefactType names which (for the MajorDomo) is contained in the
  ArtefactManager.

## Solution

## Tasks

1. Refactor settlingTimer to allow multiple timers per class/object.

2. provide list of artefact types (ArtefaceManager)

3. send "howToBuild" messages (DependencyManager)

4. receive "howToBuild" messages (RulesManager) and recognise whether or
   not a particular RulesManager understands how to build this artefact.

5. send "canBuildFrom" messages (RulesManager)

6. receive "canBuildFrom" messages (DependencyManager)

7. integrate "canBuildFrom" message into dependency graph
   (DependencyManager)

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
