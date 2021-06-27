# sshp
sshp - simple persistent ssh connections using screen

## The Main Problem To Solve

I often ssh into other machines, for work or play, and
I want to be able to put my desktop/laptop to sleep,
and not lose those connections or their context.

The same thing applies if my home internet connection goes
down, or my VPN connection resets.

Or if I move from one network to another.

sshp is a cover for ssh(1) and screen(1) that automatically
reconnects in the same context after a disruption.

For example: `xterm -e sshp user@host.example.com`

So you can have multiple ssh connections to various places
that will just re-establish themselves after a disconnect.

## How It Works

sshp takes arguments and options just like ssh(1),
plus a couple additional ones.  So you can do almost
anything with sshp as you do with ssh e.g. proxy hop
through a gateway host.

When invoked, sshp connects to the remote machine
and starts a screen(1) session running with a particular name.
It then starts a loop that ssh's to the remote machine
and connects to the existing screen session, and keeps
retrying (i.e. if/when the connection drops) to reconnect.
It continues this process, until it appears that the remote
session disconnected normally (i.e. on logout), and then exits.

## Quirks

If you forward your ssh agent to the remote machine, and reconnect,
the ssh agent environment variables in your remote shell will no
longer work.  The sshe(1) command tries to find a working current
socket for SSH_AUTH_SOCK.  You might find an alias like this useful
```
    alias ressh="eval `sshe`"
```
to reset your current shell's ssh environment.

If the remote machine reboots, your screen session dies with it.
So your sshp connection keeps trying to re-connect until the remote
reboot is complete, and then gives up (because it observes that
the screen session is gone).

if your local machine reboots, you'll leave a remote screen
session running, which you will likely want to re-connect to
and/or cleanup.  The -ls -Ls and -n options to sshp help
you deal with that situation.

## Why Did You Reinvent the Wheel?

There are other tools that do similar things, but they didn't
seem to solve the problem I had.  They often seemed to need
additional ports, protocols, or services.

The other tools obviously solve problems for lots of folks,
maybe you'll find them helpful.

Or, maybe I've overlooked something blindingly obvious.

- xpra - X persistent remote applications https://xpra.org/
- mosh(1) I keep thinking that mosh needs to pass UDP packets
  back and forth, and firewalls may not allow that?
- autossh(1) I think of autossh as being for more "permanent" connections.
- Ethernal  Terminal  https://eternal‐terminal.dev/  requires  another
  open port through your firewall(s) and (I believe) doesn't allow
  transparent host jumping the way ssh(1) does.

