### Note: This package is being phased out.

The same functionality is available with [CuArrays](https://github.com/JuliaGPU/CuArrays.jl).

# CURAND.jl

Wrapper for NVidia's cuRAND library. 

## High-level interface

CURAND.jl provides the following list of high-level functions: 

 * `curand` - generate CudaArray of uniformly distributed numbers
 * `curandn` - generate CudaArray of normally distributed numbers
 * `curand_logn` - generate CudaArray of log-normally distributed numbers
 * `curand_poisson` - generate numbers according to Poisson distribution

Here are some examples:

```
using CUDAdrv
using CURAND

# generate CudaArray of 10 Float64 numbers and copy them to host to view
d_a = curand(10)
typeof(d_a)

# generate array of Float32 numbers
d_a = curand(Float32, 10)

```

These examples use default random number generator of type `CURAND_RNG_PSEUDO_DEFAULT`, you can also create custom generator and use in your calls: 

```
rng = create_generator(CURAND_RNG_QUASI_DEFAULT)

# create array of 10 uniformly distributed numbers using custom RNG
d_a = curand(rng, Float64, 10)

# create array of 10 normally distributed numbers with mean 0.0 
# and standard deviation 0.5 using cutsom RNG
d_a = curandn(rng, Float64, 10, 0.0, 0.5)
```

cuRAND support following RNG types (see cuRAND documentation for details):

```
CURAND_RNG_TEST
CURAND_RNG_PSEUDO_DEFAULT	
CURAND_RNG_PSEUDO_XORWOW	
CURAND_RNG_PSEUDO_MRG32K3A
CURAND_RNG_PSEUDO_MTGP32
CURAND_RNG_PSEUDO_MT19937
CURAND_RNG_PSEUDO_PHILOX4_32_10
CURAND_RNG_QUASI_DEFAULT
CURAND_RNG_QUASI_SOBOL32
CURAND_RNG_QUASI_SCRAMBLED_SOBOL32
CURAND_RNG_QUASI_SOBOL64
CURAND_RNG_QUASI_SCRAMBLED_SOBOL64
```

## Low-level API

There's also low-level API that wraps most functions from original `curand.h`. E.g. you can set a seed for a pseudo generator as follows: 

```
rng = create_generator()

set_pseudo_random_generator_seed(rng, 100)

# set seed for default RNG
set_pseudo_random_generator_seed(CURAND._rng, 100)
```

## TODO

Following functions are not implemented yet: 

 * `curandSetStream`
 * `curandGetDirectionVectors32`
 * `curandGetScrambleConstants32`
 * `curandGetDirectionVectors64`
 * `curandGetScrambleConstants64`

