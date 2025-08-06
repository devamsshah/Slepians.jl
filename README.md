# Slepians.jl

[![](https://img.shields.io/badge/docs-stable-blue.svg)](https://lootie.github.io/Slepians.jl/dev)
[![](https://img.shields.io/badge/docs-dev-blue.svg)](https://lootie.github.io/Slepians.jl/dev)

Slepians.jl is a package that solves the concentration problem of Slepian,
Landau and Pollak, numerically. Loosely speaking, we find a function that is
both finite in extent, and has a Fourier transform which lies in a certain
domain. In a single dimension, with discrete sampling, the discrete prolate 
spheroidal sequences solve this optimization problem. 

## Motivation
An elegant solution to the Slepian, Landau, and Pollak problem was needed to generate 3D-ΔPDF maps. However, current methods, especially the naive "punch and fill" algorithm, introduced unintended ripples and noise due to sharp edges during interpolation. The algorithm was sequential. This was a huge bottleneck in our applications of X-Ray analysis because we needed to process multiple gigabytes of data every minute. 


## Significance
To mitigate the noise and the runtime issues, I proposed leveraging the inherent sparsity of the data to our advantage. I used functions localized both in their [Fourier transforms](https://nbviewer.org/github/devamsshah/Slepians.jl/blob/master/Examples/00_DPSS.ipynb#Start-the-Quadrature) and the real space to minimize the leakage artifacts in the 3D-ΔPDF maps. Using a sparse representation for the diffuse scattering in the real space, I created interpretable PDFs with minimal noise. I used the Nystrom method and [DPSS](https://nbviewer.org/github/devamsshah/Slepians.jl/blob/master/Examples/00_DPSS.ipynb#Discrete-prolate-spheroidal-sequences) to solve the Fredholm equations

Solving Fredholm equations using the above methods also restructured the solution to be embarrassingly parallel. This solved the crucial problem relating to [runtime](https://nbviewer.org/github/devamsshah/Slepians.jl/blob/master/Examples/multithreaded_benchmarks.ipynb#Time-taken). The benchmark for these is available [here](https://nbviewer.org/github/devamsshah/Slepians.jl/blob/master/Examples/multithreaded_benchmarks.ipynb#Benchmarking-the-two-methods)

These improvements allowed my team to perform enhanced material characterization and improved data quality with broader applicability and optimized computational efficiency. 


# Installation

Slepians.jl is unregistered and relies on unregistered packages.  To avoid
difficulties in which Julia does not know the relevant URLS, I have created a
registry bbkt-reg.jl which will tell your installation where to find this
package and its dependencies. Begin with adding the registry using 

```
pkg> registry add https://github.com/lootie/bbkt-reg.jl
```

then one can simply add the Slepians.jl package as

```
pkg> add Slepians
```

# Examples

The jupyter notebooks in the `Examples/` directory illustrate the functionality
of this package.

# Funding

This material is based upon work supported by the U.S. Department of Energy,
Office of Science, Office of Basic Energy Sciences, Division of Materials
Science and Engineering. 

# References

Please see the below papers

```
@article{bronez1988spectral,
  title={Spectral estimation of irregularly sampled multidimensional processes by generalized prolate spheroidal sequences},
  author={Bronez, Thomas P},
  journal={IEEE Transactions on Acoustics, Speech, and Signal Processing},
  volume={36},
  number={12},
  pages={1862--1873},
  year={1988},
  publisher={IEEE}
}

@article{slepian1978prolate,
  title={Prolate spheroidal wave functions, Fourier analysis, and uncertainty—V: The discrete case},
  author={Slepian, David},
  journal={Bell System Technical Journal},
  volume={57},
  number={5},
  pages={1371--1430},
  year={1978},
  publisher={Wiley Online Library}
}

@article{simons2011spatiospectral,
  title={Spatiospectral concentration in the Cartesian plane},
  author={Simons, Frederik J and Wang, Dong V},
  journal={GEM-International Journal on Geomathematics},
  volume={2},
  number={1},
  pages={1--36},
  year={2011},
  publisher={Springer}
}
```
