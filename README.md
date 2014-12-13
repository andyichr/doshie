doshie
------

A simple build-system bootstrapping program for developers who prefer to do
development in an interactive, isolated, Docker-based environment. The idea is
to enable writing software which has complex, build-time dependencies to be
worked on in a consistent, repeatable way. The typical use-case involves
quickly starting a git project, adding *doshie*, defining a Dockerfile
including build (and often runtime depencies), running *./shell*, and coding.
The end result is, hopefully, a very rapid and low-friction way to begin
developent on arbitrarily complex software projects on a dev box having only
bash, git, and Docker.

License
=======

The *doshie* project is released under the MIT license. See COPYING.md for more
information

Usage
=====

Simply add *doshie* as a git submodule in your project and *cp doshie/shell* to
the root of your project. At this point, you will be able to run *./shell* from
within the root of your project, and you will be presented with an interactive
shell to use while developing your project.

    # TODO code here

The root directory of the project will be mounted as */src* within the shell. In
this way, it is possible to use editors and IDEs outside of the shell while
having the changes instantly reflected within the shell. Typical commands run
inside the shell are build, verify, and install, but this depends on the
project itself.
