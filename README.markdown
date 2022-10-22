# Galley 

Set up a [cooking](https://github.com/scylladb/seastar/blob/master/HACKING.md)
build environment for [Seastar](https://github.com/scylladb/seastar) with
Ubuntu on Digital Ocean.

## Setup

Galley is meant to be cloned into `~/.galley`. See `etc/default` for variables
and their default values; see `etc/example` for an example config. You'll want
to put the md5 hash of one of your Digital Ocean public ssh keys in the
`GALLEY_KEY` environment variable or override the variable `key` with it in
your config. You can get the md5 hash
[here](https://cloud.digitalocean.com/account/security).

You will also need
[doctl](https://docs.digitalocean.com/reference/doctl/how-to/install) set up.

## Usage

`bin/start <config>` will start a Digital Ocean droplet, install required packages,
clone seastar + dpdk and begin the build in a tmux session. You will find logs
in `/build/apt.log` `/build/build.log` and `/build/git.log`. An ssh command to
connect to the server will be provided when `bin/start` completes. Use `tmux
attach` to connect to the build session or `tail -f /build/build.log` if you
prefer.

`bin/destroy <config>` will destroy the Digital Ocean droplet started by
`bin/start`. Note if you run `bin/start` on a config more than once before
running `bin/destroy` it will create multiple droplets and you will have to
destroy them manually--the droplet IDs will be provided in `bin/destroy`'s
error message.

`bin/start default` and `bin/destroy default` should work without further
configuration if you have a valid `GALLEY_KEY` in your environment, but you
probably at least want to use a droplet with more CPUs.
