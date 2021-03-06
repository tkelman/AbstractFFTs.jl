# FFT Implementations

## Existing packages

The following packages extend the functionality provided by AbstractFFTs:

* [FFTW.jl](https://github.com/JuliaMath/FFTW.jl): Bindings for the
  [FFTW](http://www.fftw.org) library. This also used to be part of Base Julia.

## Defining a new implementation

Implementations should implement `Base.A_mul_B!(Y, plan, X)` so as to support
pre-allocated output arrays.
We don't define `*` in terms of `A_mul_B!` generically here, however, because
of subtleties for in-place and real FFT plans.

To support `inv`, `\`, and `A_ldiv_B!(y, plan, x)`, we require `Plan` subtypes
to have a `pinv::Plan` field, which caches the inverse plan, and which should be
initially undefined.
They should also implement `plan_inv(p)` to construct the inverse of a plan `p`.

Implementations only need to provide the unnormalized backwards FFT,
similar to FFTW, and we do the scaling generically to get the inverse FFT.
