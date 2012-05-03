2600hz packages, build suite
===========================

This repo contains the toolset needed to build 2600hz packages for all 
currently supported platforms. The builds are 'omnibus' style packages
that contain everything they need to run, including their own build of Ruby
and all required gems. 

The build suite relies heavily on Vagrant and FPM. 

VM's are used to build packages native to each OS. E.g: .rpm's on CentOS, and 
.deb's on Ubuntu. 

The vagrant boxes used for building are generally default/vanilla boxes built
from Veewee templates

Dependencies
------------

- A physical box to run VirtualBox
- Oracle VirtualBox
- Vagrant (only tested with Vagrant 1.x)
- rake (should work on 0.8.7+ and 0.9.x)
- GNU parallel(1)

Usage
-----

There are multiple ways to run the builders:

### Build packages on all supported platforms.

```
$ export 2600hz_VERSION=0.9.5
$ export BUILD_NUMBER=20        # could also use the jenkins build number
$ ./para-vagrant.sh
```

The `para-vagrant.sh` script will boot the VM's sequentially then run the 
Vagrant provision process (build) in parallel on each VM.

VM's are booted sequentially to avoid any VirtualBox kernel panics.

The concurrency can be controlled by setting `$MAX_PROCS` at the top of the
`para-vagrant.sh` script.

Detailed Logs of each provision process will be generated in the `logs/` 
directory.

### Build packages on a single platform, from outside the VM.

Make sure `2600hz_VERSION` and `BUILD_NUMBER` are exported in the environment.

```
$ vagrant up <BOX_NAME>
 ## or, if the box is already running:
$ vagrant provision <BOX_NAME>
```

You can always boot a box without running a build by executing:

```
$ vagrant up <BOX_NAME> --no-provision
```

### Build packages from within a VM (or build without Vagrant)

The builds can also be run without Vagrant, directly on a system (vm or
physical).

```
$ ./build.sh <2600hz_VERSION> <BUILD_NUMBER>
```

The build script will try to install some platform specific packages that 
are typically not installed on minimal systems such as our Vagrant images.

Rake can also be called directly if you're sure the system has all of the
necessary OS packages installed:

```
$ rake 2600hz_VERSION=<2600hz_version> BUILD_NUMBER=<build_number>
```

It's good to do a `rake clean` before building, but it's not necessary. Bunchr
will try to be smart and not re-run parts of the build process that have already
succeeded. Nonetheless, a clean should probably be done before an official
build.

Known Issues
-------------

* It's not uncommon to see VirtualBox kernel panic on Mac OSX:
  https://github.com/mitchellh/vagrant/issues/797

Future / TODO
-------------

See `TODO.md`

Author
------

* [Stephen Lum](https://twitter.com/steve_lum) 

License
-------

    Author:: Stephen Lum(<stephen@2600hz.com>)
    Copyright:: Copyright (c) 2012 Stephen Lum


