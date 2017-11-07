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

GENFIRE reconstructions may be run from within the `genfire` Python package through use of the `GenfireReconstructor` class. Simply instantiate a `GenfireReconstructor` with the desired reconstruction parameters and then invoke the `reconstruct` method. The reconstruction volume and associated error curves may then be saved using the helper function `gf.fileio.saveResults`. A simple example follows. The full list of possible options and their associated default values may be viewed with `help gf.reconstruct.GenfireReconstructor`.

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

Which, when run from within Python, might produce the following output



	       ______  ________  ____  _____  ________  _____  _______     ________
	     .' ___  ||_   __  ||_   \|_   _||_   __  ||_   _||_   __ \   |_   __  |
	    / .'   \_|  | |_ \_|  |   \ | |    | |_ \_|  | |    | |__) |    | |_ \_|
	    | |   ____  |  _| _   | |\ \| |    |  _|     | |    |  __ /     |  _| _
	    \ `.___]  |_| |__/ | _| |_\   |_  _| |_     _| |_  _| |  \ \_  _| |__/ |
	     `._____.'|________||_____|\____||_____|   |_____||____| |___||________|

	        
	projectionFilename = ./data/projections.mat
	angleFilename = ./data/angles.mat
	supportFilename = ./data/support.mat
	resultsFilename = recon.mrc
	resolutionExtensionSuppressionState = 1
	numIterations = 50
	oversamplingRatio = 3
	interpolationCutoffDistance = 0.5
	useDefaultSupport = True
	calculateRfree = True
	initialObjectFilename = None
	constraint_positivity = True
	constraint_support = True
	enforceResolutionCircle = True
	permitMultipleGridding = True
	Reading projections from MATLAB file.

	Assembling Fourier grid.
	Fourier grid assembled in 1.3 seconds
	Reconstruction started
	Iteration number: 1           error = 1.00000
	Iteration number: 2           error = 0.91111
	Iteration number: 3           error = 0.89307
	Iteration number: 4           error = 0.88159
	Iteration number: 5           error = 0.56852
	Iteration number: 6           error = 0.52332
	Iteration number: 7           error = 0.34005
	Iteration number: 8           error = 0.28827
	Iteration number: 9           error = 0.26624
	Iteration number: 10           error = 0.19585
	Iteration number: 11           error = 0.16516
	Iteration number: 12           error = 0.14891
	Iteration number: 13           error = 0.11899
	Iteration number: 14           error = 0.10121
	Iteration number: 15           error = 0.09058
	Iteration number: 16           error = 0.07963
	Iteration number: 17           error = 0.07196
	Iteration number: 18           error = 0.06201
	Iteration number: 19           error = 0.05646
	Iteration number: 20           error = 0.05293
	Iteration number: 21           error = 0.05006
	Iteration number: 22           error = 0.04824
	Iteration number: 23           error = 0.04700
	Iteration number: 24           error = 0.04615
	Iteration number: 25           error = 0.04555
	Iteration number: 26           error = 0.04512
	Iteration number: 27           error = 0.04482
	Iteration number: 28           error = 0.04460
	Iteration number: 29           error = 0.04448
	Iteration number: 30           error = 0.04441
	Iteration number: 31           error = 0.04435
	Iteration number: 32           error = 0.04440
	Iteration number: 33           error = 0.04444
	Iteration number: 34           error = 0.04450
	Iteration number: 35           error = 0.04461
	Iteration number: 36           error = 0.04473
	Iteration number: 37           error = 0.04490
	Iteration number: 38           error = 0.04507
	Iteration number: 39           error = 0.04523
	Iteration number: 40           error = 0.04543
	Iteration number: 41           error = 0.04564
	Iteration number: 42           error = 0.04584
	Iteration number: 43           error = 0.04608
	Iteration number: 44           error = 0.04631
	Iteration number: 45           error = 0.04654
	Iteration number: 46           error = 0.04680
	Iteration number: 47           error = 0.04705
	Iteration number: 48           error = 0.04731
	Iteration number: 49           error = 0.04759
	Iteration number: 50           error = 0.04787
	Reconstruction finished in 16.4 seconds



