# Savanna contracts

Savanna contracts are a collection of contracts deployable to an [Antelope](https://github.com/AntelopeIO) blockchain, which implement verification of Savanna consensus rules in the context of inter blockchain communications (IBC), as well as finality violation proofs.

The collection of savanna contracts consists of the following individual contracts:

* [ibc contract](contracts/ibc/include/ibc.hpp): A contract that serves the purpose of verifying proofs of Savanna consensus finality of a distinct chain, a foundational system for inter-blockchain communications.

Note : Savanna contracts are currently NOT production-ready, have not been audited, and should not be employed for real world applications.

## Repository organization

The `main` branch contains the latest state of development; do not use this for production. Refer to the [releases page](https://github.com/AntelopeIO/savanna-contracts/releases) for current information on releases, pre-releases, and obsolete releases as well as the corresponding tags for those releases.

## Supported Operating Systems

[CDT](https://github.com/AntelopeIO/cdt) is required to build contracts. Any operating systems supported by CDT is sufficient to build the reference contracts.

To build and run the tests as well, [Spring](https://github.com/AntelopeIO/spring) is also required as a dependency, which may have its further restrictions on supported operating systems.
## Building

The build guide below will assume you are running Ubuntu 20.04. However, as mentioned above, other operating systems may also be supported.

### Build or install CDT dependency

The CDT dependency is required with a minimum version of 4.0.

The easiest way to satisfy this dependency is to install CDT on your system through a package. Find the release of a compatible version of CDT from its [releases page](https://github.com/AntelopeIO/cdt/releases), download the package file appropriate for your OS from the attached assets, and install the package.

Alternatively, you can build CDT from source. Please refer to the guide in the [CDT README](https://github.com/AntelopeIO/cdt#building-from-source) for instructions on how to do this. If you choose to go with building CDT from source, please keep the path to the build directory in the shell environment variable `CDT_BUILD_PATH` for later use when building the reference contracts.

### Optionally build Spring dependency

The Spring dependency is optional. It is only needed if you wish to also build the tests using the `BUILD_TESTS` CMake flag.

Unfortunately, it is not currently possible to satisfy the contract testing dependencies through the Spring packages made available from the [Spring releases page](https://github.com/AntelopeIO/spring/releases). So if you want to build the contract tests, you will first need to build Spring from source.

Please refer to the guide in the [Spring README](https://github.com/AntelopeIO/spring#building-from-source) for instructions on how to do this. If you choose to go with building Spring from source, please keep the path to the build directory in the shell environment variable `SPRING_BUILD_PATH` for later use when building the savanna contracts.

### Build savanna contracts

Beyond CDT and optionally Spring (if also building the tests), no additional dependencies are required to build the savanna contracts.

The instructions below assume you are building the savanna contracts with tests, have already built Spring from source, and have the CDT dependency installed on your system. For some other configurations, expand the hidden panels placed lower within this section.

For all configurations, you should first `cd` into the directory containing cloned savanna contracts repository.

Build savanna contracts with tests using Spring built from source and with installed CDT package:

```
mkdir build
cd build
cmake -DCMAKE_BUILD_TYPE=Release -DBUILD_TESTS=ON -Dspring_DIR="${SPRING_BUILD_PATH}/lib/cmake/spring" ..
make -j $(nproc)
```

**Note:** `CMAKE_BUILD_TYPE` has no impact on the WASM files generated for the contracts. It only impacts how the test binaries are built. Use `-DCMAKE_BUILD_TYPE=Debug` if you want to create test binaries that you can debug.

<details>
<summary>Build savanna contracts with tests using Spring and CDT both built from source</summary>

```
mkdir build
cd build
cmake -DCMAKE_BUILD_TYPE=Release -DBUILD_TESTS=ON -Dcdt_DIR="${CDT_BUILD_PATH}/lib/cmake/cdt" -Dspring_DIR="${SPRING_BUILD_PATH}/lib/cmake/spring" ..
make -j $(nproc)
```
</details>

<details>
<summary>Build savanna contracts without tests and with CDT build from source</summary>

```
mkdir build
cd build
cmake -DCMAKE_BUILD_TYPE=Release -DBUILD_TESTS=OFF -Dcdt_DIR="${CDT_BUILD_PATH}/lib/cmake/cdt" ..
make -j $(nproc)
```

</details>

#### Supported CMake options

The following is a list of custom CMake options supported in building the savanna contracts (default values are shown below):

```
-DBUILD_TESTS=OFF                       Do not build the tests
```

### Running tests

Assuming you built with `BUILD_TESTS=ON`, you can run the tests.

```
cd build/tests
ctest -j $(nproc)
```

## License

[MIT](LICENSE)