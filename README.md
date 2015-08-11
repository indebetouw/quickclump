# Dendroclump

Identify clumps within a 3D FITS datacube.

## Obtaining Dendroclump

GitHub: <https://github.com/vojtech-sidorin/dendroclump>  
Direct download link (GitHub):
[dendroclump-master.zip](https://github.com/vojtech-sidorin/dendroclump/archive/master.zip)  
Web: <http://galaxy.asu.cas.cz/~vosidorin/dendroclump.html>

## Quick start guide

To find clumps in your FITS data cube, type

    $ python dclump.py my_datacube.fits

To show usage help, type

    $ python dclump.py -h

To run in interactive mode, type

     $ python
     >>> import dclump
     >>> dclump.main(["my_datacube.fits"])

## Description

Dendroclump is an improved implementation of
[DENDROFIND](http://galaxy.asu.cas.cz/~richard/dendrofind/)
([Wunsch et al., 2012](http://adsabs.harvard.edu/abs/2012A%26A...539A.116W))
-- a clump-finding algorithm inspired by
[Clumpfind](http://www.ifa.hawaii.edu/users/jpw/clumpfind.shtml)
([Williams et al., 1994](http://adsabs.harvard.edu/abs/1994ApJ...428..693W)).
DENDROFIND was originally conceived by Richard Wunsch, who also published its
first implementation in Python, later rewritten in C.  Compared to DENDROFIND,
Dendroclump uses different data structures and doesn't need parameter `Nlevels`.
It, however, provides more accurate results -- comparable to what DENDROFIND
would provide with parameter `Nlevels` set to infinity.  Dendroclump is also
about 50 000 times faster than the C implementation of DENDROFIND and scales
linearly with the data cube volume (number of pixels).

Dendroclump was originally written for analysing astronomical data -- radio
observations of molecular clouds -- but its use is not limited to astronomy.
Dendroclump doesn't presume anything about the origin of the input data.  It will
find clumps in any 3-dimensional rectangular array served in a FITS file.

## Requirements

- Python 2.7/3.4 or above.
- Python packages:
    - numpy
    - astropy or pyfits

## Performance tests

Running Dendroclump on the whole LAB HI Survey (721x361x485 pixels)
([Kalberla et al., 2005](http://adsabs.harvard.edu//abs/2005A%26A...440..775K))
on CPU Intel i7-4770S took 10 minutes and the memory utilisation peaked at
870 MiB.  It found 4178 clumps.  Dendroclump was run with option
`--otext None`, i.e. producing no text output.

Tests with real CO data from the
[Galactic Ring Survey](http://www.bu.edu/galacticring/)
([Jackson et al., 2006](http://adsabs.harvard.edu//abs/2006ApJS..163..145J))
show that Dendroclump consumes up to 10 times the size of the input data cube.
Numpy's `std()` method is especially eager for memory and takes about 6 times
the size of the array (input data cube).  However, if you provide the
parameters `--dTleaf` and `--Tcutoff` at the command-line, the memory-hungry
numpy routines won't be called and the memory usage should stay below 5 times
the size of your input data cube.

## Notes

-   Tested in Python 2.7 and 3.4.
