# cxx - Rake based frontend for cxxproject

cxx defines a small ruby-dsl on top of cxxproject (see bake-toolkit
for another approach). the goal behind cxx is to have a very consise
and easy way of defining cpp-modules, that carry their own build
instructions and can easily composed into bigger software packages.

to facilitate this, the configuration of a typical software project is
split up into several project.rb files. each of these files describes
one or more components (like static or dynamic libraries). each of
these components has an unique name, that is used to reference them
from other components (e.g. a typical project would have several
static libraries that are referenced by executables).

each project.rb is a small ruby program. its entry point (called by
cxx) is named cxx_configuration. to define the components a special
api is provided (see cxx/eval_context.rb).

## plugins
in addition to the cxxproject toolchain plugins, cxx also provides the
means to extend the system. the api provided for this is a little
wider then the api of the toolchain plugins. the mechanism is very
similar to cxxproject:
- each plugin is a simple ruby gem following the naming scheme
  cxxproject_PLUGINNAME.
- each plugin must contain an entry point PLUGINNAME/plugin.rb which
  is executed by the system.
- to get more information about the system the plugin lifes in it
  should use the function cxx_plugin.
  - if the plugin does not need information of the cxx-system it can
    simply provide a block without arguments.
  - if the plugin need more information it should take 3 arguments:
    - ruby_dsl (see cxx/lib/cxx.rb)
    - a list of all building blocks (see
      cxxproject/lib/cxxproject/building_blocks)
    - a ruby logger
