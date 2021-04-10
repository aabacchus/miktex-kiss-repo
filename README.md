# miktex-kiss-repo

This is a repository which provides all the necessary dependencies to build
the MiKTeX distribution of TeX (other dependencies are in the official repos).

MiKTeX provides a package manager which can install packages as they are needed
at compilation time ("on-the-fly").

## Installation

To use, clone the repo and add it to `KISS_PATH`, then run:
```
$ kiss b miktex && kiss i miktex
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
