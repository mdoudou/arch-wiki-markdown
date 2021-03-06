fish
====

fish (the friendly interactive shell) is a user friendly command line
shell intended mostly for interactive use.

Contents
--------

-   1 Installation
-   2 Input/output
    -   2.1 File descriptors
    -   2.2 Redirection
    -   2.3 Piping
-   3 Configuration
    -   3.1 Web interface
    -   3.2 Prompt
    -   3.3 Command completion
-   4 Troubleshooting
    -   4.1 su launching Bash
-   5 Current state of fish development
-   6 See also

Installation
------------

Install fish from the official repositories.

To verify that it has been installed you can run:

    $ grep fish /etc/shells

If you want to make fish your default shell run:

    $ chsh -s /usr/bin/fish

Input/output
------------

> File descriptors

Like other shells, fish lets you redirect input/output streams. This is
useful when using text files to save programs output or errors, or when
using text files as input. Most programs use three input/output streams,
represented by numbers called file descriptors (FD). These are:

-   Standard input (FD 0), used for reading (keyboard by default).
-   Standard output (FD 1), used for writing (screen by default).
-   Standard error (FD 2), used for displaying errors and warnings
    (screen by default).

> Redirection

Any file descriptor can be directed to other files through a mechanism
called redirection:

    Redirecting standard input:
    $ command < source_file

    Redirecting standard output:
    $ command > destination

    Appending standard output to an existing file:
    $ command >> destination

    Redirecting standard error:
    $ command ^ destination

    Appending standard error to an existing file:
    $ command ^^ destination

You can use one of the following as destination:

-   A filename (the output will be written to the specified file).
-   An & followed by the number of another file descriptor. The output
    will be written to the other file descriptor.
-   An & followed by a - sign. The output will not be written anywhere.

Examples:

    Redirecting standard output to a file:
    $ command > destination_file.txt

    Redirecting both standard output and standard error to the same file:
    $ command > destination_file.txt ^ &1

    Silencing standard output:
    $ command > &-

> Piping

You can redirect standard output of one command to standard input of the
next command. This is done by separating the commands by the pipe
character (|). Example:

    cat example.txt | head

You can redirect other file descriptors to the pipe (besides standard
output). The next example shows how to use standard error of one command
as standard input of another command, prepending standard error file
descriptor's number and > to the pipe:

    $ command 2>| less

This will run command and redirect it's standard error to the less
command.

Configuration
-------------

User configurations for fish are located at ~/.config/fish/config.fish.
Adding commands or functions to the file will execute/define them when
opening a terminal, similar to .bashrc.

> Web interface

The fish prompt and terminal colors can be set with the interactive web
interface:

    fish_config

Selected settings are written to your personal configuration file. You
can also view defined functions and your history.

> Prompt

If you would like fish to display the branch and dirty status when you
are in a git directory, you can add the following to your
~/.config/fish/config.fish:

    # fish git prompt
    set __fish_git_prompt_showdirtystate 'yes'
    set __fish_git_prompt_showstashstate 'yes'
    set __fish_git_prompt_showupstream 'yes'
    set __fish_git_prompt_color_branch yellow

    # Status Chars
    set __fish_git_prompt_char_dirtystate '⚡'
    set __fish_git_prompt_char_stagedstate '→'
    set __fish_git_prompt_char_stashstate '↩'
    set __fish_git_prompt_char_upstream_ahead '↑'
    set __fish_git_prompt_char_upstream_behind '↓'
     
    function fish_prompt
            set last_status $status
            set_color $fish_color_cwd
            printf '%s' (prompt_pwd)
            set_color normal
            printf '%s ' (__fish_git_prompt)
           set_color normal
    end

> Command completion

fish can generate autocompletions from man pages. Completions are
written to ~/.config/fish/generated_completions/ and can be generated by
calling:

    fish_update_completions

You can also define your own completions in ~/.config/fish/completions/.
See /usr/share/fish/completions/ for a few examples.

Context-aware completions for Arch Linux-specific commands like pacman,
pacman-key, makepkg, cower, pbget, pacmatic are built into fish, since
the policy of the fish development is to include all the existent
completions in the upstream tarball. The memory management is clever
enough to avoid any negative impact on resources.

Troubleshooting
---------------

In Arch, there are a lot of shell scripts written for Bash, and these
have not been translated to fish. It is advisable not to set fish as
your default shell because of this. The best option is to open your
terminal emulator with a command line option that executes fish. For
most terminals this is the -e switch, so for example, to open
gnome-terminal using fish, change your shortcut to use:

    gnome-terminal -e fish

With LilyTerm and other light terminal emulators that don't support
setting the shell it would look like this:

    SHELL=/usr/bin/fish lilyterm

Another option is to set fish as the default shell for the terminal in
the terminal's configuration or for a terminal profile if your terminal
emulator has a profiles feature. This is contrast to changing the
default shell for the user which would cause the above mentioned
problem.

To set fish as the shell started in tmux, put this into your
~/.tmux.conf:

    set-option -g default-shell "/usr/bin/fish"

Not setting fish as system wide default allows the arch scripts to run
on startup, ensure the environment variables are set correctly, and
generally reduces the issues associated with using a non-Bash compatible
terminal like fish.

If you decide to set fish as your default shell, you may find that you
no longer have very much in your path. You can add a section to your
~/.config/fish/config.fish file that will set your path correctly on
login. This is much like .profile or .bash_profile as it is only
executed for login shells.

    if status --is-login
            set PATH $PATH /usr/bin /sbin
    end

Note that you will need to manually add various other environment
variables, such as $MOZ_PLUGIN_PATH. It is a huge amount of work to get
a seamless experience with fish as your default shell.

> su launching Bash

If su starts with Bash (because Bash is the default shell), define a
function in fish:

    $  funced su
    function su
            /bin/su --shell=/usr/bin/fish $argv
    end
    $ funcsave su

Current state of fish development
---------------------------------

The original developer Axel Liljencrantz abandoned the project in 2011.
The rest of his team slowly took over and transferred the codebase to
gitorius. This legacy version is available in the AUR as fish-git.

On May 30, 2012 ridiculous_fish announced a new fork of Fish which was
adopted as mainstream later, and development is now relocated to GitHub.
The AUR package fish-shell-git follows the head branch of that, while
the fish package in the official repositories provides latest stable
milestones as announced on the webpage.

Ridiculous_fish announced fish 2.0 stable version on May 17th, 2013.

See also
--------

-   http://fishshell.com/ - Home page
-   http://fishshell.com/docs/2.1/index.html - Documentation

Retrieved from
"https://wiki.archlinux.org/index.php?title=Fish&oldid=291192"

Category:

-   Command shells

-   This page was last modified on 31 December 2013, at 19:17.
-   Content is available under GNU Free Documentation License 1.3 or
    later unless otherwise noted.
-   Privacy policy
-   About ArchWiki
-   Disclaimers
