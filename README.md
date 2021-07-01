# miktex-kiss-repo

This is a repository which provides all the necessary dependencies to build
the MiKTeX distribution of TeX (other dependencies are in the official repos).
It works, but I intend to try to remove more of the dependencies.

MiKTeX provides a package manager which can install packages as they are needed
at compilation time ("on-the-fly"), which helps to keep the installed size
small(er).

NOTE: TeX Live, another TeX distribution, has also been packaged for KISS.

## NEWS

### MiKTeX 21.6.28
The latest release of MiKTeX uses a C++ class which is not implemented for Linux
(yet). See the issue [here].

### log4cxx
log4cxx 0.12.0 has been released upstream, including some changes to the version
of C++ being used, and MiKTeX's build is currently incompatible with it (see the 
[issue upstream]).
Therefore, log4cxx has not been updated (yet).

### gettext-tiny
This package of MiKTeX now uses [gettext-tiny] instead of gettext. This is much
more lightweight than the GNU version, weighing in at 340 kB compared to 19 MB.
Of course, it is less featureful, so gettext itself is still present if you want.
I'm undecided as to whether to make it replace gettext by name also or continue
to have a separate package. Let me know if you have any thoughts.

## Installation

To use, clone the repo and add it to `KISS_PATH`, then run:
```
$ kiss b graphite-harfbuzz
```
This will take a moment, then print a message telling you to run
`kiss alternatives`. MiKTeX needs the harfbuzz font libraries to be built with
graphite support, so now run:
```
$ kiss a | grep graphite-harfbuzz | kiss a -
```
to switch the necessary libraries to include graphite support.
Now, you can sit back and run
```
$ kiss b miktex
```
This might take a while.

## Finish setup

After installing, you need to complete the setup.
MiKTeX can be installed either

* for all users on your system, in which case you'll need root privileges;
* or just for your own user.

If installing user-wide, every command must be run as root (ie. use 
`sudo`, `doas`, or similar) and with the `--admin` flag.
Otherwise, omit these and run as a normal user. I'm going to give the commands
as to install MiKTeX for the whole system.

```
# mpm --admin --update-db
```

Enable on-the-fly package installation: (this is optional)
```
# initexmf --admin --set-config-value [MPM]AutoInstall=1
# initexmf --admin --update-fndb
```

Create symlinks to executables (such as `pdflatex`):
```
# initexmf --admin --mklinks
```

Install basic packages and fonts to `/usr/local/share`:
```
# mpm --admin --verbose --package-level=basic --upgrade
# initexmf --admin --mkmaps
# initexmf --admin --update-fndb
```

Finally, update the databases (you'll probably have to run these two several times)
```
# mpm --admin --update
$ mpm --update
```

Test compiling a sample document: (this will download some packages
if you enabled on-the-fly package installation)
```
$ pdflatex sample2e
```

## Troubleshooting

If anything fails, it's probably a network error, so try again.
Your first check should be the error logs (MiKTeX will tell you where
to find them).

Check the documentation at <https://miktex.org/howto/build-unx>.

The output of `initexmf --report`, both with and without
root privileges and `--admin`, may be useful.

Feel free to open an issue.

[gettext-tiny]: https://github.com/sabotage-linux/gettext-tiny
[issue upstream]: https://github.com/MiKTeX/miktex/issues/817
[here]: https://github.com/MiKTeX/miktex/issues/860
