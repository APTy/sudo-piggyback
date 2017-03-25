# sudo-piggyback

This proof of concept shows how to create a program that can override a
user's `sudo` command in order to run arbitrary code in privileged mode
without the user's knowledge.

### Usage

Install the sudo-piggyback by downloading this repo and running:

```
source add-sudo-piggyback
```

From here, your next `sudo` command should also execute the contents of our
own arbitrary privileged code. In this case, it's simply an `echo` command
that proves we are running as root.

### Discussion

This method relies on how the shell processes the user's `$PATH` variable to
find executables on the local file system. It overrides the normal `sudo`
program with a "bad" one.

Later, when a user wishes to run some command with `sudo`, we run our own
code first. Of course, since a user intends to run their command in privileged
mode, they enter the root password which will run our own code first, and then
run their command afterwards.

### Notes

This proof of concept does not adhere to very strict opsec, and a lot could be
done to hide and clean up the fact that this piggyback existed on the user's
machine. For example, immediately after running `sudo`, the "bad" sudo version
is deleted, but a cache of its path remains in the terminal session, causing
subsequent `sudo` calls to fail.

### Disclaimer

This project is for educational and research purposes only. The author is not
responsible for individuals who misuse these materials for illegal purposes.
