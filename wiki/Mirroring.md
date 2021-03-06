Mirroring
=========

Arch Linux uses a 2-tier mirroring scheme. Tier 1 mirrors sync directly
from archlinux.org every hour and all other mirrors sync from one of the
tier 1 mirrors. Syncing from archlinux.org is not allowed.

For a guide on how to select and configure mirrors for your pacman
installation, see Mirrors.

Contents
--------

-   1 Communication
-   2 How to create a official Arch Linux mirror
    -   2.1 Conform with requirements
        -   2.1.1 Tier 2 requirements
        -   2.1.2 Tier 1 requirements
    -   2.2 Create a feature-request

Communication
-------------

The arch-mirrors mailing list is used for announcements and
mirror-related communications. All mirror-administrators and Arch Linux
developers involved in mirroring subscribe to this mailing list.

How to create a official Arch Linux mirror
------------------------------------------

Note:pacman-mirrorlist only includes ftp URLs if no http URLs are
available in a given country.

Warning:We are not accepting new ftp mirrors.

Anyone who wishes to have a public Arch Linux mirror made a official
mirror should:

> Conform with requirements

Your mirror must conform with the mirror-requirements for the tier you
wish your mirror to belong to.

Tier 2 requirements

-   Disk-space >= 35 GB
-   Sync off a tier 1 mirror (see https://archlinux.org/mirrors)
-   Sync all contents of the upstream mirror (i.e. do not sync only some
    repositories)
-   Do not sync more often than every hour
-   Use the following rsync options: -rtlvH --delete-after
    --delay-updates --safe-links --max-delete=1000
-   Subscribe to arch-mirrors
-   http support

Tier 1 requirements

-   Tier 2 requirements
-   Bandwidth >= 100Mbit/s
-   rsync support
-   Proven reliability (be a tier 2 mirror for a while and have
    reasonable uptime, response to out-of-sync notifications etc.)

> Create a feature-request

Go to https://bugs.archlinux.org and create a feature-request (category:
mirrors) containing the following information:

-   Mirror domain name
-   Geographical location of the mirror (country)
-   Supported access methods (http, rsync)
-   URLs for the above access methods
-   An administrative contact email
-   An alternative administrative contact email (optional)
-   (tier 1 mirrors) Rsync IPs so your server(s) can be allowed to sync
    off tier 0 (rsync.archlinux.org)
-   (tier 2 mirrors) The name of the tier 1 mirror(s) you are syncing
    from (see tier 1 mirrors here:
    https://www.archlinux.org/mirrors/status/tier/1/)

Retrieved from
"https://wiki.archlinux.org/index.php?title=Mirroring&oldid=269726"

Categories:

-   Arch development
-   DeveloperWiki

-   This page was last modified on 4 August 2013, at 03:10.
-   Content is available under GNU Free Documentation License 1.3 or
    later unless otherwise noted.
-   Privacy policy
-   About ArchWiki
-   Disclaimers
