# Task 001 implement artefactTypes

- **Completed**: 2021-09-20

## Goal

Develop the interface between the rulesManager and the artefactManager to
be able to register new types of artefacts. This provides a common
language of artefactTypes across the ComputePods.

## Problem

## Solution

## Tasks

1. Develop and test settling timer mixin

2. Develop and test NATListener mixin

3. send artefact.register.type messages (ruleManager)

4. listen for arteface.register.type messages (artefactManager)

5. store artefactType messages (artefactManager)

## Questions

- **Q**: How do we test the end-to-end messages send/reception?

    **A**: We develop a settling timer mixin which tacks all changes to the
    parent class and returns settled after any configured timeout period in
    which no changes have been reported.

    This mixin can be configured to send a message whenever it has settled.
    This allows the testing code to wait for this message and then test the
    parent class for various properties.

- **Q**: How do we ensure these messages abide by the published interface?

    **A**: We develop a NATS listener/checker mixin class which:

      - Listens to all registered subjects
      - Can be configured to store all messages
      - Can be asked various simple questions about stored messages.
