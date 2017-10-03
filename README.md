# Overview

adl (which stands for "admin log") is a tool for recording the commands
you type on the command line while administering your computer.

So for example, when you install a package or edit a config file, you
can use `adl` to keep a track of the changes you've made. 

Your actions are logged to `/root/LOG`, in the form of a shell script.
This makes it very easy to:

- repeat all these commands if you later need to re-install your
  computer (by running `sh LOG`), and
- remind yourself how you installed or configured something.

See the [announcement] for more details.

[announcement]: http://effectif.com/articles/automate-your-sysadmin-with-adl

## Installation

Move the `adl` script to somewhere in your `$PATH`, and make sure it's
executable. For example, you might choose to put it in `/usr/local/bin`:

    $ sudo cp adl /usr/local/bin/
    $ sudo chmod +x /usr/local/bin/adl

## Usage

To record a command in the `LOG` file, prefix it with `adl`. The command
you type will be executed, and then recorded in the log.

So to give you an example, on a Debian-based computer, this command
would install the `ssh` package, and append 'apt-get install ssh' to
the `LOG` file:

    # adl apt-get install ssh

You can add block comments to document a group of commands with the `-C`
switch:

    # adl -C "Installing and configuring Apache"

If you want to record a change to a config file, use the `-e` switch to
open it in your editor:

    # adl -e /etc/hosts

When you save the file and quit your editor, `adl` will record the
change you made in a manner that can be repeated automatically with the
`patch` command.

Imagine that you've just used `adl` to edit the `/etc/hosts` file, adding the
IP address for a computer called "batman". The text that's written to the `LOG`
file when you quit your editor might look like this:

    patch /etc/hosts <<'EOPATCH'
    --- /etc/hosts.orig	2017-10-03 21:49:49.143301665 +0100
    +++ /etc/hosts	2017-10-03 21:50:00.663741961 +0100
    @@ -1,6 +1,8 @@
     127.0.0.1	localhost
     127.0.1.1	robin.lan	robin
     
    +192.168.1.2	batman.lan	batman
    +
     # The following lines are desirable for IPv6 capable hosts
     ::1     localhost ip6-localhost ip6-loopback
     ff02::1 ip6-allnodes
    EOPATCH

If we later wanted to repeat that edit for some reason, we could execute the
above code as part of a script, and it would take care of it for us.

That's pretty much it. `adl` records your system administration commands, as
you execute them, in a manner that allows you to just execute them all again later.

It's basic, yet powerful. I often find myself referring to the `LOG` file
simply to remind myself how I achieved something, and to provide me with a
boiler plate for reproducing it on other computers.
