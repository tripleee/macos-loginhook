# macos-loginhook

Simple `launchctl` hook for running scripts at login.

Rather than tweak your `launchctl` configuration, just add this one hook;
then drop whatever scripts you want to run at login into `~/.login.d/`.

This is intended for things you want to run
even before you start a shell instance.
Good examples of things you might want to do include:

* Amend your `PATH` for interactive programs run from the GUI.
* Load your SSH keys just once, automatically.
* Tweak keybindings.

To deploy, `git clone` this repository and run the `install` script.
