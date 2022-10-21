# Galley 

Set up a [cooking](https://github.com/scylladb/seastar/blob/master/HACKING.md)
build environment for [Seastar](https://github.com/scylladb/seastar) with
Ubuntu on Digital Ocean.

## Configuration

Copy `config-example` to `~/.config/galley` and update it with, at a minimum,
the md5 hash of one of your Digital Ocean public ssh keys. You can get the md5
hash [here](https://cloud.digitalocean.com/account/security).

You will also need
[doctl](https://docs.digitalocean.com/reference/doctl/how-to/install) set up.

## Usage

`bin/start` will start a Digital Ocean droplet, install required packages,
clone seastar + dpdk and begin the build in a tmux session. You will find logs
in `/build/apt.log` `/build/build.log` and `/build/git.log`. An ssh command to
connect to the server will be provided when `bin/start` completes. Use `tmux
attach` to connect to the build session or `tail -f /build/build.log` if you
prefer.

`bin/destroy` will destroy the Digital Ocean droplet started by `bin/start`.
Note if you run `bin/start` more than once before running `bin/destroy` it will
create multiple droplets and you will have to destroy them manually--the
droplet IDs will be provided in `bin/destroy`'s error message.
