Sxhkd
=====

sxhkd is a simple X hotkey daemon. It is intended to replace xbindkeys.
Its forum thread is here.

Installation
------------

Download the sxhkd-git package from the AUR. Then, as a non-root user,
run:

    $ makepkg -i

while in the directory of the saved PKGBUILD. All the files will be
retrieved, and the package will be built and installed.

Configuration
-------------

sxhkd defaults to $XDG_CONFIG_HOME/sxhkd/sxhkdrc for its configuration
file. An alternate configuration file can be specified with the -e
option.

Each line of the configuration file is interpreted as so:

-   If it starts with #, it is ignored.
-   If it starts with one or more white space commands, it is read as a
    command.
-   Otherwise, it is parsed as a hotkey: each key name is separated by
    spaces and/or + characters.

General syntax:

    [MODIFIER + ]*[@|!]KEYSYM
        COMMAND

Where MODIFIER is one of the following names: super, hyper, meta, alt,
control, ctrl, shift, mode_switch, lock, mod1, mod2, mod3, mod4, mod5.

If @ is added at the beginning of the keysym, the command will be run on
key release events, otherwise on key press events.

If ! is added at the beginning of the keysym, the command will be run on
motion notify events and must contain two integer conversion
specifications which will be replaced by the x and y coordinates of the
pointer relative to the root window referential (the only valid button
keysyms for this type of hotkeys are: button1, ..., button5).

The KEYSYM names are those your will get from xev.

Mouse hotkeys can be defined by using one of the following special
keysym names: button1, button2, button3, ..., button24.

The hotkey can contain a sequence of the form (STRING_1,…,STRING_N), in
which case, the command must also contain a sequence with N elements:
the pairing of the two sequences generates N hotkeys.

In addition, the sequences can contain ranges of the form A-Z where A
and Z are alphanumeric characters.

What is actually executed is SHELL -c COMMAND, which means you can use
environment variables in COMMAND.

SHELL will be the content of the first defined environment variable in
the following list: SXHKD_SHELL, SHELL.

If sxhkd receives a SIGUSR1 signal, it will reload its configuration
file.

Retrieved from
"https://wiki.archlinux.org/index.php?title=Sxhkd&oldid=283521"

Categories:

-   Keyboards
-   X Server

-   This page was last modified on 18 November 2013, at 12:45.
-   Content is available under GNU Free Documentation License 1.3 or
    later unless otherwise noted.
-   Privacy policy
-   About ArchWiki
-   Disclaimers
