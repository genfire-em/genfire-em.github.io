# About
`GENFIRE`, for GENeralized Fourier Iterative REconstruction, is a software package implementing the algorithm of the same name for 3D image reconstruction from arbitrarily-oriented projection images. `GENFIRE` exists as both a Python implementation with a GUI programmed with `PyQt5` and as a bundle of `MATLAB` code.

## File formats / conventions

The Euler angle convention used by GENFIRE corresponds to that established by Heymann, Chagoyen and Belnap (2005). Each Euler angle triplet is expressed as (phi, theta, psi) where:

	- phi is a rotation about the z axis
	- theta is a rotation about the y' axis
	- psi is a rotation about the z'' axis

This Euler angle data should be provided as a 2D array of dimension num_projections x 3 with one row per projection. This may be either a .npy, .mat, or .txt file delimited in a way that it can be read by default with `numpy.loadtxt` (space-delimited works).

The projection data should be a 3D volume of size N x N x num_projections containing floating-point precision data. N should be an even number, and currently must be the same for both the X and Y dimensions of the projections.

The location of the rotational center is defined to be at pixel N/2 with 0-based indexing as in Python. In MATLAB, which uses 1-based indexing, this pixel corresponds to (N/2 + 1).

## Running reconstructions with Python

GENFIRE reconstructions may be run from within the `genfire` Python package through use of the `GenfireReconstructor` class. Simply instantiate a `GenfireReconstructor` with the desired reconstruction parameters and then invoke the `reconstruct` method. The reconstruction volume and associated error curves may then be saved using the helper function `gf.fileio.saveResults`. A simple example follows:

~~~ python
import genfire as gf

pars = dict(
	projectionFilename = "./data/projections.mat",
	angleFilename = "./data/angles.mat",
	supportFilename = "./data/support.mat",
	resultsFilename = "recon.mrc",
	numIterations=50,
	)

GF = gf.reconstruct.GenfireReconstructor(**pars)
results = GF.reconstruct()
gf.fileio.saveResults(results, GF.params.resultsFilename)
~~~

The full list of possible options and their associated default values may be viewed with `help gf.reconstruct.GenfireReconstructor`.