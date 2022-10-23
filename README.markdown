# Galley 

Set up a Linux build environment with Ubuntu on Digital Ocean. Starts a
droplet, clones your project and initiates the build in a remote `tmux`
session. Use 48 CPUs to build while keeping your laptop cool with vintage
analog systems administration technology--it's like riding a code velocipede.

![BBS SysOps managing Their
Systems](https://pinecab.com/assets/images/velocipede.jpg "Is this
a kubernetes?")

## Setup

Galley is meant to be cloned into `~/.config/galley`, but you may set
`GALLEY_PATH` instead if you prefer. See `etc/defaults` for config variables
and their default values; see `etc/example` for an example config. You'll need
to put the md5 hash of one of your Digital Ocean public ssh keys in the
`GALLEY_KEY` environment variable or override the variable `key` with it in
your config. You can get the md5 hash
[here](https://cloud.digitalocean.com/account/security).

You will also need
[doctl](https://docs.digitalocean.com/reference/doctl/how-to/install)
installed.

You may wish to link or copy `bin/gal` into your `PATH` for convenience.

## 💣 Alert 🚨

These scripts are not battle hardened--in fact they're full of `grep`s and
`cut`s and wishful `sleep`s. Do not use them for production systems; don't even
use them on the same Digital Ocean account used for your production systems. I
don't run any production systems on Digital Ocean so I use Galley confident I
can't possibly do anything worse than run up an eye-watering bill forgetting to
shut down a 48-CPU build host.

## Usage

`gal s example` and `gal d example` should work without further configuration
if you have a valid `GALLEY_KEY` in your environment, but you probably at least
want to configure a project to build and use a droplet with more CPUs. For that
you use a config--see `etc/seastar` for an example config that creates a
[cooking](https://github.com/scylladb/seastar/blob/master/HACKING.md)
environment for [Seastar](https://github.com/scylladb/seastar).

### start

`gal s <config>` will start a Digital Ocean droplet, install required packages,
clone the repo and begin the build in a `tmux` session. You will find logs in
`/build/apt.log` `/build/build.log` and `/build/git.log`. An `ssh` command to
connect to the server will be provided when `gal start` completes. Use `tmux
attach` to connect to the build session or `tail -f /build/build.log` if you
prefer.

### destroy

`gal d <config>` will destroy the Digital Ocean droplet started by `gal start`.
Note if you run `gal start` on a config more than once before running `gal
destroy` it will create multiple droplets and you will have to destroy them
manually--the droplet IDs will be provided in `gal destroy`'s error message.
