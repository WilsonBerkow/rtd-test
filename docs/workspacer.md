# workspacer

Workspacer allows newbies to save their work at the end of meetings, (while on
Guest sessions), and allows us to give them class skeletons, documentation
files (without giving access to the internet) and software.

## Our presense on the school network

We have two accounts on the school network, `robotics` and `robotics-entry`,
which are the sole members of the group `robogroup`. Both are in the
`students` group and categorized with the class of 2019 (home folders
are `/home/students/2019/robotics*`). **This means that before 2019
these accounts must be moved, lest they be deleted with the graduating
class.**

On the computers in room 451, files related to `workspacer` (and software
that we need installed for them to use OpenCV) are in `/var/tmp/robo*`.
`/var/tmp` is local to each machine (as opposed to home folders which
are synchronized across machines by network magic) and is accessible
to Guest sessions. Software we have installed to `/var/tmp/robo-software`
include CMake, JDK 8, Apache Ant, and OpenCV.

All of this setup should be run from `robotics`. `robotics` is the
secure side of this; `robotics-entry` is the insecure side and should
not be used for anything nontrivial. The passwords are in the SE vault
on 1Password.

The current system works with a set of `setuid`-enabled executables
owned by `robotics` which can be run by members of `robogroup`
(`setuid` means that it executes with the permissions of the owner,
instead of the permissions of the user that ran the file). `setuid`
executables cannot be run by Guest sessions; when a newbie runs
a command like `save` or `make-user`, the script SSHes into localhost
as `robotics-entry` (validating with an SSH key that is copied onto
the host machine by `setup-all.sh` before the meeting), which in
turn runs the `setuid` executables to update the user credentials
list (which is not tracked by git).

Yes, this is much worse than running a webserver which the newbies
connect to through the machines in the lab.

## Setting up this repo

To set up an instance of this project:

    $ touch round2/users.csv
    $ mkdir round2/their-work
    $ ln -s /path/to/repo/round2 ~/round2
    $ ln -s /path/to/repo/client2 ~/client2

And the necessary libraries must be in `~/workspace-libs` (jfxrt.jar and other amd files).
These exist on the `robotics` user account.

## Setting up machines in lab

`copy-docs-all.sh` will copy the Java SE 8 API docs to all machines in rm. 451
which do not have them (at `/var/tmp/javase8-api`).

    $ bash copy-docs-all.sh

`setup-all.sh` copies necessary libraries for JavaFX to the machines that do
not have them (at `/var/tmp/robo-libs`), and updates the necessary plaintext
files on all machines.

    $ bash setup-all.sh

Between meetings (preferably at the end of each meeting, for security's sake),
`make-ssh.sh` should be run:

    $ bash make-ssh.sh
    $ bash setup-all.sh

After running `make-ssh.sh`, `setup-all.sh` *must* be run
again in order for the students to work.
