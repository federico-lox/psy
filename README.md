PathSYnchronizer
================

psy is a small utility that makes one-way sinchronization fast and easy.

Requirements
------------

psy is written in highly-portable BASH, so it will work in any environment
that provides a bash shell implementation, including Microsoft Windows.

The only dependency is on the [rsync](http://rsync.samba.org/) command
which is available for most platforms.

If you need to synchronize your tree to a remote destination via ssh you'll
also need to have ssh installed

For Windows I recommend installing [MSYS](http://www.mingw.org/wiki/MSYS)
which comes with both bash, rsync and ssh packed in and is also available
for [64bit hosts](http://sourceforge.net/apps/trac/mingw-w64/wiki/MSYS).

Installation
------------

Clone this repository to a local folder and add it to the PATH environment
variable to make it easily accessible.

Yep, that's it :)

Usage
-----

Run psy passing the *h* option to get usage instructions, here's a quick
reference:

```
Usage: psy [OPTIONS]

    OPTIONS:
        -d    Dry run mode, displays the sync command without running it.
        -e    Exclude pattern, see http://ss64.com/bash/rsync.html#exclude
        -h    Displays this usage message.
        -p    Destination path.
        -v    Verbose mode, displays syncing information.
```

When invoked, psy will look up for a psy.cfg file (more on it later on)
recursively starting from the current folder up the directory tree, the
that folder will be identified as the tree to synchronize (i.e. source),
if no psy.cfg will be found then the current directory will be used.

The settings in your psy.cfg can be overridden via the command's options.

Configuration
-------------

Create a psy.cfg in the folder that you want to synchronize, the following
settings are currently supported:

* destination, the destination path, any format supported by rsync
  would work as a BASH string
* excludes, a list of [patterns](http://ss64.com/bash/rsync.html#exclude)
  to exclude as a BASH array

### Examples ###

This is how work with SSH:
```bash
#
# The server knows you and is using the default port
destination="host:/path/to/destination"

# No ssh key deployed, pass all the info in the path
destination="user@host:port:/path/to/destination"
```

The rsync protocol and local paths are supported as well:
```bash
# This could be targetting another drive
# including network mounts and removable drives
destination="/path/to/destination"
```

And this is how you can exclude paths and files from being synchronized:
```bash
# This will exclude git's data, vim's swap files and the images folder
# at the root of the synchronization source, for more examples see
# [rsync pattern's documentation at http://ss64.com/bash/rsync.html#exclude
excludes=(".git/" "*.swp" "/images/")
```

This are the contents of a typical psy.cfg file for synchronizing
/source/path to /remote/path excuding mercurial's data and backup files:
```
# This file is stored at /source/path/psy.cfg
destination="host:/remote/path"
excludes=(".hg/" "*.bkp")
```
