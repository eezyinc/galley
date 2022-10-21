# Galley 

Shell scripts to set up a
[cooking](https://github.com/scylladb/seastar/blob/master/HACKING.md) build
environment for Seastar on Digital Ocean.

## Configuration

Copy `config-example` to `~/.config/galley` and update it with at least the md5
hash of one of your Digital Ocean public ssh keys. You can get the md5 hash
[here](https://cloud.digitalocean.com/account/security).

You will also need
[doctl](https://docs.digitalocean.com/reference/doctl/how-to/install/) set up.

## Usage

`bin/setup` will start a Digital Ocean droplet, install required packages,
clone seastar + dpdk and begin the build in a tmux session. You will find logs
in `/build/apt.log` `/build/build.log` and `/build/git.log`. Use `tmux attach`
to connect to the build session or `tail -f /build/build.log` if you prefer. An
ssh command to connect to the server will be provided when `bin/setup`
completes.

`bin/destroy` will destroy the Digital Ocean droplet started by
`bin/setup`. Note if you run `bin/setup` multiple times it will create multiple
droplets and you will have to destroy them manually--the droplet IDs will be
provided in `bin/destroy`'s error message.
