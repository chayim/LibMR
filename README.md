# LibMR
LibMR is a Map Reduce library that runs on top of Redis Cluster and agnostic to the deployment (oss/oss-cluster/enterpris-cluster). 

# Build

## Prerequisite
The following list must be installed before compiling the library
- build-essential (or anything equavalent on you OS)
- autoconf
- libtool
- libssl-dev
- gcc-9 or above (or clang)

## Build
Clone the repository and perform the following to build the static library:

```
git submodule update --init --recursive
make build_deps
make MODULE_NAME=<module name as given to RedisModule_Init function>
```

If all is done correctly, `libmr.a` file should be located under `src` directory.

# Build Tests

## Prerequisite
Other then the build prerequisite, the following list must be installed to run the tests
- python3.6 or above
- RLTest - https://github.com/RedisLabsModules/RLTest
- gevents
- redis
- rust

## Build
Clone the repository and perform the following to build and run the tests:

```
git submodule update --init --recursive
make clean
make build_deps
make run_tests
```

Notice that running the test will compile MRLib with `MODULE_NAME=MRTESTS`, please do not use this compilation with any other module other then the tests module as it will not work.
