n2p2 - The neural network potential package
===========================================

[![DOI](https://zenodo.org/badge/142296892.svg)](https://zenodo.org/badge/latestdoi/142296892)

This repository provides ready-to-use software for high-dimensional neural
network potentials in computational physics and chemistry.

# Build

This branch is a CMake-ready version of n2p2. Make you you have CMake v3.0 **at least** then:

```
mkdir -p build; cd build
cmake ..
make -j4
```

In the `build` directory there will be a `bin` and `lib` directories where the compiled executables/libraries are placed.

CMake will automatically locate Eigen3 and GSL. If that doesn't work, it could mean that you have either library in a non-standard path. Use `-DCMAKE_PREFIX_PATH=<path/to/eigen/installdir>` and `-DGSL_ROOT_DIR=</path/to/gsl>` to supply additional info to CMake.

# Documentation

## Online version
This package uses automatic documentation generation via
[Sphinx](http://www.sphinx-doc.org), [doxygen](http://www.doxygen.nl/) and
[Exhale](https://github.com/svenevs/exhale). An online version of the
documentation which is automatically updated with the main repository can be
found [__here__](http://compphysvienna.github.io/n2p2).

## Build your own documentation
It is also possible to build your own documentation for offline reading.
Install the above dependencies, change to the `src` directory and try to build
the documentation:
```
cd src
make doc
```
If the build process succeeds you can browse through the documentation starting
from the main page in:
```
doc/html/index.html
```

# Authors

 - Andreas Singraber (University of Vienna)

# License

This software is licensed under the [GNU General Public License version 3 or any later version (GPL-3.0-or-later)](https://www.gnu.org/licenses/gpl.txt).
