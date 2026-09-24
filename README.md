# macos-loginhook

Simple `launchctl` hook for running scripts at login.

Rather than tweak your `launchctl` configuration,
just add this one hook;
then drop whatever scripts you want to run
at login into `~/.login.d/`.

This is intended for things you want to run
even before you start a shell instance.
Good examples of things you might want to do include:

* Load your SSH keys just once, automatically.
* Launch background processes (like periodic reminders).
* Run startup scripts that don't need GUI interaction.

To deploy, `git clone` this repository and run the `install` script.

If you have `sudo` access, also run `sudo ./sudo-install`
to add your local `bin` directories to `/etc/paths.d/`

## `~/.login.d` vs `~/.session.d`

`~/.login.d` scripts are sourced once, in sequence, at login.
They are meant for quick, one-shot setup tasks that run to
completion without needing to retry or persist.

`~/.session.d` scripts are handed to `session-daemon`,
a separate `launchd` agent that starts each script and
restarts it whenever it exits.
This fits scripts that should keep running for the whole
session (a periodic reminder, a VPN watcher).
It also fits a one-shot job that needs to wait on a slow
dependency becoming available (a container runtime, the
login keychain): have the script retry until ready, do its
job, then block (e.g. `podman wait`, or `sleep 2147483647`
if there is nothing to wait on) so the script's own lifetime
tracks the job's; `session-daemon` will then restart the
whole sequence if the job ever dies or a retry round gives
up. See `ssh-add` for an example: it retries loading SSH
keys from the keychain, then blocks once an identity is
added.

Do not drop a bare one-shot script (one that exits right
after kicking something off) into `~/.session.d`:
`session-daemon` will treat that exit as a crash and
restart it every 30 seconds.
