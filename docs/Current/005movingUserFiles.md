# Task 005 moving user files into and out of the ComputePods

## Goal

We would like the following three things:

1. to move user files (inputs and target outputs) from/to the user's
personal workspace.

2. to be able to detect when a user has altered a file in their personal
workspace.

3. to be able to add/remove a personal workspace without
rebuilding/changing the ComputePods.

## Problem

Up until recently we had planned to bind mount a "commons" directory into
the (local) ComputePod. Unfortunately, this forces a particular work
pattern on the user rather than allowing the user to put their personal
workspace where ever they want.

We also have the problem, when the federation of ComputePods is spread
across remote computers, of moving files between ComputePods.

Equally importantly any file movement solution *must* not require
administrative powers. That is any "normal" user *must be able to make
what ever (small) configuration changes are required for themselves.

## Solution

We make the assumption that all computers used by a user are already
running a secure SSH server. All file transfers will make use of one or
more SSH channels.

For direct copying of *new* files (into a newly cleaned/created directory
in a ComputePod), we will use `scp`.

To update copies of existing files (inside a ComputePod), we will use
`rsync` (over ssh).

We will use a "star" configuration. The user's personal workspace is the
definitive repository for all changes. Any ComputePod which needs an
updated copy of a file will retrieve it directly from the *user's*
personal workspace. (This means that individual ComputePods do not need to
run an SSH server).

To implement this, the user must:

1. add ssh keys into their `~/.ssh/authorized_keys` file

2. use a copy of the `notify2nats` daemon to monitor their personal workspace

## Tasks

1. create a template python script to alter a user's authorized_keys file
with the correct rsync key. (Must download Rsync Samba's `rrsync` perl
script and update `rrsync` for our use)

2. alter `builder` to provide an SSH key for the federation's use for
`rsync` back to the user's ssh-server, as well a create the python script
to be used by the user.

3. implement a FileManager which can `rsync` files between the user's
personal workspace and a given ComputePod.

4. update `majorDomo` to register personal workspaces.

5. determine how a user will "run" the `majorDomo` tool.

## Questions

**Q**: Can/should `notify2nats` automagically register a new personal
workspace with the federation of ComputePods? (We have now brought
majorDomo outside of the pods and unified it with inotify2nats)

**A**: Yes it can... to do this it should have a command line option to
specify the "project" (so that multiple personal workspaces can be
"federated" inside a given project).

**Q**: How are files shared between pods inside a ComputePod? Do the pods
in a ComputePod share a bind mount, or do they have completely separate
files spaces?

**Q**: Must/should we have distinct keys for `scp` and `rsync` so that
these keys are limited to *only* running the respective tool?

## Resources

- [sshd: AUTHORIZED KEYS FILE
FORMAT](https://man.openbsd.org/sshd#AUTHORIZED_KEYS_FILE_FORMAT)

- [Limit ssh user to only use
rsync](https://blog.valqk.com/archives/Limit-ssh-user-to-only-use-rsync-92.html)
This forces the use of only one command with arguments... is this too
restrictive?

- [Restrict SSH to rsync ](http://gergap.de/restrict-ssh-to-rsync.html)
This includes an example Shell script.
