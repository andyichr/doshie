
    ██████╗  ██████╗ ███████╗██╗  ██╗██╗███████╗
    ██╔══██╗██╔═══██╗██╔════╝██║  ██║██║██╔════╝
    ██║  ██║██║   ██║███████╗███████║██║█████╗  
    ██║  ██║██║   ██║╚════██║██╔══██║██║██╔══╝  
    ██████╔╝╚██████╔╝███████║██║  ██║██║███████╗
    ╚═════╝  ╚═════╝ ╚══════╝╚═╝  ╚═╝╚═╝╚══════╝
                              ...at your service

A simple build-system bootstrapping program for developers who prefer to do
development in an interactive, isolated, docker-based environment. The idea is
to enable writing software which has complex, build-time dependencies to be
worked on in a consistent, repeatable way. The typical use-case involves
quickly starting a git project, adding *doshie*, running *./shell*, and coding.
The end result is, hopefully, a very rapid and low-friction way to begin
developent on arbitrarily complex software projects on a dev box having only
bash, git, and docker.

License
=======

The *doshie* project is released under the MIT license. See
[COPYING](COPYING.md) for more information

Requirements
============

Requirements were intentionally kept simple, and they are:

- git
- bash
- docker

Usage
=====

Simply add *doshie* as a git submodule in your project and *cp .doshie/shell* to
the root of your project. At this point, you will be able to run *./shell* from
within the root of your project, and you will be presented with an interactive
shell to use while developing your project.

```bash
# from the root of your project

# add the submodule
git submodule add https://github.com/chaconinc/DbConnector .doshie

# copy the shell cmd to your project's root

# typically, you want to commit this file as it rarely changes:
cp .doshie/shell ./
```

#### Start the Shell

```bash
./shell
```

The root directory of the project will be mounted as */src* within the shell.
In this way, it is possible to use editors and IDEs outside of the shell while
having the changes instantly reflected within the shell. Additionally, this
enables generating build output at a location accessible from the host machine,
such as *target/* in the root of the project. Typical commands run inside the
shell are *build*, *verify*, and *install*, but this depends on the project
itself and are not pre-defined by *doshie*.

#### Start the Shell w/ Command

It's possible to start the shell just to run a command:

```bash
./shell echo hi
```

### The Shell Container

Doshie creates a docker container (not an image) for the shell. This container
is reused and incrementally updated with each run of the shell. When the shell
is no longer needed, run

```bash
./shell --rm
```

to remove the existing shell container.

### Customizing the Shell Environment

We recommend creating a directory in the root of the project named *"env/"*.
Within this directory, add a shell script named something like *env/update*.

#### .doshie.rc

The *.doshie.rc* file, placed in the root of the project, enables some further
customization of *doshie*, such as specifying a base image (optional) and an
environment update command (optional):

```bash
DOSHIE_BASE_IMAGE=centos:centos7 # optional
DOSHIE_ENV_UPDATE=env/update     # optional
DOSHIE_DOCKER_FLAGS="-p 443:443" # optional
```

**NOTE:** If the base image is changed, there is no guarantee that the built-in
shell commands, such as *update*, will continue to function.

The command specified with *DOSHIE_ENV_UPDATE* should perform an incremental
update of the development environment. It will be executed at the first run of
*./shell*, and on every subsequent run. Typical commands executed here are
system updates, installing of packages, and so-on. Sometimes, if the project is
to be deployed as a *docker* image, a *Dockerfile* will be included in the
project root which will add and invoke the same environment update script,
producing a repeatable configuration for production as well as development.

### Updating

To update *doshie*, run

```bash
# from project root
git pull --recurse-submodules
git submodule update --recursive
```

in order to pull the latest code and update the submodule reference.

Known Issues
============

Currently, the shell runs as the default docker user (root). This may change in
the future, with a shell user being added with an id matching the host user's.
Alternatively, user namespacing may be used once docker supports it.
