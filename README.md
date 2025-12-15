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
