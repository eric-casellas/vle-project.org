+++
topics = ["VLE 2.0"]
tags = ["vle-2.0"]
title = "VLE 2.0"
+++

# What news?

### C++11/14

All the source code and API move to the C++11/14. Compiler
Requirements are now gcc 4.9 or clang 3.5 available in Ubuntu LTS
14.04 or Debian GNU/Linux Wheezy.

Many boost functions or classes are replaced with standard functions
and classes.

### Remove dependencies:

- The *libarchive-dev* is removed. We replace the *libarchive*
  dependency to build/extract tarball with the *cmake* sub-process
  command (`cmake -E tar [cxt][vf][zjJ] file.tar` command).

- All boost binary libraries are removed. `boost::random`,
  `boost::thread`, `boost::filesystem`, `boost::system`,
  `boost::date_time` and `boost::unittest_framework`.

### Changes in public API

- Less less and less.
- No singleton nor static variables.
- Only dependency with the standard C++ library.

### Kernel Parallelization

We add a thread pool into the kernel `src/vle/devs/Thread.hpp` to
parallelize with threads the PDEVS/DSDE immediate bag. It is the more
pessimist parallelization approach and gains are obtained with big
models, big transitions sources codes and big bags.

To active the parallelization in VLE, use the following command:

    vle -C vle.simulation.thread 8
    vle -C vle.simulation.block-size 128

These settings will use 8 threads (for an eight cores processor for
example) and the PDEVS/DSDE immediate bag will be treat in blocks of
128 simulators.

To disable the use of the parallelization mechanism, uses the
following commands:

    vle -C vle.simulation.thread 0
    vle -C vle.simulation.block-size 0

### 'System' binary packages

The Git repository of VLE provides some very useful packages for VLE
and GVLE (`vle.output`, `vle.aqss`, `gvle.simulation` etc.). These
packages installed in the `$prefix/lib/vle-2.0/pkgs` (`$prefix` is the
installation prefix of VLE. A read-only directory) simplify
installation of VLE: VLE can be use without using the Github packages
repository.

Default, priority goes to `$VLEHOME/pkgs-2.0` instead of system
packages `$prefix/lib/vle-2.0/pkgs`. So, if you want to develop
another `vle.output` package, nothing change.

### Debug

The Debug mode is available at run-time (without the use of
DECLARE\_DYNAMICS\_DBG macro). In the VPZ, we can provides a
`debug="yes"` attribute to the `model` element to debug the atomic
model. Before, we have to use the `DynamicsDbg.hpp` and replace the
inheritance from `vle::devs::Dynamics` to `vle::devs:DynamicsDbg`.

The `DynamicsDbg.hpp` header is removed from public API.

In private implementation, the debug mode use the
`vle::devs::DynamicsDbg` class placed between the
`vle::devs::Simulator` and user's dynamics model.

### Observation

The Observation mechanism use the same behaviors than debug mode. A
class between `vle::devs::Simulator` and the user's dynamics or debug
model (of course).

We adjust the VPZ to provide observation for output, internal,
external, confluent and finish functions by updating the `type`
attribute in the `View` element of the VPZ. Type can be:

- `event`: View::INTERNAL | View::EXTERNAL | View::CONFLUENT
- `output`: View::OUTPUT
- `internal`: View::INTERNAL;
- `external`: View::EXTERNAL;
- `confluent`: View::CONFLUENT;
- `finish`: View::FINISH;

### Model in executable

From now, `devs::ModelFactory` and `devs::StreamWriter` can load
symbol into the main program instead of the shared libraries specified
by the package information.

To use this feature, the package must be empty in the `vpz::Dynamics`
or `vpz::Output` object. Then the library is used to define the symbol
to load.

For example:

    vpz::Dynamics { Package = "toto", Library = "titi" }

Will loads the shared library
  `$VLE_HOME/pkgs-1.x/toto/lib/simulator/libtiti.so` and try to found
  the `vle_make_dynamics` symbol.

    vpz::Dynamics { Package = "", library="make_titi" }

Will load the symbol `make_titi` in the current executable.

If the library name starts with `exe_` or `wra_` then symbols
respectively define a `vle::devs::Executive` or a
`vle::devs::DynamicsWrapper`.

### GVLE

- *GVLE* is completely rewritten with QT 5. The old *GVLE* in *Gtkmm*
  [2,3] is removed and all plug-ins are disabled or rewritten.

### CMake

- The users CMake scripts are updated to simplify the development of
  packages.

----

# Upgrade models

- [upgrade from vle 1.1 to vle 1.2](upgrade-from-vle-1.1-to-vle-1.2)
- [upgrade from vle 1.2 to vle 2.0](upgrade-from-vle-1.2-to-vle-2.0)

# Download

[How to install](download).

# C++ API

The [C++ API documentations](http://www.vle-project.org/doxygen/master/)
are daily generated from the source code.
