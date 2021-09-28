# Build a plugin


This page presents how to build an external plugin,
i.e. a plugin which is not provided in the source code of SOFA.

Two ways of building a plugin is possible:
- you are compiling SOFA and you integrate your plugin project along SOFA (aka _in-tree build_).
- you have an install of SOFA (either you downloaded a [pre-built configuration](https://www.sofa-framework.org/download/), or you installed by yourself from a [SOFA build](https://www.sofa-framework.org/community/doc/getting-started/build/build-options/)), and you want to use this installation to build your plugin (aka _out-of-tree build_)

## In-tree build

#### Preferred file architecture

If you use the source code of SOFA, you want compile and use one or more
external plugins it is preferred to create one specific repository
outside SOFA where you can checkout all these external plugins.
This structure is preferred since it will allow a clean organization
of external plugins in one single repository.
Let's note the path to this repository */ext_plugin_repo/*.

In this directory, the structure is:

- ext_plugin_repo/
    - plugin1/
    - plugin2/
    - ...


#### CMakeList of the repository

In order to handle this repository as one single set of external plugins,
you need to write a short CMakeList.txt file as follows:

```
cmake_minimum_required(VERSION 2.8.12)

find_package(SofaFramework)

sofa_add_plugin(plugin1/  name_of_project_plugin1)
sofa_add_plugin(plugin2/  name_of_project_plugin2)
```

#### CMake option in SOFA

To compile all the external plugins located in this repository,
all you need to do is to set the path to this repository (*/ext_plugin_repo/*)
in the CMake variable: **SOFA\_EXTERNAL\_DIRECTORIES**.

This will directly configure and allow to compile all specified plugins from SOFA.

## Out-of-tree build

Your plugin will be considered as a standalone project, and SOFA will be simply a dependency for your CMake project.

#### CMake configuration

The main CMakeFile.txt of the plugin will not different from the _in-tree_ way.

Afterwards, in CMake (cmake, ccmake, cmake-gui), you just set the source to the root of your plugin, as any project based on CMake.

The only mandatory requirement for your project to find SOFA is to edit the CMake variable **CMAKE\_MODULE\_PATH** and set this value to lib/cmake from your SOFA installation.
<Image>

After setting the eventual third-party libraries (Qt, Eigen, etc), you will be able to build your plugin.

Finally, you are advised to install directly your plugin into the SOFA installation, essentially to load it more easily with runSofa.
This is possible by setting the CMake variable **CMAKE\_INSTALL\_PREFIX** in your plugin configuration.
